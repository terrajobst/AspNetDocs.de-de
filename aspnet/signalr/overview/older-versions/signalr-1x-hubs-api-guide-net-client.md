---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: 'ASP.net signalr Hubs-API-Handbuch: .NET-Client (signalr 1. x) | Microsoft-Dokumentation'
author: bradygaster
description: Dieses Dokument bietet eine Einführung in die Verwendung der Hubs-API für signalr Version 2 in .NET-Clients, wie z. b. Windows Store (WinRT), WPF, Silverlight und Nachteile...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 2b22b53c405a865f91b04e677f60b82dd46dbf9b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505971"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="c36b2-103">ASP.net signalr Hubs-API-Handbuch: .NET-Client (signalr 1. x)</span><span class="sxs-lookup"><span data-stu-id="c36b2-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>

<span data-ttu-id="c36b2-104">von [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c36b2-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="c36b2-105">Dieses Dokument bietet eine Einführung in die Verwendung der Hubs-API für signalr Version 2 in .NET-Clients, wie z. b. Windows Store (WinRT), WPF, Silverlight und Konsolen Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="c36b2-106">Die signalr Hubs-API ermöglicht das Ausführen von Remote Prozedur aufrufen (Remote Procedure Calls, RPCs) von einem Server an verbundene Clients und von Clients an den Server.</span><span class="sxs-lookup"><span data-stu-id="c36b2-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="c36b2-107">In Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Client ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="c36b2-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="c36b2-108">Im Client Code definieren Sie Methoden, die vom Server aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Server ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="c36b2-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="c36b2-109">Signalr kümmert sich um die gesamte Client-zu-Server-Grundlagen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="c36b2-110">Signalr bietet auch eine API auf niedrigerer Ebene, die als persistente Verbindungen bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="c36b2-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="c36b2-111">Eine Einführung in signalr, Hubs und persistente Verbindungen oder ein Tutorial, das zeigt, wie Sie eine komplette signalr-Anwendung erstellen, finden Sie unter [signalr-Getting Started](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="c36b2-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>

## <a name="overview"></a><span data-ttu-id="c36b2-112">Übersicht</span><span class="sxs-lookup"><span data-stu-id="c36b2-112">Overview</span></span>

<span data-ttu-id="c36b2-113">Dieses Dokument enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="c36b2-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="c36b2-114">Client Einrichtung</span><span class="sxs-lookup"><span data-stu-id="c36b2-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="c36b2-115">Einrichten einer Verbindung</span><span class="sxs-lookup"><span data-stu-id="c36b2-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="c36b2-116">Domänen übergreifende Verbindungen von Silverlight-Clients</span><span class="sxs-lookup"><span data-stu-id="c36b2-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="c36b2-117">Konfigurieren der Verbindung</span><span class="sxs-lookup"><span data-stu-id="c36b2-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="c36b2-118">Festlegen der maximalen Anzahl gleichzeitiger Verbindungen in WPF-Clients</span><span class="sxs-lookup"><span data-stu-id="c36b2-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="c36b2-119">Angeben von Abfrage Zeichen folgen Parametern</span><span class="sxs-lookup"><span data-stu-id="c36b2-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="c36b2-120">Angeben der Transportmethode</span><span class="sxs-lookup"><span data-stu-id="c36b2-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="c36b2-121">Angeben von HTTP-Headern</span><span class="sxs-lookup"><span data-stu-id="c36b2-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="c36b2-122">Angeben von Client Zertifikaten</span><span class="sxs-lookup"><span data-stu-id="c36b2-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="c36b2-123">Erstellen des Hub-Proxys</span><span class="sxs-lookup"><span data-stu-id="c36b2-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="c36b2-124">Definieren von Methoden auf dem Client, der vom Server aufgerufen werden kann</span><span class="sxs-lookup"><span data-stu-id="c36b2-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="c36b2-125">Methoden ohne Parameter</span><span class="sxs-lookup"><span data-stu-id="c36b2-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="c36b2-126">Methoden mit Parametern, angeben von Parametertypen</span><span class="sxs-lookup"><span data-stu-id="c36b2-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="c36b2-127">Methoden mit Parametern, die dynamische Objekte für die Parameter angeben</span><span class="sxs-lookup"><span data-stu-id="c36b2-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="c36b2-128">Vorgehensweise beim Entfernen eines Handlers</span><span class="sxs-lookup"><span data-stu-id="c36b2-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="c36b2-129">Vorgehensweise beim Abrufen von Server Methoden vom Client</span><span class="sxs-lookup"><span data-stu-id="c36b2-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="c36b2-130">Behandeln von Ereignissen der Verbindungs Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="c36b2-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="c36b2-131">Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="c36b2-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="c36b2-132">Aktivieren der Client seitigen Protokollierung</span><span class="sxs-lookup"><span data-stu-id="c36b2-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="c36b2-133">WPF-, Silverlight-und Konsolen Anwendungscode Beispiele für Client Methoden, die vom Server aufgerufen werden können</span><span class="sxs-lookup"><span data-stu-id="c36b2-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="c36b2-134">Ein Beispiel für .NET-Client Projekte finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="c36b2-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="c36b2-135">[Gustavo-Armenta/signalr-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, Konsolen-App-Beispiele).</span><span class="sxs-lookup"><span data-stu-id="c36b2-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="c36b2-136">[Damianedwards/signalr-muveshapeer Demo/muveshape. Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF-Beispiel).</span><span class="sxs-lookup"><span data-stu-id="c36b2-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="c36b2-137">[Signalr/Microsoft. Aspnet. signalr. Client. Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) auf GitHub.com (Konsolen-App-Beispiel).</span><span class="sxs-lookup"><span data-stu-id="c36b2-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="c36b2-138">Dokumentation zum Programmieren der Server-oder JavaScript-Clients finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="c36b2-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="c36b2-139">Leitfaden für signalr-Hubs-API-Server</span><span class="sxs-lookup"><span data-stu-id="c36b2-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="c36b2-140">Leitfaden für signalr-Hubs-API-JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="c36b2-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="c36b2-141">Links zu API-Referenz Themen beziehen sich auf die .NET 4,5-Version der API.</span><span class="sxs-lookup"><span data-stu-id="c36b2-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="c36b2-142">Wenn Sie .NET 4 verwenden, finden Sie weitere Informationen unter [.NET 4-Version der API-Themen](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="c36b2-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="c36b2-143">Clientsetup</span><span class="sxs-lookup"><span data-stu-id="c36b2-143">Client setup</span></span>

<span data-ttu-id="c36b2-144">Installieren Sie das nuget-Paket [Microsoft. Aspnet. signalr. Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (nicht das Paket [Microsoft. Aspnet. signalr](http://nuget.org/packages/microsoft.aspnet.signalr) ).</span><span class="sxs-lookup"><span data-stu-id="c36b2-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="c36b2-145">Dieses Paket unterstützt WinRT-, Silverlight-, WPF-, Konsolen Anwendungs-und Windows Phone Clients sowohl für .NET 4 als auch für .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="c36b2-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="c36b2-146">Wenn sich die Version von signalr, die Sie auf dem Client haben, von der auf dem Server fest zufügende Version unterscheidet, kann signalr häufig an den Unterschied angepasst werden.</span><span class="sxs-lookup"><span data-stu-id="c36b2-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="c36b2-147">Wenn z. b. signalr, Version 2,0, veröffentlicht wird und Sie diese auf dem Server installieren, unterstützt der Server Clients, auf denen 1.1. x installiert ist, sowie Clients, auf denen 2,0 installiert ist.</span><span class="sxs-lookup"><span data-stu-id="c36b2-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="c36b2-148">Wenn der Unterschied zwischen der Version auf dem Server und der Version auf dem Client zu groß ist, löst signalr eine `InvalidOperationException` Ausnahme aus, wenn der Client versucht, eine Verbindung herzustellen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="c36b2-149">Die Fehlermeldung lautet "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="c36b2-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="c36b2-150">Einrichten einer Verbindung</span><span class="sxs-lookup"><span data-stu-id="c36b2-150">How to establish a connection</span></span>

<span data-ttu-id="c36b2-151">Bevor Sie eine Verbindung herstellen können, müssen Sie ein `HubConnection` Objekt erstellen und einen Proxy erstellen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="c36b2-152">Um die Verbindung herzustellen, wenden Sie die `Start`-Methode für das `HubConnection`-Objekt an.</span><span class="sxs-lookup"><span data-stu-id="c36b2-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="c36b2-153">Bei JavaScript-Clients müssen Sie mindestens einen Ereignishandler registrieren, bevor Sie die `Start`-Methode aufrufen, um die Verbindung herzustellen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="c36b2-154">Dies ist für .NET-Clients nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c36b2-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="c36b2-155">Bei JavaScript-Clients erstellt der generierte Proxy Code automatisch Proxys für alle auf dem Server vorhandenen Hubs, und bei der Registrierung eines Handlers wird angegeben, welche Hubs der Client verwenden soll.</span><span class="sxs-lookup"><span data-stu-id="c36b2-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="c36b2-156">Für einen .NET-Client erstellen Sie aber manuell Hub-Proxys, daher geht signalr davon aus, dass Sie einen beliebigen Hub verwenden, für den Sie einen Proxy erstellen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>

<span data-ttu-id="c36b2-157">Im Beispielcode wird die Standard-URL "/signalr" verwendet, um eine Verbindung mit Ihrem signalr-Dienst herzustellen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="c36b2-158">Weitere Informationen zum Angeben einer anderen Basis-URL finden Sie unter [ASP.net signalr Hubs API Guide-Server-the/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="c36b2-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="c36b2-159">Die `Start`-Methode wird asynchron ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c36b2-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="c36b2-160">Um sicherzustellen, dass nachfolgende Codezeilen erst ausgeführt werden, nachdem die Verbindung hergestellt wurde, verwenden Sie `await` in einer asynchronen ASP.NET 4,5-Methode oder in einer synchronen Methode `.Wait()`.</span><span class="sxs-lookup"><span data-stu-id="c36b2-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="c36b2-161">Verwenden Sie `.Wait()` nicht in einem WinRT-Client.</span><span class="sxs-lookup"><span data-stu-id="c36b2-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="c36b2-162">Die `HubConnection`-Klasse ist threadsicher.</span><span class="sxs-lookup"><span data-stu-id="c36b2-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="c36b2-163">Domänen übergreifende Verbindungen von Silverlight-Clients</span><span class="sxs-lookup"><span data-stu-id="c36b2-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="c36b2-164">Informationen zum Aktivieren von Domänen übergreifenden Verbindungen von Silverlight-Clients finden Sie unter [Bereitstellen eines Dienstanbieter übergreifenden Bereichs](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="c36b2-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="c36b2-165">Konfigurieren der Verbindung</span><span class="sxs-lookup"><span data-stu-id="c36b2-165">How to configure the connection</span></span>

<span data-ttu-id="c36b2-166">Bevor Sie eine Verbindung herstellen, können Sie eine der folgenden Optionen angeben:</span><span class="sxs-lookup"><span data-stu-id="c36b2-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="c36b2-167">Limit für gleichzeitige Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="c36b2-168">Abfrage Zeichen folgen Parameter.</span><span class="sxs-lookup"><span data-stu-id="c36b2-168">Query string parameters.</span></span>
- <span data-ttu-id="c36b2-169">Die Transportmethode.</span><span class="sxs-lookup"><span data-stu-id="c36b2-169">The transport method.</span></span>
- <span data-ttu-id="c36b2-170">HTTP-Header.</span><span class="sxs-lookup"><span data-stu-id="c36b2-170">HTTP headers.</span></span>
- <span data-ttu-id="c36b2-171">Client Zertifikate.</span><span class="sxs-lookup"><span data-stu-id="c36b2-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="c36b2-172">Festlegen der maximalen Anzahl gleichzeitiger Verbindungen in WPF-Clients</span><span class="sxs-lookup"><span data-stu-id="c36b2-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="c36b2-173">In WPF-Clients müssen Sie möglicherweise die maximale Anzahl gleichzeitiger Verbindungen mit dem Standardwert 2 erhöhen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="c36b2-174">Der empfohlene Wert ist 10.</span><span class="sxs-lookup"><span data-stu-id="c36b2-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="c36b2-175">Weitere Informationen finden Sie unter [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="c36b2-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="c36b2-176">Angeben von Abfrage Zeichen folgen Parametern</span><span class="sxs-lookup"><span data-stu-id="c36b2-176">How to specify query string parameters</span></span>

<span data-ttu-id="c36b2-177">Wenn Sie Daten an den Server senden möchten, wenn der Client eine Verbindung herstellt, können Sie dem Verbindungs Objekt Abfrage Zeichenfolgen-Parameter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="c36b2-178">Im folgenden Beispiel wird gezeigt, wie ein Abfrage Zeichen folgen Parameter im Client Code festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="c36b2-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="c36b2-179">Im folgenden Beispiel wird gezeigt, wie ein Abfrage Zeichen folgen Parameter im Servercode gelesen wird.</span><span class="sxs-lookup"><span data-stu-id="c36b2-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="c36b2-180">Angeben der Transportmethode</span><span class="sxs-lookup"><span data-stu-id="c36b2-180">How to specify the transport method</span></span>

<span data-ttu-id="c36b2-181">Im Rahmen der Verbindungs Herstellung aushandiert ein signalr-Client normalerweise mit dem Server, um den optimalen Transport zu ermitteln, der von Server und Client unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="c36b2-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="c36b2-182">Wenn Sie bereits wissen, welcher Transport Sie verwenden möchten, können Sie diesen Aushandlungs Prozess umgehen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="c36b2-183">Um die Transportmethode anzugeben, übergeben Sie ein Transport Objekt an die Start Methode.</span><span class="sxs-lookup"><span data-stu-id="c36b2-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="c36b2-184">Im folgenden Beispiel wird gezeigt, wie die Transportmethode im Client Code angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="c36b2-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="c36b2-185">Der [Microsoft. Aspnet. signalr. Client. Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) -Namespace enthält die folgenden Klassen, die Sie zum Angeben des Transports verwenden können.</span><span class="sxs-lookup"><span data-stu-id="c36b2-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="c36b2-186">[Longpollingtransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="c36b2-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="c36b2-187">[Serversenteventstransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="c36b2-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="c36b2-188">[Websockettransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (nur verfügbar, wenn sowohl der Server als auch der Client .NET 4,5 verwenden.)</span><span class="sxs-lookup"><span data-stu-id="c36b2-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="c36b2-189">[Autotransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (wählt automatisch den optimalen Transport aus, der sowohl vom Client als auch vom Server unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="c36b2-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="c36b2-190">Dies ist der Standard Transport.</span><span class="sxs-lookup"><span data-stu-id="c36b2-190">This is the default transport.</span></span> <span data-ttu-id="c36b2-191">Wenn Sie diese Angabe an die `Start`-Methode vornehmen, hat dies denselben Effekt wie das Übergeben von nichts.)</span><span class="sxs-lookup"><span data-stu-id="c36b2-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="c36b2-192">Der foreverframe-Transport ist nicht in dieser Liste enthalten, da er nur von Browsern verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c36b2-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="c36b2-193">Informationen zum Überprüfen der Transportmethode im Servercode finden Sie unter [ASP.net signalr Hubs API Guide-Server-How to Get Information about the Client from the context Property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="c36b2-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="c36b2-194">Weitere Informationen zu Transporten und Fallbacks finden Sie unter [Einführung in signalr-Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="c36b2-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="c36b2-195">Angeben von HTTP-Headern</span><span class="sxs-lookup"><span data-stu-id="c36b2-195">How to specify HTTP headers</span></span>

<span data-ttu-id="c36b2-196">Um HTTP-Header festzulegen, verwenden Sie die `Headers`-Eigenschaft für das Verbindungs Objekt.</span><span class="sxs-lookup"><span data-stu-id="c36b2-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="c36b2-197">Im folgenden Beispiel wird gezeigt, wie Sie einen HTTP-Header hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="c36b2-198">Angeben von Client Zertifikaten</span><span class="sxs-lookup"><span data-stu-id="c36b2-198">How to specify client certificates</span></span>

<span data-ttu-id="c36b2-199">Verwenden Sie zum Hinzufügen von Client Zertifikaten die `AddClientCertificate`-Methode für das Verbindungs Objekt.</span><span class="sxs-lookup"><span data-stu-id="c36b2-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="c36b2-200">Erstellen des Hub-Proxys</span><span class="sxs-lookup"><span data-stu-id="c36b2-200">How to create the Hub proxy</span></span>

<span data-ttu-id="c36b2-201">Um Methoden auf dem Client zu definieren, die ein Hub vom Server aufrufen kann, und um Methoden für einen Hub auf dem Server aufzurufen, erstellen Sie einen Proxy für den Hub, indem Sie `CreateHubProxy` für das Verbindungs Objekt aufrufen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="c36b2-202">Die Zeichenfolge, die Sie an `CreateHubProxy` übergeben, ist der Name der Hub-Klasse oder der Name, der vom `HubName`-Attribut angegeben wird, wenn auf dem Server eine verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="c36b2-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="c36b2-203">Beim Namensvergleich wird die Groß-/Kleinschreibung nicht beachtet</span><span class="sxs-lookup"><span data-stu-id="c36b2-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="c36b2-204">**Hub-Klasse auf dem Server**</span><span class="sxs-lookup"><span data-stu-id="c36b2-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="c36b2-205">**Erstellen eines Client Proxys für die Hub-Klasse**</span><span class="sxs-lookup"><span data-stu-id="c36b2-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="c36b2-206">Wenn Sie Ihre Hub-Klasse mit einem `HubName`-Attribut versehen, verwenden Sie diesen Namen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="c36b2-207">**Hub-Klasse auf dem Server**</span><span class="sxs-lookup"><span data-stu-id="c36b2-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="c36b2-208">**Erstellen eines Client Proxys für die Hub-Klasse**</span><span class="sxs-lookup"><span data-stu-id="c36b2-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="c36b2-209">Das Proxy Objekt ist Thread sicher.</span><span class="sxs-lookup"><span data-stu-id="c36b2-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="c36b2-210">Wenn Sie `HubConnection.CreateHubProxy` mehrmals mit demselben `hubName`aufzurufen, erhalten Sie tatsächlich dasselbe zwischengespeicherte `IHubProxy` Objekt.</span><span class="sxs-lookup"><span data-stu-id="c36b2-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="c36b2-211">Definieren von Methoden auf dem Client, der vom Server aufgerufen werden kann</span><span class="sxs-lookup"><span data-stu-id="c36b2-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="c36b2-212">Zum Definieren einer Methode, die vom Server aufgerufen werden kann, verwenden Sie die `On`-Methode des Proxys, um einen Ereignishandler zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="c36b2-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="c36b2-213">Beim Methodennamen wird die Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="c36b2-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="c36b2-214">Beispielsweise werden auf dem-Server `Clients.All.UpdateStockPrice` auf dem-Server `updateStockPrice`, `updatestockprice`oder `UpdateStockPrice` auf dem Client ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c36b2-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="c36b2-215">Für verschiedene Client Plattformen gelten andere Anforderungen, wie Sie Methoden Code schreiben, um die Benutzeroberfläche zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="c36b2-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="c36b2-216">Die gezeigten Beispiele sind für WinRT-Clients (Windows Store .net).</span><span class="sxs-lookup"><span data-stu-id="c36b2-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="c36b2-217">Beispiele für WPF, Silverlight und Konsolen Anwendungen werden in [einem separaten Abschnitt weiter unten in diesem Thema](#wpfsl)bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="c36b2-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="c36b2-218">Methoden ohne Parameter</span><span class="sxs-lookup"><span data-stu-id="c36b2-218">Methods without parameters</span></span>

<span data-ttu-id="c36b2-219">Wenn die Methode, die Sie behandeln, keine Parameter enthält, verwenden Sie die nicht generische Überladung der `On`-Methode:</span><span class="sxs-lookup"><span data-stu-id="c36b2-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="c36b2-220">**Server Code Aufruf der Client Methode ohne Parameter**</span><span class="sxs-lookup"><span data-stu-id="c36b2-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="c36b2-221">**WinRT-Client Code für die Methode, die vom Server ohne Parameter aufgerufen wird ([siehe WPF-und Silverlight-Beispiele weiter unten in diesem Thema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="c36b2-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="c36b2-222">Methoden mit Parametern, die Parametertypen angeben</span><span class="sxs-lookup"><span data-stu-id="c36b2-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="c36b2-223">Wenn die Methode, die Sie behandeln, über Parameter verfügt, geben Sie die Typen der Parameter als generische Typen der `On` Methode an.</span><span class="sxs-lookup"><span data-stu-id="c36b2-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="c36b2-224">Es gibt generische über Ladungen der `On`-Methode, die es Ihnen ermöglichen, bis zu 8 Parameter (4 auf Windows Phone 7) anzugeben.</span><span class="sxs-lookup"><span data-stu-id="c36b2-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="c36b2-225">Im folgenden Beispiel wird ein Parameter an die `UpdateStockPrice`-Methode gesendet.</span><span class="sxs-lookup"><span data-stu-id="c36b2-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="c36b2-226">**Server Code zum Aufrufen der Client Methode mit einem Parameter**</span><span class="sxs-lookup"><span data-stu-id="c36b2-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="c36b2-227">**Die für den Parameter verwendete Aktienklasse**</span><span class="sxs-lookup"><span data-stu-id="c36b2-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="c36b2-228">**WinRT-Client Code für eine Methode, die vom Server mit einem Parameter aufgerufen wird ([Siehe Beispiele zu WPF und Silverlight weiter unten in diesem Thema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="c36b2-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="c36b2-229">Methoden mit Parametern, die dynamische Objekte für die Parameter angeben</span><span class="sxs-lookup"><span data-stu-id="c36b2-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="c36b2-230">Als Alternative zum Angeben von Parametern als generische Typen der `On`-Methode können Sie Parameter als dynamische Objekte angeben:</span><span class="sxs-lookup"><span data-stu-id="c36b2-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="c36b2-231">**Server Code zum Aufrufen der Client Methode mit einem Parameter**</span><span class="sxs-lookup"><span data-stu-id="c36b2-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="c36b2-232">**Die für den Parameter verwendete Aktienklasse**</span><span class="sxs-lookup"><span data-stu-id="c36b2-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="c36b2-233">**WinRT-Client Code für eine Methode, die vom Server mit einem Parameter aufgerufen wird, wobei ein dynamisches Objekt für den Parameter verwendet wird ([siehe WPF-und Silverlight-Beispiele weiter unten in diesem Thema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="c36b2-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="c36b2-234">Vorgehensweise beim Entfernen eines Handlers</span><span class="sxs-lookup"><span data-stu-id="c36b2-234">How to remove a handler</span></span>

<span data-ttu-id="c36b2-235">Um einen Handler zu entfernen, müssen Sie dessen `Dispose`-Methode aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="c36b2-236">**Client Code für eine vom Server aufgerufene Methode**</span><span class="sxs-lookup"><span data-stu-id="c36b2-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="c36b2-237">**Client Code zum Entfernen des Handlers**</span><span class="sxs-lookup"><span data-stu-id="c36b2-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="c36b2-238">Vorgehensweise beim Abrufen von Server Methoden vom Client</span><span class="sxs-lookup"><span data-stu-id="c36b2-238">How to call server methods from the client</span></span>

<span data-ttu-id="c36b2-239">Um eine Methode auf dem Server aufzurufen, verwenden Sie die `Invoke`-Methode des Hub-Proxys.</span><span class="sxs-lookup"><span data-stu-id="c36b2-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="c36b2-240">Wenn die Server Methode über keinen Rückgabewert verfügt, verwenden Sie die nicht generische Überladung der `Invoke`-Methode.</span><span class="sxs-lookup"><span data-stu-id="c36b2-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="c36b2-241">**Server Code für eine Methode, die über keinen Rückgabewert verfügt**</span><span class="sxs-lookup"><span data-stu-id="c36b2-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="c36b2-242">**Client Code zum Aufrufen einer Methode ohne Rückgabewert**</span><span class="sxs-lookup"><span data-stu-id="c36b2-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="c36b2-243">Wenn die Server Methode über einen Rückgabewert verfügt, geben Sie den Rückgabetyp als generischen Typ der `Invoke` Methode an.</span><span class="sxs-lookup"><span data-stu-id="c36b2-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="c36b2-244">**Server Code für eine Methode, die über einen Rückgabewert verfügt und einen komplexen Typparameter annimmt**</span><span class="sxs-lookup"><span data-stu-id="c36b2-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="c36b2-245">**Die für den Parameter und den Rückgabewert verwendete Aktienklasse**</span><span class="sxs-lookup"><span data-stu-id="c36b2-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="c36b2-246">**Client Code, der eine Methode mit einem Rückgabewert und einem komplexen Typparameter in einer ASP.NET 4,5 Async-Methode aufrufen**</span><span class="sxs-lookup"><span data-stu-id="c36b2-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="c36b2-247">**Client Code, der eine Methode mit einem Rückgabewert und einem komplexen Typparameter in einer synchronen Methode aufrufen**</span><span class="sxs-lookup"><span data-stu-id="c36b2-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="c36b2-248">Die `Invoke`-Methode wird asynchron ausgeführt und gibt ein `Task`-Objekt zurück.</span><span class="sxs-lookup"><span data-stu-id="c36b2-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="c36b2-249">Wenn Sie `await` oder `.Wait()`nicht angeben, wird die nächste Codezeile ausgeführt, bevor die von Ihnen aufgerufene Methode die Ausführung abgeschlossen hat.</span><span class="sxs-lookup"><span data-stu-id="c36b2-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="c36b2-250">Behandeln von Ereignissen der Verbindungs Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="c36b2-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="c36b2-251">Signalr stellt die folgenden Ereignisse für die Verbindungs Lebensdauer bereit, die Sie behandeln können:</span><span class="sxs-lookup"><span data-stu-id="c36b2-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="c36b2-252">`Received`: wird ausgelöst, wenn Daten über die Verbindung empfangen werden.</span><span class="sxs-lookup"><span data-stu-id="c36b2-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="c36b2-253">Stellt die empfangenen Daten bereit.</span><span class="sxs-lookup"><span data-stu-id="c36b2-253">Provides the received data.</span></span>
- <span data-ttu-id="c36b2-254">`ConnectionSlow`: wird ausgelöst, wenn der Client eine langsame oder häufig gelöschter Verbindung erkennt.</span><span class="sxs-lookup"><span data-stu-id="c36b2-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="c36b2-255">`Reconnecting`: wird ausgelöst, wenn der zugrunde liegende Transport mit dem erneuten Verbinden beginnt.</span><span class="sxs-lookup"><span data-stu-id="c36b2-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="c36b2-256">`Reconnected`: wird ausgelöst, wenn der zugrunde liegende Transport die Verbindung wieder hergestellt hat.</span><span class="sxs-lookup"><span data-stu-id="c36b2-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="c36b2-257">`StateChanged`: wird ausgelöst, wenn sich der Verbindungsstatus ändert.</span><span class="sxs-lookup"><span data-stu-id="c36b2-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="c36b2-258">Stellt den alten und den neuen Zustand bereit.</span><span class="sxs-lookup"><span data-stu-id="c36b2-258">Provides the old state and the new state.</span></span> <span data-ttu-id="c36b2-259">Weitere Informationen zu Verbindungs Zustands Werten finden Sie unter [ConnectionState-Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="c36b2-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="c36b2-260">`Closed`: wird ausgelöst, wenn die Verbindung getrennt wurde.</span><span class="sxs-lookup"><span data-stu-id="c36b2-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="c36b2-261">Wenn Sie z. b. Warnmeldungen für Fehler anzeigen möchten, die nicht schwerwiegend sind, aber zeitweilig auftretende Verbindungsprobleme verursachen, z. b. verlangsamtheit oder häufiges Löschen der Verbindung, behandeln Sie das `ConnectionSlow`-Ereignis.</span><span class="sxs-lookup"><span data-stu-id="c36b2-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="c36b2-262">Weitere Informationen finden Sie unter [verstehen und behandeln von Verbindungs Lebensdauer-Ereignissen in signalr](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="c36b2-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="c36b2-263">Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="c36b2-263">How to handle errors</span></span>

<span data-ttu-id="c36b2-264">Wenn Sie auf dem Server nicht explizit detaillierte Fehlermeldungen aktivieren, enthält das Ausnahme Objekt, das signalr nach einem Fehler zurückgibt, nur minimale Informationen über den Fehler.</span><span class="sxs-lookup"><span data-stu-id="c36b2-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="c36b2-265">Wenn beispielsweise ein `newContosoChatMessage` Fehler auftritt, enthält die Fehlermeldung im Fehler Objekt den Wert "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`", dass detaillierte Fehlermeldungen an Clients in der Produktionsumgebung nicht gesendet werden sollen. Wenn Sie jedoch ausführliche Fehlermeldungen zu Problem Behandlungszwecken aktivieren möchten, verwenden Sie den folgenden Code auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="c36b2-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="c36b2-266">Um Fehler zu behandeln, die von signalr ausgelöst werden, können Sie einen Handler für das `Error`-Ereignis für das Verbindungs Objekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c36b2-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="c36b2-267">Um Fehler von Methoden aufrufen zu behandeln, wrappen Sie den Code in einem try-catch-Block.</span><span class="sxs-lookup"><span data-stu-id="c36b2-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="c36b2-268">Aktivieren der Client seitigen Protokollierung</span><span class="sxs-lookup"><span data-stu-id="c36b2-268">How to enable client-side logging</span></span>

<span data-ttu-id="c36b2-269">Um die Client seitige Protokollierung zu aktivieren, legen Sie die Eigenschaften `TraceLevel` und `TraceWriter` für das Verbindungs Objekt fest.</span><span class="sxs-lookup"><span data-stu-id="c36b2-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="c36b2-270">WPF-, Silverlight-und Konsolen Anwendungscode Beispiele für Client Methoden, die vom Server aufgerufen werden können</span><span class="sxs-lookup"><span data-stu-id="c36b2-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="c36b2-271">Die zuvor gezeigten Codebeispiele zum Definieren von Client Methoden, die vom Server aufgerufen werden können, gelten für WinRT-Clients.</span><span class="sxs-lookup"><span data-stu-id="c36b2-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="c36b2-272">In den folgenden Beispielen wird der entsprechende Code für WPF-, Silverlight-und Konsolen Anwendungs Clients angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c36b2-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="c36b2-273">Methoden ohne Parameter</span><span class="sxs-lookup"><span data-stu-id="c36b2-273">Methods without parameters</span></span>

<span data-ttu-id="c36b2-274">**WPF-Client Code für die Methode, die vom Server ohne Parameter aufgerufen wird**</span><span class="sxs-lookup"><span data-stu-id="c36b2-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="c36b2-275">**Silverlight-Client Code für die Methode, die vom Server ohne Parameter aufgerufen wird**</span><span class="sxs-lookup"><span data-stu-id="c36b2-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="c36b2-276">**Client Code der Konsolenanwendung für die Methode, die vom Server ohne Parameter aufgerufen wird**</span><span class="sxs-lookup"><span data-stu-id="c36b2-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="c36b2-277">Methoden mit Parametern, die Parametertypen angeben</span><span class="sxs-lookup"><span data-stu-id="c36b2-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="c36b2-278">**WPF-Client Code für eine Methode, die vom Server mit einem Parameter aufgerufen wird**</span><span class="sxs-lookup"><span data-stu-id="c36b2-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="c36b2-279">**Silverlight-Client Code für eine Methode, die von einem Server mit einem Parameter aufgerufen wird**</span><span class="sxs-lookup"><span data-stu-id="c36b2-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="c36b2-280">**Client Code der Konsolenanwendung für eine Methode, die von einem Server mit einem Parameter aufgerufen wird**</span><span class="sxs-lookup"><span data-stu-id="c36b2-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="c36b2-281">Methoden mit Parametern, die dynamische Objekte für die Parameter angeben</span><span class="sxs-lookup"><span data-stu-id="c36b2-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="c36b2-282">**WPF-Client Code für eine Methode, die vom Server mit einem Parameter aufgerufen wird, wobei ein dynamisches Objekt für den Parameter verwendet wird.**</span><span class="sxs-lookup"><span data-stu-id="c36b2-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="c36b2-283">**Silverlight-Client Code für eine Methode, die von Server mit einem Parameter aufgerufen wird, wobei ein dynamisches Objekt für den Parameter verwendet wird.**</span><span class="sxs-lookup"><span data-stu-id="c36b2-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="c36b2-284">**Client Code für Konsolen Anwendungen für eine Methode, die vom Server mit einem Parameter aufgerufen wird, wobei ein dynamisches Objekt für den Parameter verwendet wird.**</span><span class="sxs-lookup"><span data-stu-id="c36b2-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
