---
title: Hostingmodelle Razor-Komponenten
author: guardrex
description: Verstehen der clientseitigen Blazor und ASP.NET Core Razor Serverkomponenten Hostingmodelle.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/hosting-models
ms.openlocfilehash: d1e0c472d7d10eeb4cef0da735cf703c98dd1645
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042437"
---
# <a name="razor-components-hosting-models"></a><span data-ttu-id="8ab71-103">Hostingmodelle Razor-Komponenten</span><span class="sxs-lookup"><span data-stu-id="8ab71-103">Razor Components hosting models</span></span>

<span data-ttu-id="8ab71-104">Durch [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="8ab71-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="8ab71-105">Razor-Komponenten ist ein Webframework, das für die clientseitige Ausführung vorgesehen im Browser auf eine WebAssembly basierenden .NET Common Language Runtime (*Blazor*) oder serverseitige in ASP.NET Core (*ASP.NET Core-Razor-Komponenten*).</span><span class="sxs-lookup"><span data-stu-id="8ab71-105">Razor Components is a web framework designed to run client-side in the browser on a WebAssembly-based .NET runtime (*Blazor*) or server-side in ASP.NET Core (*ASP.NET Core Razor Components*).</span></span> <span data-ttu-id="8ab71-106">Unabhängig von dem Modell, die app und die Komponente Hostingmodelle *unverändert*.</span><span class="sxs-lookup"><span data-stu-id="8ab71-106">Regardless of the hosting model, the app and component models *remain the same*.</span></span> <span data-ttu-id="8ab71-107">Dieser Artikel beschreibt die verfügbaren Hostingmodelle.</span><span class="sxs-lookup"><span data-stu-id="8ab71-107">This article discusses the available hosting models.</span></span>

## <a name="client-side-hosting-model"></a><span data-ttu-id="8ab71-108">Clientseitiges Hostingmodell</span><span class="sxs-lookup"><span data-stu-id="8ab71-108">Client-side hosting model</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="8ab71-109">Das Dienstprinzipale Hostingmodell für Blazor ist ausgeführten Client-Seite im Browser.</span><span class="sxs-lookup"><span data-stu-id="8ab71-109">The principal hosting model for Blazor is running client-side in the browser.</span></span> <span data-ttu-id="8ab71-110">In diesem Modell werden die Blazor-app, ihre Abhängigkeiten und die .NET Runtime an den Browser heruntergeladen.</span><span class="sxs-lookup"><span data-stu-id="8ab71-110">In this model, the Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="8ab71-111">Die App wird direkt im UI-Thread des Browsers ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8ab71-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="8ab71-112">Alle Aktualisierungen der Benutzeroberfläche und Behandlung von Ereignissen tritt innerhalb des gleichen Prozesses.</span><span class="sxs-lookup"><span data-stu-id="8ab71-112">All UI updates and event handling happens within the same process.</span></span> <span data-ttu-id="8ab71-113">Die app-Ressourcen können als statische Dateien, die mit den Webserver bevorzugt wird bereitgestellt werden (finden Sie unter [hosten und Bereitstellen von](xref:host-and-deploy/razor-components/index)).</span><span class="sxs-lookup"><span data-stu-id="8ab71-113">The app assets can be deployed as static files using whatever web server is preferred (see [Host and deploy](xref:host-and-deploy/razor-components/index)).</span></span>

![Blazor clientseitig: Die Blazor-Anwendung, die in einem UI-Thread im Browser ausgeführt werden.](hosting-models/_static/client-side.png)

<span data-ttu-id="8ab71-115">Verwenden Sie zum Erstellen einer Blazor-app, die mit das Hostingmodell für die clientseitige der **Blazor** oder **Blazor (ASP.NET Core gehostet)** -Projektvorlagen (`blazor` oder `blazorhosted` Vorlage, wenn Sie die verwenden[Dotnet neue](/dotnet/core/tools/dotnet-new) Befehl an einer Eingabeaufforderung).</span><span class="sxs-lookup"><span data-stu-id="8ab71-115">To create a Blazor app using the client-side hosting model, use the **Blazor** or **Blazor (ASP.NET Core Hosted)** project templates (`blazor` or `blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command at a command prompt).</span></span> <span data-ttu-id="8ab71-116">Die enthaltenen *blazor.webassembly.js* Skript behandelt:</span><span class="sxs-lookup"><span data-stu-id="8ab71-116">The included *blazor.webassembly.js* script handles:</span></span>

* <span data-ttu-id="8ab71-117">Herunterladen von .NET Runtime, die app und die dazugehörigen Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="8ab71-117">Downloading the .NET runtime, the app, and its dependencies.</span></span>
* <span data-ttu-id="8ab71-118">Die Initialisierung der Runtime, um die app ausführen.</span><span class="sxs-lookup"><span data-stu-id="8ab71-118">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="8ab71-119">Das Hostingmodell für die clientseitige bietet mehrere Vorteile.</span><span class="sxs-lookup"><span data-stu-id="8ab71-119">The client-side hosting model offers several benefits.</span></span> <span data-ttu-id="8ab71-120">Die clientseitige Blazor:</span><span class="sxs-lookup"><span data-stu-id="8ab71-120">Client-side Blazor:</span></span>

* <span data-ttu-id="8ab71-121">Verfügt über keine serverseitige-Abhängigkeit von .NET.</span><span class="sxs-lookup"><span data-stu-id="8ab71-121">Has no .NET server-side dependency.</span></span>
* <span data-ttu-id="8ab71-122">Verfügt über eine umfangreiche interaktive Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="8ab71-122">Has a rich interactive UI.</span></span>
* <span data-ttu-id="8ab71-123">Vollständig nutzt die Clientressourcen und Fähigkeiten.</span><span class="sxs-lookup"><span data-stu-id="8ab71-123">Fully leverages client resources and capabilities.</span></span>
* <span data-ttu-id="8ab71-124">Abladungen Arbeit vom Server an den Client.</span><span class="sxs-lookup"><span data-stu-id="8ab71-124">Offloads work from the server to the client.</span></span>
* <span data-ttu-id="8ab71-125">Unterstützt die offline-Szenarien.</span><span class="sxs-lookup"><span data-stu-id="8ab71-125">Supports offline scenarios.</span></span>

<span data-ttu-id="8ab71-126">Einige Nachteile der clientseitigen hosten.</span><span class="sxs-lookup"><span data-stu-id="8ab71-126">There are downsides to client-side hosting.</span></span> <span data-ttu-id="8ab71-127">Die clientseitige Blazor:</span><span class="sxs-lookup"><span data-stu-id="8ab71-127">Client-side Blazor:</span></span>

* <span data-ttu-id="8ab71-128">Wird die app auf die Funktionen des Browsers beschränkt.</span><span class="sxs-lookup"><span data-stu-id="8ab71-128">Restricts the app to the capabilities of the browser.</span></span>
* <span data-ttu-id="8ab71-129">Erfordert, kann der Clienthardware und Software (z. B. WebAssembly unterstützt).</span><span class="sxs-lookup"><span data-stu-id="8ab71-129">Requires capable client hardware and software (for example, WebAssembly support).</span></span>
* <span data-ttu-id="8ab71-130">Verfügt über eine größere Downloadgröße und länger Ladevorgang der app.</span><span class="sxs-lookup"><span data-stu-id="8ab71-130">Has a larger download size and longer app load time.</span></span>
* <span data-ttu-id="8ab71-131">Verfügt über weniger Reife der .NET Common Language Runtime und toolunterstützung (z. B. Einschränkungen in .NET Standard-Support und Debuggen).</span><span class="sxs-lookup"><span data-stu-id="8ab71-131">Has less mature .NET runtime and tooling support (for example, limitations in .NET Standard support and debugging).</span></span>

<span data-ttu-id="8ab71-132">Visual Studio enthält die **Blazor (ASP.NET Core gehostet)** Projektvorlage zum Erstellen einer Blazor-app, die auf WebAssembly ausgeführt wird, und klicken Sie auf eine ASP.NET Core-Server gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="8ab71-132">Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template for creating a Blazor app that runs on WebAssembly and is hosted on an ASP.NET Core server.</span></span> <span data-ttu-id="8ab71-133">Die ASP.NET Core-app ist ein separater Prozess aber dient die Blazor-app-Clients.</span><span class="sxs-lookup"><span data-stu-id="8ab71-133">The ASP.NET Core app serves the Blazor app to clients but is otherwise a separate process.</span></span> <span data-ttu-id="8ab71-134">Die clientseitige Blazor-app kann über das Netzwerk mithilfe von Web-API-Aufrufe oder SignalR-Verbindungen mit dem Server interagieren.</span><span class="sxs-lookup"><span data-stu-id="8ab71-134">The client-side Blazor app can interact with the server over the network using Web API calls or SignalR connections.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8ab71-135">Wenn eine clientseitige Blazor-app von einer ASP.NET Core-app, die als IIS-Sub-app gehostet bedient wird, deaktivieren Sie die geerbte Handler von ASP.NET Core-Modul.</span><span class="sxs-lookup"><span data-stu-id="8ab71-135">If a client-side Blazor app is served by an ASP.NET Core app hosted as an IIS sub-app, disable the inherited ASP.NET Core Module handler.</span></span> <span data-ttu-id="8ab71-136">Legen Sie die app-Basispfad der Blazor App *"Index.HTML"* Datei an den IIS-Alias verwendet, wenn die untergeordneten app in IIS zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="8ab71-136">Set the app base path in the Blazor app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>
>
> <span data-ttu-id="8ab71-137">Weitere Informationen finden Sie unter [App-Basispfad](xref:host-and-deploy/razor-components/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="8ab71-137">For more information, see [App base path](xref:host-and-deploy/razor-components/index#app-base-path).</span></span>

## <a name="server-side-hosting-model"></a><span data-ttu-id="8ab71-138">Serverseitiges Hostingmodell</span><span class="sxs-lookup"><span data-stu-id="8ab71-138">Server-side hosting model</span></span>

<span data-ttu-id="8ab71-139">In das ASP.NET Core-Razor-Komponenten serverseitige hosting-Modell wird die app auf dem Server in einer ASP.NET Core-app ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8ab71-139">In the ASP.NET Core Razor Components server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="8ab71-140">Benutzeroberflächenupdates, Ereignisbehandlung und JavaScript-Aufrufe werden über eine SignalR-Verbindung verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="8ab71-140">UI updates, event handling, and JavaScript calls are handled over a SignalR connection.</span></span>

![ASP.NET Core-Razor-Komponenten Serverseitig: Der Browser interagiert mit der app, die (gehostet in einer ASP.NET Core-app) auf dem Server über eine SignalR-Verbindung.](hosting-models/_static/server-side.png)

<span data-ttu-id="8ab71-142">Verwenden Sie zum Erstellen einer Razor-Komponenten-app, die mit das Hostingmodell für die serverseitige der **Blazor (serverseitig in ASP.NET Core)** Vorlage (`blazorserver` Verwendung [Dotnet neue](/dotnet/core/tools/dotnet-new) an einer Eingabeaufforderung).</span><span class="sxs-lookup"><span data-stu-id="8ab71-142">To create a Razor Components app using the server-side hosting model, use the **Blazor (Server-side in ASP.NET Core)** template (`blazorserver` when using [dotnet new](/dotnet/core/tools/dotnet-new) at a command prompt).</span></span> <span data-ttu-id="8ab71-143">Eine ASP.NET Core-app hostet die serverseitige app Razor-Komponenten und richtet den SignalR-Endpunkt, in dem Verbinden von Clients.</span><span class="sxs-lookup"><span data-stu-id="8ab71-143">An ASP.NET Core app hosts the Razor Components server-side app and sets up the SignalR endpoint where clients connect.</span></span> <span data-ttu-id="8ab71-144">Die ASP.NET Core-app verweist auf der app `Startup` Klasse hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="8ab71-144">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="8ab71-145">Serverseitige Komponenten der Razor-Dienste.</span><span class="sxs-lookup"><span data-stu-id="8ab71-145">Server-side Razor Components services.</span></span>
* <span data-ttu-id="8ab71-146">Die app die Anforderungspipeline.</span><span class="sxs-lookup"><span data-stu-id="8ab71-146">The app to the request handling pipeline.</span></span>

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

<span data-ttu-id="8ab71-147">Die *blazor.server.js* Skript&dagger; stellt die Clientverbindung her.</span><span class="sxs-lookup"><span data-stu-id="8ab71-147">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="8ab71-148">Es ist der Zuständigkeit der app zum beibehalten und Wiederherstellen von app-Status nach Bedarf (z. B. im Falle einer Netzwerkverbindung verloren).</span><span class="sxs-lookup"><span data-stu-id="8ab71-148">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="8ab71-149">Das Hostingmodell für die serverseitige bietet mehrere Vorteile:</span><span class="sxs-lookup"><span data-stu-id="8ab71-149">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="8ab71-150">Können Sie zum Schreiben der gesamten Apps mit .NET und C# mithilfe des Komponentenmodells.</span><span class="sxs-lookup"><span data-stu-id="8ab71-150">Allows you to write your entire app with .NET and C# using the component model.</span></span>
* <span data-ttu-id="8ab71-151">Bietet eine umfassende interaktive Verhalten und vermeidet unnötige Seite wird aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="8ab71-151">Provides a rich interactive feel and avoids unnecessary page refreshes.</span></span>
* <span data-ttu-id="8ab71-152">Verfügt über eine erheblich kleinere app-Größe als eine clientseitige Blazor-app aus, und viel schneller geladen.</span><span class="sxs-lookup"><span data-stu-id="8ab71-152">Has a significantly smaller app size than a client-side Blazor app and loads much faster.</span></span>
* <span data-ttu-id="8ab71-153">Komponente Logik kann vollständig von Serverfunktionen, einschließlich der Verwendung von .NET Core kompatible APIs nutzen.</span><span class="sxs-lookup"><span data-stu-id="8ab71-153">Component logic can take full advantage of server capabilities, including using any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="8ab71-154">Wird mit .NET Core auf dem Server ausgeführt, sodass es sich bei vorhandenen .NET Tools, z. B. Debuggen, wie erwartet funktioniert.</span><span class="sxs-lookup"><span data-stu-id="8ab71-154">Runs on .NET Core on the server, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="8ab71-155">Funktioniert mit thin Clients (z. B. Browser, WebAssembly und die Ressourcengruppe nicht unterstützen, Geräten eingeschränkt).</span><span class="sxs-lookup"><span data-stu-id="8ab71-155">Works with thin clients (for example, browsers that don't support WebAssembly and resource constrained devices).</span></span>

<span data-ttu-id="8ab71-156">Einige Nachteile: serverseitige hosten:</span><span class="sxs-lookup"><span data-stu-id="8ab71-156">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="8ab71-157">Weist auf höhere Latenz: Jedes Eingreifen des Benutzers umfasst ein Netzwerkhop.</span><span class="sxs-lookup"><span data-stu-id="8ab71-157">Has higher latency: Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="8ab71-158">Bietet keine Unterstützung für offline: Wenn die Clientverbindung fehlschlägt, funktioniert die app nicht mehr.</span><span class="sxs-lookup"><span data-stu-id="8ab71-158">Offers no offline support: If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="8ab71-159">Skalierbarkeit ist reduziert werden: Der Server muss mehrere Clientverbindungen verwalten und Clientstatus behandeln.</span><span class="sxs-lookup"><span data-stu-id="8ab71-159">Has reduced scalability: The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="8ab71-160">Erfordert einen ASP.NET Core-Server, auf die app dient.</span><span class="sxs-lookup"><span data-stu-id="8ab71-160">Requires an ASP.NET Core server to serve the app.</span></span> <span data-ttu-id="8ab71-161">Bereitstellung ohne Server (z. B. von einem CDN) ist nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="8ab71-161">Deployment without a server (for example, from a CDN) isn't possible.</span></span>

<span data-ttu-id="8ab71-162">&dagger;Die *blazor.server.js* Skript wird in folgendem Pfad veröffentlicht: *"bin" / {Debuggen | Das Release} / {ZIELFRAMEWORK} /publish/ {ANWENDUNGSNAME}. App/Dist/_framework*.</span><span class="sxs-lookup"><span data-stu-id="8ab71-162">&dagger;The *blazor.server.js* script is published to the following path: *bin/{Debug|Release}/{TARGET FRAMEWORK}/publish/{APPLICATION NAME}.App/dist/_framework*.</span></span>
