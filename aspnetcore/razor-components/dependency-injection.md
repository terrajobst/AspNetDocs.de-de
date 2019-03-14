---
title: Abhängigkeitsinjektion für Razor-Komponenten
author: guardrex
description: Sehen Sie, wie Blazor und Razor-Komponenten apps integrierten Dienste verwenden können, indem sie in Komponenten eingefügt.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 6ce8fa74f20145f48797d267c20ef2593368b941
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042427"
---
# <a name="razor-components-dependency-injection"></a><span data-ttu-id="c0f2f-103">Abhängigkeitsinjektion für Razor-Komponenten</span><span class="sxs-lookup"><span data-stu-id="c0f2f-103">Razor Components dependency injection</span></span>

<span data-ttu-id="c0f2f-104">Durch [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="c0f2f-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="c0f2f-105">[Abhängigkeitsinjektion (Dependency Injection)](/aspnet/core/fundamentals/dependency-injection) ist eine integrierte Funktion.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-105">[Dependency injection (DI)](/aspnet/core/fundamentals/dependency-injection) is built-in.</span></span> <span data-ttu-id="c0f2f-106">Apps können die integrierten Dienste verwenden, indem sie in Komponenten eingefügt.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-106">Apps can use built-in services by having them injected into components.</span></span> <span data-ttu-id="c0f2f-107">Apps können auch benutzerdefinierte Dienste definieren und über Dependency Injection zur Verfügung stellen.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-107">Apps can also define custom services and make them available via DI.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="c0f2f-108">Dependency Injection</span><span class="sxs-lookup"><span data-stu-id="c0f2f-108">Dependency injection</span></span>

<span data-ttu-id="c0f2f-109">DI ist eine Technik für den Zugriff auf Dienste an einem zentralen Ort konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-109">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="c0f2f-110">Dies kann nützlich zu sein:</span><span class="sxs-lookup"><span data-stu-id="c0f2f-110">This can be useful to:</span></span>

* <span data-ttu-id="c0f2f-111">Geben Sie eine einzelne Instanz einer Dienstklasse für viele Komponenten frei (bekannt als eine *Singleton* Service).</span><span class="sxs-lookup"><span data-stu-id="c0f2f-111">Share a single instance of a service class across many components (known as a *singleton* service).</span></span>
* <span data-ttu-id="c0f2f-112">Entkoppeln von bestimmten konkreten Dienstklassen und nur auf Abstraktionen verweisen.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-112">Decouple components from particular concrete service classes and only reference abstractions.</span></span> <span data-ttu-id="c0f2f-113">Z. B. eine Schnittstelle `IDataAccess` wird durch eine konkrete Klasse implementiert `DataAccess`.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-113">For example, an interface `IDataAccess` is implemented by a concrete class `DataAccess`.</span></span> <span data-ttu-id="c0f2f-114">Wenn eine Komponente DI verwendet, zum Empfangen einer `IDataAccess` Implementierung, die Komponente ist nicht mit den konkreten Typ verknüpft.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="c0f2f-115">Die Implementierung kann z. B. um eine pseudoimplementierung in Komponententests ausgetauscht werden.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-115">The implementation can be swapped, perhaps to a mock implementation in unit tests.</span></span>

<span data-ttu-id="c0f2f-116">Das DI-System dient zum Angeben von Instanzen der Dienste für die Komponenten zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-116">The DI system is responsible for supplying instances of services to components.</span></span> <span data-ttu-id="c0f2f-117">DI löst auch Abhängigkeiten rekursiv auf, damit Dienste selbst von weiteren Diensten abhängen können.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-117">DI also resolves dependencies recursively so that services themselves can depend on further services.</span></span> <span data-ttu-id="c0f2f-118">DI ist während des Starts der app konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-118">DI is configured during startup of the app.</span></span> <span data-ttu-id="c0f2f-119">Ein Beispiel ist unten in diesem Thema dargestellt.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-119">An example is shown later in this topic.</span></span>

## <a name="add-services-to-di"></a><span data-ttu-id="c0f2f-120">Hinzufügen von Diensten zu Dependency Injection</span><span class="sxs-lookup"><span data-stu-id="c0f2f-120">Add services to DI</span></span>

<span data-ttu-id="c0f2f-121">Überprüfen Sie nach dem Erstellen einer neuen app, die `Startup.ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="c0f2f-121">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="c0f2f-122">Die `ConfigureServices` -Methode übergeben eine ["iservicecollection"](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), eine Liste der Dienstobjekte-Deskriptor (["servicedescriptor" vorhanden](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)).</span><span class="sxs-lookup"><span data-stu-id="c0f2f-122">The `ConfigureServices` method is passed an [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), which is a list of service descriptor objects ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)).</span></span> <span data-ttu-id="c0f2f-123">Dienste werden hinzugefügt, durch die Bereitstellung von Dienst-Deskriptoren, um die Sammlung von Diensten.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-123">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="c0f2f-124">Im folgenden Codebeispiel wird das Konzept veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="c0f2f-124">The following code sample demonstrates the concept:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="c0f2f-125">Dienste können mit folgender Lebensdauer konfiguriert werden:</span><span class="sxs-lookup"><span data-stu-id="c0f2f-125">Services can be configured with the following lifetimes:</span></span>

| <span data-ttu-id="c0f2f-126">Methode</span><span class="sxs-lookup"><span data-stu-id="c0f2f-126">Method</span></span>      | <span data-ttu-id="c0f2f-127">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="c0f2f-127">Description</span></span> |
| ----------- | ----------- |
| [<span data-ttu-id="c0f2f-128">Singleton</span><span class="sxs-lookup"><span data-stu-id="c0f2f-128">Singleton</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.singleton#Microsoft_Extensions_DependencyInjection_ServiceDescriptor_Singleton__1_System_Func_System_IServiceProvider___0__) | <span data-ttu-id="c0f2f-129">DI erstellt eine *Einzelinstanz* des Diensts.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-129">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="c0f2f-130">Alle Komponenten, die diesen Dienst erfordern, erhalten einen Verweis auf diese Instanz.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-130">All components requiring this service receive a reference to this instance.</span></span> |
| <span data-ttu-id="c0f2f-131">[Transient](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.transient) (vorübergehend)</span><span class="sxs-lookup"><span data-stu-id="c0f2f-131">[Transient](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.transient)</span></span> | <span data-ttu-id="c0f2f-132">Wenn eine Komponente dieses Diensts erfordert, erhält es eine *neue Instanz* des Diensts.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-132">Whenever a component requires this service, it receives a *new instance* of the service.</span></span> |
| <span data-ttu-id="c0f2f-133">[Scoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.scoped) (bereichsbezogen)</span><span class="sxs-lookup"><span data-stu-id="c0f2f-133">[Scoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.scoped)</span></span> | <span data-ttu-id="c0f2f-134">Die clientseitige Blazor keine derzeit das Konzept der Dependency Injection-Bereiche.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-134">Client-side Blazor doesn't currently have the concept of DI scopes.</span></span> <span data-ttu-id="c0f2f-135">`Scoped` verhält sich wie `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-135">`Scoped` behaves like `Singleton`.</span></span> <span data-ttu-id="c0f2f-136">ASP.NET Core-Razor-Komponenten unterstützen jedoch die `Scoped` Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-136">However, ASP.NET Core Razor Components support the `Scoped` lifetime.</span></span> <span data-ttu-id="c0f2f-137">In einer Razor-Komponente ist eine Bereichsbezogene dienstregistrierung für die Verbindung beschränkt.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-137">In a Razor Component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="c0f2f-138">Aus diesem Grund Bereichsbezogene Dienste ist die Verwendung für Dienste, die für den aktuellen Benutzer zugeordnet werden soll (auch wenn die aktuelle Absicht ist, führen Sie die clientseitige im Browser).</span><span class="sxs-lookup"><span data-stu-id="c0f2f-138">For this reason, using scoped services is preferred for services that should be scoped to the current user (even if the current intent is to run client-side in the browser).</span></span> |

<span data-ttu-id="c0f2f-139">Das DI-System basiert auf der in ASP.NET Core DI-System.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-139">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="c0f2f-140">Weitere Informationen finden Sie unter [Abhängigkeitsinjektion in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c0f2f-140">For more information, see [Dependency injection in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).</span></span>

## <a name="default-services"></a><span data-ttu-id="c0f2f-141">Standarddienste</span><span class="sxs-lookup"><span data-stu-id="c0f2f-141">Default services</span></span>

<span data-ttu-id="c0f2f-142">Standarddienste werden automatisch auf die Auflistung der app hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-142">Default services are automatically added to the service collection of an app.</span></span> <span data-ttu-id="c0f2f-143">Die folgende Tabelle zeigt einige der bereitgestellten Standarddienste nützlich.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-143">The following table shows some of the useful default services provided.</span></span>

| <span data-ttu-id="c0f2f-144">Methode</span><span class="sxs-lookup"><span data-stu-id="c0f2f-144">Method</span></span>       | <span data-ttu-id="c0f2f-145">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="c0f2f-145">Description</span></span> |
| ------------ | ----------- |
| [<span data-ttu-id="c0f2f-146">HttpClient</span><span class="sxs-lookup"><span data-stu-id="c0f2f-146">HttpClient</span></span>](/dotnet/api/system.net.http.httpclient) | <span data-ttu-id="c0f2f-147">Stellt Methoden zum Senden von HTTP-Anforderungen und Empfangen von HTTP-Antworten aus einer Ressource identifiziert, die durch einen URI (Singleton).</span><span class="sxs-lookup"><span data-stu-id="c0f2f-147">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI (singleton).</span></span> <span data-ttu-id="c0f2f-148">Beachten Sie, dass diese Instanz von `HttpClient` verwendet der Browser für die Verarbeitung des HTTP-Datenverkehrs im Hintergrund.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-148">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="c0f2f-149">[HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) wird automatisch auf der Basis-URI-Präfix der app festgelegt.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-149">[HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="c0f2f-150">`HttpClient` wird nur für die clientseitige Blazor apps bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-150">`HttpClient` is only provided to client-side Blazor apps.</span></span> |
| `IJSRuntime` | <span data-ttu-id="c0f2f-151">Stellt eine Instanz einer JavaScript-Laufzeit, die an den Aufrufe verteilt werden können.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-151">Represents an instance of a JavaScript runtime to which calls may be dispatched.</span></span> <span data-ttu-id="c0f2f-152">Weitere Informationen finden Sie unter <xref:razor-components/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-152">For more information, see <xref:razor-components/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="c0f2f-153">Hilfsprogramme für die Arbeit mit URIs und Navigation-Zustand (Singleton).</span><span class="sxs-lookup"><span data-stu-id="c0f2f-153">Helpers for working with URIs and navigation state (singleton).</span></span> <span data-ttu-id="c0f2f-154">`IUriHelper` wird für beide apps eine clientseitige, Blazor und ASP.NET Core-Razor-Komponenten bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-154">`IUriHelper` is provided to both client-side Blazor and ASP.NET Core Razor Components apps.</span></span> |

<span data-ttu-id="c0f2f-155">Beachten Sie, dass es ist möglich, einen Anbieter von benutzerdefinierten Diensten anstelle des Standard-Service-Anbieters zu verwenden, die durch die Standardvorlage hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-155">Note that it is possible to use a custom services provider instead of the default service provider that's added by the default template.</span></span> <span data-ttu-id="c0f2f-156">Ein benutzerdefinierter Dienst-Anbieter bereit nicht automatisch die Standarddienste, die in der Tabelle aufgeführten.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-156">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="c0f2f-157">Diese Dienste müssen explizit an den neuen Dienstanbieter hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-157">Those services must be added to the new service provider explicitly.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="c0f2f-158">Anfordern eines Diensts in einer Komponente</span><span class="sxs-lookup"><span data-stu-id="c0f2f-158">Request a service in a component</span></span>

<span data-ttu-id="c0f2f-159">Sobald die Sammlung von Diensten Dienste hinzugefügt werden, können sie in der Serverkomponenten Razor-Vorlagen mit eingefügt werden die `@inject` Razor-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-159">Once services are added to the service collection, they can be injected into the components' Razor templates using the `@inject` Razor directive.</span></span> <span data-ttu-id="c0f2f-160">`@inject` hat zwei Parameter:</span><span class="sxs-lookup"><span data-stu-id="c0f2f-160">`@inject` has two parameters:</span></span>

* <span data-ttu-id="c0f2f-161">Typname: Der Typ des Diensts eingefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-161">Type name: The type of the service to inject.</span></span>
* <span data-ttu-id="c0f2f-162">Name der Eigenschaft: Der Name der Eigenschaft, die den eingefügten app-Dienst empfangen werden soll.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-162">Property name: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="c0f2f-163">Beachten Sie, dass die Eigenschaft keine manuelle Erstellung erfordert.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-163">Note that the property doesn't require manual creation.</span></span> <span data-ttu-id="c0f2f-164">Der Compiler erstellt die-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-164">The compiler creates the property.</span></span>

<span data-ttu-id="c0f2f-165">Mehrere `@inject` -Anweisungen können zum Einfügen von verschiedenen Diensten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-165">Multiple `@inject` statements can be used to inject different services.</span></span>

<span data-ttu-id="c0f2f-166">Das folgende Beispiel veranschaulicht die Verwendung von `@inject`.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-166">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="c0f2f-167">Der Dienst implementiert `Services.IDataAccess` wird eingefügt, in der Eigenschaft der Komponente `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-167">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="c0f2f-168">Beachten Sie, wie der Code nur nutzt die `IDataAccess` Abstraktion:</span><span class="sxs-lookup"><span data-stu-id="c0f2f-168">Note how the code is only using the `IDataAccess` abstraction:</span></span>

```csharp
@page "/customer-list"
@using Services
@inject IDataAccess DataRepository

<ul>
    @if (Customers != null)
    {
        @foreach (var customer in Customers)
        {
            <li>@customer.FirstName @customer.LastName</li>
        }
    }
</ul>

@functions {
    private IReadOnlyList<Customer> Customers;

    protected override async Task OnInitAsync()
    {
        // The property DataRepository received an implementation
        // of IDataAccess through dependency injection. Use 
        // DataRepository to obtain data from the server.
        Customers = await DataRepository.GetAllCustomersAsync();
    }
}
```

<span data-ttu-id="c0f2f-169">Intern wird die generierte Eigenschaft (`DataRepository`) versehen ist, mit der `InjectAttribute` Attribut.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-169">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="c0f2f-170">In der Regel wird dieses Attribut nicht direkt verwendet.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-170">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="c0f2f-171">Wenn eine Basisklasse für Komponenten erforderlich ist, und eingefügte Eigenschaften auch erforderlich, für die Basisklasse sind, `InjectAttribute` manuell hinzugefügt werden können:</span><span class="sxs-lookup"><span data-stu-id="c0f2f-171">If a base class is required for components and injected properties are also required for the base class, `InjectAttribute` can be manually added:</span></span>

```csharp
public class ComponentBase : BlazorComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="c0f2f-172">In Komponenten, die von der Basisklasse abgeleitete der `@inject` Richtlinie ist nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-172">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="c0f2f-173">Die `InjectAttribute` der Basisklasse ist ausreichend:</span><span class="sxs-lookup"><span data-stu-id="c0f2f-173">The `InjectAttribute` of the base class is sufficient:</span></span>

```csharp
@page "/demo"
@inherits ComponentBase

<h1>...</h1>
...
```

## <a name="dependency-injection-in-services"></a><span data-ttu-id="c0f2f-174">Abhängigkeitsinjektion in Diensten</span><span class="sxs-lookup"><span data-stu-id="c0f2f-174">Dependency injection in services</span></span>

<span data-ttu-id="c0f2f-175">Komplexen Dienste müssen möglicherweise zusätzliche Dienste.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-175">Complex services might require additional services.</span></span> <span data-ttu-id="c0f2f-176">Im vorherigen Beispiel `DataAccess` möglicherweise die `HttpClient` Standarddienst.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-176">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="c0f2f-177">`@inject` oder der `InjectAttribute` kann nicht in Diensten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-177">`@inject` or the `InjectAttribute` can't be used in services.</span></span> <span data-ttu-id="c0f2f-178">*Konstruktorinjektion* muss stattdessen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-178">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="c0f2f-179">Die erforderlichen Dienste werden durch Hinzufügen von Parametern zum Konstruktor des Diensts hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-179">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="c0f2f-180">Abhängigkeitsinjektion auf den Dienst erstellt, erkennt es die Dienste, die im Konstruktor erfordert und stellt diese entsprechend an.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-180">When dependency injection creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

<span data-ttu-id="c0f2f-181">Im folgenden Codebeispiel wird das Konzept veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="c0f2f-181">The following code sample demonstrates the concept:</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
    ...
}
```

<span data-ttu-id="c0f2f-182">Beachten Sie die folgenden Voraussetzungen Konstruktorinjektion:</span><span class="sxs-lookup"><span data-stu-id="c0f2f-182">Note the following prerequisites for constructor injection:</span></span>

* <span data-ttu-id="c0f2f-183">Es muss einen Konstruktor, deren Argumente alle durch Dependency Injection erfüllt werden können.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-183">There must be one constructor whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="c0f2f-184">Beachten Sie, dass zusätzliche Parameter, die nicht durch Dependency Injection abgedeckt zulässig sind, wenn Sie Standardwerte angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-184">Note that additional parameters not covered by DI are allowed if default values are specified for them.</span></span>
* <span data-ttu-id="c0f2f-185">Anwendbarer Konstruktor muss *öffentliche*.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-185">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="c0f2f-186">Es muss nur ein anwendbarer Konstruktor vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-186">There must only be one applicable constructor.</span></span> <span data-ttu-id="c0f2f-187">Im Falle einer Mehrdeutigkeit werden von DI eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="c0f2f-187">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c0f2f-188">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="c0f2f-188">Additional resources</span></span>

* [<span data-ttu-id="c0f2f-189">Abhängigkeitsinjektion in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c0f2f-189">Dependency injection in ASP.NET Core</span></span>](/aspnet/core/fundamentals/dependency-injection)
