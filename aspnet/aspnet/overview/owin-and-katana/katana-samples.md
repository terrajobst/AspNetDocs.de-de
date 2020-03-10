---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana-Beispiele | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 1238f7d09492a6856d49dece5de75184ccfa4838
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472347"
---
# <a name="katana-samples"></a><span data-ttu-id="531ee-102">Katana-Beispiele</span><span class="sxs-lookup"><span data-stu-id="531ee-102">Katana Samples</span></span>

<span data-ttu-id="531ee-103">von [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="531ee-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="531ee-104">Katana-Beispiele</span><span class="sxs-lookup"><span data-stu-id="531ee-104">Katana Samples</span></span>

<span data-ttu-id="531ee-105">**ASP.net Routes-Beispiel** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span><span class="sxs-lookup"><span data-stu-id="531ee-105">**ASP.NET Routes Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span></span>  
<span data-ttu-id="531ee-106">In einigen Anwendungen möchten Sie owin-Komponenten in der ASP.NET-Routing Tabelle nebeneinander mit nicht-owin-Komponenten verbinden.</span><span class="sxs-lookup"><span data-stu-id="531ee-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="531ee-107">In diesem Beispiel wird gezeigt, wie die RouteCollection-Erweiterungs Methoden mapowinpath und mapowinroute verwendet werden, die von Microsoft. owin. Host. systemWeb bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="531ee-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="531ee-108">**Beispiel für Verzweigungs Pipelines** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span><span class="sxs-lookup"><span data-stu-id="531ee-108">**Branching Pipelines Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span></span>  
<span data-ttu-id="531ee-109">Owin-Anforderungs Verarbeitungs Pipelines müssen nicht linear sein, Sie können so verzweigt werden, dass Anforderungen auf unterschiedliche Weise verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="531ee-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="531ee-110">In diesem Beispiel wird gezeigt, wie eine Verzweigungs Pipeline basierend auf Anforderungs Pfaden oder anderen Anforderungs Daten, z. b. Headern, erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="531ee-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="531ee-111">Diese Komponenten sind im nuget-Paket Microsoft. owin. Mapping verfügbar.</span><span class="sxs-lookup"><span data-stu-id="531ee-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="531ee-112">**Beispiel für benutzerdefinierte Server** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span><span class="sxs-lookup"><span data-stu-id="531ee-112">**Custom Server Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span></span>  
<span data-ttu-id="531ee-113">Zeigt, wie ein benutzerdefinierter owin-Server verwendet wird, wenn das selbst Hosting von owin verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="531ee-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="531ee-114">**Eingebetteter Beispiel** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span><span class="sxs-lookup"><span data-stu-id="531ee-114">**Embedded Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span></span>  
<span data-ttu-id="531ee-115">Einige owin-Server können in Ihrem eigenen Prozess ausgeführt werden (&quot;selbstgeh ostete&quot;).</span><span class="sxs-lookup"><span data-stu-id="531ee-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="531ee-116">In diesem Beispiel wird gezeigt, wie eine owin-Anwendung mit den Tools gestartet wird, die vom Microsoft. owin. Hosting-nuget-Paket bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="531ee-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="531ee-117">**HelloWorld-Beispiel** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span><span class="sxs-lookup"><span data-stu-id="531ee-117">**HelloWorld Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span></span>  
<span data-ttu-id="531ee-118">Owin ist eine HTTP-Server-API-Abstraktion, die die Portabilität von Anwendungen auf verschiedenen Servern ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="531ee-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="531ee-119">In diesem Beispiel wird veranschaulicht, wie Sie eine Hallo Welt Anwendung mit einigen **einfachen Wrappern** für die unformatierte owin-Abstraktion schreiben und Sie auf einem Webserver wie ASP.net ausführen.</span><span class="sxs-lookup"><span data-stu-id="531ee-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="531ee-120">Hallo Welt unformatierte **owin-Beispiel** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span><span class="sxs-lookup"><span data-stu-id="531ee-120">**Hello World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span></span>  
<span data-ttu-id="531ee-121">In diesem Beispiel wird veranschaulicht, wie Sie eine Hallo Welt Anwendung **mit der** unformatierten owin-Abstraktion schreiben und Sie auf einem Webserver wie ASP.net ausführen.</span><span class="sxs-lookup"><span data-stu-id="531ee-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="531ee-122">**Signalr-Beispiel** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span><span class="sxs-lookup"><span data-stu-id="531ee-122">**SignalR Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span></span>  
<span data-ttu-id="531ee-123">Zeigt, wie Sie signalr mithilfe von owin/Katana selbst hosten können.</span><span class="sxs-lookup"><span data-stu-id="531ee-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="531ee-124">Weitere Informationen zum Selbsthosting von signalr finden Sie unter [Tutorial: signalr Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="531ee-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="531ee-125">**Beispiel für statische Dateien** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span><span class="sxs-lookup"><span data-stu-id="531ee-125">**Static Files Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span></span>  
<span data-ttu-id="531ee-126">Zeigt, wie HTTP-Anforderungen für statische Dateien mit owin/Katana unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="531ee-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="531ee-127">**Web-API** - | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span><span class="sxs-lookup"><span data-stu-id="531ee-127">**Web API** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span></span>  
<span data-ttu-id="531ee-128">Dieses Beispiel zeigt, wie Sie owin in IIS hosten und der owin-Pipeline eine Web-API hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="531ee-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="531ee-129">**WebSocket-Beispiel** | [Quellcode](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span><span class="sxs-lookup"><span data-stu-id="531ee-129">**Web Socket Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span></span>  
<span data-ttu-id="531ee-130">Zeigt, wie websockets in owin mithilfe der [System .net. websockets. WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) -Klasse unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="531ee-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
