---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: ASP.net signalr Hubs-API-Handbuch-C#Server () | Microsoft-Dokumentation
author: bradygaster
description: Dieses Dokument enthält eine Einführung in die Programmierung der Serverseite der ASP.net signalr Hubs-API für signalr, Version 2, mit Codebeispielen, die zeigen...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c681b104b15bfc4a04587c7abf685dcf20def2ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431367"
---
# <a name="aspnet-signalr-hubs-api-guide---server-c"></a><span data-ttu-id="428c0-103">ASP.net signalr Hubs-API-Handbuch-C#Server ()</span><span class="sxs-lookup"><span data-stu-id="428c0-103">ASP.NET SignalR Hubs API Guide - Server (C#)</span></span>

<span data-ttu-id="428c0-104">von [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="428c0-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="428c0-105">Dieses Dokument enthält eine Einführung in die Programmierung der Serverseite der ASP.net signalr Hubs-API für signalr, Version 2, mit Codebeispielen, die allgemeine Optionen veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="428c0-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 2, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="428c0-106">Die signalr Hubs-API ermöglicht das Ausführen von Remote Prozedur aufrufen (Remote Procedure Calls, RPCs) von einem Server an verbundene Clients und von Clients an den Server.</span><span class="sxs-lookup"><span data-stu-id="428c0-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="428c0-107">In Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Client ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="428c0-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="428c0-108">Im Client Code definieren Sie Methoden, die vom Server aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Server ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="428c0-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="428c0-109">Signalr kümmert sich um die gesamte Client-zu-Server-Grundlagen.</span><span class="sxs-lookup"><span data-stu-id="428c0-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="428c0-110">Signalr bietet auch eine API auf niedrigerer Ebene, die als persistente Verbindungen bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="428c0-111">Eine Einführung in signalr, Hubs und persistente Verbindungen finden Sie unter [Einführung in signalr 2](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="428c0-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR 2](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="428c0-112">In diesem Thema verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="428c0-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="428c0-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="428c0-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="428c0-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="428c0-114">.NET 4.5</span></span>
> - <span data-ttu-id="428c0-115">Signalr Version 2</span><span class="sxs-lookup"><span data-stu-id="428c0-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="topic-versions"></a><span data-ttu-id="428c0-116">Themen Versionen</span><span class="sxs-lookup"><span data-stu-id="428c0-116">Topic versions</span></span>
> 
> <span data-ttu-id="428c0-117">Informationen zu früheren Versionen von signalr finden Sie unter [signalr ältere Versionen](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="428c0-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="428c0-118">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="428c0-118">Questions and comments</span></span>
> 
> <span data-ttu-id="428c0-119">Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten.</span><span class="sxs-lookup"><span data-stu-id="428c0-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="428c0-120">Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="428c0-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="428c0-121">Übersicht</span><span class="sxs-lookup"><span data-stu-id="428c0-121">Overview</span></span>

<span data-ttu-id="428c0-122">Dieses Dokument enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="428c0-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="428c0-123">Registrieren von signalr-Middleware</span><span class="sxs-lookup"><span data-stu-id="428c0-123">How to register SignalR middleware</span></span>](#route)

    - [<span data-ttu-id="428c0-124">Die/signalr-URL</span><span class="sxs-lookup"><span data-stu-id="428c0-124">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="428c0-125">Konfigurieren von signalr-Optionen</span><span class="sxs-lookup"><span data-stu-id="428c0-125">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="428c0-126">Erstellen und Verwenden von Hub-Klassen</span><span class="sxs-lookup"><span data-stu-id="428c0-126">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="428c0-127">Hub-Objekt Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="428c0-127">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="428c0-128">Kamel Schreibweise von Hub-Namen in JavaScript-Clients</span><span class="sxs-lookup"><span data-stu-id="428c0-128">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="428c0-129">Mehrere Hubs</span><span class="sxs-lookup"><span data-stu-id="428c0-129">Multiple Hubs</span></span>](#multiplehubs)
    - [<span data-ttu-id="428c0-130">Stark typisierte Hubs</span><span class="sxs-lookup"><span data-stu-id="428c0-130">Strongly-Typed Hubs</span></span>](#stronglytypedhubs)
- [<span data-ttu-id="428c0-131">Definieren von Methoden in der hubklasse, die von Clients aufgerufen werden können</span><span class="sxs-lookup"><span data-stu-id="428c0-131">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="428c0-132">Kamel Schreibweise von Methodennamen in JavaScript-Clients</span><span class="sxs-lookup"><span data-stu-id="428c0-132">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="428c0-133">Zeitpunkt der asynchronen Ausführung</span><span class="sxs-lookup"><span data-stu-id="428c0-133">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="428c0-134">Definieren von über Ladungen</span><span class="sxs-lookup"><span data-stu-id="428c0-134">Defining overloads</span></span>](#overloads)
    - [<span data-ttu-id="428c0-135">Berichten des Fortschritts von hubmethoden aufrufen</span><span class="sxs-lookup"><span data-stu-id="428c0-135">Reporting progress from hub method invocations</span></span>](#progress)
- [<span data-ttu-id="428c0-136">So werden Client Methoden aus der Hub-Klasse aufgerufen</span><span class="sxs-lookup"><span data-stu-id="428c0-136">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="428c0-137">Auswählen, welche Clients den RPC erhalten</span><span class="sxs-lookup"><span data-stu-id="428c0-137">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="428c0-138">Keine Validierung der Kompilierzeit für Methodennamen</span><span class="sxs-lookup"><span data-stu-id="428c0-138">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="428c0-139">Nicht Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="428c0-139">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="428c0-140">Asynchrone Ausführung</span><span class="sxs-lookup"><span data-stu-id="428c0-140">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="428c0-141">Verwalten der Gruppenmitgliedschaft mit der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="428c0-141">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="428c0-142">Asynchrone Ausführung von Add-und Remove-Methoden</span><span class="sxs-lookup"><span data-stu-id="428c0-142">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="428c0-143">Persistenz der Gruppenmitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="428c0-143">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="428c0-144">Einzelbenutzer Gruppen</span><span class="sxs-lookup"><span data-stu-id="428c0-144">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="428c0-145">Behandeln von Verbindungs Lebensdauer-Ereignissen in der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="428c0-145">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="428c0-146">Wenn "onconnected", "ongetrennte" und "onreconnected" aufgerufen werden</span><span class="sxs-lookup"><span data-stu-id="428c0-146">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="428c0-147">Aufruferstatus nicht aufgefüllt</span><span class="sxs-lookup"><span data-stu-id="428c0-147">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="428c0-148">So erhalten Sie Informationen über den Client aus der Kontext Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="428c0-148">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="428c0-149">Übergeben des Zustands zwischen Clients und der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="428c0-149">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="428c0-150">Behandeln von Fehlern in der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="428c0-150">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="428c0-151">Abrufen von Client Methoden und Verwalten von Gruppen von außerhalb der hubklasse</span><span class="sxs-lookup"><span data-stu-id="428c0-151">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="428c0-152">Aufrufen von Client Methoden</span><span class="sxs-lookup"><span data-stu-id="428c0-152">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="428c0-153">Verwalten der Gruppenmitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="428c0-153">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="428c0-154">Aktivieren der Ablauf Verfolgung</span><span class="sxs-lookup"><span data-stu-id="428c0-154">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="428c0-155">Vorgehensweise beim Anpassen der Hubs-Pipeline</span><span class="sxs-lookup"><span data-stu-id="428c0-155">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="428c0-156">Dokumentation zum Programmieren von Clients finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="428c0-156">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="428c0-157">Leitfaden für signalr-Hubs-API-JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="428c0-157">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)
- [<span data-ttu-id="428c0-158">Leitfaden für signalr-Hubs-API: .NET-Client</span><span class="sxs-lookup"><span data-stu-id="428c0-158">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="428c0-159">Die Serverkomponenten für signalr 2 sind nur in .NET 4,5 verfügbar.</span><span class="sxs-lookup"><span data-stu-id="428c0-159">The server components for SignalR 2 are only available in .NET 4.5.</span></span> <span data-ttu-id="428c0-160">Server, auf denen .NET 4,0 ausgeführt wird, müssen signalr v1. x verwenden.</span><span class="sxs-lookup"><span data-stu-id="428c0-160">Servers running .NET 4.0 must use SignalR v1.x.</span></span>

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a><span data-ttu-id="428c0-161">Registrieren von signalr-Middleware</span><span class="sxs-lookup"><span data-stu-id="428c0-161">How to register SignalR middleware</span></span>

<span data-ttu-id="428c0-162">Um die Route zu definieren, die von Clients zum Herstellen einer Verbindung mit dem Hub verwendet wird, müssen Sie beim Starten der Anwendung die `MapSignalR`-Methode abrufen.</span><span class="sxs-lookup"><span data-stu-id="428c0-162">To define the route that clients will use to connect to your Hub, call the `MapSignalR` method when the application starts.</span></span> <span data-ttu-id="428c0-163">`MapSignalR` ist eine [Erweiterungsmethode](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) für die `OwinExtensions`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="428c0-163">`MapSignalR` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `OwinExtensions` class.</span></span> <span data-ttu-id="428c0-164">Im folgenden Beispiel wird gezeigt, wie die signalr Hubs-Route mithilfe einer owin-Startklasse definiert wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-164">The following example shows how to define the SignalR Hubs route using an OWIN startup class.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

<span data-ttu-id="428c0-165">Wenn Sie signalr-Funktionen zu einer ASP.NET MVC-Anwendung hinzufügen, stellen Sie sicher, dass die signalr-Route vor den anderen Routen hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-165">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="428c0-166">Weitere Informationen finden Sie unter [Tutorial: ersten Schritte mit signalr 2 und MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="428c0-166">For more information, see [Tutorial: Getting Started with SignalR 2 and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="428c0-167">Die/signalr-URL</span><span class="sxs-lookup"><span data-stu-id="428c0-167">The /signalr URL</span></span>

<span data-ttu-id="428c0-168">Standardmäßig ist die Routen-URL, die von Clients zum Herstellen einer Verbindung mit ihrem Hub verwendet wird, "/signalr".</span><span class="sxs-lookup"><span data-stu-id="428c0-168">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="428c0-169">(Verwechseln Sie diese URL nicht mit der URL "/signalr/Hubs", die für die automatisch generierte JavaScript-Datei gilt.</span><span class="sxs-lookup"><span data-stu-id="428c0-169">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="428c0-170">Weitere Informationen zum generierten Proxy finden [Sie im signalr Hubs-API-Handbuch-JavaScript-Client-der generierte Proxy und dessen](hubs-api-guide-javascript-client.md#genproxy)Funktionsweise.)</span><span class="sxs-lookup"><span data-stu-id="428c0-170">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span></span>

<span data-ttu-id="428c0-171">Möglicherweise gibt es außergewöhnliche Umstände, in denen diese Basis-URL für signalr nicht verwendbar ist. Beispielsweise verfügen Sie über einen Ordner in Ihrem Projekt mit dem Namen *signalr* , und Sie möchten den Namen nicht ändern.</span><span class="sxs-lookup"><span data-stu-id="428c0-171">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="428c0-172">In diesem Fall können Sie die Basis-URL ändern, wie in den folgenden Beispielen gezeigt (ersetzen Sie "/signalr" im Beispielcode durch die gewünschte URL).</span><span class="sxs-lookup"><span data-stu-id="428c0-172">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="428c0-173">**Server Code, der die URL angibt**</span><span class="sxs-lookup"><span data-stu-id="428c0-173">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

<span data-ttu-id="428c0-174">**JavaScript-Client Code, der die URL (mit dem generierten Proxy) angibt**</span><span class="sxs-lookup"><span data-stu-id="428c0-174">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

<span data-ttu-id="428c0-175">**JavaScript-Client Code, der die URL (ohne den generierten Proxy) angibt**</span><span class="sxs-lookup"><span data-stu-id="428c0-175">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="428c0-176">**.NET-Client Code, der die URL angibt**</span><span class="sxs-lookup"><span data-stu-id="428c0-176">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="428c0-177">Konfigurieren von signalr-Optionen</span><span class="sxs-lookup"><span data-stu-id="428c0-177">Configuring SignalR Options</span></span>

<span data-ttu-id="428c0-178">Über Ladungen der `MapSignalR`-Methode ermöglichen es Ihnen, eine benutzerdefinierte URL, einen benutzerdefinierten Abhängigkeits Konflikt Löser und die folgenden Optionen anzugeben:</span><span class="sxs-lookup"><span data-stu-id="428c0-178">Overloads of the `MapSignalR` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="428c0-179">Aktivieren Sie Domänen übergreifende Aufrufe mithilfe von cors oder JSONP von Browser Clients.</span><span class="sxs-lookup"><span data-stu-id="428c0-179">Enable cross-domain calls using CORS or JSONP from browser clients.</span></span>

    <span data-ttu-id="428c0-180">Wenn der Browser eine Seite aus `http://contoso.com`lädt, befindet sich die signalr-Verbindung in der Regel in der gleichen Domäne `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="428c0-180">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="428c0-181">Wenn die Seite aus `http://contoso.com` eine Verbindung mit `http://fabrikam.com/signalr`herstellt, handelt es sich um eine Domänen übergreifende Verbindung.</span><span class="sxs-lookup"><span data-stu-id="428c0-181">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="428c0-182">Aus Sicherheitsgründen sind Domänen übergreifende Verbindungen standardmäßig deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="428c0-182">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="428c0-183">Weitere Informationen finden Sie unter [ASP.net signalr Hubs API Guide-JavaScript Client (Einrichten einer Domänen übergreifenden Verbindung](hubs-api-guide-javascript-client.md#crossdomain)).</span><span class="sxs-lookup"><span data-stu-id="428c0-183">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span></span>
- <span data-ttu-id="428c0-184">Aktivieren Sie detaillierte Fehlermeldungen.</span><span class="sxs-lookup"><span data-stu-id="428c0-184">Enable detailed error messages.</span></span>

    <span data-ttu-id="428c0-185">Wenn Fehler auftreten, besteht das Standardverhalten von signalr darin, eine Benachrichtigung an Clients zu senden, ohne Details dazu zu erhalten, was passiert ist.</span><span class="sxs-lookup"><span data-stu-id="428c0-185">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="428c0-186">Das Senden ausführlicher Fehlerinformationen an Clients wird in der Produktion nicht empfohlen, da böswillige Benutzer möglicherweise die Informationen in Angriffen gegen Ihre Anwendung verwenden können.</span><span class="sxs-lookup"><span data-stu-id="428c0-186">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="428c0-187">Zur Problembehandlung können Sie diese Option verwenden, um eine informative Fehlerberichterstattung temporär zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="428c0-187">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="428c0-188">Automatisch generierte JavaScript-Proxy Dateien deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="428c0-188">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="428c0-189">Standardmäßig wird eine JavaScript-Datei mit Proxys für Ihre Hub-Klassen als Antwort auf die URL "/signalr/Hubs" generiert.</span><span class="sxs-lookup"><span data-stu-id="428c0-189">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="428c0-190">Wenn Sie die JavaScript-Proxys nicht verwenden möchten oder wenn Sie diese Datei manuell generieren und auf eine physische Datei in ihren Clients verweisen möchten, können Sie diese Option verwenden, um die Proxy Generierung zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="428c0-190">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="428c0-191">Weitere Informationen finden Sie unter [signalr Hubs-API-Handbuch-JavaScript-Client-Erstellen einer physischen Datei für den von signalr generierten Proxy](hubs-api-guide-javascript-client.md#manualproxy).</span><span class="sxs-lookup"><span data-stu-id="428c0-191">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span></span>

<span data-ttu-id="428c0-192">Im folgenden Beispiel wird gezeigt, wie die signalr-Verbindungs-URL und diese Optionen in einem aufzurufenden `MapSignalR`-Methode angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="428c0-192">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapSignalR` method.</span></span> <span data-ttu-id="428c0-193">Um eine benutzerdefinierte URL anzugeben, ersetzen Sie "/signalr" im Beispiel durch die URL, die Sie verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="428c0-193">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="428c0-194">Erstellen und Verwenden von Hub-Klassen</span><span class="sxs-lookup"><span data-stu-id="428c0-194">How to create and use Hub classes</span></span>

<span data-ttu-id="428c0-195">Um einen Hub zu erstellen, erstellen Sie eine Klasse, die von [Microsoft. Aspnet. signalr. Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-195">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="428c0-196">Das folgende Beispiel zeigt eine einfache Hub-Klasse für eine Chat-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="428c0-196">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

<span data-ttu-id="428c0-197">In diesem Beispiel kann ein verbundener Client die `NewContosoChatMessage`-Methode aufzurufen. wenn dies der Fall ist, werden die empfangenen Daten an alle verbundenen Clients übermittelt.</span><span class="sxs-lookup"><span data-stu-id="428c0-197">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="428c0-198">Hub-Objekt Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="428c0-198">Hub object lifetime</span></span>

<span data-ttu-id="428c0-199">Die hubklasse wird nicht instanziiert, oder es werden keine Methoden aus dem eigenen Code auf dem Server aufgerufen. Das ist alles, was Sie von der signalr Hubs-Pipeline für Sie erledigt haben.</span><span class="sxs-lookup"><span data-stu-id="428c0-199">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="428c0-200">Signalr erstellt immer dann eine neue Instanz der Hub-Klasse, wenn Sie einen Hub-Vorgang verarbeiten muss, z. b. Wenn ein Client eine Verbindung herstellt, die Verbindung trennt oder einen Methodenaufruf an den Server ausführt.</span><span class="sxs-lookup"><span data-stu-id="428c0-200">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="428c0-201">Da Instanzen der Hub-Klasse flüchtig sind, können Sie Sie nicht verwenden, um den Zustand von einem Methodenaufrufe zum nächsten beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="428c0-201">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="428c0-202">Jedes Mal, wenn der Server einen Methodenaufruf von einem Client empfängt, wird die Nachricht von einer neuen Instanz der Hub-Klasse verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="428c0-202">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="428c0-203">Um den Zustand über mehrere Verbindungen und Methodenaufrufe beizubehalten, verwenden Sie eine andere Methode, z. b. eine Datenbank oder eine statische Variable für die Hub-Klasse, oder eine andere Klasse, die nicht von `Hub`abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="428c0-203">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="428c0-204">Wenn Sie Daten im Arbeitsspeicher beibehalten, werden die Daten bei Verwendung einer Methode, z. b. einer statischen Variablen in der Hub-Klasse, verloren gehen, wenn die APP-Domäne wieder verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-204">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="428c0-205">Wenn Sie Nachrichten aus Ihrem eigenen Code, der außerhalb der Hub-Klasse ausgeführt wird, an Clients senden möchten, können Sie dies nicht tun, indem Sie eine Hub-Klasseninstanz instanziieren. Sie können Sie jedoch durch einen Verweis auf das signalr-Kontext Objekt für die Hub-Klasse erstellen.</span><span class="sxs-lookup"><span data-stu-id="428c0-205">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="428c0-206">Weitere Informationen finden Sie weiter unten in diesem Thema unter [Abrufen von Client Methoden und Verwalten von Gruppen von außerhalb der Hub-Klasse](#callfromoutsidehub) .</span><span class="sxs-lookup"><span data-stu-id="428c0-206">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="428c0-207">Kamel Schreibweise von Hub-Namen in JavaScript-Clients</span><span class="sxs-lookup"><span data-stu-id="428c0-207">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="428c0-208">Standardmäßig verweisen JavaScript-Clients auf Hubs, indem Sie eine Version des Klassen namens mit Kamel Schreibung verwenden.</span><span class="sxs-lookup"><span data-stu-id="428c0-208">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="428c0-209">Signalr nimmt diese Änderung automatisch vor, damit JavaScript-Code JavaScript-Konventionen entsprechen kann.</span><span class="sxs-lookup"><span data-stu-id="428c0-209">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="428c0-210">Das vorherige Beispiel wird im JavaScript-Code als `contosoChatHub` bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="428c0-210">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="428c0-211">**Server**</span><span class="sxs-lookup"><span data-stu-id="428c0-211">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

<span data-ttu-id="428c0-212">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="428c0-212">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

<span data-ttu-id="428c0-213">Wenn Sie einen anderen Namen angeben möchten, der von Clients verwendet werden soll, fügen Sie das `HubName`-Attribut hinzu.</span><span class="sxs-lookup"><span data-stu-id="428c0-213">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="428c0-214">Wenn Sie ein `HubName` Attribut verwenden, gibt es keine Namensänderung in der Camel-Schreibweise auf JavaScript-Clients.</span><span class="sxs-lookup"><span data-stu-id="428c0-214">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="428c0-215">**Server**</span><span class="sxs-lookup"><span data-stu-id="428c0-215">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

<span data-ttu-id="428c0-216">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="428c0-216">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="428c0-217">Mehrere Hubs</span><span class="sxs-lookup"><span data-stu-id="428c0-217">Multiple Hubs</span></span>

<span data-ttu-id="428c0-218">Sie können mehrere Hub-Klassen in einer Anwendung definieren.</span><span class="sxs-lookup"><span data-stu-id="428c0-218">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="428c0-219">Wenn Sie dies tun, wird die Verbindung freigegeben, die Gruppen sind jedoch getrennt:</span><span class="sxs-lookup"><span data-stu-id="428c0-219">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="428c0-220">Alle Clients verwenden dieselbe URL, um eine signalr-Verbindung mit Ihrem Dienst herzustellen ("/signalr" oder Ihre benutzerdefinierte URL, wenn Sie eine solche URL angegeben haben), und diese Verbindung wird für alle vom Dienst definierten Hubs verwendet.</span><span class="sxs-lookup"><span data-stu-id="428c0-220">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="428c0-221">Es gibt keinen Leistungsunterschied für mehrere Hubs im Vergleich zum Definieren aller Hubfunktionen in einer einzelnen Klasse.</span><span class="sxs-lookup"><span data-stu-id="428c0-221">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="428c0-222">Alle Hubs erhalten die gleichen HTTP-Anforderungs Informationen.</span><span class="sxs-lookup"><span data-stu-id="428c0-222">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="428c0-223">Da alle Hubs die gleiche Verbindung gemeinsam nutzen, werden die HTTP-Anforderungs Informationen, die der Server erhält, in der ursprünglichen HTTP-Anforderung angezeigt, die die signalr-Verbindung herstellt.</span><span class="sxs-lookup"><span data-stu-id="428c0-223">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="428c0-224">Wenn Sie die Verbindungsanforderung verwenden, um Informationen vom Client an den Server zu übergeben, indem Sie eine Abfrage Zeichenfolge angeben, können Sie verschiedene Abfrage Zeichenfolgen nicht für verschiedene Hubs bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="428c0-224">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="428c0-225">Alle Hubs erhalten die gleichen Informationen.</span><span class="sxs-lookup"><span data-stu-id="428c0-225">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="428c0-226">Die generierte JavaScript-Proxydatei enthält Proxys für alle Hubs in einer Datei.</span><span class="sxs-lookup"><span data-stu-id="428c0-226">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="428c0-227">Weitere Informationen zu JavaScript [-Proxys finden Sie im signalr Hubs-API-Handbuch-JavaScript-Client-der generierte Proxy und dessen](hubs-api-guide-javascript-client.md#genproxy)Funktionsweise.</span><span class="sxs-lookup"><span data-stu-id="428c0-227">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span></span>
- <span data-ttu-id="428c0-228">Gruppen werden innerhalb von Hubs definiert.</span><span class="sxs-lookup"><span data-stu-id="428c0-228">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="428c0-229">In signalr können Sie benannte Gruppen für die Übertragung an Teilmengen verbundener Clients definieren.</span><span class="sxs-lookup"><span data-stu-id="428c0-229">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="428c0-230">Gruppen werden für jeden Hub separat verwaltet.</span><span class="sxs-lookup"><span data-stu-id="428c0-230">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="428c0-231">Beispielsweise würde eine Gruppe mit dem Namen "Administratoren" eine Reihe von Clients für Ihre `ContosoChatHub`-Klasse enthalten, und der gleiche Gruppenname bezieht sich auf einen anderen Satz von Clients für die `StockTickerHub` Klasse.</span><span class="sxs-lookup"><span data-stu-id="428c0-231">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a><span data-ttu-id="428c0-232">Stark typisierte Hubs</span><span class="sxs-lookup"><span data-stu-id="428c0-232">Strongly-Typed Hubs</span></span>

<span data-ttu-id="428c0-233">Um eine Schnittstelle für Ihre hubmethoden zu definieren, auf die der Client verweisen kann (und um IntelliSense für Ihre hubmethoden zu aktivieren), leiten Sie Ihren Hub von `Hub<T>` (eingeführt in signalr 2,1) anstatt `Hub`ab:</span><span class="sxs-lookup"><span data-stu-id="428c0-233">To define an interface for your hub methods that your client can reference (and enable Intellisense on your hub methods), derive your hub from `Hub<T>` (introduced in SignalR 2.1) rather than `Hub`:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="428c0-234">Definieren von Methoden in der hubklasse, die von Clients aufgerufen werden können</span><span class="sxs-lookup"><span data-stu-id="428c0-234">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="428c0-235">Wenn Sie eine Methode auf dem Hub verfügbar machen möchten, die vom Client aufgerufen werden soll, deklarieren Sie eine öffentliche Methode, wie in den folgenden Beispielen gezeigt.</span><span class="sxs-lookup"><span data-stu-id="428c0-235">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="428c0-236">Sie können einen Rückgabetyp und Parameter, einschließlich komplexer Typen und Arrays, wie in jeder beliebigen C# Methode angeben.</span><span class="sxs-lookup"><span data-stu-id="428c0-236">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="428c0-237">Alle Daten, die Sie in Parametern empfangen oder an den Aufrufer zurückgeben, werden mithilfe von JSON zwischen dem Client und dem Server übermittelt, und signalr übernimmt automatisch die Bindung komplexer Objekte und Arrays von Objekten.</span><span class="sxs-lookup"><span data-stu-id="428c0-237">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="428c0-238">Kamel Schreibweise von Methodennamen in JavaScript-Clients</span><span class="sxs-lookup"><span data-stu-id="428c0-238">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="428c0-239">Standardmäßig verweisen JavaScript-Clients auf hubmethoden, indem Sie eine Version des Methoden namens mit Kamel Schreibung verwenden.</span><span class="sxs-lookup"><span data-stu-id="428c0-239">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="428c0-240">Signalr nimmt diese Änderung automatisch vor, damit JavaScript-Code JavaScript-Konventionen entsprechen kann.</span><span class="sxs-lookup"><span data-stu-id="428c0-240">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="428c0-241">**Server**</span><span class="sxs-lookup"><span data-stu-id="428c0-241">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="428c0-242">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="428c0-242">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="428c0-243">Wenn Sie einen anderen Namen angeben möchten, der von Clients verwendet werden soll, fügen Sie das `HubMethodName`-Attribut hinzu.</span><span class="sxs-lookup"><span data-stu-id="428c0-243">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="428c0-244">**Server**</span><span class="sxs-lookup"><span data-stu-id="428c0-244">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="428c0-245">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="428c0-245">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="428c0-246">Zeitpunkt der asynchronen Ausführung</span><span class="sxs-lookup"><span data-stu-id="428c0-246">When to execute asynchronously</span></span>

<span data-ttu-id="428c0-247">Wenn die Methode eine lange Ausführungszeit erfordert oder Aufgaben ausführen müssen, die warten würden, wie z. b. eine Datenbanksuche oder ein Webdienst-aufrufbedarf, machen Sie die Hub-Methode asynchron, indem Sie eine [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (anstelle von `void` Rückgabe) oder [Aufgabe&lt;t&gt;](https://msdn.microsoft.com/library/dd321424.aspx) Objekt (anstelle `T` Rückgabe Typs) zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="428c0-247">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="428c0-248">Wenn Sie ein `Task` Objekt von der-Methode zurückgeben, wartet signalr darauf, dass die `Task` abgeschlossen ist, und sendet dann das entpackte Ergebnis an den Client zurück. es gibt also keinen Unterschied in der Art und Weise, wie Sie den Methodenaufrufe im Client codieren.</span><span class="sxs-lookup"><span data-stu-id="428c0-248">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="428c0-249">Durch die asynchrone Erstellung einer hubmethode wird verhindert, dass die Verbindung blockiert wird, wenn der WebSocket-Transport verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-249">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="428c0-250">Wenn eine hubmethode synchron ausgeführt wird und der Transport WebSocket ist, werden nachfolgende Aufrufe von Methoden auf dem Hub desselben Clients blockiert, bis die hubmethode abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="428c0-250">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="428c0-251">Im folgenden Beispiel wird die gleiche Methode gezeigt, die für die synchrone oder asynchrone Ausführung programmiert ist, gefolgt von JavaScript-Client Code, der zum Aufrufen einer beliebigen Version funktioniert.</span><span class="sxs-lookup"><span data-stu-id="428c0-251">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="428c0-252">**Synchrone**</span><span class="sxs-lookup"><span data-stu-id="428c0-252">**Synchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="428c0-253">**Asynchronen**</span><span class="sxs-lookup"><span data-stu-id="428c0-253">**Asynchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="428c0-254">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="428c0-254">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="428c0-255">Weitere Informationen zum Verwenden von asynchronen Methoden in ASP.NET 4,5 finden [Sie unter Verwenden von asynchronen Methoden in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="428c0-255">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="428c0-256">Definieren von über Ladungen</span><span class="sxs-lookup"><span data-stu-id="428c0-256">Defining Overloads</span></span>

<span data-ttu-id="428c0-257">Wenn Sie über Ladungen für eine Methode definieren möchten, muss die Anzahl von Parametern in den einzelnen über Ladungen abweichen.</span><span class="sxs-lookup"><span data-stu-id="428c0-257">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="428c0-258">Wenn Sie eine Überladung nur durch Angabe verschiedener Parametertypen unterscheiden, wird die hubklasse kompiliert, aber der signalr-Dienst löst zur Laufzeit eine Ausnahme aus, wenn Clients versuchen, eine der über Ladungen aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="428c0-258">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a><span data-ttu-id="428c0-259">Berichten des Fortschritts von hubmethoden aufrufen</span><span class="sxs-lookup"><span data-stu-id="428c0-259">Reporting progress from hub method invocations</span></span>

<span data-ttu-id="428c0-260">Signalr 2,1 fügt Unterstützung für das in .NET 4,5 eingeführte [Progress Reporting-Muster](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) hinzu.</span><span class="sxs-lookup"><span data-stu-id="428c0-260">SignalR 2.1 adds support for the [progress reporting pattern](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduced in .NET 4.5.</span></span> <span data-ttu-id="428c0-261">Um die Status Berichterstattung zu implementieren, definieren Sie einen `IProgress<T>` Parameter für die Hub-Methode, auf den der Client zugreifen kann:</span><span class="sxs-lookup"><span data-stu-id="428c0-261">To implement progress reporting, define an `IProgress<T>` parameter for your hub method that your client can access:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

<span data-ttu-id="428c0-262">Beim Schreiben einer Server Methode mit langer Ausführungszeit ist es wichtig, ein asynchrones Programmier Muster wie Async/warten zu verwenden, anstatt den Hub-Thread zu blockieren.</span><span class="sxs-lookup"><span data-stu-id="428c0-262">When writing a long-running server method, it is important to use an asynchronous programming pattern like Async/ Await rather than blocking the hub thread.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="428c0-263">So werden Client Methoden aus der Hub-Klasse aufgerufen</span><span class="sxs-lookup"><span data-stu-id="428c0-263">How to call client methods from the Hub class</span></span>

<span data-ttu-id="428c0-264">Um Client Methoden vom Server aufzurufen, verwenden Sie die `Clients`-Eigenschaft in einer Methode in ihrer Hub-Klasse.</span><span class="sxs-lookup"><span data-stu-id="428c0-264">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="428c0-265">Das folgende Beispiel zeigt Servercode, der `addNewMessageToPage` auf allen verbundenen Clients aufruft, und Client Code, der die-Methode in einem JavaScript-Client definiert.</span><span class="sxs-lookup"><span data-stu-id="428c0-265">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="428c0-266">**Server**</span><span class="sxs-lookup"><span data-stu-id="428c0-266">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

<span data-ttu-id="428c0-267">Das Aufrufen einer Client Methode ist ein asynchroner Vorgang und gibt einen `Task`zurück.</span><span class="sxs-lookup"><span data-stu-id="428c0-267">Invoking a client method is an asynchronous operation and returns a `Task`.</span></span> <span data-ttu-id="428c0-268">Verwenden Sie `await`:</span><span class="sxs-lookup"><span data-stu-id="428c0-268">Use `await`:</span></span>

* <span data-ttu-id="428c0-269">Um sicherzustellen, dass die Nachricht ohne Fehler gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-269">To ensure the message is sent without error.</span></span> 
* <span data-ttu-id="428c0-270">Um das Abfangen und behandeln von Fehlern in einem try-catch-Block zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="428c0-270">To enable catching and handling errors in a try-catch block.</span></span>

<span data-ttu-id="428c0-271">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="428c0-271">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

<span data-ttu-id="428c0-272">Sie können keinen Rückgabewert von einer Client Methode erhalten. Syntax, wie z. b. `int x = Clients.All.add(1,1)`, funktioniert nicht.</span><span class="sxs-lookup"><span data-stu-id="428c0-272">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="428c0-273">Sie können komplexe Typen und Arrays für die Parameter angeben.</span><span class="sxs-lookup"><span data-stu-id="428c0-273">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="428c0-274">Im folgenden Beispiel wird ein komplexer Typ in einem Methoden Parameter an den Client übergeben.</span><span class="sxs-lookup"><span data-stu-id="428c0-274">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="428c0-275">**Server Code, der eine Client Methode mithilfe eines komplexen Objekts aufruft**</span><span class="sxs-lookup"><span data-stu-id="428c0-275">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

<span data-ttu-id="428c0-276">**Server Code, der das komplexe Objekt definiert**</span><span class="sxs-lookup"><span data-stu-id="428c0-276">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

<span data-ttu-id="428c0-277">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="428c0-277">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="428c0-278">Auswählen, welche Clients den RPC erhalten</span><span class="sxs-lookup"><span data-stu-id="428c0-278">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="428c0-279">Die Clients-Eigenschaft gibt ein [hubconnectioncontext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) -Objekt zurück, das mehrere Optionen zum Angeben von Clients bereitstellt, die den RPC empfangen:</span><span class="sxs-lookup"><span data-stu-id="428c0-279">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="428c0-280">Alle verbundenen Clients.</span><span class="sxs-lookup"><span data-stu-id="428c0-280">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="428c0-281">Nur der aufrufenden Client.</span><span class="sxs-lookup"><span data-stu-id="428c0-281">Only the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="428c0-282">Alle Clients mit Ausnahme des aufrufenden Clients.</span><span class="sxs-lookup"><span data-stu-id="428c0-282">All clients except the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- <span data-ttu-id="428c0-283">Ein spezifischer Client, der durch die Verbindungs-ID identifiziert wird</span><span class="sxs-lookup"><span data-stu-id="428c0-283">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    <span data-ttu-id="428c0-284">In diesem Beispiel wird `addContosoChatMessageToPage` auf dem aufrufenden Client aufgerufen und hat dieselbe Auswirkung wie die Verwendung von `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="428c0-284">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="428c0-285">Alle verbundenen Clients mit Ausnahme der angegebenen Clients, identifiziert durch die Verbindungs-ID.</span><span class="sxs-lookup"><span data-stu-id="428c0-285">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- <span data-ttu-id="428c0-286">Alle verbundenen Clients in einer bestimmten Gruppe.</span><span class="sxs-lookup"><span data-stu-id="428c0-286">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- <span data-ttu-id="428c0-287">Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme der angegebenen Clients, identifiziert durch die Verbindungs-ID.</span><span class="sxs-lookup"><span data-stu-id="428c0-287">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- <span data-ttu-id="428c0-288">Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme des aufrufenden Clients.</span><span class="sxs-lookup"><span data-stu-id="428c0-288">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- <span data-ttu-id="428c0-289">Ein bestimmter Benutzer, der durch UserID identifiziert wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-289">A specific user, identified by userId.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    <span data-ttu-id="428c0-290">Standardmäßig ist dies `IPrincipal.Identity.Name`. Dies kann jedoch geändert werden, indem [eine Implementierung von iuseridprovider beim globalen Host registriert](mapping-users-to-connections.md#IUserIdProvider)wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-290">By default, this is `IPrincipal.Identity.Name`, but this can be changed by [registering an implementation of IUserIdProvider with the global host](mapping-users-to-connections.md#IUserIdProvider).</span></span>
- <span data-ttu-id="428c0-291">Alle Clients und Gruppen in einer Liste mit Verbindungs-IDs.</span><span class="sxs-lookup"><span data-stu-id="428c0-291">All clients and groups in a list of connection IDs.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- <span data-ttu-id="428c0-292">Eine Liste von Gruppen.</span><span class="sxs-lookup"><span data-stu-id="428c0-292">A list of groups.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- <span data-ttu-id="428c0-293">Ein Benutzer anhand des Namens.</span><span class="sxs-lookup"><span data-stu-id="428c0-293">A user by name.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- <span data-ttu-id="428c0-294">Eine Liste von Benutzernamen (eingeführt in signalr 2,1).</span><span class="sxs-lookup"><span data-stu-id="428c0-294">A list of user names (introduced in SignalR 2.1).</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="428c0-295">Keine Validierung der Kompilierzeit für Methodennamen</span><span class="sxs-lookup"><span data-stu-id="428c0-295">No compile-time validation for method names</span></span>

<span data-ttu-id="428c0-296">Der von Ihnen angegebene Methodenname wird als dynamisches Objekt interpretiert, was bedeutet, dass keine IntelliSense-oder Kompilierzeit Validierung dafür vorliegt.</span><span class="sxs-lookup"><span data-stu-id="428c0-296">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="428c0-297">Der Ausdruck wird zur Laufzeit ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="428c0-297">The expression is evaluated at run time.</span></span> <span data-ttu-id="428c0-298">Wenn der Methodenaufruf ausgeführt wird, sendet signalr den Methodennamen und die Parameterwerte an den Client. wenn der Client über eine Methode verfügt, die mit dem Namen übereinstimmt, wird diese Methode aufgerufen und die Parameterwerte an Sie übergeben.</span><span class="sxs-lookup"><span data-stu-id="428c0-298">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="428c0-299">Wenn keine übereinstimmende Methode auf dem Client gefunden wird, wird kein Fehler ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="428c0-299">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="428c0-300">Informationen über das Format der Daten, die von signalr im Hintergrund an den Client übermittelt werden, wenn Sie eine Client Methode aufruft, finden Sie unter [Introduction to signalr (Einführung in signalr](../getting-started/introduction-to-signalr.md)).</span><span class="sxs-lookup"><span data-stu-id="428c0-300">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="428c0-301">Nicht Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="428c0-301">Case-insensitive method name matching</span></span>

<span data-ttu-id="428c0-302">Beim Methodennamen wird die Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="428c0-302">Method name matching is case-insensitive.</span></span> <span data-ttu-id="428c0-303">Beispielsweise werden auf dem-Server `Clients.All.addContosoChatMessageToPage` auf dem-Server `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`oder `addContosoChatMessageToPage` auf dem Client ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="428c0-303">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="428c0-304">Asynchrone Ausführung</span><span class="sxs-lookup"><span data-stu-id="428c0-304">Asynchronous execution</span></span>

<span data-ttu-id="428c0-305">Die von Ihnen aufzurufende Methode wird asynchron ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="428c0-305">The method that you call executes asynchronously.</span></span> <span data-ttu-id="428c0-306">Jeglicher Code, der nach einem Methodenaufrufe an einen Client gesendet wird, wird sofort ausgeführt, ohne darauf zu warten, dass signalr die Übertragung von Daten an Clients abgeschlossen hat, es sei denn, Sie geben an, dass die nachfolgenden Codezeilen auf die</span><span class="sxs-lookup"><span data-stu-id="428c0-306">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="428c0-307">Im folgenden Codebeispiel wird gezeigt, wie zwei Client Methoden sequenziell ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="428c0-307">The following code example shows how to execute two client methods sequentially.</span></span>

<span data-ttu-id="428c0-308">**Verwenden von "warten" (.NET 4,5)**</span><span class="sxs-lookup"><span data-stu-id="428c0-308">**Using Await (.NET 4.5)**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="428c0-309">Wenn Sie `await` verwenden, um zu warten, bis eine Client Methode abgeschlossen ist, bevor die nächste Codezeile ausgeführt wird, bedeutet dies nicht, dass Clients die Nachricht tatsächlich empfangen, bevor die nächste Codezeile ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-309">If you use `await` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="428c0-310">Die Vervollständigung eines Client Methoden Aufrufes bedeutet nur, dass signalr alle notwendigen Schritte zum Senden der Nachricht ausgeführt hat.</span><span class="sxs-lookup"><span data-stu-id="428c0-310">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="428c0-311">Wenn Sie überprüfen müssen, ob die Nachricht von Clients empfangen wurde, müssen Sie diesen Mechanismus selbst programmieren.</span><span class="sxs-lookup"><span data-stu-id="428c0-311">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="428c0-312">Beispielsweise können Sie eine `MessageReceived` Methode auf dem Hub programmieren, und in der `addContosoChatMessageToPage`-Methode auf dem Client können Sie `MessageReceived` abrufen, nachdem Sie die Aufgaben ausgeführt haben, die Sie auf dem Client ausführen müssen.</span><span class="sxs-lookup"><span data-stu-id="428c0-312">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="428c0-313">In `MessageReceived` im Hub können Sie jede beliebige Arbeit von tatsächlichem Client Empfang und Verarbeitung des ursprünglichen Methoden Aufrufes abhängig machen.</span><span class="sxs-lookup"><span data-stu-id="428c0-313">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="428c0-314">Verwenden einer Zeichen folgen Variablen als Methodenname</span><span class="sxs-lookup"><span data-stu-id="428c0-314">How to use a string variable as the method name</span></span>

<span data-ttu-id="428c0-315">Wenn Sie eine Client Methode aufrufen möchten, indem Sie eine Zeichen folgen Variable als Methodenname verwenden, wandeln Sie `Clients.All` (oder `Clients.Others`, `Clients.Caller`usw.) in `IClientProxy` und rufen Sie dann aufrufen [(MethodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx)auf.</span><span class="sxs-lookup"><span data-stu-id="428c0-315">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="428c0-316">Verwalten der Gruppenmitgliedschaft mit der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="428c0-316">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="428c0-317">Gruppen in signalr bieten eine Methode zum Senden von Nachrichten an bestimmte Teilmengen verbundener Clients.</span><span class="sxs-lookup"><span data-stu-id="428c0-317">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="428c0-318">Eine Gruppe kann über eine beliebige Anzahl von Clients verfügen, und ein Client kann Mitglied einer beliebigen Anzahl von Gruppen sein.</span><span class="sxs-lookup"><span data-stu-id="428c0-318">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="428c0-319">Verwenden Sie zum Verwalten der Gruppenmitgliedschaft die Methoden zum [Hinzufügen](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) und [Entfernen](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) , die von der `Groups`-Eigenschaft der Hub-Klasse bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="428c0-319">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="428c0-320">Das folgende Beispiel zeigt die `Groups.Add`-und `Groups.Remove` Methoden, die in hubmethoden verwendet werden, die vom Client Code aufgerufen werden, gefolgt von JavaScript-Client Code, der Sie aufruft.</span><span class="sxs-lookup"><span data-stu-id="428c0-320">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="428c0-321">**Server**</span><span class="sxs-lookup"><span data-stu-id="428c0-321">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

<span data-ttu-id="428c0-322">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="428c0-322">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

<span data-ttu-id="428c0-323">Sie müssen nicht explizit Gruppen erstellen.</span><span class="sxs-lookup"><span data-stu-id="428c0-323">You don't have to explicitly create groups.</span></span> <span data-ttu-id="428c0-324">Tatsächlich wird eine Gruppe automatisch erstellt, wenn Sie Ihren Namen zum ersten Mal in einem Aufruf von `Groups.Add`angeben. Sie wird gelöscht, wenn Sie die letzte Verbindung aus der Mitgliedschaft entfernen.</span><span class="sxs-lookup"><span data-stu-id="428c0-324">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="428c0-325">Es ist keine API zum erhalten einer Gruppen Mitgliedschafts Liste oder einer Liste von Gruppen vorhanden.</span><span class="sxs-lookup"><span data-stu-id="428c0-325">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="428c0-326">Signalr sendet Nachrichten auf der Grundlage eines [Pub/Sub-Modells](http://en.wikipedia.org/wiki/Publish/subscribe)an Clients und Gruppen, und der Server verwaltet keine Listen mit Gruppen oder Gruppenmitgliedschaften.</span><span class="sxs-lookup"><span data-stu-id="428c0-326">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="428c0-327">Dadurch wird die Skalierbarkeit maximiert, denn wenn Sie einer Webfarm einen Knoten hinzufügen, muss jeder Status, den signalr beibehält, an den neuen Knoten weitergegeben werden.</span><span class="sxs-lookup"><span data-stu-id="428c0-327">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="428c0-328">Asynchrone Ausführung von Add-und Remove-Methoden</span><span class="sxs-lookup"><span data-stu-id="428c0-328">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="428c0-329">Die Methoden `Groups.Add` und `Groups.Remove` werden asynchron ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="428c0-329">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="428c0-330">Wenn Sie einer Gruppe einen Client hinzufügen und sofort mithilfe der-Gruppe eine Nachricht an den Client senden möchten, müssen Sie sicherstellen, dass die `Groups.Add`-Methode zuerst abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-330">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="428c0-331">Das folgende Codebeispiel zeigt, wie Sie dies tun.</span><span class="sxs-lookup"><span data-stu-id="428c0-331">The following code example shows how to do that.</span></span>

<span data-ttu-id="428c0-332">**Hinzufügen eines Clients zu einer Gruppe und anschließendes Messaging dieses Clients**</span><span class="sxs-lookup"><span data-stu-id="428c0-332">**Adding a client to a group and then messaging that client**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="428c0-333">Persistenz der Gruppenmitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="428c0-333">Group membership persistence</span></span>

<span data-ttu-id="428c0-334">Signalr verfolgt Verbindungen und keine Benutzer nach. Wenn Sie also möchten, dass sich ein Benutzer jedes Mal, wenn der Benutzer eine Verbindung herstellt, in derselben Gruppe befindet, müssen Sie `Groups.Add` jedes Mal anrufen, wenn der Benutzer eine neue Verbindung herstellt.</span><span class="sxs-lookup"><span data-stu-id="428c0-334">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="428c0-335">Nach einem vorübergehenden Verbindungsverlust kann die Verbindung manchmal von signalr automatisch wieder hergestellt werden.</span><span class="sxs-lookup"><span data-stu-id="428c0-335">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="428c0-336">In diesem Fall wird die gleiche Verbindung von signalr wieder hergestellt, und es wird keine neue Verbindung hergestellt, sodass die Gruppenmitgliedschaft des Clients automatisch wieder hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-336">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="428c0-337">Dies ist auch dann möglich, wenn die temporäre Unterbrechung das Ergebnis eines Serverneustarts oder eines Fehlers ist, da der Verbindungsstatus für jeden Client, einschließlich der Gruppenmitgliedschaften, auf den Client gerundet wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-337">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="428c0-338">Wenn ein Server ausfällt und vor dem Verbindungs Timeout durch einen neuen Server ersetzt wird, kann ein Client automatisch eine Verbindung mit dem neuen Server herstellen und sich erneut bei Gruppen anmelden, bei denen er Mitglied ist.</span><span class="sxs-lookup"><span data-stu-id="428c0-338">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="428c0-339">Wenn eine Verbindung nach einem Verbindungsverlust oder einem Verbindungs Timeout nicht automatisch wieder hergestellt werden kann, oder wenn der Client die Verbindung trennt (z. b. Wenn ein Browser zu einer neuen Seite navigiert), gehen Gruppenmitgliedschaften verloren.</span><span class="sxs-lookup"><span data-stu-id="428c0-339">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="428c0-340">Wenn der Benutzer das nächste Mal eine Verbindung herstellt, wird eine neue Verbindung hergestellt.</span><span class="sxs-lookup"><span data-stu-id="428c0-340">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="428c0-341">Um die Gruppenmitgliedschaften beizubehalten, wenn derselbe Benutzer eine neue Verbindung herstellt, muss Ihre Anwendung die Zuordnungen zwischen Benutzern und Gruppen nachverfolgen und Gruppenmitgliedschaften jedes Mal wiederherstellen, wenn ein Benutzer eine neue Verbindung herstellt.</span><span class="sxs-lookup"><span data-stu-id="428c0-341">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="428c0-342">Weitere Informationen zu Verbindungen und erneuten Verbindungen finden Sie unter [Behandeln von Verbindungs Lebensdauer-Ereignissen in der Hub-Klasse](#connectionlifetime) weiter unten in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="428c0-342">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="428c0-343">Einzelbenutzer Gruppen</span><span class="sxs-lookup"><span data-stu-id="428c0-343">Single-user groups</span></span>

<span data-ttu-id="428c0-344">Anwendungen, die signalr verwenden, müssen in der Regel die Zuordnungen zwischen Benutzern und Verbindungen nachverfolgen, um zu ermitteln, welcher Benutzer eine Nachricht gesendet hat und welche Benutzer eine Nachricht erhalten sollen.</span><span class="sxs-lookup"><span data-stu-id="428c0-344">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="428c0-345">Gruppen werden in einem der beiden häufig verwendeten Muster verwendet.</span><span class="sxs-lookup"><span data-stu-id="428c0-345">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="428c0-346">Einzelbenutzer Gruppen.</span><span class="sxs-lookup"><span data-stu-id="428c0-346">Single-user groups.</span></span>

    <span data-ttu-id="428c0-347">Sie können den Benutzernamen als Gruppennamen angeben und die aktuelle Verbindungs-ID der Gruppe jedes Mal hinzufügen, wenn der Benutzer eine Verbindung herstellt oder die Verbindung wiederherstellt.</span><span class="sxs-lookup"><span data-stu-id="428c0-347">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="428c0-348">Zum Senden von Nachrichten an den Benutzer, den Sie an die Gruppe senden.</span><span class="sxs-lookup"><span data-stu-id="428c0-348">To send messages to the user you send to the group.</span></span> <span data-ttu-id="428c0-349">Ein Nachteil dieser Methode besteht darin, dass die Gruppe Ihnen keine Möglichkeit bietet, herauszufinden, ob der Benutzer online oder offline ist.</span><span class="sxs-lookup"><span data-stu-id="428c0-349">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="428c0-350">Nachverfolgen von Zuordnungen zwischen Benutzernamen und Verbindungs-IDs.</span><span class="sxs-lookup"><span data-stu-id="428c0-350">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="428c0-351">Sie können eine Zuordnung zwischen den einzelnen Benutzernamen und mindestens einer Verbindungs-ID in einem Wörterbuch oder einer Datenbank speichern und die gespeicherten Daten jedes Mal aktualisieren, wenn der Benutzer eine Verbindung herstellt oder die Verbindung trennt.</span><span class="sxs-lookup"><span data-stu-id="428c0-351">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="428c0-352">Um Nachrichten an den Benutzer zu senden, geben Sie die Verbindungs-IDs an.</span><span class="sxs-lookup"><span data-stu-id="428c0-352">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="428c0-353">Ein Nachteil dieser Methode besteht darin, dass Sie mehr Arbeitsspeicher benötigt.</span><span class="sxs-lookup"><span data-stu-id="428c0-353">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="428c0-354">Behandeln von Verbindungs Lebensdauer-Ereignissen in der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="428c0-354">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="428c0-355">Typische Gründe für die Behandlung von Verbindungs Lebensdauer-Ereignissen besteht darin, zu verfolgen, ob ein Benutzer verbunden ist oder nicht, und die Zuordnung zwischen Benutzernamen und Verbindungs-IDs nachzuverfolgen.</span><span class="sxs-lookup"><span data-stu-id="428c0-355">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="428c0-356">Um eigenen Code auszuführen, wenn Clients eine Verbindung herstellen oder trennen, überschreiben Sie die virtuellen Methoden `OnConnected`, `OnDisconnected`und `OnReconnected` der Hub-Klasse, wie im folgenden Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="428c0-356">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="428c0-357">Wenn "onconnected", "ongetrennte" und "onreconnected" aufgerufen werden</span><span class="sxs-lookup"><span data-stu-id="428c0-357">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="428c0-358">Jedes Mal, wenn ein Browser zu einer neuen Seite navigiert, muss eine neue Verbindung eingerichtet werden. Dies bedeutet, dass signalr die `OnDisconnected`-Methode gefolgt von der `OnConnected`-Methode ausführt.</span><span class="sxs-lookup"><span data-stu-id="428c0-358">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="428c0-359">Signalr erstellt immer eine neue Verbindungs-ID, wenn eine neue Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-359">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="428c0-360">Die `OnReconnected`-Methode wird aufgerufen, wenn eine temporäre Verbindungsunterbrechung vorliegt, von der signalr automatisch wieder hergestellt werden kann, z. b. Wenn ein Kabel vorübergehend getrennt wird und die Verbindung wieder hergestellt wird, bevor ein Timeout der Verbindung auftritt. Die `OnDisconnected`-Methode wird aufgerufen, wenn der Client getrennt wird und signalr nicht automatisch wieder verbunden werden kann, z. b. Wenn ein Browser zu einer neuen Seite navigiert.</span><span class="sxs-lookup"><span data-stu-id="428c0-360">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="428c0-361">Daher ist eine mögliche Sequenz von Ereignissen für einen bestimmten Client `OnConnected`, `OnReconnected``OnDisconnected`; oder `OnConnected``OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="428c0-361">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="428c0-362">Die Sequenz `OnConnected`, `OnDisconnected``OnReconnected` für eine bestimmte Verbindung wird nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="428c0-362">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="428c0-363">Die `OnDisconnected`-Methode wird in einigen Szenarios nicht aufgerufen, z. b. Wenn ein Server ausfällt oder die APP-Domäne wieder verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-363">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="428c0-364">Wenn ein anderer Server Online ist oder die APP-Domäne die Wiederverwendung beendet, können einige Clients möglicherweise erneut eine Verbindung herstellen und das `OnReconnected` Ereignis auslösen.</span><span class="sxs-lookup"><span data-stu-id="428c0-364">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="428c0-365">Weitere Informationen finden Sie unter [verstehen und behandeln von Verbindungs Lebensdauer-Ereignissen in signalr](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="428c0-365">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="428c0-366">Aufruferstatus nicht aufgefüllt</span><span class="sxs-lookup"><span data-stu-id="428c0-366">Caller state not populated</span></span>

<span data-ttu-id="428c0-367">Die Ereignishandlermethoden der Verbindungs Lebensdauer werden vom Server aufgerufen, d. h. jeder Zustand, den Sie im `state` Objekt auf dem Client ablegen, wird nicht in der `Caller`-Eigenschaft auf dem Server aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="428c0-367">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="428c0-368">Weitere Informationen über das `state`-Objekt und die `Caller`-Eigenschaft finden Sie weiter unten in diesem Thema unter [übergeben des Zustands zwischen Clients und der Hub-Klasse](#passstate) .</span><span class="sxs-lookup"><span data-stu-id="428c0-368">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="428c0-369">So erhalten Sie Informationen über den Client aus der Kontext Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="428c0-369">How to get information about the client from the Context property</span></span>

<span data-ttu-id="428c0-370">Um Informationen über den Client zu erhalten, verwenden Sie die `Context`-Eigenschaft der Hub-Klasse.</span><span class="sxs-lookup"><span data-stu-id="428c0-370">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="428c0-371">Die `Context`-Eigenschaft gibt ein [hubcallercontext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) -Objekt zurück, das den Zugriff auf die folgenden Informationen ermöglicht:</span><span class="sxs-lookup"><span data-stu-id="428c0-371">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="428c0-372">Die Verbindungs-ID des aufrufenden Clients.</span><span class="sxs-lookup"><span data-stu-id="428c0-372">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    <span data-ttu-id="428c0-373">Die Verbindungs-ID ist eine GUID, die von signalr zugewiesen wird (Sie können den Wert nicht in Ihrem eigenen Code angeben).</span><span class="sxs-lookup"><span data-stu-id="428c0-373">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="428c0-374">Es gibt eine Verbindungs-ID für jede Verbindung, und die gleiche Verbindungs-ID wird von allen Hubs verwendet, wenn Sie über mehrere Hubs in Ihrer Anwendung verfügen.</span><span class="sxs-lookup"><span data-stu-id="428c0-374">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="428c0-375">HTTP-Header Daten.</span><span class="sxs-lookup"><span data-stu-id="428c0-375">HTTP header data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="428c0-376">Sie können auch HTTP-Header aus `Context.Headers`erhalten.</span><span class="sxs-lookup"><span data-stu-id="428c0-376">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="428c0-377">Der Grund für mehrere Verweise auf dasselbe Ergebnis ist, dass `Context.Headers` zuerst erstellt wurde, die `Context.Request`-Eigenschaft später hinzugefügt wurde und `Context.Headers` aus Gründen der Abwärtskompatibilität beibehalten wurde.</span><span class="sxs-lookup"><span data-stu-id="428c0-377">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="428c0-378">Abfrage Zeichen folgen Daten.</span><span class="sxs-lookup"><span data-stu-id="428c0-378">Query string data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    <span data-ttu-id="428c0-379">Sie können auch Abfrage Zeichenfolgen-Daten aus `Context.QueryString`erhalten.</span><span class="sxs-lookup"><span data-stu-id="428c0-379">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="428c0-380">Die Abfrage Zeichenfolge, die Sie in dieser Eigenschaft erhalten, ist die Abfrage Zeichenfolge, die mit der HTTP-Anforderung verwendet wurde, die die signalr-Verbindung</span><span class="sxs-lookup"><span data-stu-id="428c0-380">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="428c0-381">Sie können dem Client Abfrage Zeichenfolgen-Parameter hinzufügen, indem Sie die Verbindung konfigurieren. Dies ist eine bequeme Möglichkeit, Daten über den Client vom Client an den Server zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="428c0-381">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="428c0-382">Das folgende Beispiel zeigt eine Möglichkeit, eine Abfrage Zeichenfolge in einem JavaScript-Client hinzuzufügen, wenn Sie den generierten Proxy verwenden.</span><span class="sxs-lookup"><span data-stu-id="428c0-382">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    <span data-ttu-id="428c0-383">Weitere Informationen zum Festlegen von Abfrage Zeichen folgen Parametern finden Sie in den API-Handbüchern für die [JavaScript](hubs-api-guide-javascript-client.md) -und [.net](hubs-api-guide-net-client.md) -Clients.</span><span class="sxs-lookup"><span data-stu-id="428c0-383">For more information about setting query string parameters, see the API guides for the [JavaScript](hubs-api-guide-javascript-client.md) and [.NET](hubs-api-guide-net-client.md) clients.</span></span>

    <span data-ttu-id="428c0-384">Sie finden die Transportmethode, die für die Verbindung verwendet wird, in den Abfrage Zeichenfolgen-Daten zusammen mit einigen anderen Werten, die intern von signalr verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="428c0-384">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    <span data-ttu-id="428c0-385">Der Wert von `transportMethod` wird "websockets", "serversentevents", "foreverframe" oder "longabruf" lauten.</span><span class="sxs-lookup"><span data-stu-id="428c0-385">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="428c0-386">Beachten Sie Folgendes: Wenn Sie diesen Wert in der `OnConnected`-Ereignishandlermethode überprüfen, erhalten Sie in einigen Szenarien anfänglich möglicherweise einen Transport Wert, der nicht die endgültige aushandelte Transportmethode für die Verbindung ist.</span><span class="sxs-lookup"><span data-stu-id="428c0-386">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="428c0-387">In diesem Fall löst die Methode eine Ausnahme aus und wird später erneut aufgerufen, wenn die letzte Transportmethode eingerichtet wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-387">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="428c0-388">KS.</span><span class="sxs-lookup"><span data-stu-id="428c0-388">Cookies.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    <span data-ttu-id="428c0-389">Sie können auch Cookies aus `Context.RequestCookies`erhalten.</span><span class="sxs-lookup"><span data-stu-id="428c0-389">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="428c0-390">Benutzerinformationen.</span><span class="sxs-lookup"><span data-stu-id="428c0-390">User information.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- <span data-ttu-id="428c0-391">Das HttpContext-Objekt für die Anforderung:</span><span class="sxs-lookup"><span data-stu-id="428c0-391">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    <span data-ttu-id="428c0-392">Verwenden Sie diese Methode, anstatt `HttpContext.Current`, um das `HttpContext`-Objekt für die signalr-Verbindung zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="428c0-392">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="428c0-393">Übergeben des Zustands zwischen Clients und der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="428c0-393">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="428c0-394">Der-Client Proxy stellt ein `state` Objekt bereit, in dem Sie Daten speichern können, die mit jedem Methoden aufrufan den Server übertragen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="428c0-394">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="428c0-395">Auf dem-Server können Sie auf diese Daten in der `Clients.Caller`-Eigenschaft in hubmethoden zugreifen, die von-Clients aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="428c0-395">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="428c0-396">Die `Clients.Caller`-Eigenschaft wird für die Verbindungs Lebensdauer-Ereignishandlermethoden `OnConnected`, `OnDisconnected`und `OnReconnected`nicht aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="428c0-396">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="428c0-397">Das Erstellen oder Aktualisieren von Daten im `state` Objekt und in der `Clients.Caller`-Eigenschaft funktioniert in beide Richtungen.</span><span class="sxs-lookup"><span data-stu-id="428c0-397">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="428c0-398">Sie können Werte auf dem Server aktualisieren, die an den Client zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="428c0-398">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="428c0-399">Das folgende Beispiel zeigt JavaScript-Client Code, der den Status für die Übertragung an den Server mit jedem Methoden Aufrufwert speichert.</span><span class="sxs-lookup"><span data-stu-id="428c0-399">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

<span data-ttu-id="428c0-400">Das folgende Beispiel zeigt den entsprechenden Code in einem .NET-Client.</span><span class="sxs-lookup"><span data-stu-id="428c0-400">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

<span data-ttu-id="428c0-401">In ihrer Hub-Klasse können Sie auf diese Daten in der `Clients.Caller`-Eigenschaft zugreifen.</span><span class="sxs-lookup"><span data-stu-id="428c0-401">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="428c0-402">Das folgende Beispiel zeigt Code, der den Zustand abruft, auf den im vorherigen Beispiel verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-402">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="428c0-403">Dieser Mechanismus für das Beibehalten des Zustands ist nicht für große Datenmengen gedacht, da alles, was Sie in der `state`-oder `Clients.Caller`-Eigenschaft ablegen, bei jedem Methodenaufruf Roundtrip umfasst.</span><span class="sxs-lookup"><span data-stu-id="428c0-403">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="428c0-404">Dies ist nützlich für kleinere Elemente, wie z. b. Benutzernamen oder Leistungsindikatoren.</span><span class="sxs-lookup"><span data-stu-id="428c0-404">It's useful for smaller items such as user names or counters.</span></span>

<span data-ttu-id="428c0-405">In VB.net oder in einem stark typisierten Hub kann nicht über `Clients.Caller`auf das aufruferstatusobjekt zugegriffen werden. Verwenden Sie stattdessen `Clients.CallerState` (eingeführt in signalr 2,1):</span><span class="sxs-lookup"><span data-stu-id="428c0-405">In VB.NET or in a strongly-typed hub, the caller state object can't be accessed through `Clients.Caller`; instead, use `Clients.CallerState` (introduced in SignalR 2.1):</span></span>

<span data-ttu-id="428c0-406">**Verwenden von callerstate inC#**</span><span class="sxs-lookup"><span data-stu-id="428c0-406">**Using CallerState in C#**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

<span data-ttu-id="428c0-407">**Verwenden von callerstate in Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="428c0-407">**Using CallerState in Visual Basic**</span></span>

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="428c0-408">Behandeln von Fehlern in der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="428c0-408">How to handle errors in the Hub class</span></span>

<span data-ttu-id="428c0-409">Um Fehler zu behandeln, die in den hubklassen Methoden auftreten, stellen Sie zunächst sicher, dass Sie alle Ausnahmen von asynchronen Vorgängen (z. b. das Aufrufen von Client Methoden) mithilfe von `await`"beobachten.</span><span class="sxs-lookup"><span data-stu-id="428c0-409">To handle errors that occur in your Hub class methods, first ensure you "observe" any exceptions from async operations (such as invoking client methods) by using `await`.</span></span> <span data-ttu-id="428c0-410">Verwenden Sie dann mindestens eine der folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="428c0-410">Then use one or more of the following methods:</span></span>

- <span data-ttu-id="428c0-411">Packen Sie den Methoden Code in try-catch-Blöcken, und protokollieren Sie das Ausnahme Objekt.</span><span class="sxs-lookup"><span data-stu-id="428c0-411">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="428c0-412">Zu Debuggingzwecken können Sie die Ausnahme an den Client senden, aber aus Sicherheitsgründen wird das Senden detaillierter Informationen an Clients in der Produktionsumgebung nicht empfohlen.</span><span class="sxs-lookup"><span data-stu-id="428c0-412">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="428c0-413">Erstellen Sie ein Hubs Pipeline Modul, das die [onincomingerror](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) -Methode behandelt.</span><span class="sxs-lookup"><span data-stu-id="428c0-413">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="428c0-414">Das folgende Beispiel zeigt ein Pipeline Modul, das Fehler protokolliert, gefolgt von Code in Startup.cs, der das Modul in die Hubs-Pipeline einfügt.</span><span class="sxs-lookup"><span data-stu-id="428c0-414">The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- <span data-ttu-id="428c0-415">Verwenden Sie die `HubException`-Klasse (eingeführt in signalr 2).</span><span class="sxs-lookup"><span data-stu-id="428c0-415">Use the `HubException` class (introduced in SignalR 2).</span></span> <span data-ttu-id="428c0-416">Dieser Fehler kann von jedem hubaufruf ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="428c0-416">This error can be thrown from any hub invocation.</span></span> <span data-ttu-id="428c0-417">Der `HubError`-Konstruktor nimmt eine Zeichen folgen Nachricht und ein Objekt zum Speichern zusätzlicher Fehler Daten an.</span><span class="sxs-lookup"><span data-stu-id="428c0-417">The `HubError` constructor takes a string message, and an object for storing extra error data.</span></span> <span data-ttu-id="428c0-418">Signalr serialisiert die Ausnahme automatisch und sendet Sie an den Client, wo Sie verwendet wird, um den Aufruf der hubmethode abzulehnen oder fehlschlagen zu lassen.</span><span class="sxs-lookup"><span data-stu-id="428c0-418">SignalR will auto-serialize the exception and send it to the client, where it will be used to reject or fail the hub method invocation.</span></span>

    <span data-ttu-id="428c0-419">Die folgenden Codebeispiele veranschaulichen, wie eine `HubException` während eines hubaufzurufenden ausgelöst wird und wie die Ausnahme auf JavaScript-und .NET-Clients behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="428c0-419">The following code samples demonstrate how to throw a `HubException` during a Hub invocation, and how to handle the exception on JavaScript and .NET clients.</span></span>

    <span data-ttu-id="428c0-420">**Der Server Code demonstriert die hubexception-Klasse.**</span><span class="sxs-lookup"><span data-stu-id="428c0-420">**Server code demonstrating the HubException class**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    <span data-ttu-id="428c0-421">**JavaScript-Client Code, der die Antwort auf das Auslösen einer hubexception in einem Hub demonstriert**</span><span class="sxs-lookup"><span data-stu-id="428c0-421">**JavaScript client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    <span data-ttu-id="428c0-422">**.NET-Client Code, der die Antwort auf das Auslösen einer hubexception in einem Hub demonstriert**</span><span class="sxs-lookup"><span data-stu-id="428c0-422">**.NET client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

<span data-ttu-id="428c0-423">Weitere Informationen zu Hub-Pipeline Modulen finden Sie weiter unten in diesem Thema unter [Anpassen der Hubs-Pipeline](#hubpipeline) .</span><span class="sxs-lookup"><span data-stu-id="428c0-423">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="428c0-424">Aktivieren der Ablauf Verfolgung</span><span class="sxs-lookup"><span data-stu-id="428c0-424">How to enable tracing</span></span>

<span data-ttu-id="428c0-425">Um die serverseitige Ablauf Verfolgung zu aktivieren, fügen Sie der Datei "Web. config" ein System. Diagnostics-Element hinzu, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="428c0-425">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

<span data-ttu-id="428c0-426">Wenn Sie die Anwendung in Visual Studio ausführen, können Sie die Protokolle im **Ausgabe** Fenster anzeigen.</span><span class="sxs-lookup"><span data-stu-id="428c0-426">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="428c0-427">Abrufen von Client Methoden und Verwalten von Gruppen von außerhalb der hubklasse</span><span class="sxs-lookup"><span data-stu-id="428c0-427">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="428c0-428">Um Client Methoden aus einer anderen Klasse als der hubklasse aufzurufen, erhalten Sie einen Verweis auf das signalr-Kontext Objekt für den Hub und verwenden dieses, um Methoden auf dem Client aufzurufen oder Gruppen zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="428c0-428">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="428c0-429">Das folgende Beispiel `StockTicker` Klasse ruft das Kontext Objekt ab, speichert es in einer Instanz der-Klasse, speichert die-Klasseninstanz in einer statischen-Eigenschaft und verwendet den Kontext aus der Singleton-Klasseninstanz, um die `updateStockPrice`-Methode auf Clients aufzurufen, die mit einem Hub mit dem Namen `StockTickerHub`verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="428c0-429">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

<span data-ttu-id="428c0-430">Wenn Sie den Kontext mehrmals in einem langlebiges Objekt verwenden müssen, sollten Sie den Verweis einmal erhalten und speichern, anstatt ihn jedes Mal erneut zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="428c0-430">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="428c0-431">Wenn Sie den Kontext einmal erhalten, stellen Sie sicher, dass signalr Nachrichten in derselben Reihenfolge an Clients sendet, in der die hubmethoden Aufrufe der Client Methode ausführen.</span><span class="sxs-lookup"><span data-stu-id="428c0-431">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="428c0-432">Ein Tutorial, das zeigt, wie der signalr-Kontext für einen Hub verwendet wird, finden Sie unter [Server Broadcast mit ASP.net signalr](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="428c0-432">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="428c0-433">Aufrufen von Client Methoden</span><span class="sxs-lookup"><span data-stu-id="428c0-433">Calling client methods</span></span>

<span data-ttu-id="428c0-434">Sie können angeben, welche Clients den RPC erhalten sollen, aber Sie haben weniger Optionen als beim Aufruf von einer Hub-Klasse.</span><span class="sxs-lookup"><span data-stu-id="428c0-434">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="428c0-435">Der Grund hierfür ist, dass der Kontext keinem bestimmten Client von einem Client zugeordnet ist, sodass alle Methoden, die Kenntnisse der aktuellen Verbindungs-ID erfordern (z. b. `Clients.Others`oder `Clients.Caller`oder `Clients.OthersInGroup`), nicht verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="428c0-435">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="428c0-436">Die folgenden Optionen sind verfügbar:</span><span class="sxs-lookup"><span data-stu-id="428c0-436">The following options are available:</span></span>

- <span data-ttu-id="428c0-437">Alle verbundenen Clients.</span><span class="sxs-lookup"><span data-stu-id="428c0-437">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- <span data-ttu-id="428c0-438">Ein spezifischer Client, der durch die Verbindungs-ID identifiziert wird</span><span class="sxs-lookup"><span data-stu-id="428c0-438">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- <span data-ttu-id="428c0-439">Alle verbundenen Clients mit Ausnahme der angegebenen Clients, identifiziert durch die Verbindungs-ID.</span><span class="sxs-lookup"><span data-stu-id="428c0-439">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- <span data-ttu-id="428c0-440">Alle verbundenen Clients in einer bestimmten Gruppe.</span><span class="sxs-lookup"><span data-stu-id="428c0-440">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- <span data-ttu-id="428c0-441">Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme der angegebenen Clients, die durch die Verbindungs-ID identifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="428c0-441">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

<span data-ttu-id="428c0-442">Wenn Sie die nicht-Hub-Klasse von Methoden in ihrer Hub-Klasse aufrufen, können Sie die aktuelle Verbindungs-ID übergeben und diese mit `Clients.Client`, `Clients.AllExcept`oder `Clients.Group` verwenden, um `Clients.Caller`, `Clients.Others`oder `Clients.OthersInGroup`zu simulieren.</span><span class="sxs-lookup"><span data-stu-id="428c0-442">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="428c0-443">Im folgenden Beispiel übergibt die `MoveShapeHub`-Klasse die Verbindungs-ID an die `Broadcaster`-Klasse, damit die `Broadcaster`-Klasse `Clients.Others`simulieren kann.</span><span class="sxs-lookup"><span data-stu-id="428c0-443">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="428c0-444">Verwalten der Gruppenmitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="428c0-444">Managing group membership</span></span>

<span data-ttu-id="428c0-445">Zum Verwalten von Gruppen haben Sie dieselben Optionen wie in einer Hub-Klasse.</span><span class="sxs-lookup"><span data-stu-id="428c0-445">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="428c0-446">Hinzufügen eines Clients zu einer Gruppe</span><span class="sxs-lookup"><span data-stu-id="428c0-446">Add a client to a group</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- <span data-ttu-id="428c0-447">Entfernen eines Clients aus einer Gruppe</span><span class="sxs-lookup"><span data-stu-id="428c0-447">Remove a client from a group</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="428c0-448">Vorgehensweise beim Anpassen der Hubs-Pipeline</span><span class="sxs-lookup"><span data-stu-id="428c0-448">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="428c0-449">Mit signalr können Sie Ihren eigenen Code in die Hub-Pipeline einfügen.</span><span class="sxs-lookup"><span data-stu-id="428c0-449">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="428c0-450">Das folgende Beispiel zeigt ein benutzerdefiniertes Hub Pipeline Modul, das jeden eingehenden Methodenaufruf protokolliert, der vom Client empfangen wurde, und den Aufruf der ausgehenden Methode, der auf dem Client</span><span class="sxs-lookup"><span data-stu-id="428c0-450">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

<span data-ttu-id="428c0-451">Der folgende Code in der Datei *Startup.cs* registriert das Modul, das in der Hub-Pipeline ausgeführt werden soll:</span><span class="sxs-lookup"><span data-stu-id="428c0-451">The following code in the *Startup.cs* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

<span data-ttu-id="428c0-452">Es gibt viele verschiedene Methoden, die Sie überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="428c0-452">There are many different methods that you can override.</span></span> <span data-ttu-id="428c0-453">Eine umfassende Liste finden Sie unter [hubpipelinemodule-Methoden](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="428c0-453">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
