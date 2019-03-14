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
# <a name="razor-components-routing"></a>Razor-Komponenten, die routing

Von [Luke Latham](https://github.com/guardrex)

Erfahren Sie, wie Sie Anforderungen in apps und über die NavLink-Komponente weiterleiten.

[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample)). Informieren Sie sich in dem Thema [Erste Schritte](xref:razor-components/get-started) über die Voraussetzungen.

## <a name="route-templates"></a>Routenvorlagen

Die `<Router>` Komponente ermöglicht die Weiterleitung, und eine routenvorlage wird bereitgestellt, um jede Komponente zugegriffen werden kann. Die `<Router>` Komponente angezeigt wird, der *App.cshtml* Datei:

```cshtml
<Router AppAssembly=typeof(Program).Assembly />
```

Wenn eine  *\*cshtml* -Datei mit ein `@page` Richtlinie wird kompiliert, wird die generierte Klasse wird eine [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) die routenvorlage angeben. Zur Laufzeit sucht der Router Komponentenklassen mit einem `RouteAttribute` und rendert, welche Komponente eine routenvorlage verfügt, die mit der angeforderten URL übereinstimmt.

Mehrere routenvorlagen können auf eine Komponente angewendet werden. In der [Beispiel-app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), die folgende Komponente antwortet auf Anfragen für `/BlazorRoute` und `/DifferentBlazorRoute`.

*Pages/BlazorRoute.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

`<Router>` unterstützt das Festlegen einer fallback Komponente für das Rendering, wenn eine angeforderte Route nicht aufgelöst werden. Aktivieren Sie dieses Szenario durch Festlegen der `FallbackComponent` Parameter, um den Typ der fallback Komponentenklasse.

Im folgenden Beispiel wird eine Komponente, die in definierten *Pages/MyFallbackRazorComponent.cshtml* als fallback für die Komponente eine `<Router>`:

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> Um Routen ordnungsgemäß generieren zu können, muss die app enthalten eine `<base>` -Tag in die *wwwroot/Index.HTML* -Datei mit den app-Basispfad angegeben, der `href` Attribut (`<base href="/" />`). Weitere Informationen finden Sie unter [hosten und bereitstellen: App-Basispfad](xref:host-and-deploy/razor-components/index#app-base-path).

## <a name="route-parameters"></a>Routenparameter

Der Router verwendet Routenparameter zum Auffüllen der entsprechenden Komponentenparameter mit dem gleichen Namen (Groß-/Kleinschreibung).

*Pages/RouteParameter.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=8)]

Optionale Parameter werden nicht unterstützt, daher werden zwei `@page` Anweisungen sind im obigen Beispiel angewendet. Die erste ermöglicht die Navigation zu der Komponente ohne Parameter. Die zweite `@page` Direktive nimmt die `{text}` Routenparameter und weist den Wert der `Text` Eigenschaft.

## <a name="route-constraints"></a>Einschränkungen

Eine routeneinschränkung erzwingt typübereinstimmung auf einem routensegment zu einer Komponente.

Im folgenden Beispiel entspricht die Route für die Benutzer-Komponente nur, wenn:

* Ein `Id` routensegment auf die Anforderungs-URL vorhanden ist.
* Die `Id` -Segment ist eine ganze Zahl (`int`).

```cshtml
@page "/Users/{Id:int}"

<h1>The user Id is @Id!</h1>

@functions {
    [Parameter]
    private int Id { get; set; }
}
```

Die routeneinschränkungen, die in der folgenden Tabelle dargestellt sind für die Verwendung verfügbar. Die routeneinschränkungen, die mit der invarianten Kultur entsprechen, finden Sie in der Warnung unterhalb der Tabelle Weitere Informationen.

| Constraint | Beispiel           | Beispiele für Übereinstimmungen                                                                  | Invariante<br>Kultur<br>Übereinstimmend |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | Nein                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Ja                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Ja                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Ja                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Ja                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Nein                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Ja                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Ja                              |

> [!WARNING]
> Für Routeneinschränkungen, mit denen die URL überprüft wird und die in den CLR-Typ umgewandelt werden (beispielsweise `int` oder `DateTime`), wird immer die invariante Kultur verwendet. Diese Einschränkungen setzen voraus, dass die URL nicht lokalisierbar ist.

## <a name="navlink-component"></a>NavLink-Komponente

Verwenden Sie eine Komponente NavLink anstelle von HTML-  **\<eine >** Elemente beim Erstellen von Navigationslinks. Eine NavLink-Komponente verhält sich wie ein  **\<eine >** -Element, mit der Ausnahme umgeschaltet ein `active` CSS-Klasse abhängig davon, ob die `href` mit der aktuelle URL übereinstimmt. Die `active` Klasse hilft, einen Benutzer zu verstehen, welche Seite zur aktiven Seite für die Navigationslinks angezeigt wird.

Die NavMenu-Komponente in der [Beispiel-app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) erstellt eine [Bootstrap](https://getbootstrap.com/docs/) Navigationsleiste, die zeigt, wie NavLink Komponenten verwendet. Das folgende Markup zeigt die ersten beiden NavLinks in die *Shared/NavMenu.cshtml* Datei.

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?start=13&end=24&highlight=4-6,9-11)]

Es gibt zwei `NavLinkMatch` Optionen:

* `NavLinkMatch.All` &ndash; Gibt an, dass die NavLink aktiv sein sollte, wenn es sich um die gesamte aktuelle URL übereinstimmt.
* `NavLinkMatch.Prefix` &ndash; Gibt an, dass die NavLink aktiv sein sollte, wenn es sich um Präfix des aktuellen URLs übereinstimmt.

Im vorherigen Beispiel ist die Startseite NavLink (`href=""`) entspricht allen URLs und erhält immer das `active` CSS-Klasse. Die zweite NavLink erhält nur die `active` Klasse, wenn der Benutzer die Komponente BlazorRoute besucht (`href="BlazorRoute"`).
