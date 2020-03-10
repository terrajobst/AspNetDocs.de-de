---
uid: signalr/overview/advanced/dependency-injection
title: Abhängigkeitsinjektion in signalr | Microsoft-Dokumentation
author: bradygaster
description: In den in diesem Thema verwendeten Software Versionen Visual Studio 2013 .NET 4,5 signalr Version 2 in früheren Versionen dieses Themas Informationen zu früheren Versionen von...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 52978b10b6c131ac8eff4535216cc60b43fdf3de
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431865"
---
# <a name="dependency-injection-in-signalr"></a><span data-ttu-id="5077d-103">Abhängigkeitsinjektion in SignalR</span><span class="sxs-lookup"><span data-stu-id="5077d-103">Dependency Injection in SignalR</span></span>

<span data-ttu-id="5077d-104">von [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5077d-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5077d-105">In diesem Thema verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="5077d-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="5077d-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5077d-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="5077d-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5077d-107">.NET 4.5</span></span>
> - <span data-ttu-id="5077d-108">Signalr Version 2</span><span class="sxs-lookup"><span data-stu-id="5077d-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="5077d-109">Vorherige Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="5077d-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="5077d-110">Informationen zu früheren Versionen von signalr finden Sie unter [signalr ältere Versionen](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="5077d-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="5077d-111">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="5077d-111">Questions and comments</span></span>
>
> <span data-ttu-id="5077d-112">Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten.</span><span class="sxs-lookup"><span data-stu-id="5077d-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5077d-113">Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="5077d-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="5077d-114">Die Abhängigkeitsinjektion ist eine Möglichkeit, hart codierte Abhängigkeiten zwischen Objekten zu entfernen. Dadurch wird das Ersetzen der Abhängigkeiten eines Objekts vereinfacht, entweder zum Testen (mit Pseudo Objekten) oder zum Ändern des Lauf Zeit Verhaltens.</span><span class="sxs-lookup"><span data-stu-id="5077d-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="5077d-115">In diesem Tutorial wird gezeigt, wie Sie eine Abhängigkeitsinjektion für signalr-Hubs ausführen.</span><span class="sxs-lookup"><span data-stu-id="5077d-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="5077d-116">Außerdem wird gezeigt, wie IOC-Container mit signalr verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5077d-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="5077d-117">Ein IOC-Container ist ein allgemeines Framework für die Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="5077d-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="5077d-118">Was ist eine Abhängigkeitsinjektion?</span><span class="sxs-lookup"><span data-stu-id="5077d-118">What is Dependency Injection?</span></span>

<span data-ttu-id="5077d-119">Überspringen Sie diesen Abschnitt, wenn Sie bereits mit der Abhängigkeitsinjektion vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="5077d-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="5077d-120">Die *Abhängigkeitsinjektion* (di) ist ein Muster, in dem Objekte nicht für das Erstellen eigener Abhängigkeiten verantwortlich sind.</span><span class="sxs-lookup"><span data-stu-id="5077d-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="5077d-121">Im folgenden finden Sie ein einfaches Beispiel zum motivieren von di.</span><span class="sxs-lookup"><span data-stu-id="5077d-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="5077d-122">Angenommen, Sie verfügen über ein Objekt, das Nachrichten protokollieren muss.</span><span class="sxs-lookup"><span data-stu-id="5077d-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="5077d-123">Sie können eine Protokollierungs Schnittstelle definieren:</span><span class="sxs-lookup"><span data-stu-id="5077d-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="5077d-124">In Ihrem-Objekt können Sie eine `ILogger` erstellen, um Meldungen zu protokollieren:</span><span class="sxs-lookup"><span data-stu-id="5077d-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="5077d-125">Dies funktioniert, ist aber nicht das beste Design.</span><span class="sxs-lookup"><span data-stu-id="5077d-125">This works, but it's not the best design.</span></span> <span data-ttu-id="5077d-126">Wenn Sie `FileLogger` durch eine andere `ILogger` Implementierung ersetzen möchten, müssen Sie `SomeComponent`ändern.</span><span class="sxs-lookup"><span data-stu-id="5077d-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="5077d-127">Unter der Annahme, dass viele andere Objekte `FileLogger`verwenden, müssen Sie alle ändern.</span><span class="sxs-lookup"><span data-stu-id="5077d-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="5077d-128">Wenn Sie sich entscheiden, `FileLogger` einen Singleton zu erstellen, müssen Sie auch Änderungen in der gesamten Anwendung vornehmen.</span><span class="sxs-lookup"><span data-stu-id="5077d-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="5077d-129">Ein besserer Ansatz besteht darin, eine `ILogger` in das Objekt einzufügen, z. –. mithilfe eines Konstruktorarguments:</span><span class="sxs-lookup"><span data-stu-id="5077d-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="5077d-130">Nun ist das Objekt nicht für die Auswahl der zu verwendenden `ILogger` verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="5077d-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="5077d-131">Sie können `ILogger`-Implementierungen wechseln, ohne die Objekte zu ändern, die von ihm abhängen.</span><span class="sxs-lookup"><span data-stu-id="5077d-131">You can switch `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="5077d-132">Dieses Muster wird als [Konstruktorinjektion](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="5077d-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="5077d-133">Ein weiteres Muster ist Setter Injection, bei dem Sie die Abhängigkeit über eine Setter-Methode oder-Eigenschaft festlegen.</span><span class="sxs-lookup"><span data-stu-id="5077d-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="5077d-134">Einfache Abhängigkeitsinjektion in signalr</span><span class="sxs-lookup"><span data-stu-id="5077d-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="5077d-135">Sehen Sie sich die Chat-Anwendung aus dem Tutorial zu den ersten Schritten [mit signalr](../getting-started/tutorial-getting-started-with-signalr.md)an.</span><span class="sxs-lookup"><span data-stu-id="5077d-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="5077d-136">Hier ist die Hub-Klasse aus der Anwendung:</span><span class="sxs-lookup"><span data-stu-id="5077d-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="5077d-137">Angenommen, Sie möchten Chat Nachrichten auf dem Server speichern, bevor Sie Sie senden.</span><span class="sxs-lookup"><span data-stu-id="5077d-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="5077d-138">Sie können eine Schnittstelle definieren, die diese Funktionalität abstrahiert, und die Schnittstelle mit di in die `ChatHub` Klasse einfügen.</span><span class="sxs-lookup"><span data-stu-id="5077d-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="5077d-139">Das einzige Problem besteht darin, dass eine signalr-Anwendung keine Hubs direkt erstellt. Sie werden von signalr für Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="5077d-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="5077d-140">Standardmäßig erwartet signalr, dass eine Hub-Klasse einen Parameter losen Konstruktor hat.</span><span class="sxs-lookup"><span data-stu-id="5077d-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="5077d-141">Sie können jedoch problemlos eine Funktion registrieren, um Hub-Instanzen zu erstellen, und diese Funktion verwenden, um di auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5077d-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="5077d-142">Registrieren Sie die Funktion, indem Sie **globalhost. DependencyResolver. Register**aufrufen.</span><span class="sxs-lookup"><span data-stu-id="5077d-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="5077d-143">Nun ruft signalr diese anonyme Funktion immer dann auf, wenn eine `ChatHub` Instanz erstellt werden muss.</span><span class="sxs-lookup"><span data-stu-id="5077d-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="5077d-144">IOC-Container</span><span class="sxs-lookup"><span data-stu-id="5077d-144">IoC Containers</span></span>

<span data-ttu-id="5077d-145">Der vorherige Code eignet sich gut für einfache Fälle.</span><span class="sxs-lookup"><span data-stu-id="5077d-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="5077d-146">Aber trotzdem mussten Sie Folgendes schreiben:</span><span class="sxs-lookup"><span data-stu-id="5077d-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="5077d-147">In einer komplexen Anwendung mit vielen Abhängigkeiten müssen Sie möglicherweise einen großen Teil dieses "Verdrahtungs Codes" schreiben.</span><span class="sxs-lookup"><span data-stu-id="5077d-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="5077d-148">Dieser Code kann schwierig zu verwalten sein, insbesondere, wenn Abhängigkeiten eingebettet sind.</span><span class="sxs-lookup"><span data-stu-id="5077d-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="5077d-149">Der Komponenten Test ist auch schwierig.</span><span class="sxs-lookup"><span data-stu-id="5077d-149">It is also hard to unit test.</span></span>

<span data-ttu-id="5077d-150">Eine Lösung ist die Verwendung eines IOC-Containers.</span><span class="sxs-lookup"><span data-stu-id="5077d-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="5077d-151">Ein IOC-Container ist eine Softwarekomponente, die für die Verwaltung von Abhängigkeiten zuständig ist. Sie registrieren Typen beim Container und verwenden dann den Container, um Objekte zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5077d-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="5077d-152">Der Container ermittelt automatisch die Abhängigkeitsbeziehungen.</span><span class="sxs-lookup"><span data-stu-id="5077d-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="5077d-153">Viele IOC-Container ermöglichen es Ihnen außerdem, Dinge wie Objekt Lebensdauer und Bereich zu steuern.</span><span class="sxs-lookup"><span data-stu-id="5077d-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="5077d-154">"IOC" steht für "Inversion of Control", bei dem es sich um ein allgemeines Muster handelt, bei dem ein Framework Anwendungscode aufruft.</span><span class="sxs-lookup"><span data-stu-id="5077d-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="5077d-155">Ein IOC-Container erstellt Ihre Objekte für Sie, was die übliche Ablauf Steuerung "Rück kehrt".</span><span class="sxs-lookup"><span data-stu-id="5077d-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="5077d-156">Verwenden von IOC-Containern in signalr</span><span class="sxs-lookup"><span data-stu-id="5077d-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="5077d-157">Die Chat-Anwendung ist wahrscheinlich zu einfach für den Vorteil eines IOC-Containers.</span><span class="sxs-lookup"><span data-stu-id="5077d-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="5077d-158">Sehen wir uns stattdessen das [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) -Beispiel an.</span><span class="sxs-lookup"><span data-stu-id="5077d-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="5077d-159">Das StockTicker-Beispiel definiert zwei Hauptklassen:</span><span class="sxs-lookup"><span data-stu-id="5077d-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="5077d-160">`StockTickerHub`: die Hub-Klasse, die Clientverbindungen verwaltet.</span><span class="sxs-lookup"><span data-stu-id="5077d-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="5077d-161">`StockTicker`: ein Singleton mit Aktienkursen, der in regelmäßigen Abständen aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="5077d-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="5077d-162">`StockTickerHub` enthält einen Verweis auf die `StockTicker` Singleton, während `StockTicker` einen Verweis auf den **ihubconnectioncontext** für die `StockTickerHub`enthält.</span><span class="sxs-lookup"><span data-stu-id="5077d-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="5077d-163">Diese Schnittstelle wird verwendet, um mit `StockTickerHub` Instanzen zu kommunizieren.</span><span class="sxs-lookup"><span data-stu-id="5077d-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="5077d-164">(Weitere Informationen finden Sie unter [Server Broadcast mit ASP.net signalr](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="5077d-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="5077d-165">Wir können einen IOC-Container verwenden, um diese Abhängigkeiten etwas zu lösen.</span><span class="sxs-lookup"><span data-stu-id="5077d-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="5077d-166">Vereinfachen Sie zunächst die Klassen `StockTickerHub` und `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="5077d-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="5077d-167">Im folgenden Code werden die Teile, die wir nicht benötigen, auskommentiert.</span><span class="sxs-lookup"><span data-stu-id="5077d-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="5077d-168">Entfernen Sie den Parameter losen Konstruktor aus `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="5077d-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="5077d-169">Stattdessen verwenden wir immer di zum Erstellen des Hubs.</span><span class="sxs-lookup"><span data-stu-id="5077d-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="5077d-170">Entfernen Sie für StockTicker die Singleton-Instanz.</span><span class="sxs-lookup"><span data-stu-id="5077d-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="5077d-171">Später verwenden wir den IOC-Container, um die StockTicker-Lebensdauer zu steuern.</span><span class="sxs-lookup"><span data-stu-id="5077d-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="5077d-172">Legen Sie außerdem den Konstruktor als öffentlich.</span><span class="sxs-lookup"><span data-stu-id="5077d-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="5077d-173">Als nächstes können wir den Code umgestalten, indem wir eine Schnittstelle für `StockTicker`erstellen.</span><span class="sxs-lookup"><span data-stu-id="5077d-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="5077d-174">Wir verwenden diese Schnittstelle, um die `StockTickerHub` von der `StockTicker`-Klasse zu entkoppeln.</span><span class="sxs-lookup"><span data-stu-id="5077d-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="5077d-175">Visual Studio macht diese Art von Umgestaltung einfach.</span><span class="sxs-lookup"><span data-stu-id="5077d-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="5077d-176">Öffnen Sie die Datei StockTicker.cs, klicken Sie mit der rechten Maustaste auf die Deklaration der `StockTicker`-Klasse, und wählen Sie **umgestalten** ... **Schnittstelle extrahieren**.</span><span class="sxs-lookup"><span data-stu-id="5077d-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="5077d-177">Klicken Sie im Dialogfeld **Schnittstelle extrahieren** auf **Alles auswählen**.</span><span class="sxs-lookup"><span data-stu-id="5077d-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="5077d-178">Behalten Sie die restlichen Standardwerte bei.</span><span class="sxs-lookup"><span data-stu-id="5077d-178">Leave the other defaults.</span></span> <span data-ttu-id="5077d-179">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="5077d-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="5077d-180">Visual Studio erstellt eine neue Schnittstelle mit dem Namen `IStockTicker`und ändert `StockTicker`, sodass Sie von `IStockTicker`abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="5077d-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="5077d-181">Öffnen Sie die Datei IStockTicker.cs, und ändern Sie die Schnittstelle in **Public**.</span><span class="sxs-lookup"><span data-stu-id="5077d-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="5077d-182">Ändern Sie in der `StockTickerHub`-Klasse die beiden Instanzen von `StockTicker` in `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="5077d-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="5077d-183">Das Erstellen einer `IStockTicker` Schnittstelle ist nicht zwingend notwendig, aber ich wollte zeigen, wie di die Kopplung zwischen Komponenten in Ihrer Anwendung reduzieren kann.</span><span class="sxs-lookup"><span data-stu-id="5077d-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="5077d-184">Hinzufügen der Ninject-Bibliothek</span><span class="sxs-lookup"><span data-stu-id="5077d-184">Add the Ninject Library</span></span>

<span data-ttu-id="5077d-185">Es gibt viele Open-Source-IoC-Container für .net.</span><span class="sxs-lookup"><span data-stu-id="5077d-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="5077d-186">Für dieses Tutorial verwende ich [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="5077d-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="5077d-187">(Weitere beliebte Bibliotheken sind [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)und [StructureMap](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="5077d-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="5077d-188">Verwenden Sie den nuget-Paket-Manager zum Installieren der [Ninject-Bibliothek](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="5077d-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="5077d-189">Klicken Sie in Visual Studio im **Menü Extras** auf **nuget-Paket-Manager** > Paket-Manager- **Konsole**.</span><span class="sxs-lookup"><span data-stu-id="5077d-189">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="5077d-190">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="5077d-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="5077d-191">Ersetzen des signalr-Abhängigkeits Konflikt Lösers</span><span class="sxs-lookup"><span data-stu-id="5077d-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="5077d-192">Um Ninject innerhalb von signalr zu verwenden, erstellen Sie eine Klasse, die von **defaultdependencyresolver**abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="5077d-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="5077d-193">Diese Klasse überschreibt die **GetService** -Methode und die **GetServices** -Methode von **defaultdependencyresolver**.</span><span class="sxs-lookup"><span data-stu-id="5077d-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="5077d-194">Signalr ruft diese Methoden auf, um verschiedene Objekte zur Laufzeit zu erstellen, einschließlich hubinstanzen, sowie verschiedene Dienste, die intern von signalr verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5077d-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="5077d-195">Die **GetService** -Methode erstellt eine einzelne Instanz eines Typs.</span><span class="sxs-lookup"><span data-stu-id="5077d-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="5077d-196">Überschreiben Sie diese Methode, um die **TryGet** -Methode des Ninject-Kernels aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="5077d-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="5077d-197">Wenn diese Methode NULL zurückgibt, greifen Sie auf den Standard Konflikt Löser zurück.</span><span class="sxs-lookup"><span data-stu-id="5077d-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="5077d-198">Die **GetServices** -Methode erstellt eine Auflistung von Objekten eines angegebenen Typs.</span><span class="sxs-lookup"><span data-stu-id="5077d-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="5077d-199">Überschreiben Sie diese Methode, um die Ergebnisse von Ninject mit den Ergebnissen des standardresolvers zu verketten.</span><span class="sxs-lookup"><span data-stu-id="5077d-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="5077d-200">Konfigurieren von Ninject-Bindungen</span><span class="sxs-lookup"><span data-stu-id="5077d-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="5077d-201">Nun verwenden wir Ninject, um Typbindungen zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="5077d-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="5077d-202">Öffnen Sie die Startup.cs-Klasse Ihrer Anwendung (die Sie entweder manuell gemäß den Paket Anweisungen in `readme.txt`erstellt haben, oder die durch Hinzufügen der Authentifizierung zu Ihrem Projekt erstellt wurde).</span><span class="sxs-lookup"><span data-stu-id="5077d-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="5077d-203">Erstellen Sie in der `Startup.Configuration`-Methode den Ninject-Container, der Ninject den *Kernel*aufruft.</span><span class="sxs-lookup"><span data-stu-id="5077d-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="5077d-204">Erstellen Sie eine Instanz des benutzerdefinierten Abhängigkeits Konflikt Lösers:</span><span class="sxs-lookup"><span data-stu-id="5077d-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="5077d-205">Erstellen Sie eine Bindung für `IStockTicker` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="5077d-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="5077d-206">In diesem Code werden zwei Dinge aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="5077d-206">This code is saying two things.</span></span> <span data-ttu-id="5077d-207">Wenn die Anwendung eine `IStockTicker`benötigt, sollte der Kernel zunächst eine Instanz von `StockTicker`erstellen.</span><span class="sxs-lookup"><span data-stu-id="5077d-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="5077d-208">Zweitens sollte die `StockTicker`-Klasse als Singleton-Objekt erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="5077d-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="5077d-209">Ninject erstellt eine Instanz des-Objekts und gibt für jede Anforderung dieselbe Instanz zurück.</span><span class="sxs-lookup"><span data-stu-id="5077d-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="5077d-210">Erstellen Sie wie folgt eine Bindung für " **ihubconnectioncontext** ":</span><span class="sxs-lookup"><span data-stu-id="5077d-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="5077d-211">Dieser Code erstellt eine anonyme Funktion, die eine **ihubconnection**zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="5077d-211">This code creates an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="5077d-212">Die Methode " **accessinjetedinto** " weist Ninject an, diese Funktion nur beim Erstellen von `IStockTicker` Instanzen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="5077d-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="5077d-213">Der Grund hierfür ist, dass signalr eine interne **ihubconnectioncontext** -Instanz erstellt und nicht überschreiben soll, wie Sie von signalr erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="5077d-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="5077d-214">Diese Funktion gilt nur für unsere `StockTicker`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="5077d-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="5077d-215">Übergeben Sie den Abhängigkeits Konflikt Löser an die **mapsignalr** -Methode, indem Sie eine Hub-Konfiguration hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="5077d-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="5077d-216">Aktualisieren Sie die Methode Startup. konfiguriersignalr in der Startup-Klasse des Beispiels mit dem neuen Parameter:</span><span class="sxs-lookup"><span data-stu-id="5077d-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="5077d-217">Jetzt verwendet signalr den in **mapsignalr**angegebenen Konflikt Löser anstelle des Standard Konflikt Lösers.</span><span class="sxs-lookup"><span data-stu-id="5077d-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="5077d-218">Hier ist das komplette Codelisting für `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="5077d-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="5077d-219">Drücken Sie F5, um die StockTicker-Anwendung in Visual Studio auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5077d-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="5077d-220">Navigieren Sie im Browserfenster zu `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="5077d-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="5077d-221">Die Anwendung verfügt über genau die gleiche Funktionalität wie zuvor.</span><span class="sxs-lookup"><span data-stu-id="5077d-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="5077d-222">(Eine Beschreibung finden Sie unter [Server Broadcast mit ASP.net signalr](../getting-started/tutorial-server-broadcast-with-signalr.md).) Das Verhalten wurde nicht geändert. der Code wurde einfach getestet, gewartet und entwickelt.</span><span class="sxs-lookup"><span data-stu-id="5077d-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
