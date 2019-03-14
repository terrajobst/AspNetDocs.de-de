---
title: Razor-Komponenten, die routing
author: guardrex
description: Erfahren Sie, wie Sie Anforderungen in apps und über die NavLink-Komponente weiterleiten.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/01/2019
uid: razor-components/routing
ms.openlocfilehash: 5c648ba1bb3846f5baa515e808a98a3b33f81438
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031607"
---
# <a name="razor-components-routing"></a><span data-ttu-id="867f5-103">Razor-Komponenten, die routing</span><span class="sxs-lookup"><span data-stu-id="867f5-103">Razor Components routing</span></span>

<span data-ttu-id="867f5-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="867f5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="867f5-105">Erfahren Sie, wie Sie Anforderungen in apps und über die NavLink-Komponente weiterleiten.</span><span class="sxs-lookup"><span data-stu-id="867f5-105">Learn how to route requests in apps and about the NavLink component.</span></span>

<span data-ttu-id="867f5-106">[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="867f5-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="867f5-107">Informieren Sie sich in dem Thema [Erste Schritte](xref:razor-components/get-started) über die Voraussetzungen.</span><span class="sxs-lookup"><span data-stu-id="867f5-107">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

## <a name="route-templates"></a><span data-ttu-id="867f5-108">Routenvorlagen</span><span class="sxs-lookup"><span data-stu-id="867f5-108">Route templates</span></span>

<span data-ttu-id="867f5-109">Die `<Router>` Komponente ermöglicht die Weiterleitung, und eine routenvorlage wird bereitgestellt, um jede Komponente zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="867f5-109">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="867f5-110">Die `<Router>` Komponente angezeigt wird, der *App.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="867f5-110">The `<Router>` component appears in the *App.cshtml* file:</span></span>

```cshtml
<Router AppAssembly=typeof(Program).Assembly />
```

<span data-ttu-id="867f5-111">Wenn eine  *\*cshtml* -Datei mit ein `@page` Richtlinie wird kompiliert, wird die generierte Klasse wird eine [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) die routenvorlage angeben.</span><span class="sxs-lookup"><span data-stu-id="867f5-111">When a *\*.cshtml* file with an `@page` directive is compiled, the generated class is given a [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) specifying the route template.</span></span> <span data-ttu-id="867f5-112">Zur Laufzeit sucht der Router Komponentenklassen mit einem `RouteAttribute` und rendert, welche Komponente eine routenvorlage verfügt, die mit der angeforderten URL übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="867f5-112">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="867f5-113">Mehrere routenvorlagen können auf eine Komponente angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="867f5-113">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="867f5-114">In der [Beispiel-app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), die folgende Komponente antwortet auf Anfragen für `/BlazorRoute` und `/DifferentBlazorRoute`.</span><span class="sxs-lookup"><span data-stu-id="867f5-114">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), the following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`.</span></span>

<span data-ttu-id="867f5-115">*Pages/BlazorRoute.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="867f5-115">*Pages/BlazorRoute.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

<span data-ttu-id="867f5-116">`<Router>` unterstützt das Festlegen einer fallback Komponente für das Rendering, wenn eine angeforderte Route nicht aufgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="867f5-116">`<Router>` supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="867f5-117">Aktivieren Sie dieses Szenario durch Festlegen der `FallbackComponent` Parameter, um den Typ der fallback Komponentenklasse.</span><span class="sxs-lookup"><span data-stu-id="867f5-117">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="867f5-118">Im folgenden Beispiel wird eine Komponente, die in definierten *Pages/MyFallbackRazorComponent.cshtml* als fallback für die Komponente eine `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="867f5-118">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="867f5-119">Um Routen ordnungsgemäß generieren zu können, muss die app enthalten eine `<base>` -Tag in die *wwwroot/Index.HTML* -Datei mit den app-Basispfad angegeben, der `href` Attribut (`<base href="/" />`).</span><span class="sxs-lookup"><span data-stu-id="867f5-119">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/" />`).</span></span> <span data-ttu-id="867f5-120">Weitere Informationen finden Sie unter [hosten und bereitstellen: App-Basispfad](xref:host-and-deploy/razor-components/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="867f5-120">For more information, see [Host and deploy: App base path](xref:host-and-deploy/razor-components/index#app-base-path).</span></span>

