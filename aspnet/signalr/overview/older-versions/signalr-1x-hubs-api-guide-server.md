---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: ASP.net signalr Hubs-API-Handbuch-Server (signalr 1. x) | Microsoft-Dokumentation
author: bradygaster
description: Dieses Dokument enthält eine Einführung in die Programmierung der Serverseite der ASP.net signalr Hubs-API für signalr, Version 1,1, mit den Codebeispielen
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: d9cd3fad36c0300d96c6dbdc61291ef119da2327
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468129"
---
# <a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="31071-103">ASP.net signalr Hubs-API-Handbuch-Server (signalr 1. x)</span><span class="sxs-lookup"><span data-stu-id="31071-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>

<span data-ttu-id="31071-104">von [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="31071-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="31071-105">Dieses Dokument enthält eine Einführung in die Programmierung der Serverseite der ASP.net signalr Hubs-API für signalr, Version 1,1, mit Codebeispielen, die allgemeine Optionen veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="31071-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="31071-106">Die signalr Hubs-API ermöglicht das Ausführen von Remote Prozedur aufrufen (Remote Procedure Calls, RPCs) von einem Server an verbundene Clients und von Clients an den Server.</span><span class="sxs-lookup"><span data-stu-id="31071-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="31071-107">In Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Client ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="31071-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="31071-108">Im Client Code definieren Sie Methoden, die vom Server aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Server ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="31071-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="31071-109">Signalr kümmert sich um die gesamte Client-zu-Server-Grundlagen.</span><span class="sxs-lookup"><span data-stu-id="31071-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="31071-110">Signalr bietet auch eine API auf niedrigerer Ebene, die als persistente Verbindungen bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="31071-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="31071-111">Eine Einführung in signalr, Hubs und persistente Verbindungen oder ein Tutorial, das zeigt, wie Sie eine komplette signalr-Anwendung erstellen, finden Sie unter [signalr-Getting Started](index.md).</span><span class="sxs-lookup"><span data-stu-id="31071-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>

## <a name="overview"></a><span data-ttu-id="31071-112">Übersicht</span><span class="sxs-lookup"><span data-stu-id="31071-112">Overview</span></span>

<span data-ttu-id="31071-113">Dieses Dokument enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="31071-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="31071-114">Registrieren der signalr-Route und Konfigurieren von signalr-Optionen</span><span class="sxs-lookup"><span data-stu-id="31071-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="31071-115">Die/signalr-URL</span><span class="sxs-lookup"><span data-stu-id="31071-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="31071-116">Konfigurieren von signalr-Optionen</span><span class="sxs-lookup"><span data-stu-id="31071-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="31071-117">Erstellen und Verwenden von Hub-Klassen</span><span class="sxs-lookup"><span data-stu-id="31071-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="31071-118">Hub-Objekt Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="31071-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="31071-119">Kamel Schreibweise von Hub-Namen in JavaScript-Clients</span><span class="sxs-lookup"><span data-stu-id="31071-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="31071-120">Mehrere Hubs</span><span class="sxs-lookup"><span data-stu-id="31071-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="31071-121">Definieren von Methoden in der hubklasse, die von Clients aufgerufen werden können</span><span class="sxs-lookup"><span data-stu-id="31071-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="31071-122">Kamel Schreibweise von Methodennamen in JavaScript-Clients</span><span class="sxs-lookup"><span data-stu-id="31071-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="31071-123">Zeitpunkt der asynchronen Ausführung</span><span class="sxs-lookup"><span data-stu-id="31071-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="31071-124">Definieren von über Ladungen</span><span class="sxs-lookup"><span data-stu-id="31071-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="31071-125">So werden Client Methoden aus der Hub-Klasse aufgerufen</span><span class="sxs-lookup"><span data-stu-id="31071-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="31071-126">Auswählen, welche Clients den RPC erhalten</span><span class="sxs-lookup"><span data-stu-id="31071-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="31071-127">Keine Validierung der Kompilierzeit für Methodennamen</span><span class="sxs-lookup"><span data-stu-id="31071-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="31071-128">Nicht Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="31071-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="31071-129">Asynchrone Ausführung</span><span class="sxs-lookup"><span data-stu-id="31071-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="31071-130">Verwalten der Gruppenmitgliedschaft mit der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="31071-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="31071-131">Asynchrone Ausführung von Add-und Remove-Methoden</span><span class="sxs-lookup"><span data-stu-id="31071-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="31071-132">Persistenz der Gruppenmitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="31071-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="31071-133">Einzelbenutzer Gruppen</span><span class="sxs-lookup"><span data-stu-id="31071-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="31071-134">Behandeln von Verbindungs Lebensdauer-Ereignissen in der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="31071-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="31071-135">Wenn "onconnected", "ongetrennte" und "onreconnected" aufgerufen werden</span><span class="sxs-lookup"><span data-stu-id="31071-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="31071-136">Aufruferstatus nicht aufgefüllt</span><span class="sxs-lookup"><span data-stu-id="31071-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="31071-137">So erhalten Sie Informationen über den Client aus der Kontext Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="31071-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="31071-138">Übergeben des Zustands zwischen Clients und der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="31071-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="31071-139">Behandeln von Fehlern in der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="31071-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="31071-140">Abrufen von Client Methoden und Verwalten von Gruppen von außerhalb der hubklasse</span><span class="sxs-lookup"><span data-stu-id="31071-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="31071-141">Aufrufen von Client Methoden</span><span class="sxs-lookup"><span data-stu-id="31071-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="31071-142">Verwalten der Gruppenmitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="31071-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="31071-143">Aktivieren der Ablauf Verfolgung</span><span class="sxs-lookup"><span data-stu-id="31071-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="31071-144">Vorgehensweise beim Anpassen der Hubs-Pipeline</span><span class="sxs-lookup"><span data-stu-id="31071-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="31071-145">Dokumentation zum Programmieren von Clients finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="31071-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="31071-146">Leitfaden für signalr-Hubs-API-JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="31071-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="31071-147">Leitfaden für signalr-Hubs-API: .NET-Client</span><span class="sxs-lookup"><span data-stu-id="31071-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="31071-148">Links zu API-Referenz Themen beziehen sich auf die .NET 4,5-Version der API.</span><span class="sxs-lookup"><span data-stu-id="31071-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="31071-149">Wenn Sie .NET 4 verwenden, finden Sie weitere Informationen unter [.NET 4-Version der API-Themen](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="31071-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="31071-150">Registrieren der signalr-Route und Konfigurieren von signalr-Optionen</span><span class="sxs-lookup"><span data-stu-id="31071-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="31071-151">Um die Route zu definieren, die von Clients zum Herstellen einer Verbindung mit dem Hub verwendet wird, müssen Sie beim Starten der Anwendung die [maphubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) -Methode abrufen.</span><span class="sxs-lookup"><span data-stu-id="31071-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="31071-152">`MapHubs` ist eine [Erweiterungsmethode](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) für die `System.Web.Routing.RouteCollection`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="31071-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="31071-153">Im folgenden Beispiel wird gezeigt, wie die signalr Hubs-Route in der Datei " *Global. asax* " definiert wird.</span><span class="sxs-lookup"><span data-stu-id="31071-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="31071-154">Wenn Sie signalr-Funktionen zu einer ASP.NET MVC-Anwendung hinzufügen, stellen Sie sicher, dass die signalr-Route vor den anderen Routen hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="31071-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="31071-155">Weitere Informationen finden Sie unter [Tutorial: ersten Schritte mit signalr und MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="31071-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="31071-156">Die/signalr-URL</span><span class="sxs-lookup"><span data-stu-id="31071-156">The /signalr URL</span></span>

<span data-ttu-id="31071-157">Standardmäßig ist die Routen-URL, die von Clients zum Herstellen einer Verbindung mit ihrem Hub verwendet wird, "/signalr".</span><span class="sxs-lookup"><span data-stu-id="31071-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="31071-158">(Verwechseln Sie diese URL nicht mit der URL "/signalr/Hubs", die für die automatisch generierte JavaScript-Datei gilt.</span><span class="sxs-lookup"><span data-stu-id="31071-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="31071-159">Weitere Informationen zum generierten Proxy finden [Sie im signalr Hubs-API-Handbuch-JavaScript-Client-der generierte Proxy und dessen](index.md)Funktionsweise.)</span><span class="sxs-lookup"><span data-stu-id="31071-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="31071-160">Möglicherweise gibt es außergewöhnliche Umstände, in denen diese Basis-URL für signalr nicht verwendbar ist. Beispielsweise verfügen Sie über einen Ordner in Ihrem Projekt mit dem Namen *signalr* , und Sie möchten den Namen nicht ändern.</span><span class="sxs-lookup"><span data-stu-id="31071-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="31071-161">In diesem Fall können Sie die Basis-URL ändern, wie in den folgenden Beispielen gezeigt (ersetzen Sie "/signalr" im Beispielcode durch die gewünschte URL).</span><span class="sxs-lookup"><span data-stu-id="31071-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="31071-162">**Server Code, der die URL angibt**</span><span class="sxs-lookup"><span data-stu-id="31071-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="31071-163">**JavaScript-Client Code, der die URL (mit dem generierten Proxy) angibt**</span><span class="sxs-lookup"><span data-stu-id="31071-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="31071-164">**JavaScript-Client Code, der die URL (ohne den generierten Proxy) angibt**</span><span class="sxs-lookup"><span data-stu-id="31071-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="31071-165">**.NET-Client Code, der die URL angibt**</span><span class="sxs-lookup"><span data-stu-id="31071-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="31071-166">Konfigurieren von signalr-Optionen</span><span class="sxs-lookup"><span data-stu-id="31071-166">Configuring SignalR Options</span></span>

<span data-ttu-id="31071-167">Über Ladungen der `MapHubs`-Methode ermöglichen es Ihnen, eine benutzerdefinierte URL, einen benutzerdefinierten Abhängigkeits Konflikt Löser und die folgenden Optionen anzugeben:</span><span class="sxs-lookup"><span data-stu-id="31071-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="31071-168">Aktivieren Sie Domänen übergreifende Aufrufe von Browser Clients.</span><span class="sxs-lookup"><span data-stu-id="31071-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="31071-169">Wenn der Browser eine Seite aus `http://contoso.com`lädt, befindet sich die signalr-Verbindung in der Regel in der gleichen Domäne `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="31071-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="31071-170">Wenn die Seite aus `http://contoso.com` eine Verbindung mit `http://fabrikam.com/signalr`herstellt, handelt es sich um eine Domänen übergreifende Verbindung.</span><span class="sxs-lookup"><span data-stu-id="31071-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="31071-171">Aus Sicherheitsgründen sind Domänen übergreifende Verbindungen standardmäßig deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="31071-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="31071-172">Weitere Informationen finden Sie unter [ASP.net signalr Hubs API Guide-JavaScript Client (Einrichten einer Domänen übergreifenden Verbindung](index.md)).</span><span class="sxs-lookup"><span data-stu-id="31071-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="31071-173">Aktivieren Sie detaillierte Fehlermeldungen.</span><span class="sxs-lookup"><span data-stu-id="31071-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="31071-174">Wenn Fehler auftreten, besteht das Standardverhalten von signalr darin, eine Benachrichtigung an Clients zu senden, ohne Details dazu zu erhalten, was passiert ist.</span><span class="sxs-lookup"><span data-stu-id="31071-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="31071-175">Das Senden ausführlicher Fehlerinformationen an Clients wird in der Produktion nicht empfohlen, da böswillige Benutzer möglicherweise die Informationen in Angriffen gegen Ihre Anwendung verwenden können.</span><span class="sxs-lookup"><span data-stu-id="31071-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="31071-176">Zur Problembehandlung können Sie diese Option verwenden, um eine informative Fehlerberichterstattung temporär zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="31071-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="31071-177">Automatisch generierte JavaScript-Proxy Dateien deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="31071-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="31071-178">Standardmäßig wird eine JavaScript-Datei mit Proxys für Ihre Hub-Klassen als Antwort auf die URL "/signalr/Hubs" generiert.</span><span class="sxs-lookup"><span data-stu-id="31071-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="31071-179">Wenn Sie die JavaScript-Proxys nicht verwenden möchten oder wenn Sie diese Datei manuell generieren und auf eine physische Datei in ihren Clients verweisen möchten, können Sie diese Option verwenden, um die Proxy Generierung zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="31071-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="31071-180">Weitere Informationen finden Sie unter [signalr Hubs-API-Handbuch-JavaScript-Client-Erstellen einer physischen Datei für den von signalr generierten Proxy](index.md).</span><span class="sxs-lookup"><span data-stu-id="31071-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="31071-181">Im folgenden Beispiel wird gezeigt, wie die signalr-Verbindungs-URL und diese Optionen in einem aufzurufenden `MapHubs`-Methode angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="31071-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="31071-182">Um eine benutzerdefinierte URL anzugeben, ersetzen Sie "/signalr" im Beispiel durch die URL, die Sie verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="31071-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="31071-183">Erstellen und Verwenden von Hub-Klassen</span><span class="sxs-lookup"><span data-stu-id="31071-183">How to create and use Hub classes</span></span>

<span data-ttu-id="31071-184">Um einen Hub zu erstellen, erstellen Sie eine Klasse, die von [Microsoft. Aspnet. signalr. Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="31071-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="31071-185">Das folgende Beispiel zeigt eine einfache Hub-Klasse für eine Chat-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="31071-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="31071-186">In diesem Beispiel kann ein verbundener Client die `NewContosoChatMessage`-Methode aufzurufen. wenn dies der Fall ist, werden die empfangenen Daten an alle verbundenen Clients übermittelt.</span><span class="sxs-lookup"><span data-stu-id="31071-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="31071-187">Hub-Objekt Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="31071-187">Hub object lifetime</span></span>

<span data-ttu-id="31071-188">Die hubklasse wird nicht instanziiert, oder es werden keine Methoden aus dem eigenen Code auf dem Server aufgerufen. Das ist alles, was Sie von der signalr Hubs-Pipeline für Sie erledigt haben.</span><span class="sxs-lookup"><span data-stu-id="31071-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="31071-189">Signalr erstellt immer dann eine neue Instanz der Hub-Klasse, wenn Sie einen Hub-Vorgang verarbeiten muss, z. b. Wenn ein Client eine Verbindung herstellt, die Verbindung trennt oder einen Methodenaufruf an den Server ausführt.</span><span class="sxs-lookup"><span data-stu-id="31071-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="31071-190">Da Instanzen der Hub-Klasse flüchtig sind, können Sie Sie nicht verwenden, um den Zustand von einem Methodenaufrufe zum nächsten beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="31071-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="31071-191">Jedes Mal, wenn der Server einen Methodenaufruf von einem Client empfängt, wird die Nachricht von einer neuen Instanz der Hub-Klasse verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="31071-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="31071-192">Um den Zustand über mehrere Verbindungen und Methodenaufrufe beizubehalten, verwenden Sie eine andere Methode, z. b. eine Datenbank oder eine statische Variable für die Hub-Klasse, oder eine andere Klasse, die nicht von `Hub`abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="31071-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="31071-193">Wenn Sie Daten im Arbeitsspeicher beibehalten, werden die Daten bei Verwendung einer Methode, z. b. einer statischen Variablen in der Hub-Klasse, verloren gehen, wenn die APP-Domäne wieder verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="31071-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="31071-194">Wenn Sie Nachrichten aus Ihrem eigenen Code, der außerhalb der Hub-Klasse ausgeführt wird, an Clients senden möchten, können Sie dies nicht tun, indem Sie eine Hub-Klasseninstanz instanziieren. Sie können Sie jedoch durch einen Verweis auf das signalr-Kontext Objekt für die Hub-Klasse erstellen.</span><span class="sxs-lookup"><span data-stu-id="31071-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="31071-195">Weitere Informationen finden Sie weiter unten in diesem Thema unter [Abrufen von Client Methoden und Verwalten von Gruppen von außerhalb der Hub-Klasse](#callfromoutsidehub) .</span><span class="sxs-lookup"><span data-stu-id="31071-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="31071-196">Kamel Schreibweise von Hub-Namen in JavaScript-Clients</span><span class="sxs-lookup"><span data-stu-id="31071-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="31071-197">Standardmäßig verweisen JavaScript-Clients auf Hubs, indem Sie eine Version des Klassen namens mit Kamel Schreibung verwenden.</span><span class="sxs-lookup"><span data-stu-id="31071-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="31071-198">Signalr nimmt diese Änderung automatisch vor, damit JavaScript-Code JavaScript-Konventionen entsprechen kann.</span><span class="sxs-lookup"><span data-stu-id="31071-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="31071-199">Das vorherige Beispiel wird im JavaScript-Code als `contosoChatHub` bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="31071-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="31071-200">**Server**</span><span class="sxs-lookup"><span data-stu-id="31071-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="31071-201">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="31071-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="31071-202">Wenn Sie einen anderen Namen angeben möchten, der von Clients verwendet werden soll, fügen Sie das `HubName`-Attribut hinzu.</span><span class="sxs-lookup"><span data-stu-id="31071-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="31071-203">Wenn Sie ein `HubName` Attribut verwenden, gibt es keine Namensänderung in der Camel-Schreibweise auf JavaScript-Clients.</span><span class="sxs-lookup"><span data-stu-id="31071-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="31071-204">**Server**</span><span class="sxs-lookup"><span data-stu-id="31071-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="31071-205">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="31071-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="31071-206">Mehrere Hubs</span><span class="sxs-lookup"><span data-stu-id="31071-206">Multiple Hubs</span></span>

<span data-ttu-id="31071-207">Sie können mehrere Hub-Klassen in einer Anwendung definieren.</span><span class="sxs-lookup"><span data-stu-id="31071-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="31071-208">Wenn Sie dies tun, wird die Verbindung freigegeben, die Gruppen sind jedoch getrennt:</span><span class="sxs-lookup"><span data-stu-id="31071-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="31071-209">Alle Clients verwenden dieselbe URL, um eine signalr-Verbindung mit Ihrem Dienst herzustellen ("/signalr" oder Ihre benutzerdefinierte URL, wenn Sie eine solche URL angegeben haben), und diese Verbindung wird für alle vom Dienst definierten Hubs verwendet.</span><span class="sxs-lookup"><span data-stu-id="31071-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="31071-210">Es gibt keinen Leistungsunterschied für mehrere Hubs im Vergleich zum Definieren aller Hubfunktionen in einer einzelnen Klasse.</span><span class="sxs-lookup"><span data-stu-id="31071-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="31071-211">Alle Hubs erhalten die gleichen HTTP-Anforderungs Informationen.</span><span class="sxs-lookup"><span data-stu-id="31071-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="31071-212">Da alle Hubs die gleiche Verbindung gemeinsam nutzen, werden die HTTP-Anforderungs Informationen, die der Server erhält, in der ursprünglichen HTTP-Anforderung angezeigt, die die signalr-Verbindung herstellt.</span><span class="sxs-lookup"><span data-stu-id="31071-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="31071-213">Wenn Sie die Verbindungsanforderung verwenden, um Informationen vom Client an den Server zu übergeben, indem Sie eine Abfrage Zeichenfolge angeben, können Sie verschiedene Abfrage Zeichenfolgen nicht für verschiedene Hubs bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="31071-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="31071-214">Alle Hubs erhalten die gleichen Informationen.</span><span class="sxs-lookup"><span data-stu-id="31071-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="31071-215">Die generierte JavaScript-Proxydatei enthält Proxys für alle Hubs in einer Datei.</span><span class="sxs-lookup"><span data-stu-id="31071-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="31071-216">Weitere Informationen zu JavaScript [-Proxys finden Sie im signalr Hubs-API-Handbuch-JavaScript-Client-der generierte Proxy und dessen](index.md)Funktionsweise.</span><span class="sxs-lookup"><span data-stu-id="31071-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="31071-217">Gruppen werden innerhalb von Hubs definiert.</span><span class="sxs-lookup"><span data-stu-id="31071-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="31071-218">In signalr können Sie benannte Gruppen für die Übertragung an Teilmengen verbundener Clients definieren.</span><span class="sxs-lookup"><span data-stu-id="31071-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="31071-219">Gruppen werden für jeden Hub separat verwaltet.</span><span class="sxs-lookup"><span data-stu-id="31071-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="31071-220">Beispielsweise würde eine Gruppe mit dem Namen "Administratoren" eine Reihe von Clients für Ihre `ContosoChatHub`-Klasse enthalten, und der gleiche Gruppenname bezieht sich auf einen anderen Satz von Clients für die `StockTickerHub` Klasse.</span><span class="sxs-lookup"><span data-stu-id="31071-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="31071-221">Definieren von Methoden in der hubklasse, die von Clients aufgerufen werden können</span><span class="sxs-lookup"><span data-stu-id="31071-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="31071-222">Wenn Sie eine Methode auf dem Hub verfügbar machen möchten, die vom Client aufgerufen werden soll, deklarieren Sie eine öffentliche Methode, wie in den folgenden Beispielen gezeigt.</span><span class="sxs-lookup"><span data-stu-id="31071-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="31071-223">Sie können einen Rückgabetyp und Parameter, einschließlich komplexer Typen und Arrays, wie in jeder beliebigen C# Methode angeben.</span><span class="sxs-lookup"><span data-stu-id="31071-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="31071-224">Alle Daten, die Sie in Parametern empfangen oder an den Aufrufer zurückgeben, werden mithilfe von JSON zwischen dem Client und dem Server übermittelt, und signalr übernimmt automatisch die Bindung komplexer Objekte und Arrays von Objekten.</span><span class="sxs-lookup"><span data-stu-id="31071-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="31071-225">Kamel Schreibweise von Methodennamen in JavaScript-Clients</span><span class="sxs-lookup"><span data-stu-id="31071-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="31071-226">Standardmäßig verweisen JavaScript-Clients auf hubmethoden, indem Sie eine Version des Methoden namens mit Kamel Schreibung verwenden.</span><span class="sxs-lookup"><span data-stu-id="31071-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="31071-227">Signalr nimmt diese Änderung automatisch vor, damit JavaScript-Code JavaScript-Konventionen entsprechen kann.</span><span class="sxs-lookup"><span data-stu-id="31071-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="31071-228">**Server**</span><span class="sxs-lookup"><span data-stu-id="31071-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="31071-229">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="31071-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="31071-230">Wenn Sie einen anderen Namen angeben möchten, der von Clients verwendet werden soll, fügen Sie das `HubMethodName`-Attribut hinzu.</span><span class="sxs-lookup"><span data-stu-id="31071-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="31071-231">**Server**</span><span class="sxs-lookup"><span data-stu-id="31071-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="31071-232">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="31071-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="31071-233">Zeitpunkt der asynchronen Ausführung</span><span class="sxs-lookup"><span data-stu-id="31071-233">When to execute asynchronously</span></span>

<span data-ttu-id="31071-234">Wenn die Methode eine lange Ausführungszeit erfordert oder Aufgaben ausführen müssen, die warten würden, wie z. b. eine Datenbanksuche oder ein Webdienst-aufrufbedarf, machen Sie die Hub-Methode asynchron, indem Sie eine [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (anstelle von `void` Rückgabe) oder [Aufgabe&lt;t&gt;](https://msdn.microsoft.com/library/dd321424.aspx) Objekt (anstelle `T` Rückgabe Typs) zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="31071-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="31071-235">Wenn Sie ein `Task` Objekt von der-Methode zurückgeben, wartet signalr darauf, dass die `Task` abgeschlossen ist, und sendet dann das entpackte Ergebnis an den Client zurück. es gibt also keinen Unterschied in der Art und Weise, wie Sie den Methodenaufrufe im Client codieren.</span><span class="sxs-lookup"><span data-stu-id="31071-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="31071-236">Durch die asynchrone Erstellung einer hubmethode wird verhindert, dass die Verbindung blockiert wird, wenn der WebSocket-Transport verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="31071-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="31071-237">Wenn eine hubmethode synchron ausgeführt wird und der Transport WebSocket ist, werden nachfolgende Aufrufe von Methoden auf dem Hub desselben Clients blockiert, bis die hubmethode abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="31071-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="31071-238">Im folgenden Beispiel wird die gleiche Methode gezeigt, die für die synchrone oder asynchrone Ausführung programmiert ist, gefolgt von JavaScript-Client Code, der zum Aufrufen einer beliebigen Version funktioniert.</span><span class="sxs-lookup"><span data-stu-id="31071-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="31071-239">**Synchrone**</span><span class="sxs-lookup"><span data-stu-id="31071-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="31071-240">**Asynchronous-ASP.NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="31071-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="31071-241">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="31071-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="31071-242">Weitere Informationen zum Verwenden von asynchronen Methoden in ASP.NET 4,5 finden [Sie unter Verwenden von asynchronen Methoden in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="31071-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="31071-243">Definieren von über Ladungen</span><span class="sxs-lookup"><span data-stu-id="31071-243">Defining Overloads</span></span>

<span data-ttu-id="31071-244">Wenn Sie über Ladungen für eine Methode definieren möchten, muss die Anzahl von Parametern in den einzelnen über Ladungen abweichen.</span><span class="sxs-lookup"><span data-stu-id="31071-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="31071-245">Wenn Sie eine Überladung nur durch Angabe verschiedener Parametertypen unterscheiden, wird die hubklasse kompiliert, aber der signalr-Dienst löst zur Laufzeit eine Ausnahme aus, wenn Clients versuchen, eine der über Ladungen aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="31071-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="31071-246">So werden Client Methoden aus der Hub-Klasse aufgerufen</span><span class="sxs-lookup"><span data-stu-id="31071-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="31071-247">Um Client Methoden vom Server aufzurufen, verwenden Sie die `Clients`-Eigenschaft in einer Methode in ihrer Hub-Klasse.</span><span class="sxs-lookup"><span data-stu-id="31071-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="31071-248">Das folgende Beispiel zeigt Servercode, der `addNewMessageToPage` auf allen verbundenen Clients aufruft, und Client Code, der die-Methode in einem JavaScript-Client definiert.</span><span class="sxs-lookup"><span data-stu-id="31071-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="31071-249">**Server**</span><span class="sxs-lookup"><span data-stu-id="31071-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="31071-250">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="31071-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="31071-251">Sie können keinen Rückgabewert von einer Client Methode erhalten. Syntax, wie z. b. `int x = Clients.All.add(1,1)`, funktioniert nicht.</span><span class="sxs-lookup"><span data-stu-id="31071-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="31071-252">Sie können komplexe Typen und Arrays für die Parameter angeben.</span><span class="sxs-lookup"><span data-stu-id="31071-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="31071-253">Im folgenden Beispiel wird ein komplexer Typ in einem Methoden Parameter an den Client übergeben.</span><span class="sxs-lookup"><span data-stu-id="31071-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="31071-254">**Server Code, der eine Client Methode mithilfe eines komplexen Objekts aufruft**</span><span class="sxs-lookup"><span data-stu-id="31071-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="31071-255">**Server Code, der das komplexe Objekt definiert**</span><span class="sxs-lookup"><span data-stu-id="31071-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="31071-256">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="31071-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="31071-257">Auswählen, welche Clients den RPC erhalten</span><span class="sxs-lookup"><span data-stu-id="31071-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="31071-258">Die Clients-Eigenschaft gibt ein [hubconnectioncontext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) -Objekt zurück, das mehrere Optionen zum Angeben von Clients bereitstellt, die den RPC empfangen:</span><span class="sxs-lookup"><span data-stu-id="31071-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="31071-259">Alle verbundenen Clients.</span><span class="sxs-lookup"><span data-stu-id="31071-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="31071-260">Nur der aufrufenden Client.</span><span class="sxs-lookup"><span data-stu-id="31071-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="31071-261">Alle Clients mit Ausnahme des aufrufenden Clients.</span><span class="sxs-lookup"><span data-stu-id="31071-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="31071-262">Ein spezifischer Client, der durch die Verbindungs-ID identifiziert wird</span><span class="sxs-lookup"><span data-stu-id="31071-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="31071-263">In diesem Beispiel wird `addContosoChatMessageToPage` auf dem aufrufenden Client aufgerufen und hat dieselbe Auswirkung wie die Verwendung von `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="31071-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="31071-264">Alle verbundenen Clients mit Ausnahme der angegebenen Clients, identifiziert durch die Verbindungs-ID.</span><span class="sxs-lookup"><span data-stu-id="31071-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="31071-265">Alle verbundenen Clients in einer bestimmten Gruppe.</span><span class="sxs-lookup"><span data-stu-id="31071-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="31071-266">Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme der angegebenen Clients, identifiziert durch die Verbindungs-ID.</span><span class="sxs-lookup"><span data-stu-id="31071-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="31071-267">Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme des aufrufenden Clients.</span><span class="sxs-lookup"><span data-stu-id="31071-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="31071-268">Keine Validierung der Kompilierzeit für Methodennamen</span><span class="sxs-lookup"><span data-stu-id="31071-268">No compile-time validation for method names</span></span>

<span data-ttu-id="31071-269">Der von Ihnen angegebene Methodenname wird als dynamisches Objekt interpretiert, was bedeutet, dass keine IntelliSense-oder Kompilierzeit Validierung dafür vorliegt.</span><span class="sxs-lookup"><span data-stu-id="31071-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="31071-270">Der Ausdruck wird zur Laufzeit ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="31071-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="31071-271">Wenn der Methodenaufruf ausgeführt wird, sendet signalr den Methodennamen und die Parameterwerte an den Client. wenn der Client über eine Methode verfügt, die mit dem Namen übereinstimmt, wird diese Methode aufgerufen und die Parameterwerte an Sie übergeben.</span><span class="sxs-lookup"><span data-stu-id="31071-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="31071-272">Wenn keine übereinstimmende Methode auf dem Client gefunden wird, wird kein Fehler ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="31071-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="31071-273">Informationen über das Format der Daten, die von signalr im Hintergrund an den Client übermittelt werden, wenn Sie eine Client Methode aufruft, finden Sie unter [Introduction to signalr (Einführung in signalr](index.md)).</span><span class="sxs-lookup"><span data-stu-id="31071-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="31071-274">Nicht Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="31071-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="31071-275">Beim Methodennamen wird die Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="31071-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="31071-276">Beispielsweise werden auf dem-Server `Clients.All.addContosoChatMessageToPage` auf dem-Server `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`oder `addContosoChatMessageToPage` auf dem Client ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="31071-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="31071-277">Asynchrone Ausführung</span><span class="sxs-lookup"><span data-stu-id="31071-277">Asynchronous execution</span></span>

<span data-ttu-id="31071-278">Die von Ihnen aufzurufende Methode wird asynchron ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="31071-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="31071-279">Jeglicher Code, der nach einem Methodenaufrufe an einen Client gesendet wird, wird sofort ausgeführt, ohne darauf zu warten, dass signalr die Übertragung von Daten an Clients abgeschlossen hat, es sei denn, Sie geben an, dass die nachfolgenden Codezeilen auf die</span><span class="sxs-lookup"><span data-stu-id="31071-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="31071-280">In den folgenden Codebeispielen wird veranschaulicht, wie zwei Client Methoden nacheinander ausgeführt werden, eine mit Code, der in .NET 4,5 funktioniert, und eine mit Code, der in .NET 4 funktioniert.</span><span class="sxs-lookup"><span data-stu-id="31071-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="31071-281">**Beispiel für .NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="31071-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="31071-282">**Beispiel für .NET 4**</span><span class="sxs-lookup"><span data-stu-id="31071-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="31071-283">Wenn Sie `await` oder `ContinueWith` verwenden, um zu warten, bis eine Client Methode abgeschlossen ist, bevor die nächste Codezeile ausgeführt wird, bedeutet dies nicht, dass Clients die Nachricht tatsächlich empfangen, bevor die nächste Codezeile ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="31071-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="31071-284">Die Vervollständigung eines Client Methoden Aufrufes bedeutet nur, dass signalr alle notwendigen Schritte zum Senden der Nachricht ausgeführt hat.</span><span class="sxs-lookup"><span data-stu-id="31071-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="31071-285">Wenn Sie überprüfen müssen, ob die Nachricht von Clients empfangen wurde, müssen Sie diesen Mechanismus selbst programmieren.</span><span class="sxs-lookup"><span data-stu-id="31071-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="31071-286">Beispielsweise können Sie eine `MessageReceived` Methode auf dem Hub programmieren, und in der `addContosoChatMessageToPage`-Methode auf dem Client können Sie `MessageReceived` abrufen, nachdem Sie die Aufgaben ausgeführt haben, die Sie auf dem Client ausführen müssen.</span><span class="sxs-lookup"><span data-stu-id="31071-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="31071-287">In `MessageReceived` im Hub können Sie jede beliebige Arbeit von tatsächlichem Client Empfang und Verarbeitung des ursprünglichen Methoden Aufrufes abhängig machen.</span><span class="sxs-lookup"><span data-stu-id="31071-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="31071-288">Verwenden einer Zeichen folgen Variablen als Methodenname</span><span class="sxs-lookup"><span data-stu-id="31071-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="31071-289">Wenn Sie eine Client Methode aufrufen möchten, indem Sie eine Zeichen folgen Variable als Methodenname verwenden, wandeln Sie `Clients.All` (oder `Clients.Others`, `Clients.Caller`usw.) in `IClientProxy` und rufen Sie dann aufrufen [(MethodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx)auf.</span><span class="sxs-lookup"><span data-stu-id="31071-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="31071-290">Verwalten der Gruppenmitgliedschaft mit der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="31071-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="31071-291">Gruppen in signalr bieten eine Methode zum Senden von Nachrichten an bestimmte Teilmengen verbundener Clients.</span><span class="sxs-lookup"><span data-stu-id="31071-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="31071-292">Eine Gruppe kann über eine beliebige Anzahl von Clients verfügen, und ein Client kann Mitglied einer beliebigen Anzahl von Gruppen sein.</span><span class="sxs-lookup"><span data-stu-id="31071-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="31071-293">Verwenden Sie zum Verwalten der Gruppenmitgliedschaft die Methoden zum [Hinzufügen](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) und [Entfernen](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) , die von der `Groups`-Eigenschaft der Hub-Klasse bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="31071-293">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="31071-294">Das folgende Beispiel zeigt die `Groups.Add`-und `Groups.Remove` Methoden, die in hubmethoden verwendet werden, die vom Client Code aufgerufen werden, gefolgt von JavaScript-Client Code, der Sie aufruft.</span><span class="sxs-lookup"><span data-stu-id="31071-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="31071-295">**Server**</span><span class="sxs-lookup"><span data-stu-id="31071-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="31071-296">**JavaScript-Client mit generiertem Proxy**</span><span class="sxs-lookup"><span data-stu-id="31071-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="31071-297">Sie müssen nicht explizit Gruppen erstellen.</span><span class="sxs-lookup"><span data-stu-id="31071-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="31071-298">Tatsächlich wird eine Gruppe automatisch erstellt, wenn Sie Ihren Namen zum ersten Mal in einem Aufruf von `Groups.Add`angeben. Sie wird gelöscht, wenn Sie die letzte Verbindung aus der Mitgliedschaft entfernen.</span><span class="sxs-lookup"><span data-stu-id="31071-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="31071-299">Es ist keine API zum erhalten einer Gruppen Mitgliedschafts Liste oder einer Liste von Gruppen vorhanden.</span><span class="sxs-lookup"><span data-stu-id="31071-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="31071-300">Signalr sendet Nachrichten auf der Grundlage eines [Pub/Sub-Modells](http://en.wikipedia.org/wiki/Publish/subscribe)an Clients und Gruppen, und der Server verwaltet keine Listen mit Gruppen oder Gruppenmitgliedschaften.</span><span class="sxs-lookup"><span data-stu-id="31071-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="31071-301">Dadurch wird die Skalierbarkeit maximiert, denn wenn Sie einer Webfarm einen Knoten hinzufügen, muss jeder Status, den signalr beibehält, an den neuen Knoten weitergegeben werden.</span><span class="sxs-lookup"><span data-stu-id="31071-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="31071-302">Asynchrone Ausführung von Add-und Remove-Methoden</span><span class="sxs-lookup"><span data-stu-id="31071-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="31071-303">Die Methoden `Groups.Add` und `Groups.Remove` werden asynchron ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="31071-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="31071-304">Wenn Sie einer Gruppe einen Client hinzufügen und sofort mithilfe der-Gruppe eine Nachricht an den Client senden möchten, müssen Sie sicherstellen, dass die `Groups.Add`-Methode zuerst abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="31071-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="31071-305">In den folgenden Codebeispielen wird veranschaulicht, wie Sie dies tun, eine mit Code, der in .NET 4,5 funktioniert, und eine mit Code, der in .NET 4 funktioniert.</span><span class="sxs-lookup"><span data-stu-id="31071-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="31071-306">**Beispiel für .NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="31071-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="31071-307">**Beispiel für .NET 4**</span><span class="sxs-lookup"><span data-stu-id="31071-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="31071-308">Persistenz der Gruppenmitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="31071-308">Group membership persistence</span></span>

<span data-ttu-id="31071-309">Signalr verfolgt Verbindungen und keine Benutzer nach. Wenn Sie also möchten, dass sich ein Benutzer jedes Mal, wenn der Benutzer eine Verbindung herstellt, in derselben Gruppe befindet, müssen Sie `Groups.Add` jedes Mal anrufen, wenn der Benutzer eine neue Verbindung herstellt.</span><span class="sxs-lookup"><span data-stu-id="31071-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="31071-310">Nach einem vorübergehenden Verbindungsverlust kann die Verbindung manchmal von signalr automatisch wieder hergestellt werden.</span><span class="sxs-lookup"><span data-stu-id="31071-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="31071-311">In diesem Fall wird die gleiche Verbindung von signalr wieder hergestellt, und es wird keine neue Verbindung hergestellt, sodass die Gruppenmitgliedschaft des Clients automatisch wieder hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="31071-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="31071-312">Dies ist auch dann möglich, wenn die temporäre Unterbrechung das Ergebnis eines Serverneustarts oder eines Fehlers ist, da der Verbindungsstatus für jeden Client, einschließlich der Gruppenmitgliedschaften, auf den Client gerundet wird.</span><span class="sxs-lookup"><span data-stu-id="31071-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="31071-313">Wenn ein Server ausfällt und vor dem Verbindungs Timeout durch einen neuen Server ersetzt wird, kann ein Client automatisch eine Verbindung mit dem neuen Server herstellen und sich erneut bei Gruppen anmelden, bei denen er Mitglied ist.</span><span class="sxs-lookup"><span data-stu-id="31071-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="31071-314">Wenn eine Verbindung nach einem Verbindungsverlust oder einem Verbindungs Timeout nicht automatisch wieder hergestellt werden kann, oder wenn der Client die Verbindung trennt (z. b. Wenn ein Browser zu einer neuen Seite navigiert), gehen Gruppenmitgliedschaften verloren.</span><span class="sxs-lookup"><span data-stu-id="31071-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="31071-315">Wenn der Benutzer das nächste Mal eine Verbindung herstellt, wird eine neue Verbindung hergestellt.</span><span class="sxs-lookup"><span data-stu-id="31071-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="31071-316">Um die Gruppenmitgliedschaften beizubehalten, wenn derselbe Benutzer eine neue Verbindung herstellt, muss Ihre Anwendung die Zuordnungen zwischen Benutzern und Gruppen nachverfolgen und Gruppenmitgliedschaften jedes Mal wiederherstellen, wenn ein Benutzer eine neue Verbindung herstellt.</span><span class="sxs-lookup"><span data-stu-id="31071-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="31071-317">Weitere Informationen zu Verbindungen und erneuten Verbindungen finden Sie unter [Behandeln von Verbindungs Lebensdauer-Ereignissen in der Hub-Klasse](#connectionlifetime) weiter unten in diesem Thema.</span><span class="sxs-lookup"><span data-stu-id="31071-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="31071-318">Einzelbenutzer Gruppen</span><span class="sxs-lookup"><span data-stu-id="31071-318">Single-user groups</span></span>

<span data-ttu-id="31071-319">Anwendungen, die signalr verwenden, müssen in der Regel die Zuordnungen zwischen Benutzern und Verbindungen nachverfolgen, um zu ermitteln, welcher Benutzer eine Nachricht gesendet hat und welche Benutzer eine Nachricht erhalten sollen.</span><span class="sxs-lookup"><span data-stu-id="31071-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="31071-320">Gruppen werden in einem der beiden häufig verwendeten Muster verwendet.</span><span class="sxs-lookup"><span data-stu-id="31071-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="31071-321">Einzelbenutzer Gruppen.</span><span class="sxs-lookup"><span data-stu-id="31071-321">Single-user groups.</span></span>

    <span data-ttu-id="31071-322">Sie können den Benutzernamen als Gruppennamen angeben und die aktuelle Verbindungs-ID der Gruppe jedes Mal hinzufügen, wenn der Benutzer eine Verbindung herstellt oder die Verbindung wiederherstellt.</span><span class="sxs-lookup"><span data-stu-id="31071-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="31071-323">Zum Senden von Nachrichten an den Benutzer, den Sie an die Gruppe senden.</span><span class="sxs-lookup"><span data-stu-id="31071-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="31071-324">Ein Nachteil dieser Methode besteht darin, dass die Gruppe Ihnen keine Möglichkeit bietet, herauszufinden, ob der Benutzer online oder offline ist.</span><span class="sxs-lookup"><span data-stu-id="31071-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="31071-325">Nachverfolgen von Zuordnungen zwischen Benutzernamen und Verbindungs-IDs.</span><span class="sxs-lookup"><span data-stu-id="31071-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="31071-326">Sie können eine Zuordnung zwischen den einzelnen Benutzernamen und mindestens einer Verbindungs-ID in einem Wörterbuch oder einer Datenbank speichern und die gespeicherten Daten jedes Mal aktualisieren, wenn der Benutzer eine Verbindung herstellt oder die Verbindung trennt.</span><span class="sxs-lookup"><span data-stu-id="31071-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="31071-327">Um Nachrichten an den Benutzer zu senden, geben Sie die Verbindungs-IDs an.</span><span class="sxs-lookup"><span data-stu-id="31071-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="31071-328">Ein Nachteil dieser Methode besteht darin, dass Sie mehr Arbeitsspeicher benötigt.</span><span class="sxs-lookup"><span data-stu-id="31071-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="31071-329">Behandeln von Verbindungs Lebensdauer-Ereignissen in der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="31071-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="31071-330">Typische Gründe für die Behandlung von Verbindungs Lebensdauer-Ereignissen besteht darin, zu verfolgen, ob ein Benutzer verbunden ist oder nicht, und die Zuordnung zwischen Benutzernamen und Verbindungs-IDs nachzuverfolgen.</span><span class="sxs-lookup"><span data-stu-id="31071-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="31071-331">Um eigenen Code auszuführen, wenn Clients eine Verbindung herstellen oder trennen, überschreiben Sie die virtuellen Methoden `OnConnected`, `OnDisconnected`und `OnReconnected` der Hub-Klasse, wie im folgenden Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="31071-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="31071-332">Wenn "onconnected", "ongetrennte" und "onreconnected" aufgerufen werden</span><span class="sxs-lookup"><span data-stu-id="31071-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="31071-333">Jedes Mal, wenn ein Browser zu einer neuen Seite navigiert, muss eine neue Verbindung eingerichtet werden. Dies bedeutet, dass signalr die `OnDisconnected`-Methode gefolgt von der `OnConnected`-Methode ausführt.</span><span class="sxs-lookup"><span data-stu-id="31071-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="31071-334">Signalr erstellt immer eine neue Verbindungs-ID, wenn eine neue Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="31071-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="31071-335">Die `OnReconnected`-Methode wird aufgerufen, wenn eine temporäre Verbindungsunterbrechung vorliegt, von der signalr automatisch wieder hergestellt werden kann, z. b. Wenn ein Kabel vorübergehend getrennt wird und die Verbindung wieder hergestellt wird, bevor ein Timeout der Verbindung auftritt. Die `OnDisconnected`-Methode wird aufgerufen, wenn der Client getrennt wird und signalr nicht automatisch wieder verbunden werden kann, z. b. Wenn ein Browser zu einer neuen Seite navigiert.</span><span class="sxs-lookup"><span data-stu-id="31071-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="31071-336">Daher ist eine mögliche Sequenz von Ereignissen für einen bestimmten Client `OnConnected`, `OnReconnected``OnDisconnected`; oder `OnConnected``OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="31071-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="31071-337">Die Sequenz `OnConnected`, `OnDisconnected``OnReconnected` für eine bestimmte Verbindung wird nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="31071-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="31071-338">Die `OnDisconnected`-Methode wird in einigen Szenarios nicht aufgerufen, z. b. Wenn ein Server ausfällt oder die APP-Domäne wieder verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="31071-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="31071-339">Wenn ein anderer Server Online ist oder die APP-Domäne die Wiederverwendung beendet, können einige Clients möglicherweise erneut eine Verbindung herstellen und das `OnReconnected` Ereignis auslösen.</span><span class="sxs-lookup"><span data-stu-id="31071-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="31071-340">Weitere Informationen finden Sie unter [verstehen und behandeln von Verbindungs Lebensdauer-Ereignissen in signalr](index.md).</span><span class="sxs-lookup"><span data-stu-id="31071-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="31071-341">Aufruferstatus nicht aufgefüllt</span><span class="sxs-lookup"><span data-stu-id="31071-341">Caller state not populated</span></span>

<span data-ttu-id="31071-342">Die Ereignishandlermethoden der Verbindungs Lebensdauer werden vom Server aufgerufen, d. h. jeder Zustand, den Sie im `state` Objekt auf dem Client ablegen, wird nicht in der `Caller`-Eigenschaft auf dem Server aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="31071-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="31071-343">Weitere Informationen über das `state`-Objekt und die `Caller`-Eigenschaft finden Sie weiter unten in diesem Thema unter [übergeben des Zustands zwischen Clients und der Hub-Klasse](#passstate) .</span><span class="sxs-lookup"><span data-stu-id="31071-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="31071-344">So erhalten Sie Informationen über den Client aus der Kontext Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="31071-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="31071-345">Um Informationen über den Client zu erhalten, verwenden Sie die `Context`-Eigenschaft der Hub-Klasse.</span><span class="sxs-lookup"><span data-stu-id="31071-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="31071-346">Die `Context`-Eigenschaft gibt ein [hubcallercontext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) -Objekt zurück, das den Zugriff auf die folgenden Informationen ermöglicht:</span><span class="sxs-lookup"><span data-stu-id="31071-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="31071-347">Die Verbindungs-ID des aufrufenden Clients.</span><span class="sxs-lookup"><span data-stu-id="31071-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="31071-348">Die Verbindungs-ID ist eine GUID, die von signalr zugewiesen wird (Sie können den Wert nicht in Ihrem eigenen Code angeben).</span><span class="sxs-lookup"><span data-stu-id="31071-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="31071-349">Es gibt eine Verbindungs-ID für jede Verbindung, und die gleiche Verbindungs-ID wird von allen Hubs verwendet, wenn Sie über mehrere Hubs in Ihrer Anwendung verfügen.</span><span class="sxs-lookup"><span data-stu-id="31071-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="31071-350">HTTP-Header Daten.</span><span class="sxs-lookup"><span data-stu-id="31071-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="31071-351">Sie können auch HTTP-Header aus `Context.Headers`erhalten.</span><span class="sxs-lookup"><span data-stu-id="31071-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="31071-352">Der Grund für mehrere Verweise auf dasselbe Ergebnis ist, dass `Context.Headers` zuerst erstellt wurde, die `Context.Request`-Eigenschaft später hinzugefügt wurde und `Context.Headers` aus Gründen der Abwärtskompatibilität beibehalten wurde.</span><span class="sxs-lookup"><span data-stu-id="31071-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="31071-353">Abfrage Zeichen folgen Daten.</span><span class="sxs-lookup"><span data-stu-id="31071-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="31071-354">Sie können auch Abfrage Zeichenfolgen-Daten aus `Context.QueryString`erhalten.</span><span class="sxs-lookup"><span data-stu-id="31071-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="31071-355">Die Abfrage Zeichenfolge, die Sie in dieser Eigenschaft erhalten, ist die Abfrage Zeichenfolge, die mit der HTTP-Anforderung verwendet wurde, die die signalr-Verbindung</span><span class="sxs-lookup"><span data-stu-id="31071-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="31071-356">Sie können dem Client Abfrage Zeichenfolgen-Parameter hinzufügen, indem Sie die Verbindung konfigurieren. Dies ist eine bequeme Möglichkeit, Daten über den Client vom Client an den Server zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="31071-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="31071-357">Das folgende Beispiel zeigt eine Möglichkeit, eine Abfrage Zeichenfolge in einem JavaScript-Client hinzuzufügen, wenn Sie den generierten Proxy verwenden.</span><span class="sxs-lookup"><span data-stu-id="31071-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="31071-358">Weitere Informationen zum Festlegen von Abfrage Zeichen folgen Parametern finden Sie in den API-Handbüchern für die [JavaScript](index.md) -und [.net](index.md) -Clients.</span><span class="sxs-lookup"><span data-stu-id="31071-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="31071-359">Sie finden die Transportmethode, die für die Verbindung verwendet wird, in den Abfrage Zeichenfolgen-Daten zusammen mit einigen anderen Werten, die intern von signalr verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="31071-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="31071-360">Der Wert von `transportMethod` wird "websockets", "serversentevents", "foreverframe" oder "longabruf" lauten.</span><span class="sxs-lookup"><span data-stu-id="31071-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="31071-361">Beachten Sie Folgendes: Wenn Sie diesen Wert in der `OnConnected`-Ereignishandlermethode überprüfen, erhalten Sie in einigen Szenarien anfänglich möglicherweise einen Transport Wert, der nicht die endgültige aushandelte Transportmethode für die Verbindung ist.</span><span class="sxs-lookup"><span data-stu-id="31071-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="31071-362">In diesem Fall löst die Methode eine Ausnahme aus und wird später erneut aufgerufen, wenn die letzte Transportmethode eingerichtet wird.</span><span class="sxs-lookup"><span data-stu-id="31071-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="31071-363">KS.</span><span class="sxs-lookup"><span data-stu-id="31071-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="31071-364">Sie können auch Cookies aus `Context.RequestCookies`erhalten.</span><span class="sxs-lookup"><span data-stu-id="31071-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="31071-365">Benutzerinformationen.</span><span class="sxs-lookup"><span data-stu-id="31071-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="31071-366">Das HttpContext-Objekt für die Anforderung:</span><span class="sxs-lookup"><span data-stu-id="31071-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="31071-367">Verwenden Sie diese Methode, anstatt `HttpContext.Current`, um das `HttpContext`-Objekt für die signalr-Verbindung zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="31071-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="31071-368">Übergeben des Zustands zwischen Clients und der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="31071-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="31071-369">Der-Client Proxy stellt ein `state` Objekt bereit, in dem Sie Daten speichern können, die mit jedem Methoden aufrufan den Server übertragen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="31071-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="31071-370">Auf dem-Server können Sie auf diese Daten in der `Clients.Caller`-Eigenschaft in hubmethoden zugreifen, die von-Clients aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="31071-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="31071-371">Die `Clients.Caller`-Eigenschaft wird für die Verbindungs Lebensdauer-Ereignishandlermethoden `OnConnected`, `OnDisconnected`und `OnReconnected`nicht aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="31071-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="31071-372">Das Erstellen oder Aktualisieren von Daten im `state` Objekt und in der `Clients.Caller`-Eigenschaft funktioniert in beide Richtungen.</span><span class="sxs-lookup"><span data-stu-id="31071-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="31071-373">Sie können Werte auf dem Server aktualisieren, die an den Client zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="31071-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="31071-374">Das folgende Beispiel zeigt JavaScript-Client Code, der den Status für die Übertragung an den Server mit jedem Methoden Aufrufwert speichert.</span><span class="sxs-lookup"><span data-stu-id="31071-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="31071-375">Das folgende Beispiel zeigt den entsprechenden Code in einem .NET-Client.</span><span class="sxs-lookup"><span data-stu-id="31071-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="31071-376">In ihrer Hub-Klasse können Sie auf diese Daten in der `Clients.Caller`-Eigenschaft zugreifen.</span><span class="sxs-lookup"><span data-stu-id="31071-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="31071-377">Das folgende Beispiel zeigt Code, der den Zustand abruft, auf den im vorherigen Beispiel verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="31071-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="31071-378">Dieser Mechanismus für das Beibehalten des Zustands ist nicht für große Datenmengen gedacht, da alles, was Sie in der `state`-oder `Clients.Caller`-Eigenschaft ablegen, bei jedem Methodenaufruf Roundtrip umfasst.</span><span class="sxs-lookup"><span data-stu-id="31071-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="31071-379">Dies ist nützlich für kleinere Elemente, wie z. b. Benutzernamen oder Leistungsindikatoren.</span><span class="sxs-lookup"><span data-stu-id="31071-379">It's useful for smaller items such as user names or counters.</span></span>

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="31071-380">Behandeln von Fehlern in der Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="31071-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="31071-381">Verwenden Sie eine oder beide der folgenden Methoden, um Fehler zu behandeln, die in den hubklassen Methoden auftreten:</span><span class="sxs-lookup"><span data-stu-id="31071-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="31071-382">Packen Sie den Methoden Code in try-catch-Blöcken, und protokollieren Sie das Ausnahme Objekt.</span><span class="sxs-lookup"><span data-stu-id="31071-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="31071-383">Zu Debuggingzwecken können Sie die Ausnahme an den Client senden, aber aus Sicherheitsgründen wird das Senden detaillierter Informationen an Clients in der Produktionsumgebung nicht empfohlen.</span><span class="sxs-lookup"><span data-stu-id="31071-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="31071-384">Erstellen Sie ein Hubs Pipeline Modul, das die [onincomingerror](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) -Methode behandelt.</span><span class="sxs-lookup"><span data-stu-id="31071-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="31071-385">Das folgende Beispiel zeigt ein Pipeline Modul, das Fehler protokolliert, gefolgt von Code in Global. asax, der das Modul in die Hubs-Pipeline einfügt.</span><span class="sxs-lookup"><span data-stu-id="31071-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="31071-386">Weitere Informationen zu Hub-Pipeline Modulen finden Sie weiter unten in diesem Thema unter [Anpassen der Hubs-Pipeline](#hubpipeline) .</span><span class="sxs-lookup"><span data-stu-id="31071-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="31071-387">Aktivieren der Ablauf Verfolgung</span><span class="sxs-lookup"><span data-stu-id="31071-387">How to enable tracing</span></span>

<span data-ttu-id="31071-388">Um die serverseitige Ablauf Verfolgung zu aktivieren, fügen Sie der Datei "Web. config" ein System. Diagnostics-Element hinzu, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="31071-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="31071-389">Wenn Sie die Anwendung in Visual Studio ausführen, können Sie die Protokolle im **Ausgabe** Fenster anzeigen.</span><span class="sxs-lookup"><span data-stu-id="31071-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="31071-390">Abrufen von Client Methoden und Verwalten von Gruppen von außerhalb der hubklasse</span><span class="sxs-lookup"><span data-stu-id="31071-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="31071-391">Um Client Methoden aus einer anderen Klasse als der hubklasse aufzurufen, erhalten Sie einen Verweis auf das signalr-Kontext Objekt für den Hub und verwenden dieses, um Methoden auf dem Client aufzurufen oder Gruppen zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="31071-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="31071-392">Das folgende Beispiel `StockTicker` Klasse ruft das Kontext Objekt ab, speichert es in einer Instanz der-Klasse, speichert die-Klasseninstanz in einer statischen-Eigenschaft und verwendet den Kontext aus der Singleton-Klasseninstanz, um die `updateStockPrice`-Methode auf Clients aufzurufen, die mit einem Hub mit dem Namen `StockTickerHub`verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="31071-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="31071-393">Wenn Sie den Kontext mehrmals in einem langlebiges Objekt verwenden müssen, sollten Sie den Verweis einmal erhalten und speichern, anstatt ihn jedes Mal erneut zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="31071-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="31071-394">Wenn Sie den Kontext einmal erhalten, stellen Sie sicher, dass signalr Nachrichten in derselben Reihenfolge an Clients sendet, in der die hubmethoden Aufrufe der Client Methode ausführen.</span><span class="sxs-lookup"><span data-stu-id="31071-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="31071-395">Ein Tutorial, das zeigt, wie der signalr-Kontext für einen Hub verwendet wird, finden Sie unter [Server Broadcast mit ASP.net signalr](index.md).</span><span class="sxs-lookup"><span data-stu-id="31071-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="31071-396">Aufrufen von Client Methoden</span><span class="sxs-lookup"><span data-stu-id="31071-396">Calling client methods</span></span>

<span data-ttu-id="31071-397">Sie können angeben, welche Clients den RPC erhalten sollen, aber Sie haben weniger Optionen als beim Aufruf von einer Hub-Klasse.</span><span class="sxs-lookup"><span data-stu-id="31071-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="31071-398">Der Grund hierfür ist, dass der Kontext keinem bestimmten Client von einem Client zugeordnet ist, sodass alle Methoden, die Kenntnisse der aktuellen Verbindungs-ID erfordern (z. b. `Clients.Others`oder `Clients.Caller`oder `Clients.OthersInGroup`), nicht verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="31071-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="31071-399">Die folgenden Optionen sind verfügbar:</span><span class="sxs-lookup"><span data-stu-id="31071-399">The following options are available:</span></span>

- <span data-ttu-id="31071-400">Alle verbundenen Clients.</span><span class="sxs-lookup"><span data-stu-id="31071-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="31071-401">Ein spezifischer Client, der durch die Verbindungs-ID identifiziert wird</span><span class="sxs-lookup"><span data-stu-id="31071-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="31071-402">Alle verbundenen Clients mit Ausnahme der angegebenen Clients, identifiziert durch die Verbindungs-ID.</span><span class="sxs-lookup"><span data-stu-id="31071-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="31071-403">Alle verbundenen Clients in einer bestimmten Gruppe.</span><span class="sxs-lookup"><span data-stu-id="31071-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="31071-404">Alle verbundenen Clients in einer angegebenen Gruppe mit Ausnahme der angegebenen Clients, die durch die Verbindungs-ID identifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="31071-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="31071-405">Wenn Sie die nicht-Hub-Klasse von Methoden in ihrer Hub-Klasse aufrufen, können Sie die aktuelle Verbindungs-ID übergeben und diese mit `Clients.Client`, `Clients.AllExcept`oder `Clients.Group` verwenden, um `Clients.Caller`, `Clients.Others`oder `Clients.OthersInGroup`zu simulieren.</span><span class="sxs-lookup"><span data-stu-id="31071-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="31071-406">Im folgenden Beispiel übergibt die `MoveShapeHub`-Klasse die Verbindungs-ID an die `Broadcaster`-Klasse, damit die `Broadcaster`-Klasse `Clients.Others`simulieren kann.</span><span class="sxs-lookup"><span data-stu-id="31071-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="31071-407">Verwalten der Gruppenmitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="31071-407">Managing group membership</span></span>

<span data-ttu-id="31071-408">Zum Verwalten von Gruppen haben Sie dieselben Optionen wie in einer Hub-Klasse.</span><span class="sxs-lookup"><span data-stu-id="31071-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="31071-409">Hinzufügen eines Clients zu einer Gruppe</span><span class="sxs-lookup"><span data-stu-id="31071-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="31071-410">Entfernen eines Clients aus einer Gruppe</span><span class="sxs-lookup"><span data-stu-id="31071-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="31071-411">Vorgehensweise beim Anpassen der Hubs-Pipeline</span><span class="sxs-lookup"><span data-stu-id="31071-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="31071-412">Mit signalr können Sie Ihren eigenen Code in die Hub-Pipeline einfügen.</span><span class="sxs-lookup"><span data-stu-id="31071-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="31071-413">Das folgende Beispiel zeigt ein benutzerdefiniertes Hub Pipeline Modul, das jeden eingehenden Methodenaufruf protokolliert, der vom Client empfangen wurde, und den Aufruf der ausgehenden Methode, der auf dem Client</span><span class="sxs-lookup"><span data-stu-id="31071-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="31071-414">Der folgende Code in der Datei *Global. asax* registriert das Modul, das in der Hub-Pipeline ausgeführt werden soll:</span><span class="sxs-lookup"><span data-stu-id="31071-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="31071-415">Es gibt viele verschiedene Methoden, die Sie überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="31071-415">There are many different methods that you can override.</span></span> <span data-ttu-id="31071-416">Eine umfassende Liste finden Sie unter [hubpipelinemodule-Methoden](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="31071-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
