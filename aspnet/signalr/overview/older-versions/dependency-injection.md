---
uid: signalr/overview/older-versions/dependency-injection
title: Abhängigkeitsinjektion in signalr 1. x | Microsoft-Dokumentation
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/15/2013
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: de838ab6b3a299eb1e5ebeb9fa3c583478ce3e56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431541"
---
# <a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="47923-102">Abhängigkeitsinjektion in SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="47923-102">Dependency Injection in SignalR 1.x</span></span>

<span data-ttu-id="47923-103">von [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="47923-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="47923-104">Die Abhängigkeitsinjektion ist eine Möglichkeit, hart codierte Abhängigkeiten zwischen Objekten zu entfernen. Dadurch wird das Ersetzen der Abhängigkeiten eines Objekts vereinfacht, entweder zum Testen (mit Pseudo Objekten) oder zum Ändern des Lauf Zeit Verhaltens.</span><span class="sxs-lookup"><span data-stu-id="47923-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="47923-105">In diesem Tutorial wird gezeigt, wie Sie eine Abhängigkeitsinjektion für signalr-Hubs ausführen.</span><span class="sxs-lookup"><span data-stu-id="47923-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="47923-106">Außerdem wird gezeigt, wie IOC-Container mit signalr verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="47923-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="47923-107">Ein IOC-Container ist ein allgemeines Framework für die Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="47923-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="47923-108">Was ist eine Abhängigkeitsinjektion?</span><span class="sxs-lookup"><span data-stu-id="47923-108">What is Dependency Injection?</span></span>

<span data-ttu-id="47923-109">Überspringen Sie diesen Abschnitt, wenn Sie bereits mit der Abhängigkeitsinjektion vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="47923-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="47923-110">Die *Abhängigkeitsinjektion* (di) ist ein Muster, in dem Objekte nicht für das Erstellen eigener Abhängigkeiten verantwortlich sind.</span><span class="sxs-lookup"><span data-stu-id="47923-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="47923-111">Im folgenden finden Sie ein einfaches Beispiel zum motivieren von di.</span><span class="sxs-lookup"><span data-stu-id="47923-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="47923-112">Angenommen, Sie verfügen über ein Objekt, das Nachrichten protokollieren muss.</span><span class="sxs-lookup"><span data-stu-id="47923-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="47923-113">Sie können eine Protokollierungs Schnittstelle definieren:</span><span class="sxs-lookup"><span data-stu-id="47923-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="47923-114">In Ihrem-Objekt können Sie eine `ILogger` erstellen, um Meldungen zu protokollieren:</span><span class="sxs-lookup"><span data-stu-id="47923-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="47923-115">Dies funktioniert, ist aber nicht das beste Design.</span><span class="sxs-lookup"><span data-stu-id="47923-115">This works, but it's not the best design.</span></span> <span data-ttu-id="47923-116">Wenn Sie `FileLogger` durch eine andere `ILogger` Implementierung ersetzen möchten, müssen Sie `SomeComponent`ändern.</span><span class="sxs-lookup"><span data-stu-id="47923-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="47923-117">Unter der Annahme, dass viele andere Objekte `FileLogger`verwenden, müssen Sie alle ändern.</span><span class="sxs-lookup"><span data-stu-id="47923-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="47923-118">Wenn Sie sich entscheiden, `FileLogger` einen Singleton zu erstellen, müssen Sie auch Änderungen in der gesamten Anwendung vornehmen.</span><span class="sxs-lookup"><span data-stu-id="47923-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="47923-119">Ein besserer Ansatz besteht darin, eine `ILogger` in das Objekt einzufügen, z. –. mithilfe eines Konstruktorarguments:</span><span class="sxs-lookup"><span data-stu-id="47923-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="47923-120">Nun ist das Objekt nicht für die Auswahl der zu verwendenden `ILogger` verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="47923-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="47923-121">Sie können `ILogger`-Implementierungen wechseln, ohne die Objekte zu ändern, die von ihm abhängen.</span><span class="sxs-lookup"><span data-stu-id="47923-121">You can switch `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="47923-122">Dieses Muster wird als [Konstruktorinjektion](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="47923-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="47923-123">Ein weiteres Muster ist Setter Injection, bei dem Sie die Abhängigkeit über eine Setter-Methode oder-Eigenschaft festlegen.</span><span class="sxs-lookup"><span data-stu-id="47923-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="47923-124">Einfache Abhängigkeitsinjektion in signalr</span><span class="sxs-lookup"><span data-stu-id="47923-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="47923-125">Sehen Sie sich die Chat-Anwendung aus dem Tutorial zu den ersten Schritten [mit signalr](../getting-started/tutorial-getting-started-with-signalr.md)an.</span><span class="sxs-lookup"><span data-stu-id="47923-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="47923-126">Hier ist die Hub-Klasse aus der Anwendung:</span><span class="sxs-lookup"><span data-stu-id="47923-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="47923-127">Angenommen, Sie möchten Chat Nachrichten auf dem Server speichern, bevor Sie Sie senden.</span><span class="sxs-lookup"><span data-stu-id="47923-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="47923-128">Sie können eine Schnittstelle definieren, die diese Funktionalität abstrahiert, und die Schnittstelle mit di in die `ChatHub` Klasse einfügen.</span><span class="sxs-lookup"><span data-stu-id="47923-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="47923-129">Das einzige Problem besteht darin, dass eine signalr-Anwendung keine Hubs direkt erstellt. Sie werden von signalr für Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="47923-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="47923-130">Standardmäßig erwartet signalr, dass eine Hub-Klasse einen Parameter losen Konstruktor hat.</span><span class="sxs-lookup"><span data-stu-id="47923-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="47923-131">Sie können jedoch problemlos eine Funktion registrieren, um Hub-Instanzen zu erstellen, und diese Funktion verwenden, um di auszuführen.</span><span class="sxs-lookup"><span data-stu-id="47923-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="47923-132">Registrieren Sie die Funktion, indem Sie **globalhost. DependencyResolver. Register**aufrufen.</span><span class="sxs-lookup"><span data-stu-id="47923-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="47923-133">Nun ruft signalr diese anonyme Funktion immer dann auf, wenn eine `ChatHub` Instanz erstellt werden muss.</span><span class="sxs-lookup"><span data-stu-id="47923-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="47923-134">IOC-Container</span><span class="sxs-lookup"><span data-stu-id="47923-134">IoC Containers</span></span>

<span data-ttu-id="47923-135">Der vorherige Code eignet sich gut für einfache Fälle.</span><span class="sxs-lookup"><span data-stu-id="47923-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="47923-136">Aber trotzdem mussten Sie Folgendes schreiben:</span><span class="sxs-lookup"><span data-stu-id="47923-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="47923-137">In einer komplexen Anwendung mit vielen Abhängigkeiten müssen Sie möglicherweise einen großen Teil dieses "Verdrahtungs Codes" schreiben.</span><span class="sxs-lookup"><span data-stu-id="47923-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="47923-138">Dieser Code kann schwierig zu verwalten sein, insbesondere, wenn Abhängigkeiten eingebettet sind.</span><span class="sxs-lookup"><span data-stu-id="47923-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="47923-139">Der Komponenten Test ist auch schwierig.</span><span class="sxs-lookup"><span data-stu-id="47923-139">It is also hard to unit test.</span></span>

<span data-ttu-id="47923-140">Eine Lösung ist die Verwendung eines IOC-Containers.</span><span class="sxs-lookup"><span data-stu-id="47923-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="47923-141">Ein IOC-Container ist eine Softwarekomponente, die für die Verwaltung von Abhängigkeiten zuständig ist. Sie registrieren Typen beim Container und verwenden dann den Container, um Objekte zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="47923-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="47923-142">Der Container ermittelt automatisch die Abhängigkeitsbeziehungen.</span><span class="sxs-lookup"><span data-stu-id="47923-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="47923-143">Viele IOC-Container ermöglichen es Ihnen außerdem, Dinge wie Objekt Lebensdauer und Bereich zu steuern.</span><span class="sxs-lookup"><span data-stu-id="47923-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="47923-144">"IOC" steht für "Inversion of Control", bei dem es sich um ein allgemeines Muster handelt, bei dem ein Framework Anwendungscode aufruft.</span><span class="sxs-lookup"><span data-stu-id="47923-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="47923-145">Ein IOC-Container erstellt Ihre Objekte für Sie, was die übliche Ablauf Steuerung "Rück kehrt".</span><span class="sxs-lookup"><span data-stu-id="47923-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="47923-146">Verwenden von IOC-Containern in signalr</span><span class="sxs-lookup"><span data-stu-id="47923-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="47923-147">Die Chat-Anwendung ist wahrscheinlich zu einfach für den Vorteil eines IOC-Containers.</span><span class="sxs-lookup"><span data-stu-id="47923-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="47923-148">Sehen wir uns stattdessen das [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) -Beispiel an.</span><span class="sxs-lookup"><span data-stu-id="47923-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="47923-149">Das StockTicker-Beispiel definiert zwei Hauptklassen:</span><span class="sxs-lookup"><span data-stu-id="47923-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="47923-150">`StockTickerHub`: die Hub-Klasse, die Clientverbindungen verwaltet.</span><span class="sxs-lookup"><span data-stu-id="47923-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="47923-151">`StockTicker`: ein Singleton mit Aktienkursen, der in regelmäßigen Abständen aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="47923-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="47923-152">`StockTickerHub` enthält einen Verweis auf die `StockTicker` Singleton, während `StockTicker` einen Verweis auf den **ihubconnectioncontext** für die `StockTickerHub`enthält.</span><span class="sxs-lookup"><span data-stu-id="47923-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="47923-153">Diese Schnittstelle wird verwendet, um mit `StockTickerHub` Instanzen zu kommunizieren.</span><span class="sxs-lookup"><span data-stu-id="47923-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="47923-154">(Weitere Informationen finden Sie unter [Server Broadcast mit ASP.net signalr](index.md).)</span><span class="sxs-lookup"><span data-stu-id="47923-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="47923-155">Wir können einen IOC-Container verwenden, um diese Abhängigkeiten etwas zu lösen.</span><span class="sxs-lookup"><span data-stu-id="47923-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="47923-156">Vereinfachen Sie zunächst die Klassen `StockTickerHub` und `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="47923-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="47923-157">Im folgenden Code werden die Teile, die wir nicht benötigen, auskommentiert.</span><span class="sxs-lookup"><span data-stu-id="47923-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="47923-158">Entfernen Sie den Parameter losen Konstruktor aus `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="47923-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="47923-159">Stattdessen verwenden wir immer di zum Erstellen des Hubs.</span><span class="sxs-lookup"><span data-stu-id="47923-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="47923-160">Entfernen Sie für StockTicker die Singleton-Instanz.</span><span class="sxs-lookup"><span data-stu-id="47923-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="47923-161">Später verwenden wir den IOC-Container, um die StockTicker-Lebensdauer zu steuern.</span><span class="sxs-lookup"><span data-stu-id="47923-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="47923-162">Legen Sie außerdem den Konstruktor als öffentlich.</span><span class="sxs-lookup"><span data-stu-id="47923-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="47923-163">Als nächstes können wir den Code umgestalten, indem wir eine Schnittstelle für `StockTicker`erstellen.</span><span class="sxs-lookup"><span data-stu-id="47923-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="47923-164">Wir verwenden diese Schnittstelle, um die `StockTickerHub` von der `StockTicker`-Klasse zu entkoppeln.</span><span class="sxs-lookup"><span data-stu-id="47923-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="47923-165">Visual Studio macht diese Art von Umgestaltung einfach.</span><span class="sxs-lookup"><span data-stu-id="47923-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="47923-166">Öffnen Sie die Datei StockTicker.cs, klicken Sie mit der rechten Maustaste auf die Deklaration der `StockTicker`-Klasse, und wählen Sie **umgestalten** ... **Schnittstelle extrahieren**.</span><span class="sxs-lookup"><span data-stu-id="47923-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="47923-167">Klicken Sie im Dialogfeld **Schnittstelle extrahieren** auf **Alles auswählen**.</span><span class="sxs-lookup"><span data-stu-id="47923-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="47923-168">Behalten Sie die restlichen Standardwerte bei.</span><span class="sxs-lookup"><span data-stu-id="47923-168">Leave the other defaults.</span></span> <span data-ttu-id="47923-169">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="47923-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="47923-170">Visual Studio erstellt eine neue Schnittstelle mit dem Namen `IStockTicker`und ändert `StockTicker`, sodass Sie von `IStockTicker`abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="47923-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="47923-171">Öffnen Sie die Datei IStockTicker.cs, und ändern Sie die Schnittstelle in **Public**.</span><span class="sxs-lookup"><span data-stu-id="47923-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="47923-172">Ändern Sie in der `StockTickerHub`-Klasse die beiden Instanzen von `StockTicker` in `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="47923-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="47923-173">Das Erstellen einer `IStockTicker` Schnittstelle ist nicht zwingend notwendig, aber ich wollte zeigen, wie di die Kopplung zwischen Komponenten in Ihrer Anwendung reduzieren kann.</span><span class="sxs-lookup"><span data-stu-id="47923-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="47923-174">Hinzufügen der Ninject-Bibliothek</span><span class="sxs-lookup"><span data-stu-id="47923-174">Add the Ninject Library</span></span>

<span data-ttu-id="47923-175">Es gibt viele Open-Source-IoC-Container für .net.</span><span class="sxs-lookup"><span data-stu-id="47923-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="47923-176">Für dieses Tutorial verwende ich [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="47923-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="47923-177">(Weitere beliebte Bibliotheken sind [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)und [StructureMap](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="47923-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="47923-178">Verwenden Sie den nuget-Paket-Manager zum Installieren der [Ninject-Bibliothek](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="47923-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="47923-179">Klicken Sie in Visual Studio im **Menü Extras** auf **nuget-Paket-Manager** > Paket-Manager- **Konsole**.</span><span class="sxs-lookup"><span data-stu-id="47923-179">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="47923-180">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="47923-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="47923-181">Ersetzen des signalr-Abhängigkeits Konflikt Lösers</span><span class="sxs-lookup"><span data-stu-id="47923-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="47923-182">Um Ninject innerhalb von signalr zu verwenden, erstellen Sie eine Klasse, die von **defaultdependencyresolver**abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="47923-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="47923-183">Diese Klasse überschreibt die **GetService** -Methode und die **GetServices** -Methode von **defaultdependencyresolver**.</span><span class="sxs-lookup"><span data-stu-id="47923-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="47923-184">Signalr ruft diese Methoden auf, um verschiedene Objekte zur Laufzeit zu erstellen, einschließlich hubinstanzen, sowie verschiedene Dienste, die intern von signalr verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="47923-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="47923-185">Die **GetService** -Methode erstellt eine einzelne Instanz eines Typs.</span><span class="sxs-lookup"><span data-stu-id="47923-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="47923-186">Überschreiben Sie diese Methode, um die **TryGet** -Methode des Ninject-Kernels aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="47923-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="47923-187">Wenn diese Methode NULL zurückgibt, greifen Sie auf den Standard Konflikt Löser zurück.</span><span class="sxs-lookup"><span data-stu-id="47923-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="47923-188">Die **GetServices** -Methode erstellt eine Auflistung von Objekten eines angegebenen Typs.</span><span class="sxs-lookup"><span data-stu-id="47923-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="47923-189">Überschreiben Sie diese Methode, um die Ergebnisse von Ninject mit den Ergebnissen des standardresolvers zu verketten.</span><span class="sxs-lookup"><span data-stu-id="47923-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="47923-190">Konfigurieren von Ninject-Bindungen</span><span class="sxs-lookup"><span data-stu-id="47923-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="47923-191">Nun verwenden wir Ninject, um Typbindungen zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="47923-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="47923-192">Öffnen Sie die Datei RegisterHubs.cs.</span><span class="sxs-lookup"><span data-stu-id="47923-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="47923-193">Erstellen Sie in der `RegisterHubs.Start`-Methode den Ninject-Container, der Ninject den *Kernel*aufruft.</span><span class="sxs-lookup"><span data-stu-id="47923-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="47923-194">Erstellen Sie eine Instanz des benutzerdefinierten Abhängigkeits Konflikt Lösers:</span><span class="sxs-lookup"><span data-stu-id="47923-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="47923-195">Erstellen Sie eine Bindung für `IStockTicker` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="47923-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="47923-196">In diesem Code werden zwei Dinge aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="47923-196">This code is saying two things.</span></span> <span data-ttu-id="47923-197">Wenn die Anwendung eine `IStockTicker`benötigt, sollte der Kernel zunächst eine Instanz von `StockTicker`erstellen.</span><span class="sxs-lookup"><span data-stu-id="47923-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="47923-198">Zweitens sollte die `StockTicker`-Klasse als Singleton-Objekt erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="47923-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="47923-199">Ninject erstellt eine Instanz des-Objekts und gibt für jede Anforderung dieselbe Instanz zurück.</span><span class="sxs-lookup"><span data-stu-id="47923-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="47923-200">Erstellen Sie wie folgt eine Bindung für " **ihubconnectioncontext** ":</span><span class="sxs-lookup"><span data-stu-id="47923-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="47923-201">Dieser Code erstellt eine anonyme Funktion, die eine **ihubconnection**zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="47923-201">This code creates an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="47923-202">Die Methode " **accessinjetedinto** " weist Ninject an, diese Funktion nur beim Erstellen von `IStockTicker` Instanzen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="47923-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="47923-203">Der Grund hierfür ist, dass signalr eine interne **ihubconnectioncontext** -Instanz erstellt und nicht überschreiben soll, wie Sie von signalr erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="47923-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="47923-204">Diese Funktion gilt nur für unsere `StockTicker`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="47923-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="47923-205">Übergeben Sie den Abhängigkeits Konflikt Löser an die **maphubs** -Methode:</span><span class="sxs-lookup"><span data-stu-id="47923-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="47923-206">Jetzt verwendet signalr den in **maphubs**angegebenen Konflikt Löser anstelle des Standard Konflikt Lösers.</span><span class="sxs-lookup"><span data-stu-id="47923-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="47923-207">Hier ist das komplette Codelisting für `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="47923-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="47923-208">Drücken Sie F5, um die StockTicker-Anwendung in Visual Studio auszuführen.</span><span class="sxs-lookup"><span data-stu-id="47923-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="47923-209">Navigieren Sie im Browserfenster zu `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="47923-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="47923-210">Die Anwendung verfügt über genau die gleiche Funktionalität wie zuvor.</span><span class="sxs-lookup"><span data-stu-id="47923-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="47923-211">(Eine Beschreibung finden Sie unter [Server Broadcast mit ASP.net signalr](index.md).) Das Verhalten wurde nicht geändert. der Code wurde einfach getestet, gewartet und entwickelt.</span><span class="sxs-lookup"><span data-stu-id="47923-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