## <a name="route-parameters"></a><span data-ttu-id="867f5-121">Routenparameter</span><span class="sxs-lookup"><span data-stu-id="867f5-121">Route parameters</span></span>

<span data-ttu-id="867f5-122">Der Router verwendet Routenparameter zum Auffüllen der entsprechenden Komponentenparameter mit dem gleichen Namen (Groß-/Kleinschreibung).</span><span class="sxs-lookup"><span data-stu-id="867f5-122">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive).</span></span>

<span data-ttu-id="867f5-123">*Pages/RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="867f5-123">*Pages/RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=8)]

<span data-ttu-id="867f5-124">Optionale Parameter werden nicht unterstützt, daher werden zwei `@page` Anweisungen sind im obigen Beispiel angewendet.</span><span class="sxs-lookup"><span data-stu-id="867f5-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="867f5-125">Die erste ermöglicht die Navigation zu der Komponente ohne Parameter.</span><span class="sxs-lookup"><span data-stu-id="867f5-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="867f5-126">Die zweite `@page` Direktive nimmt die `{text}` Routenparameter und weist den Wert der `Text` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="867f5-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="867f5-127">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="867f5-127">Route constraints</span></span>

<span data-ttu-id="867f5-128">Eine routeneinschränkung erzwingt typübereinstimmung auf einem routensegment zu einer Komponente.</span><span class="sxs-lookup"><span data-stu-id="867f5-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="867f5-129">Im folgenden Beispiel entspricht die Route für die Benutzer-Komponente nur, wenn:</span><span class="sxs-lookup"><span data-stu-id="867f5-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="867f5-130">Ein `Id` routensegment auf die Anforderungs-URL vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="867f5-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="867f5-131">Die `Id` -Segment ist eine ganze Zahl (`int`).</span><span class="sxs-lookup"><span data-stu-id="867f5-131">The `Id` segment is an integer (`int`).</span></span>

```cshtml
@page "/Users/{Id:int}"

<h1>The user Id is @Id!</h1>

@functions {
    [Parameter]
    private int Id { get; set; }
}
```

<span data-ttu-id="867f5-132">Die routeneinschränkungen, die in der folgenden Tabelle dargestellt sind für die Verwendung verfügbar.</span><span class="sxs-lookup"><span data-stu-id="867f5-132">The route constraints shown in the following table are available for use.</span></span> <span data-ttu-id="867f5-133">Die routeneinschränkungen, die mit der invarianten Kultur entsprechen, finden Sie in der Warnung unterhalb der Tabelle Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="867f5-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="867f5-134">Constraint</span><span class="sxs-lookup"><span data-stu-id="867f5-134">Constraint</span></span> | <span data-ttu-id="867f5-135">Beispiel</span><span class="sxs-lookup"><span data-stu-id="867f5-135">Example</span></span>           | <span data-ttu-id="867f5-136">Beispiele für Übereinstimmungen</span><span class="sxs-lookup"><span data-stu-id="867f5-136">Example Matches</span></span>                                                                  | <span data-ttu-id="867f5-137">Invariante</span><span class="sxs-lookup"><span data-stu-id="867f5-137">Invariant</span></span><br><span data-ttu-id="867f5-138">Kultur</span><span class="sxs-lookup"><span data-stu-id="867f5-138">culture</span></span><br><span data-ttu-id="867f5-139">Übereinstimmend</span><span class="sxs-lookup"><span data-stu-id="867f5-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="867f5-140">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="867f5-140">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="867f5-141">Nein</span><span class="sxs-lookup"><span data-stu-id="867f5-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="867f5-142">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="867f5-142">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="867f5-143">Ja</span><span class="sxs-lookup"><span data-stu-id="867f5-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="867f5-144">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="867f5-144">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="867f5-145">Ja</span><span class="sxs-lookup"><span data-stu-id="867f5-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="867f5-146">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="867f5-146">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="867f5-147">Ja</span><span class="sxs-lookup"><span data-stu-id="867f5-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="867f5-148">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="867f5-148">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="867f5-149">Ja</span><span class="sxs-lookup"><span data-stu-id="867f5-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="867f5-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="867f5-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="867f5-151">Nein</span><span class="sxs-lookup"><span data-stu-id="867f5-151">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="867f5-152">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="867f5-152">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="867f5-153">Ja</span><span class="sxs-lookup"><span data-stu-id="867f5-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="867f5-154">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="867f5-154">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="867f5-155">Ja</span><span class="sxs-lookup"><span data-stu-id="867f5-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="867f5-156">Für Routeneinschränkungen, mit denen die URL überprüft wird und die in den CLR-Typ umgewandelt werden (beispielsweise `int` oder `DateTime`), wird immer die invariante Kultur verwendet.</span><span class="sxs-lookup"><span data-stu-id="867f5-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="867f5-157">Diese Einschränkungen setzen voraus, dass die URL nicht lokalisierbar ist.</span><span class="sxs-lookup"><span data-stu-id="867f5-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="867f5-158">NavLink-Komponente</span><span class="sxs-lookup"><span data-stu-id="867f5-158">NavLink component</span></span>

