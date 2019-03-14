---
title: Ansichtsbasierte Autorisierung in ASP.NET Core MVC
author: rick-anderson
description: In diesem Dokument wird das Einfügen und Verwenden des autorisierungsdiensts in einer ASP.NET Core Razor-Ansicht veranschaulicht.
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: e497c41d4dca29fed8733f18cf727804e3f06d8c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047887"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="50832-103">Ansichtsbasierte Autorisierung in ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="50832-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="50832-104">Ein Entwickler möchte häufig einblenden, ausblenden oder anderweitig ändern eine Benutzeroberfläche, die basierend auf der Identität des aktuellen Benutzers.</span><span class="sxs-lookup"><span data-stu-id="50832-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="50832-105">Sie erreichen des autorisierungsdiensts in MVC-Ansichten über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="50832-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="50832-106">Um den Autorisierungsdienst in einer Razor-Ansicht einfügen möchten, verwenden Sie die `@inject` Richtlinie:</span><span class="sxs-lookup"><span data-stu-id="50832-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="50832-107">Platzieren Sie Sie ggf. den Autorisierungsdienst in jeder Ansicht der `@inject` -Direktive in der *_ViewImports.cshtml* Datei die *Ansichten* Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="50832-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="50832-108">Weitere Informationen finden Sie unter [Dependency Injection in Ansichten](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="50832-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="50832-109">Verwenden Sie den eingefügten Autorisierungsdienst Aufrufen `AuthorizeAsync` auf genau die gleiche Weise Sie, während der überprüfen würden [ressourcenbasierte Autorisierung](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="50832-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50832-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="50832-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50832-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="50832-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="50832-112">In einigen Fällen wird die Ressource Ihrem Ansichtsmodell sein.</span><span class="sxs-lookup"><span data-stu-id="50832-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="50832-113">Rufen Sie `AuthorizeAsync` auf genau die gleiche Weise Sie, während der überprüfen würden [ressourcenbasierte Autorisierung](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="50832-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50832-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="50832-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50832-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="50832-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="50832-116">Im vorangehenden Code wird das Modell als Ressource, die die richtlinienauswertung durchführen soll übergeben, zu berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="50832-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="50832-117">Verlassen Sie sich nicht auf die Umschaltfläche Sichtbarkeit von Ihrer app-UI-Elementen wie die einzige autorisierungsprüfung.</span><span class="sxs-lookup"><span data-stu-id="50832-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="50832-118">Ausblenden von einem Element der Benutzeroberfläche möglicherweise nicht vollständig Zugriff auf der zugehörigen Controlleraktion verhindern.</span><span class="sxs-lookup"><span data-stu-id="50832-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="50832-119">Betrachten Sie beispielsweise die Schaltfläche im vorhergehenden Codeausschnitt aus.</span><span class="sxs-lookup"><span data-stu-id="50832-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="50832-120">Benutzer kann Aufrufen der `Edit` Action-Methode, wenn er die relative Ressource kennt-URL ist */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="50832-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="50832-121">Aus diesem Grund die `Edit` Aktionsmethode sollten eigene autorisierungsprüfung ausführen.</span><span class="sxs-lookup"><span data-stu-id="50832-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
