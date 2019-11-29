---
uid: web-api/overview/advanced/dependency-injection
title: Abhängigkeitsinjektion in ASP.net-Web-API 2-ASP.NET 4. x
author: MikeWasson
description: In diesem Tutorial wird gezeigt, wie Sie Abhängigkeiten in den ASP.net-Web-API Controller für ASP.NET 4. x einfügen.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: f9c212af92168ac02644625b9aa8ec1bef329cab
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600409"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="44740-103">Abhängigkeitsinjektion in ASP.net-Web-API 2</span><span class="sxs-lookup"><span data-stu-id="44740-103">Dependency Injection in ASP.NET Web API 2</span></span>

<span data-ttu-id="44740-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="44740-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="44740-105">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="44740-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="44740-106">In diesem Tutorial wird gezeigt, wie Sie Abhängigkeiten in Ihren ASP.net-Web-API Controller einfügen.</span><span class="sxs-lookup"><span data-stu-id="44740-106">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="44740-107">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="44740-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="44740-108">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="44740-108">Web API 2</span></span>
> - [<span data-ttu-id="44740-109">Unity-Anwendungs Block</span><span class="sxs-lookup"><span data-stu-id="44740-109">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="44740-110">Entity Framework 6 (Version 5 funktioniert auch)</span><span class="sxs-lookup"><span data-stu-id="44740-110">Entity Framework 6 (version 5 also works)</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="44740-111">Was ist eine Abhängigkeitsinjektion?</span><span class="sxs-lookup"><span data-stu-id="44740-111">What is Dependency Injection?</span></span>

<span data-ttu-id="44740-112">Eine *Abhängigkeit* ist ein beliebiges Objekt, das ein anderes Objekt benötigt.</span><span class="sxs-lookup"><span data-stu-id="44740-112">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="44740-113">Beispielsweise ist es üblich, ein [Repository](http://martinfowler.com/eaaCatalog/repository.html) zu definieren, das den Datenzugriff behandelt.</span><span class="sxs-lookup"><span data-stu-id="44740-113">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="44740-114">Sehen wir uns ein Beispiel an.</span><span class="sxs-lookup"><span data-stu-id="44740-114">Let's illustrate with an example.</span></span> <span data-ttu-id="44740-115">Zuerst definieren wir ein Domänen Modell:</span><span class="sxs-lookup"><span data-stu-id="44740-115">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="44740-116">Im folgenden finden Sie eine einfache Repository-Klasse, in der Elemente in einer Datenbank mithilfe von Entity Framework gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="44740-116">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="44740-117">Definieren wir nun einen Web-API-Controller, der Get-Anforderungen für `Product` Entitäten unterstützt.</span><span class="sxs-lookup"><span data-stu-id="44740-117">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="44740-118">(Ich verlasse Post und andere Methoden zur Vereinfachung.) Hier ist ein erster Versuch:</span><span class="sxs-lookup"><span data-stu-id="44740-118">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="44740-119">Beachten Sie, dass die Controller Klasse von `ProductRepository`abhängt und dass der Controller die `ProductRepository` Instanz erstellen kann.</span><span class="sxs-lookup"><span data-stu-id="44740-119">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="44740-120">Es ist jedoch eine gute Idee, die Abhängigkeit aus verschiedenen Gründen hart zu codieren.</span><span class="sxs-lookup"><span data-stu-id="44740-120">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="44740-121">Wenn Sie `ProductRepository` durch eine andere Implementierung ersetzen möchten, müssen Sie auch die Controller Klasse ändern.</span><span class="sxs-lookup"><span data-stu-id="44740-121">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="44740-122">Wenn die `ProductRepository` Abhängigkeiten aufweist, müssen Sie diese innerhalb des Controllers konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="44740-122">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="44740-123">Bei einem großen Projekt mit mehreren Controllern wird der Konfigurations Code in Ihrem Projekt verstreut.</span><span class="sxs-lookup"><span data-stu-id="44740-123">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="44740-124">Der Komponenten Test ist schwierig, da der Controller hart codiert ist, um die Datenbank abzufragen.</span><span class="sxs-lookup"><span data-stu-id="44740-124">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="44740-125">Für einen-Komponenten Test sollten Sie ein Mock-oder Stub-Repository verwenden, das mit dem aktuellen Entwurf nicht möglich ist.</span><span class="sxs-lookup"><span data-stu-id="44740-125">For a unit test, you should use a mock or stub repository, which is not possible with the current design.</span></span>

<span data-ttu-id="44740-126">Wir können diese Probleme beheben, indem Sie das Repository in den Controller *Einfügen* .</span><span class="sxs-lookup"><span data-stu-id="44740-126">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="44740-127">Umgestalten Sie zunächst die `ProductRepository` Klasse in eine Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="44740-127">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="44740-128">Geben Sie dann die `IProductRepository` als Konstruktorparameter an:</span><span class="sxs-lookup"><span data-stu-id="44740-128">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="44740-129">In diesem Beispiel wird die [Konstruktorinjektion](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)verwendet.</span><span class="sxs-lookup"><span data-stu-id="44740-129">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="44740-130">Sie können auch *Setter Injection*verwenden, bei dem Sie die Abhängigkeit über eine Setter-Methode oder-Eigenschaft festlegen.</span><span class="sxs-lookup"><span data-stu-id="44740-130">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="44740-131">Doch jetzt liegt ein Problem vor, da Ihre Anwendung den Controller nicht direkt erstellt.</span><span class="sxs-lookup"><span data-stu-id="44740-131">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="44740-132">Die Web-API erstellt den Controller, wenn Sie die Anforderung weiterleitet, und die Web-API weiß nichts über `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="44740-132">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="44740-133">An dieser Stelle kommt der Web-API-Abhängigkeits Konflikt Löser ins Spiel.</span><span class="sxs-lookup"><span data-stu-id="44740-133">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="44740-134">Der Web-API-Abhängigkeits Konflikt Löser</span><span class="sxs-lookup"><span data-stu-id="44740-134">The Web API Dependency Resolver</span></span>

<span data-ttu-id="44740-135">Die Web-API definiert die **idepdencyresolver** -Schnittstelle zum Auflösen von Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="44740-135">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="44740-136">Hier ist die Definition der-Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="44740-136">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="44740-137">Die **idepdencyscope** -Schnittstelle verfügt über zwei Methoden:</span><span class="sxs-lookup"><span data-stu-id="44740-137">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="44740-138">**GetService** erstellt eine Instanz eines Typs.</span><span class="sxs-lookup"><span data-stu-id="44740-138">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="44740-139">**GetServices** erstellt eine Auflistung von Objekten eines angegebenen Typs.</span><span class="sxs-lookup"><span data-stu-id="44740-139">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="44740-140">Die **idepdencyresolver** -Methode erbt **idepdencyscope** und fügt die **BeginScope** -Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="44740-140">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="44740-141">Später in diesem Tutorial werde ich über Bereiche sprechen.</span><span class="sxs-lookup"><span data-stu-id="44740-141">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="44740-142">Wenn die Web-API eine Controller Instanz erstellt, ruft Sie zuerst **idepdencyresolver. GetService**auf und übergibt dabei den Controllertyp.</span><span class="sxs-lookup"><span data-stu-id="44740-142">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="44740-143">Mit diesem Erweiterbarkeits Hook können Sie den Controller erstellen und alle Abhängigkeiten auflösen.</span><span class="sxs-lookup"><span data-stu-id="44740-143">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="44740-144">Wenn **GetService** NULL zurückgibt, sucht die Web-API in der Controller Klasse nach einem Parameter losen Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="44740-144">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="44740-145">Abhängigkeitsauflösung mit dem Unity-Container</span><span class="sxs-lookup"><span data-stu-id="44740-145">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="44740-146">Obwohl Sie eine vollständige **idepdencyresolver** -Implementierung von Grund auf neu schreiben könnten, ist die Schnittstelle eigentlich so konzipiert, dass Sie als Bridge zwischen Web-API und vorhandenen IOC-Containern fungiert.</span><span class="sxs-lookup"><span data-stu-id="44740-146">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="44740-147">Ein IOC-Container ist eine Softwarekomponente, die für die Verwaltung von Abhängigkeiten zuständig ist.</span><span class="sxs-lookup"><span data-stu-id="44740-147">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="44740-148">Sie registrieren Typen beim Container und verwenden dann den Container, um Objekte zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="44740-148">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="44740-149">Der Container ermittelt automatisch die Abhängigkeitsbeziehungen.</span><span class="sxs-lookup"><span data-stu-id="44740-149">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="44740-150">Viele IOC-Container ermöglichen es Ihnen außerdem, Dinge wie Objekt Lebensdauer und Bereich zu steuern.</span><span class="sxs-lookup"><span data-stu-id="44740-150">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="44740-151">"IOC" steht für "Inversion of Control", bei dem es sich um ein allgemeines Muster handelt, bei dem ein Framework Anwendungscode aufruft.</span><span class="sxs-lookup"><span data-stu-id="44740-151">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="44740-152">Ein IOC-Container erstellt Ihre Objekte für Sie, was die übliche Ablauf Steuerung "Rück kehrt".</span><span class="sxs-lookup"><span data-stu-id="44740-152">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

<span data-ttu-id="44740-153">In diesem Tutorial verwenden wir [Unity](https://msdn.microsoft.com/library/ff647202.aspx) aus Microsoft Patterns &amp; Practices.</span><span class="sxs-lookup"><span data-stu-id="44740-153">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="44740-154">(Weitere beliebte Bibliotheken sind [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/)und [StructureMap](http://structuremap.github.io/documentation/).) Sie können den nuget-Paket-Manager verwenden, um Unity zu installieren.</span><span class="sxs-lookup"><span data-stu-id="44740-154">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://structuremap.github.io/documentation/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="44740-155">Wählen Sie **im Menü Extras** in Visual Studio **nuget-Paket-Manager**und dann **Paket-Manager-Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="44740-155">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="44740-156">Geben Sie im Fenster der Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="44740-156">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="44740-157">Im folgenden finden Sie eine Implementierung von **idepdencyresolver** , der einen Unity-Container umschließt.</span><span class="sxs-lookup"><span data-stu-id="44740-157">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="44740-158">Wenn die **GetService** -Methode einen Typ nicht auflösen kann, sollte Sie **null**zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="44740-158">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="44740-159">Wenn die **GetServices** -Methode einen Typ nicht auflösen kann, sollte ein leeres Auflistungs Objekt zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="44740-159">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="44740-160">Lösen Sie keine Ausnahmen für unbekannte Typen aus.</span><span class="sxs-lookup"><span data-stu-id="44740-160">Don't throw exceptions for unknown types.</span></span>

## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="44740-161">Konfigurieren des Abhängigkeits Konflikt Lösers</span><span class="sxs-lookup"><span data-stu-id="44740-161">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="44740-162">Legen Sie den Abhängigkeits Konflikt Löser für die **DependencyResolver** -Eigenschaft des globalen **httpconfiguration** -Objekts fest.</span><span class="sxs-lookup"><span data-stu-id="44740-162">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="44740-163">Der folgende Code registriert die `IProductRepository`-Schnittstelle bei Unity und erstellt dann eine `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="44740-163">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="44740-164">Abhängigkeits Bereich und Controller Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="44740-164">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="44740-165">Controller werden pro Anforderung erstellt.</span><span class="sxs-lookup"><span data-stu-id="44740-165">Controllers are created per request.</span></span> <span data-ttu-id="44740-166">Zum Verwalten der Objekt Lebensdauer verwendet **idepdencyresolver** das Konzept eines *Bereichs*.</span><span class="sxs-lookup"><span data-stu-id="44740-166">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="44740-167">Der an das **httpconfiguration** -Objekt angefügte Abhängigkeits Konflikt Löser verfügt über einen globalen Gültigkeitsbereich.</span><span class="sxs-lookup"><span data-stu-id="44740-167">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="44740-168">Wenn die Web-API einen Controller erstellt, wird **BeginScope**aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="44740-168">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="44740-169">Diese Methode gibt einen **idepdencyscope** -Wert zurück, der einen untergeordneten Bereich darstellt.</span><span class="sxs-lookup"><span data-stu-id="44740-169">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="44740-170">Die Web-API ruft dann **GetService** für den untergeordneten Bereich auf, um den Controller zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="44740-170">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="44740-171">Wenn die Anforderung erfüllt ist, löschen Web-API **-Aufrufe den untergeordneten** Bereich.</span><span class="sxs-lookup"><span data-stu-id="44740-171">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="44740-172">Verwenden Sie die verwerfen **-Methode, um die Abhängigkeiten** des Controllers zu verwerfen.</span><span class="sxs-lookup"><span data-stu-id="44740-172">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="44740-173">Wie Sie **BeginScope** implementieren, hängt vom IOC-Container ab.</span><span class="sxs-lookup"><span data-stu-id="44740-173">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="44740-174">Für Unity entspricht der Gültigkeitsbereich einem untergeordneten Container:</span><span class="sxs-lookup"><span data-stu-id="44740-174">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="44740-175">Die meisten IOC-Container verfügen über ähnliche Entsprechungen.</span><span class="sxs-lookup"><span data-stu-id="44740-175">Most IoC containers have similar equivalents.</span></span>
