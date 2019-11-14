---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: 'ASP.net signalr Hubs-API-Handbuch: .netC#-Client () | Microsoft-Dokumentation'
author: bradygaster
description: Dieses Dokument bietet eine Einführung in die Verwendung der Hubs-API für signalr Version 2 in .NET-Clients, wie z. b. Windows Store (WinRT), WPF, Silverlight und Nachteile...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: d3536f1c15cd7dad7cd660becf0577e5c131f707
ms.sourcegitcommit: 295cf898a4c87e264b0c35c7254b0fa4169f2278
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74057011"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="f14ce-103">ASP.net signalr Hubs-API-Handbuch: .netC#-Client ()</span><span class="sxs-lookup"><span data-stu-id="f14ce-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="f14ce-104">Dieses Dokument bietet eine Einführung in die Verwendung der Hubs-API für signalr Version 2 in .NET-Clients, wie z. b. Windows Store (WinRT), WPF, Silverlight und Konsolen Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-104">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
>
> <span data-ttu-id="f14ce-105">Die signalr Hubs-API ermöglicht das Ausführen von Remote Prozedur aufrufen (Remote Procedure Calls, RPCs) von einem Server an verbundene Clients und von Clients an den Server.</span><span class="sxs-lookup"><span data-stu-id="f14ce-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="f14ce-106">In Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Client ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="f14ce-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="f14ce-107">Im Client Code definieren Sie Methoden, die vom Server aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Server ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="f14ce-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="f14ce-108">Signalr kümmert sich um die gesamte Client-zu-Server-Grundlagen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="f14ce-109">Signalr bietet auch eine API auf niedrigerer Ebene, die als persistente Verbindungen bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="f14ce-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="f14ce-110">Eine Einführung in signalr, Hubs und persistente Verbindungen oder ein Tutorial, das zeigt, wie Sie eine komplette signalr-Anwendung erstellen, finden Sie unter [signalr-Getting Started](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="f14ce-110">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="f14ce-111">In diesem Thema verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="f14ce-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="f14ce-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="f14ce-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="f14ce-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f14ce-113">.NET 4.5</span></span>
> - <span data-ttu-id="f14ce-114">Signalr Version 2</span><span class="sxs-lookup"><span data-stu-id="f14ce-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="f14ce-115">Vorherige Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="f14ce-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="f14ce-116">Informationen zu früheren Versionen von signalr finden Sie unter [signalr ältere Versionen](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="f14ce-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="f14ce-117">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="f14ce-117">Questions and comments</span></span>
>
> <span data-ttu-id="f14ce-118">Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten.</span><span class="sxs-lookup"><span data-stu-id="f14ce-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f14ce-119">Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="f14ce-120">Übersicht</span><span class="sxs-lookup"><span data-stu-id="f14ce-120">Overview</span></span>

<span data-ttu-id="f14ce-121">Dieses Dokument enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="f14ce-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="f14ce-122">Client Einrichtung</span><span class="sxs-lookup"><span data-stu-id="f14ce-122">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="f14ce-123">Einrichten einer Verbindung</span><span class="sxs-lookup"><span data-stu-id="f14ce-123">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="f14ce-124">Domänen übergreifende Verbindungen von Silverlight-Clients</span><span class="sxs-lookup"><span data-stu-id="f14ce-124">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="f14ce-125">Konfigurieren der Verbindung</span><span class="sxs-lookup"><span data-stu-id="f14ce-125">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="f14ce-126">Festlegen der maximalen Anzahl gleichzeitiger Verbindungen in WPF-Clients</span><span class="sxs-lookup"><span data-stu-id="f14ce-126">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="f14ce-127">Angeben von Abfrage Zeichen folgen Parametern</span><span class="sxs-lookup"><span data-stu-id="f14ce-127">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="f14ce-128">Angeben der Transportmethode</span><span class="sxs-lookup"><span data-stu-id="f14ce-128">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="f14ce-129">Angeben von HTTP-Headern</span><span class="sxs-lookup"><span data-stu-id="f14ce-129">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="f14ce-130">Angeben von Client Zertifikaten</span><span class="sxs-lookup"><span data-stu-id="f14ce-130">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="f14ce-131">Erstellen des Hub-Proxys</span><span class="sxs-lookup"><span data-stu-id="f14ce-131">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="f14ce-132">Definieren von Methoden auf dem Client, der vom Server aufgerufen werden kann</span><span class="sxs-lookup"><span data-stu-id="f14ce-132">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="f14ce-133">Methoden ohne Parameter</span><span class="sxs-lookup"><span data-stu-id="f14ce-133">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="f14ce-134">Methoden mit Parametern, angeben von Parametertypen</span><span class="sxs-lookup"><span data-stu-id="f14ce-134">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="f14ce-135">Methoden mit Parametern, die dynamische Objekte für die Parameter angeben</span><span class="sxs-lookup"><span data-stu-id="f14ce-135">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="f14ce-136">Vorgehensweise beim Entfernen eines Handlers</span><span class="sxs-lookup"><span data-stu-id="f14ce-136">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="f14ce-137">Vorgehensweise beim Abrufen von Server Methoden vom Client</span><span class="sxs-lookup"><span data-stu-id="f14ce-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="f14ce-138">Behandeln von Ereignissen der Verbindungs Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="f14ce-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="f14ce-139">Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="f14ce-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="f14ce-140">Aktivieren der Client seitigen Protokollierung</span><span class="sxs-lookup"><span data-stu-id="f14ce-140">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="f14ce-141">WPF-, Silverlight-und Konsolen Anwendungscode Beispiele für Client Methoden, die vom Server aufgerufen werden können</span><span class="sxs-lookup"><span data-stu-id="f14ce-141">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="f14ce-142">Ein Beispiel für .NET-Client Projekte finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="f14ce-142">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="f14ce-143">[Gustavo-Armenta/signalr-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, Konsolen-App-Beispiele).</span><span class="sxs-lookup"><span data-stu-id="f14ce-143">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="f14ce-144">[Damianedwards/signalr-muveshapeer Demo/muveshape. Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF-Beispiel).</span><span class="sxs-lookup"><span data-stu-id="f14ce-144">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="f14ce-145">[Signalr/Microsoft. Aspnet. signalr. Client. Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) auf GitHub.com (Konsolen-App-Beispiel).</span><span class="sxs-lookup"><span data-stu-id="f14ce-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="f14ce-146">Dokumentation zum Programmieren der Server-oder JavaScript-Clients finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="f14ce-146">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="f14ce-147">Leitfaden für signalr-Hubs-API-Server</span><span class="sxs-lookup"><span data-stu-id="f14ce-147">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="f14ce-148">Leitfaden für signalr-Hubs-API-JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="f14ce-148">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="f14ce-149">Links zu API-Referenz Themen beziehen sich auf die .NET 4,5-Version der API.</span><span class="sxs-lookup"><span data-stu-id="f14ce-149">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="f14ce-150">Wenn Sie .NET 4 verwenden, finden Sie weitere Informationen unter [.NET 4-Version der API-Themen](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="f14ce-150">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="f14ce-151">Client Einrichtung</span><span class="sxs-lookup"><span data-stu-id="f14ce-151">Client setup</span></span>

<span data-ttu-id="f14ce-152">Installieren Sie das nuget-Paket [Microsoft. Aspnet. signalr. Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (nicht das Paket [Microsoft. Aspnet. signalr](http://nuget.org/packages/microsoft.aspnet.signalr) ).</span><span class="sxs-lookup"><span data-stu-id="f14ce-152">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="f14ce-153">Dieses Paket unterstützt WinRT-, Silverlight-, WPF-, Konsolen Anwendungs-und Windows Phone Clients sowohl für .NET 4 als auch für .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="f14ce-153">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="f14ce-154">Wenn sich die Version von signalr, die Sie auf dem Client haben, von der auf dem Server fest zufügende Version unterscheidet, kann signalr häufig an den Unterschied angepasst werden.</span><span class="sxs-lookup"><span data-stu-id="f14ce-154">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="f14ce-155">Beispielsweise unterstützt ein Server, auf dem signalr Version 2 ausgeführt wird, Clients, auf denen 1.1. x installiert ist, sowie Clients, auf denen Version 2 installiert ist.</span><span class="sxs-lookup"><span data-stu-id="f14ce-155">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="f14ce-156">Wenn der Unterschied zwischen der Version auf dem Server und der Version auf dem Client zu groß ist, oder wenn der Client neuer als der Server ist, löst signalr eine `InvalidOperationException` Ausnahme aus, wenn der Client versucht, eine Verbindung herzustellen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-156">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="f14ce-157">Die Fehlermeldung lautet "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="f14ce-157">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="f14ce-158">Einrichten einer Verbindung</span><span class="sxs-lookup"><span data-stu-id="f14ce-158">How to establish a connection</span></span>

<span data-ttu-id="f14ce-159">Bevor Sie eine Verbindung herstellen können, müssen Sie ein `HubConnection` Objekt erstellen und einen Proxy erstellen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-159">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="f14ce-160">Um die Verbindung herzustellen, wenden Sie die `Start`-Methode für das `HubConnection`-Objekt an.</span><span class="sxs-lookup"><span data-stu-id="f14ce-160">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,5)]

> [!NOTE]
> <span data-ttu-id="f14ce-161">Bei JavaScript-Clients müssen Sie mindestens einen Ereignishandler registrieren, bevor Sie die `Start`-Methode aufrufen, um die Verbindung herzustellen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-161">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="f14ce-162">Dies ist für .NET-Clients nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="f14ce-162">This is not necessary for .NET clients.</span></span> <span data-ttu-id="f14ce-163">Bei JavaScript-Clients erstellt der generierte Proxy Code automatisch Proxys für alle auf dem Server vorhandenen Hubs, und bei der Registrierung eines Handlers wird angegeben, welche Hubs der Client verwenden soll.</span><span class="sxs-lookup"><span data-stu-id="f14ce-163">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="f14ce-164">Für einen .NET-Client erstellen Sie aber manuell Hub-Proxys, daher geht signalr davon aus, dass Sie einen beliebigen Hub verwenden, für den Sie einen Proxy erstellen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-164">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>

<span data-ttu-id="f14ce-165">Im Beispielcode wird die Standard-URL "/signalr" verwendet, um eine Verbindung mit Ihrem signalr-Dienst herzustellen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-165">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="f14ce-166">Weitere Informationen zum Angeben einer anderen Basis-URL finden Sie unter [ASP.net signalr Hubs API Guide-Server-the/signalr URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="f14ce-166">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="f14ce-167">Die `Start`-Methode wird asynchron ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="f14ce-167">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="f14ce-168">Um sicherzustellen, dass nachfolgende Codezeilen erst ausgeführt werden, nachdem die Verbindung hergestellt wurde, verwenden Sie `await` in einer asynchronen ASP.NET 4,5-Methode oder in einer synchronen Methode `.Wait()`.</span><span class="sxs-lookup"><span data-stu-id="f14ce-168">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="f14ce-169">Verwenden Sie `.Wait()` nicht in einem WinRT-Client.</span><span class="sxs-lookup"><span data-stu-id="f14ce-169">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="f14ce-170">Domänen übergreifende Verbindungen von Silverlight-Clients</span><span class="sxs-lookup"><span data-stu-id="f14ce-170">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="f14ce-171">Informationen zum Aktivieren von Domänen übergreifenden Verbindungen von Silverlight-Clients finden Sie unter [Bereitstellen eines Dienstanbieter übergreifenden Bereichs](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="f14ce-171">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="f14ce-172">Konfigurieren der Verbindung</span><span class="sxs-lookup"><span data-stu-id="f14ce-172">How to configure the connection</span></span>

<span data-ttu-id="f14ce-173">Bevor Sie eine Verbindung herstellen, können Sie eine der folgenden Optionen angeben:</span><span class="sxs-lookup"><span data-stu-id="f14ce-173">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="f14ce-174">Limit für gleichzeitige Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-174">Concurrent connections limit.</span></span>
- <span data-ttu-id="f14ce-175">Abfrage Zeichen folgen Parameter.</span><span class="sxs-lookup"><span data-stu-id="f14ce-175">Query string parameters.</span></span>
- <span data-ttu-id="f14ce-176">Die Transportmethode.</span><span class="sxs-lookup"><span data-stu-id="f14ce-176">The transport method.</span></span>
- <span data-ttu-id="f14ce-177">HTTP-Header.</span><span class="sxs-lookup"><span data-stu-id="f14ce-177">HTTP headers.</span></span>
- <span data-ttu-id="f14ce-178">Client Zertifikate.</span><span class="sxs-lookup"><span data-stu-id="f14ce-178">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="f14ce-179">Festlegen der maximalen Anzahl gleichzeitiger Verbindungen in WPF-Clients</span><span class="sxs-lookup"><span data-stu-id="f14ce-179">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="f14ce-180">In WPF-Clients müssen Sie möglicherweise die maximale Anzahl gleichzeitiger Verbindungen mit dem Standardwert 2 erhöhen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-180">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="f14ce-181">Der empfohlene Wert ist 10.</span><span class="sxs-lookup"><span data-stu-id="f14ce-181">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=5)]

<span data-ttu-id="f14ce-182">Weitere Informationen finden Sie unter [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="f14ce-182">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="f14ce-183">Angeben von Abfrage Zeichen folgen Parametern</span><span class="sxs-lookup"><span data-stu-id="f14ce-183">How to specify query string parameters</span></span>

<span data-ttu-id="f14ce-184">Wenn Sie Daten an den Server senden möchten, wenn der Client eine Verbindung herstellt, können Sie dem Verbindungs Objekt Abfrage Zeichenfolgen-Parameter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-184">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="f14ce-185">Im folgenden Beispiel wird gezeigt, wie ein Abfrage Zeichen folgen Parameter im Client Code festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="f14ce-185">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="f14ce-186">Im folgenden Beispiel wird gezeigt, wie ein Abfrage Zeichen folgen Parameter im Servercode gelesen wird.</span><span class="sxs-lookup"><span data-stu-id="f14ce-186">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="f14ce-187">Angeben der Transportmethode</span><span class="sxs-lookup"><span data-stu-id="f14ce-187">How to specify the transport method</span></span>

<span data-ttu-id="f14ce-188">Im Rahmen der Verbindungs Herstellung aushandiert ein signalr-Client normalerweise mit dem Server, um den optimalen Transport zu ermitteln, der von Server und Client unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="f14ce-188">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="f14ce-189">Wenn Sie bereits wissen, welcher Transport Sie verwenden möchten, können Sie diesen Aushandlungs Prozess umgehen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-189">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="f14ce-190">Um die Transportmethode anzugeben, übergeben Sie ein Transport Objekt an die Start Methode.</span><span class="sxs-lookup"><span data-stu-id="f14ce-190">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="f14ce-191">Im folgenden Beispiel wird gezeigt, wie die Transportmethode im Client Code angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="f14ce-191">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=5)]

<span data-ttu-id="f14ce-192">Der [Microsoft. Aspnet. signalr. Client. Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) -Namespace enthält die folgenden Klassen, die Sie zum Angeben des Transports verwenden können.</span><span class="sxs-lookup"><span data-stu-id="f14ce-192">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="f14ce-193">[Longpollingtransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="f14ce-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="f14ce-194">[Serversenteventstransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="f14ce-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="f14ce-195">[Websockettransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (nur verfügbar, wenn sowohl der Server als auch der Client .NET 4,5 verwenden.)</span><span class="sxs-lookup"><span data-stu-id="f14ce-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="f14ce-196">[Autotransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (wählt automatisch den optimalen Transport aus, der sowohl vom Client als auch vom Server unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="f14ce-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="f14ce-197">Dies ist der Standard Transport.</span><span class="sxs-lookup"><span data-stu-id="f14ce-197">This is the default transport.</span></span> <span data-ttu-id="f14ce-198">Wenn Sie diese Angabe an die `Start`-Methode vornehmen, hat dies denselben Effekt wie das Übergeben von nichts.)</span><span class="sxs-lookup"><span data-stu-id="f14ce-198">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="f14ce-199">Der foreverframe-Transport ist nicht in dieser Liste enthalten, da er nur von Browsern verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="f14ce-199">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="f14ce-200">Informationen zum Überprüfen der Transportmethode im Servercode finden Sie unter [ASP.net signalr Hubs API Guide-Server-How to Get Information about the Client from the context Property](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="f14ce-200">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="f14ce-201">Weitere Informationen zu Transporten und Fallbacks finden Sie unter [Einführung in signalr-Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="f14ce-201">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="f14ce-202">Angeben von HTTP-Headern</span><span class="sxs-lookup"><span data-stu-id="f14ce-202">How to specify HTTP headers</span></span>

<span data-ttu-id="f14ce-203">Um HTTP-Header festzulegen, verwenden Sie die `Headers`-Eigenschaft für das Verbindungs Objekt.</span><span class="sxs-lookup"><span data-stu-id="f14ce-203">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="f14ce-204">Im folgenden Beispiel wird gezeigt, wie Sie einen HTTP-Header hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-204">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="f14ce-205">Angeben von Client Zertifikaten</span><span class="sxs-lookup"><span data-stu-id="f14ce-205">How to specify client certificates</span></span>

<span data-ttu-id="f14ce-206">Verwenden Sie zum Hinzufügen von Client Zertifikaten die `AddClientCertificate`-Methode für das Verbindungs Objekt.</span><span class="sxs-lookup"><span data-stu-id="f14ce-206">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="f14ce-207">Erstellen des Hub-Proxys</span><span class="sxs-lookup"><span data-stu-id="f14ce-207">How to create the Hub proxy</span></span>

<span data-ttu-id="f14ce-208">Um Methoden auf dem Client zu definieren, die ein Hub vom Server aufrufen kann, und um Methoden für einen Hub auf dem Server aufzurufen, erstellen Sie einen Proxy für den Hub, indem Sie `CreateHubProxy` für das Verbindungs Objekt aufrufen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-208">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="f14ce-209">Die Zeichenfolge, die Sie an `CreateHubProxy` übergeben, ist der Name der Hub-Klasse oder der Name, der vom `HubName`-Attribut angegeben wird, wenn auf dem Server eine verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="f14ce-209">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="f14ce-210">Beim Namensvergleich wird die Groß-/Kleinschreibung nicht beachtet</span><span class="sxs-lookup"><span data-stu-id="f14ce-210">Name matching is case-insensitive.</span></span>

<span data-ttu-id="f14ce-211">**Hub-Klasse auf dem Server**</span><span class="sxs-lookup"><span data-stu-id="f14ce-211">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="f14ce-212">**Erstellen eines Client Proxys für die Hub-Klasse**</span><span class="sxs-lookup"><span data-stu-id="f14ce-212">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=3)]

<span data-ttu-id="f14ce-213">Wenn Sie Ihre Hub-Klasse mit einem `HubName`-Attribut versehen, verwenden Sie diesen Namen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-213">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="f14ce-214">**Hub-Klasse auf dem Server**</span><span class="sxs-lookup"><span data-stu-id="f14ce-214">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="f14ce-215">**Erstellen eines Client Proxys für die Hub-Klasse**</span><span class="sxs-lookup"><span data-stu-id="f14ce-215">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=3)]

<span data-ttu-id="f14ce-216">Wenn Sie `HubConnection.CreateHubProxy` mehrmals mit demselben `hubName`aufzurufen, erhalten Sie dasselbe zwischengespeicherte `IHubProxy` Objekt.</span><span class="sxs-lookup"><span data-stu-id="f14ce-216">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="f14ce-217">Definieren von Methoden auf dem Client, der vom Server aufgerufen werden kann</span><span class="sxs-lookup"><span data-stu-id="f14ce-217">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="f14ce-218">Zum Definieren einer Methode, die vom Server aufgerufen werden kann, verwenden Sie die `On`-Methode des Proxys, um einen Ereignishandler zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="f14ce-218">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="f14ce-219">Beim Methodennamen wird die Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="f14ce-219">Method name matching is case-insensitive.</span></span> <span data-ttu-id="f14ce-220">Beispielsweise werden auf dem-Server `Clients.All.UpdateStockPrice` auf dem-Server `updateStockPrice`, `updatestockprice`oder `UpdateStockPrice` auf dem Client ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="f14ce-220">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="f14ce-221">Für verschiedene Client Plattformen gelten andere Anforderungen, wie Sie Methoden Code schreiben, um die Benutzeroberfläche zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="f14ce-221">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="f14ce-222">Die gezeigten Beispiele sind für WinRT-Clients (Windows Store .net).</span><span class="sxs-lookup"><span data-stu-id="f14ce-222">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="f14ce-223">Beispiele für WPF, Silverlight und Konsolen Anwendungen werden in [einem separaten Abschnitt weiter unten in diesem Thema](#wpfsl)bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="f14ce-223">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="f14ce-224">Methoden ohne Parameter</span><span class="sxs-lookup"><span data-stu-id="f14ce-224">Methods without parameters</span></span>

<span data-ttu-id="f14ce-225">Wenn die Methode, die Sie behandeln, keine Parameter enthält, verwenden Sie die nicht generische Überladung der `On`-Methode:</span><span class="sxs-lookup"><span data-stu-id="f14ce-225">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="f14ce-226">**Server Code Aufruf der Client Methode ohne Parameter**</span><span class="sxs-lookup"><span data-stu-id="f14ce-226">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="f14ce-227">**WinRT-Client Code für die Methode, die vom Server ohne Parameter aufgerufen wird ([siehe WPF-und Silverlight-Beispiele weiter unten in diesem Thema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="f14ce-227">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="f14ce-228">Methoden mit Parametern, die Parametertypen angeben</span><span class="sxs-lookup"><span data-stu-id="f14ce-228">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="f14ce-229">Wenn die Methode, die Sie behandeln, über Parameter verfügt, geben Sie die Typen der Parameter als generische Typen der `On` Methode an.</span><span class="sxs-lookup"><span data-stu-id="f14ce-229">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="f14ce-230">Es gibt generische über Ladungen der `On`-Methode, die es Ihnen ermöglichen, bis zu 8 Parameter (4 auf Windows Phone 7) anzugeben.</span><span class="sxs-lookup"><span data-stu-id="f14ce-230">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="f14ce-231">Im folgenden Beispiel wird ein Parameter an die `UpdateStockPrice`-Methode gesendet.</span><span class="sxs-lookup"><span data-stu-id="f14ce-231">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="f14ce-232">**Server Code zum Aufrufen der Client Methode mit einem Parameter**</span><span class="sxs-lookup"><span data-stu-id="f14ce-232">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="f14ce-233">**Die für den Parameter verwendete Aktienklasse**</span><span class="sxs-lookup"><span data-stu-id="f14ce-233">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="f14ce-234">**WinRT-Client Code für eine Methode, die vom Server mit einem Parameter aufgerufen wird ([Siehe Beispiele zu WPF und Silverlight weiter unten in diesem Thema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="f14ce-234">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="f14ce-235">Methoden mit Parametern, die dynamische Objekte für die Parameter angeben</span><span class="sxs-lookup"><span data-stu-id="f14ce-235">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="f14ce-236">Als Alternative zum Angeben von Parametern als generische Typen der `On`-Methode können Sie Parameter als dynamische Objekte angeben:</span><span class="sxs-lookup"><span data-stu-id="f14ce-236">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="f14ce-237">**Server Code zum Aufrufen der Client Methode mit einem Parameter**</span><span class="sxs-lookup"><span data-stu-id="f14ce-237">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="f14ce-238">**Die für den Parameter verwendete Aktienklasse**</span><span class="sxs-lookup"><span data-stu-id="f14ce-238">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="f14ce-239">**WinRT-Client Code für eine Methode, die vom Server mit einem Parameter aufgerufen wird, wobei ein dynamisches Objekt für den Parameter verwendet wird ([siehe WPF-und Silverlight-Beispiele weiter unten in diesem Thema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="f14ce-239">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="f14ce-240">Vorgehensweise beim Entfernen eines Handlers</span><span class="sxs-lookup"><span data-stu-id="f14ce-240">How to remove a handler</span></span>

<span data-ttu-id="f14ce-241">Um einen Handler zu entfernen, müssen Sie dessen `Dispose`-Methode aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-241">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="f14ce-242">**Client Code für eine vom Server aufgerufene Methode**</span><span class="sxs-lookup"><span data-stu-id="f14ce-242">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="f14ce-243">**Client Code zum Entfernen des Handlers**</span><span class="sxs-lookup"><span data-stu-id="f14ce-243">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="f14ce-244">Vorgehensweise beim Abrufen von Server Methoden vom Client</span><span class="sxs-lookup"><span data-stu-id="f14ce-244">How to call server methods from the client</span></span>

<span data-ttu-id="f14ce-245">Um eine Methode auf dem Server aufzurufen, verwenden Sie die `Invoke`-Methode des Hub-Proxys.</span><span class="sxs-lookup"><span data-stu-id="f14ce-245">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="f14ce-246">Wenn die Server Methode über keinen Rückgabewert verfügt, verwenden Sie die nicht generische Überladung der `Invoke`-Methode.</span><span class="sxs-lookup"><span data-stu-id="f14ce-246">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="f14ce-247">**Server Code für eine Methode, die über keinen Rückgabewert verfügt**</span><span class="sxs-lookup"><span data-stu-id="f14ce-247">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="f14ce-248">**Client Code zum Aufrufen einer Methode ohne Rückgabewert**</span><span class="sxs-lookup"><span data-stu-id="f14ce-248">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="f14ce-249">Wenn die Server Methode über einen Rückgabewert verfügt, geben Sie den Rückgabetyp als generischen Typ der `Invoke` Methode an.</span><span class="sxs-lookup"><span data-stu-id="f14ce-249">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="f14ce-250">**Server Code für eine Methode, die über einen Rückgabewert verfügt und einen komplexen Typparameter annimmt**</span><span class="sxs-lookup"><span data-stu-id="f14ce-250">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="f14ce-251">**Die für den Parameter und den Rückgabewert verwendete Aktienklasse**</span><span class="sxs-lookup"><span data-stu-id="f14ce-251">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="f14ce-252">**Client Code, der eine Methode mit einem Rückgabewert und einem komplexen Typparameter in einer ASP.NET 4,5 Async-Methode aufrufen**</span><span class="sxs-lookup"><span data-stu-id="f14ce-252">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="f14ce-253">**Client Code, der eine Methode mit einem Rückgabewert und einem komplexen Typparameter in einer synchronen Methode aufrufen**</span><span class="sxs-lookup"><span data-stu-id="f14ce-253">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="f14ce-254">Die `Invoke`-Methode wird asynchron ausgeführt und gibt ein `Task`-Objekt zurück.</span><span class="sxs-lookup"><span data-stu-id="f14ce-254">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="f14ce-255">Wenn Sie `await` oder `.Wait()`nicht angeben, wird die nächste Codezeile ausgeführt, bevor die von Ihnen aufgerufene Methode die Ausführung abgeschlossen hat.</span><span class="sxs-lookup"><span data-stu-id="f14ce-255">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="f14ce-256">Behandeln von Ereignissen der Verbindungs Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="f14ce-256">How to handle connection lifetime events</span></span>

<span data-ttu-id="f14ce-257">Signalr stellt die folgenden Ereignisse für die Verbindungs Lebensdauer bereit, die Sie behandeln können:</span><span class="sxs-lookup"><span data-stu-id="f14ce-257">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="f14ce-258">`Received`: wird ausgelöst, wenn Daten über die Verbindung empfangen werden.</span><span class="sxs-lookup"><span data-stu-id="f14ce-258">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="f14ce-259">Stellt die empfangenen Daten bereit.</span><span class="sxs-lookup"><span data-stu-id="f14ce-259">Provides the received data.</span></span>
- <span data-ttu-id="f14ce-260">`ConnectionSlow`: wird ausgelöst, wenn der Client eine langsame oder häufig gelöschter Verbindung erkennt.</span><span class="sxs-lookup"><span data-stu-id="f14ce-260">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="f14ce-261">`Reconnecting`: wird ausgelöst, wenn der zugrunde liegende Transport mit dem erneuten Verbinden beginnt.</span><span class="sxs-lookup"><span data-stu-id="f14ce-261">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="f14ce-262">`Reconnected`: wird ausgelöst, wenn der zugrunde liegende Transport die Verbindung wieder hergestellt hat.</span><span class="sxs-lookup"><span data-stu-id="f14ce-262">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="f14ce-263">`StateChanged`: wird ausgelöst, wenn sich der Verbindungsstatus ändert.</span><span class="sxs-lookup"><span data-stu-id="f14ce-263">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="f14ce-264">Stellt den alten und den neuen Zustand bereit.</span><span class="sxs-lookup"><span data-stu-id="f14ce-264">Provides the old state and the new state.</span></span> <span data-ttu-id="f14ce-265">Weitere Informationen zu Verbindungs Zustands Werten finden Sie unter [ConnectionState-Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="f14ce-265">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="f14ce-266">`Closed`: wird ausgelöst, wenn die Verbindung getrennt wurde.</span><span class="sxs-lookup"><span data-stu-id="f14ce-266">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="f14ce-267">Wenn Sie z. b. Warnmeldungen für Fehler anzeigen möchten, die nicht schwerwiegend sind, aber zeitweilig auftretende Verbindungsprobleme verursachen, z. b. verlangsamtheit oder häufiges Löschen der Verbindung, behandeln Sie das `ConnectionSlow`-Ereignis.</span><span class="sxs-lookup"><span data-stu-id="f14ce-267">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="f14ce-268">Weitere Informationen finden Sie unter [verstehen und behandeln von Verbindungs Lebensdauer-Ereignissen in signalr](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="f14ce-268">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="f14ce-269">Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="f14ce-269">How to handle errors</span></span>

<span data-ttu-id="f14ce-270">Wenn Sie auf dem Server nicht explizit detaillierte Fehlermeldungen aktivieren, enthält das Ausnahme Objekt, das signalr nach einem Fehler zurückgibt, nur minimale Informationen über den Fehler.</span><span class="sxs-lookup"><span data-stu-id="f14ce-270">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="f14ce-271">Wenn beispielsweise ein `newContosoChatMessage` Fehler auftritt, enthält die Fehlermeldung im Fehler Objekt den Wert "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`", dass detaillierte Fehlermeldungen an Clients in der Produktionsumgebung nicht gesendet werden sollen. Wenn Sie jedoch ausführliche Fehlermeldungen zu Problem Behandlungszwecken aktivieren möchten, verwenden Sie den folgenden Code auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="f14ce-271">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="f14ce-272">Um Fehler zu behandeln, die von signalr ausgelöst werden, können Sie einen Handler für das `Error`-Ereignis für das Verbindungs Objekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f14ce-272">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="f14ce-273">Um Fehler von Methoden aufrufen zu behandeln, wrappen Sie den Code in einem try-catch-Block.</span><span class="sxs-lookup"><span data-stu-id="f14ce-273">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="f14ce-274">Aktivieren der Client seitigen Protokollierung</span><span class="sxs-lookup"><span data-stu-id="f14ce-274">How to enable client-side logging</span></span>

<span data-ttu-id="f14ce-275">Um die Client seitige Protokollierung zu aktivieren, legen Sie die Eigenschaften `TraceLevel` und `TraceWriter` für das Verbindungs Objekt fest.</span><span class="sxs-lookup"><span data-stu-id="f14ce-275">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=3-4)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="f14ce-276">WPF-, Silverlight-und Konsolen Anwendungscode Beispiele für Client Methoden, die vom Server aufgerufen werden können</span><span class="sxs-lookup"><span data-stu-id="f14ce-276">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="f14ce-277">Die zuvor gezeigten Codebeispiele zum Definieren von Client Methoden, die vom Server aufgerufen werden können, gelten für WinRT-Clients.</span><span class="sxs-lookup"><span data-stu-id="f14ce-277">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="f14ce-278">In den folgenden Beispielen wird der entsprechende Code für WPF-, Silverlight-und Konsolen Anwendungs Clients angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f14ce-278">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="f14ce-279">Methoden ohne Parameter</span><span class="sxs-lookup"><span data-stu-id="f14ce-279">Methods without parameters</span></span>

<span data-ttu-id="f14ce-280">**WPF-Client Code für die Methode, die vom Server ohne Parameter aufgerufen wird**</span><span class="sxs-lookup"><span data-stu-id="f14ce-280">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="f14ce-281">**Silverlight-Client Code für die Methode, die vom Server ohne Parameter aufgerufen wird**</span><span class="sxs-lookup"><span data-stu-id="f14ce-281">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="f14ce-282">**Client Code der Konsolenanwendung für die Methode, die vom Server ohne Parameter aufgerufen wird**</span><span class="sxs-lookup"><span data-stu-id="f14ce-282">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="f14ce-283">Methoden mit Parametern, die Parametertypen angeben</span><span class="sxs-lookup"><span data-stu-id="f14ce-283">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="f14ce-284">**WPF-Client Code für eine Methode, die vom Server mit einem Parameter aufgerufen wird**</span><span class="sxs-lookup"><span data-stu-id="f14ce-284">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="f14ce-285">**Silverlight-Client Code für eine Methode, die von einem Server mit einem Parameter aufgerufen wird**</span><span class="sxs-lookup"><span data-stu-id="f14ce-285">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="f14ce-286">**Client Code der Konsolenanwendung für eine Methode, die von einem Server mit einem Parameter aufgerufen wird**</span><span class="sxs-lookup"><span data-stu-id="f14ce-286">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="f14ce-287">Methoden mit Parametern, die dynamische Objekte für die Parameter angeben</span><span class="sxs-lookup"><span data-stu-id="f14ce-287">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="f14ce-288">**WPF-Client Code für eine Methode, die vom Server mit einem Parameter aufgerufen wird, wobei ein dynamisches Objekt für den Parameter verwendet wird.**</span><span class="sxs-lookup"><span data-stu-id="f14ce-288">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="f14ce-289">**Silverlight-Client Code für eine Methode, die von Server mit einem Parameter aufgerufen wird, wobei ein dynamisches Objekt für den Parameter verwendet wird.**</span><span class="sxs-lookup"><span data-stu-id="f14ce-289">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="f14ce-290">**Client Code für Konsolen Anwendungen für eine Methode, die vom Server mit einem Parameter aufgerufen wird, wobei ein dynamisches Objekt für den Parameter verwendet wird.**</span><span class="sxs-lookup"><span data-stu-id="f14ce-290">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