<span data-ttu-id="867f5-159">Verwenden Sie eine Komponente NavLink anstelle von HTML-  **\<eine >** Elemente beim Erstellen von Navigationslinks.</span><span class="sxs-lookup"><span data-stu-id="867f5-159">Use a NavLink component in place of HTML **\<a>** elements when creating navigation links.</span></span> <span data-ttu-id="867f5-160">Eine NavLink-Komponente verhält sich wie ein  **\<eine >** -Element, mit der Ausnahme umgeschaltet ein `active` CSS-Klasse abhängig davon, ob die `href` mit der aktuelle URL übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="867f5-160">A NavLink component behaves like an **\<a>** element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="867f5-161">Die `active` Klasse hilft, einen Benutzer zu verstehen, welche Seite zur aktiven Seite für die Navigationslinks angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="867f5-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="867f5-162">Die NavMenu-Komponente in der [Beispiel-app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) erstellt eine [Bootstrap](https://getbootstrap.com/docs/) Navigationsleiste, die zeigt, wie NavLink Komponenten verwendet.</span><span class="sxs-lookup"><span data-stu-id="867f5-162">The NavMenu component in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) creates a [Bootstrap](https://getbootstrap.com/docs/) nav bar that demonstrates how to use NavLink components.</span></span> <span data-ttu-id="867f5-163">Das folgende Markup zeigt die ersten beiden NavLinks in die *Shared/NavMenu.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="867f5-163">The following markup shows the first two NavLinks in the *Shared/NavMenu.cshtml* file.</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?start=13&end=24&highlight=4-6,9-11)]

<span data-ttu-id="867f5-164">Es gibt zwei `NavLinkMatch` Optionen:</span><span class="sxs-lookup"><span data-stu-id="867f5-164">There are two `NavLinkMatch` options:</span></span>

* <span data-ttu-id="867f5-165">`NavLinkMatch.All` &ndash; Gibt an, dass die NavLink aktiv sein sollte, wenn es sich um die gesamte aktuelle URL übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="867f5-165">`NavLinkMatch.All` &ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* <span data-ttu-id="867f5-166">`NavLinkMatch.Prefix` &ndash; Gibt an, dass die NavLink aktiv sein sollte, wenn es sich um Präfix des aktuellen URLs übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="867f5-166">`NavLinkMatch.Prefix` &ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="867f5-167">Im vorherigen Beispiel ist die Startseite NavLink (`href=""`) entspricht allen URLs und erhält immer das `active` CSS-Klasse.</span><span class="sxs-lookup"><span data-stu-id="867f5-167">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="867f5-168">Die zweite NavLink erhält nur die `active` Klasse, wenn der Benutzer die Komponente BlazorRoute besucht (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="867f5-168">The second NavLink only receives the `active` class when the user visits the BlazorRoute component (`href="BlazorRoute"`).</span></span>
