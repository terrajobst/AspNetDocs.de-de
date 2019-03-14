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
# <a name="razor-components-dependency-injection"></a>Abhängigkeitsinjektion für Razor-Komponenten

Durch [Rainer Stropek](https://www.timecockpit.com)

[Abhängigkeitsinjektion (Dependency Injection)](/aspnet/core/fundamentals/dependency-injection) ist eine integrierte Funktion. Apps können die integrierten Dienste verwenden, indem sie in Komponenten eingefügt. Apps können auch benutzerdefinierte Dienste definieren und über Dependency Injection zur Verfügung stellen.

## <a name="dependency-injection"></a>Dependency Injection

DI ist eine Technik für den Zugriff auf Dienste an einem zentralen Ort konfiguriert. Dies kann nützlich zu sein:

* Geben Sie eine einzelne Instanz einer Dienstklasse für viele Komponenten frei (bekannt als eine *Singleton* Service).
* Entkoppeln von bestimmten konkreten Dienstklassen und nur auf Abstraktionen verweisen. Z. B. eine Schnittstelle `IDataAccess` wird durch eine konkrete Klasse implementiert `DataAccess`. Wenn eine Komponente DI verwendet, zum Empfangen einer `IDataAccess` Implementierung, die Komponente ist nicht mit den konkreten Typ verknüpft. Die Implementierung kann z. B. um eine pseudoimplementierung in Komponententests ausgetauscht werden.

Das DI-System dient zum Angeben von Instanzen der Dienste für die Komponenten zur Verfügung. DI löst auch Abhängigkeiten rekursiv auf, damit Dienste selbst von weiteren Diensten abhängen können. DI ist während des Starts der app konfiguriert. Ein Beispiel ist unten in diesem Thema dargestellt.

## <a name="add-services-to-di"></a>Hinzufügen von Diensten zu Dependency Injection

Überprüfen Sie nach dem Erstellen einer neuen app, die `Startup.ConfigureServices` Methode:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Die `ConfigureServices` -Methode übergeben eine ["iservicecollection"](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), eine Liste der Dienstobjekte-Deskriptor (["servicedescriptor" vorhanden](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)). Dienste werden hinzugefügt, durch die Bereitstellung von Dienst-Deskriptoren, um die Sammlung von Diensten. Im folgenden Codebeispiel wird das Konzept veranschaulicht:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Dienste können mit folgender Lebensdauer konfiguriert werden:

| Methode      | Beschreibung |
| ----------- | ----------- |
| [Singleton](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.singleton#Microsoft_Extensions_DependencyInjection_ServiceDescriptor_Singleton__1_System_Func_System_IServiceProvider___0__) | DI erstellt eine *Einzelinstanz* des Diensts. Alle Komponenten, die diesen Dienst erfordern, erhalten einen Verweis auf diese Instanz. |
| [Transient](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.transient) (vorübergehend) | Wenn eine Komponente dieses Diensts erfordert, erhält es eine *neue Instanz* des Diensts. |
| [Scoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.scoped) (bereichsbezogen) | Die clientseitige Blazor keine derzeit das Konzept der Dependency Injection-Bereiche. `Scoped` verhält sich wie `Singleton`. ASP.NET Core-Razor-Komponenten unterstützen jedoch die `Scoped` Lebensdauer. In einer Razor-Komponente ist eine Bereichsbezogene dienstregistrierung für die Verbindung beschränkt. Aus diesem Grund Bereichsbezogene Dienste ist die Verwendung für Dienste, die für den aktuellen Benutzer zugeordnet werden soll (auch wenn die aktuelle Absicht ist, führen Sie die clientseitige im Browser). |

Das DI-System basiert auf der in ASP.NET Core DI-System. Weitere Informationen finden Sie unter [Abhängigkeitsinjektion in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).

## <a name="default-services"></a>Standarddienste

Standarddienste werden automatisch auf die Auflistung der app hinzugefügt. Die folgende Tabelle zeigt einige der bereitgestellten Standarddienste nützlich.

| Methode       | Beschreibung |
| ------------ | ----------- |
| [HttpClient](/dotnet/api/system.net.http.httpclient) | Stellt Methoden zum Senden von HTTP-Anforderungen und Empfangen von HTTP-Antworten aus einer Ressource identifiziert, die durch einen URI (Singleton). Beachten Sie, dass diese Instanz von `HttpClient` verwendet der Browser für die Verarbeitung des HTTP-Datenverkehrs im Hintergrund. [HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) wird automatisch auf der Basis-URI-Präfix der app festgelegt. `HttpClient` wird nur für die clientseitige Blazor apps bereitgestellt werden. |
| `IJSRuntime` | Stellt eine Instanz einer JavaScript-Laufzeit, die an den Aufrufe verteilt werden können. Weitere Informationen finden Sie unter <xref:razor-components/javascript-interop>. |
| `IUriHelper` | Hilfsprogramme für die Arbeit mit URIs und Navigation-Zustand (Singleton). `IUriHelper` wird für beide apps eine clientseitige, Blazor und ASP.NET Core-Razor-Komponenten bereitgestellt. |

Beachten Sie, dass es ist möglich, einen Anbieter von benutzerdefinierten Diensten anstelle des Standard-Service-Anbieters zu verwenden, die durch die Standardvorlage hinzugefügt wird. Ein benutzerdefinierter Dienst-Anbieter bereit nicht automatisch die Standarddienste, die in der Tabelle aufgeführten. Diese Dienste müssen explizit an den neuen Dienstanbieter hinzugefügt werden.

## <a name="request-a-service-in-a-component"></a>Anfordern eines Diensts in einer Komponente

Sobald die Sammlung von Diensten Dienste hinzugefügt werden, können sie in der Serverkomponenten Razor-Vorlagen mit eingefügt werden die `@inject` Razor-Anweisung. `@inject` hat zwei Parameter:

* Typname: Der Typ des Diensts eingefügt werden soll.
* Name der Eigenschaft: Der Name der Eigenschaft, die den eingefügten app-Dienst empfangen werden soll. Beachten Sie, dass die Eigenschaft keine manuelle Erstellung erfordert. Der Compiler erstellt die-Eigenschaft.

Mehrere `@inject` -Anweisungen können zum Einfügen von verschiedenen Diensten verwendet werden.

Das folgende Beispiel veranschaulicht die Verwendung von `@inject`. Der Dienst implementiert `Services.IDataAccess` wird eingefügt, in der Eigenschaft der Komponente `DataRepository`. Beachten Sie, wie der Code nur nutzt die `IDataAccess` Abstraktion:

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

Intern wird die generierte Eigenschaft (`DataRepository`) versehen ist, mit der `InjectAttribute` Attribut. In der Regel wird dieses Attribut nicht direkt verwendet. Wenn eine Basisklasse für Komponenten erforderlich ist, und eingefügte Eigenschaften auch erforderlich, für die Basisklasse sind, `InjectAttribute` manuell hinzugefügt werden können:

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

In Komponenten, die von der Basisklasse abgeleitete der `@inject` Richtlinie ist nicht erforderlich. Die `InjectAttribute` der Basisklasse ist ausreichend:

```csharp
@page "/demo"
@inherits ComponentBase

<h1>...</h1>
...
```

## <a name="dependency-injection-in-services"></a>Abhängigkeitsinjektion in Diensten

Komplexen Dienste müssen möglicherweise zusätzliche Dienste. Im vorherigen Beispiel `DataAccess` möglicherweise die `HttpClient` Standarddienst. `@inject` oder der `InjectAttribute` kann nicht in Diensten verwendet werden. *Konstruktorinjektion* muss stattdessen verwendet werden. Die erforderlichen Dienste werden durch Hinzufügen von Parametern zum Konstruktor des Diensts hinzugefügt. Abhängigkeitsinjektion auf den Dienst erstellt, erkennt es die Dienste, die im Konstruktor erfordert und stellt diese entsprechend an.

Im folgenden Codebeispiel wird das Konzept veranschaulicht:

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

Beachten Sie die folgenden Voraussetzungen Konstruktorinjektion:

* Es muss einen Konstruktor, deren Argumente alle durch Dependency Injection erfüllt werden können. Beachten Sie, dass zusätzliche Parameter, die nicht durch Dependency Injection abgedeckt zulässig sind, wenn Sie Standardwerte angegeben werden.
* Anwendbarer Konstruktor muss *öffentliche*.
* Es muss nur ein anwendbarer Konstruktor vorhanden sein. Im Falle einer Mehrdeutigkeit werden von DI eine Ausnahme ausgelöst.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Abhängigkeitsinjektion in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection)
