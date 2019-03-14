---
title: Razor-Komponenten-JavaScript-interop
author: guardrex
description: Erfahren Sie, wie zum Aufrufen von JavaScript-Funktionen von .NET und .NET Methoden von JavaScript in Blazor und Razor-Komponenten-apps.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: 9f822fee8990b03ff15ffa9857a2ddd95328a6ce
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050367"
---
# <a name="razor-components-javascript-interop"></a><span data-ttu-id="84925-103">Razor-Komponenten-JavaScript-interop</span><span class="sxs-lookup"><span data-stu-id="84925-103">Razor Components JavaScript interop</span></span>

<span data-ttu-id="84925-104">Durch [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="84925-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="84925-105">Eine app für Razor-Komponenten kann JavaScript-Funktionen von .NET und .NET Aufrufen der Methoden von JavaScript-Code.</span><span class="sxs-lookup"><span data-stu-id="84925-105">A Razor Components app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="84925-106">JavaScript-Funktionen von .NET Methoden aufrufen</span><span class="sxs-lookup"><span data-stu-id="84925-106">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="84925-107">Es gibt Zeiten, in denen .NET Code erforderlich, um eine JavaScript-Funktion aufrufen.</span><span class="sxs-lookup"><span data-stu-id="84925-107">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="84925-108">Beispielsweise kann ein JavaScript-Aufruf Browserfunktionen oder Funktionen aus einer JavaScript-Bibliothek für die app verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="84925-108">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="84925-109">Verwenden Sie zum Aufrufen von .NET in JavaScript die `IJSRuntime` Abstraktion.</span><span class="sxs-lookup"><span data-stu-id="84925-109">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="84925-110">Die `InvokeAsync<T>` Methode `IJSRuntime` akzeptiert einen Bezeichner für die JavaScript-Funktion, die Sie zusammen mit der eine beliebige Anzahl von JSON-serialisierbare Argumenten aufrufen möchten.</span><span class="sxs-lookup"><span data-stu-id="84925-110">The `InvokeAsync<T>` method on `IJSRuntime` takes an identifier for the JavaScript function you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="84925-111">Der Funktionsbezeichner ist relativ zu den globalen Bereich (`window`).</span><span class="sxs-lookup"><span data-stu-id="84925-111">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="84925-112">Wenn Sie aufrufen möchten `window.someScope.someFunction`, der Bezeichner ist `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="84925-112">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="84925-113">Es ist nicht erforderlich, die die Funktion zu registrieren, bevor sie aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="84925-113">There's no need to register the function before it's called.</span></span> <span data-ttu-id="84925-114">Der Rückgabetyp `T` muss auch JSON serialisierbar sein.</span><span class="sxs-lookup"><span data-stu-id="84925-114">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="84925-115">Für serverseitige Komponenten von ASP.NET Core Razor-apps:</span><span class="sxs-lookup"><span data-stu-id="84925-115">For server-side ASP.NET Core Razor Components apps:</span></span>

* <span data-ttu-id="84925-116">Die serverseitige app werden mehrere benutzeranforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="84925-116">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="84925-117">Rufen Sie nicht `JSRuntime.Current` in einer Komponente zum Aufrufen von JavaScript-Funktionen.</span><span class="sxs-lookup"><span data-stu-id="84925-117">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="84925-118">Einfügen der `IJSRuntime` Abstraktion und verwenden das eingefügte Objekt, das JavaScript-interop-Aufrufe ausgeben.</span><span class="sxs-lookup"><span data-stu-id="84925-118">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>

<span data-ttu-id="84925-119">Das folgende Beispiel basiert auf [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), einen experimentellen JavaScript-basierten Decoder.</span><span class="sxs-lookup"><span data-stu-id="84925-119">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="84925-120">Im Beispiel wird veranschaulicht, wie zum Aufrufen einer JavaScript-Funktion aus einem C# Methode.</span><span class="sxs-lookup"><span data-stu-id="84925-120">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="84925-121">Die JavaScript-Funktion akzeptiert ein Bytearray aus einer C# Methode, die das Array decodiert und gibt den Text an die Komponente für die Anzeige zurück.</span><span class="sxs-lookup"><span data-stu-id="84925-121">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="84925-122">In der `<head>` Element *wwwroot/Index.HTML*, geben Sie eine Funktion, verwendet `TextDecoder` zu einem übergebenen Array zu decodierende:</span><span class="sxs-lookup"><span data-stu-id="84925-122">Inside the `<head>` element of *wwwroot/index.html*, provide a function that uses `TextDecoder` to decode a passed array:</span></span>

```html
<script>
  window.ConvertArray = (win1251Array) => {
    var win1251decoder = new TextDecoder('windows-1251');
    var bytes = new Uint8Array(win1251Array);
    var decodedArray = win1251decoder.decode(bytes);
    console.log(decodedArray);
    return decodedArray;
  };
</script>
```

<span data-ttu-id="84925-123">JavaScript-Code, z. B. den Code im vorherigen Beispiel kann auch durch ein JavaScript-Datei mit einem Verweis auf die Skriptdatei im geladen werden die *wwwroot/Index.HTML* Datei.</span><span class="sxs-lookup"><span data-stu-id="84925-123">JavaScript code, such as the code shown in the preceding example, can also be loaded by a JavaScript file with a reference to the script file in the *wwwroot/index.html* file.</span></span>

<span data-ttu-id="84925-124">Die folgende Komponente:</span><span class="sxs-lookup"><span data-stu-id="84925-124">The following component:</span></span>

* <span data-ttu-id="84925-125">Ruft die `ConvertArray` JavaScript-Funktion verwendet `JsRuntime` beim Klicken auf die Schaltfläche eine Komponente (**Array konvertieren**) ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="84925-125">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="84925-126">Nachdem die JavaScript-Funktion aufgerufen wurde, wird das übergebene Array in eine Zeichenfolge konvertiert.</span><span class="sxs-lookup"><span data-stu-id="84925-126">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="84925-127">Die Zeichenfolge wird an die Komponente für die Anzeige zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="84925-127">The string is returned to the component for display.</span></span>

```cshtml
@page "/"
@using Microsoft.JSInterop;
@inject IJSRuntime JsRuntime;

<h1>Call JavaScript Function Example</h1>

<button type="button" class="btn btn-primary" onclick="@ConvertArray">
    Convert Array
</button>

<p class="mt-2" style="font-size:1.6em">
    <span class="badge badge-success">
        @ConvertedText
    </span>
</p>

@functions {
    // Quote (c)2005 Universal Pictures: Serenity
    // https://www.uphe.com/movies/serenity
    // David Krumholtz on IMDB: https://www.imdb.com/name/nm0472710/

    private MarkupString ConvertedText =
        new MarkupString("Select the <b>Convert Array</b> button.");

    private uint[] QuoteArray = new uint[]
        {
            60, 101, 109, 62, 67, 97, 110, 39, 116, 32, 115, 116, 111, 112, 32,
            116, 104, 101, 32, 115, 105, 103, 110, 97, 108, 44, 32, 77, 97,
            108, 46, 60, 47, 101, 109, 62, 32, 45, 32, 77, 114, 46, 32, 85, 110,
            105, 118, 101, 114, 115, 101, 10, 10,
        };

    async void ConvertArray()
    {
        var text =
            await JsRuntime.InvokeAsync<string>("ConvertArray", QuoteArray);

        ConvertedText = new MarkupString(text);

        StateHasChanged();
    }
}
```

<span data-ttu-id="84925-128">Für die clientseitige Blazor apps die `IJSRuntime` Abstraktion ist über `JSRuntime.Current`, die bezieht sich auf die aktuelle Anforderung des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="84925-128">For client-side Blazor apps, the `IJSRuntime` abstraction is accessible from `JSRuntime.Current`, which refers to the current user's request.</span></span> <span data-ttu-id="84925-129">Da nur ein einziger Benutzer einer clientseitigen Blazor-App wird mithilfe von `JSRuntime.Current` aufzurufenden JavaScript Funktion arbeitet normal.</span><span class="sxs-lookup"><span data-stu-id="84925-129">Because there's only a single user of a client-side Blazor app, using `JSRuntime.Current` to invoke a JavaScript function works normally.</span></span> <span data-ttu-id="84925-130">Verwenden Sie nur `JSRuntime.Current` in der clientseitigen Blazor-apps.</span><span class="sxs-lookup"><span data-stu-id="84925-130">Only use `JSRuntime.Current` in client-side Blazor apps.</span></span>

<span data-ttu-id="84925-131">In der Client-Side-Beispiel-app, die in diesem Thema begleitet, werden zwei JavaScript-Funktionen für die clientseitige Anwendung die Interaktion mit dem DOM zum Empfangen von Benutzereingaben und Anzeigen von eine Begrüßungsnachricht an verfügbar:</span><span class="sxs-lookup"><span data-stu-id="84925-131">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="84925-132">`showPrompt` &ndash; Erzeugt eine Aufforderung zum Akzeptieren der Benutzereingabe (Name des Benutzers), und gibt den Namen an den Aufrufer zurück.</span><span class="sxs-lookup"><span data-stu-id="84925-132">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="84925-133">`displayWelcome` &ndash; Weist eine Begrüßungsnachricht an vom Aufrufer ein DOM-Objekt mit einer `id` von `welcome`.</span><span class="sxs-lookup"><span data-stu-id="84925-133">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="84925-134">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="84925-134">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="84925-135">Ort der `<script>` Tag, das in der JavaScript-Datei verweist auf die *wwwroot/Index.HTML* Datei:</span><span class="sxs-lookup"><span data-stu-id="84925-135">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file:</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=16)]

<span data-ttu-id="84925-136">Ein Skripttag in einer Komponentendatei nicht platziert werden, da das Skripttag dynamisch aktualisiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="84925-136">Don't place a script tag in a component file because the script tag can't be updated dynamically.</span></span>

<span data-ttu-id="84925-137">.NET Methoden das Zusammenarbeiten mit der JavaScript-Funktionen, durch den Aufruf `InvokeAsync<T>` Methode `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="84925-137">.NET methods interop with the JavaScript functions by calling `InvokeAsync<T>` method on `IJSRuntime`.</span></span>

<span data-ttu-id="84925-138">Die Beispiel-app verwendet ein Paar von C# Methoden `Prompt` und `Display`, zum Aufrufen der `showPrompt` und `displayWelcome` JavaScript-Funktionen:</span><span class="sxs-lookup"><span data-stu-id="84925-138">The sample app uses a pair of C# methods, `Prompt` and `Display`, to invoke the `showPrompt` and `displayWelcome` JavaScript functions:</span></span>

<span data-ttu-id="84925-139">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="84925-139">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=6-8,14-16)]

<span data-ttu-id="84925-140">Die `IJSRuntime` Abstraktion ist asynchron, um die serverseitigen Szenarios zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="84925-140">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="84925-141">Wenn die app ausgeführt, die clientseitige wird und eine JavaScript-Funktion synchron aufgerufen werden soll, Typumwandlung `IJSInProcessRuntime` , und rufen Sie `Invoke<T>` stattdessen.</span><span class="sxs-lookup"><span data-stu-id="84925-141">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="84925-142">Es wird empfohlen, dass die meisten JavaScript-Interop-Bibliotheken mithilfe die asynchronen APIs, um sicherzustellen, dass die Bibliotheken in allen Szenarien, die clientseitige oder serverseitige verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="84925-142">We recommend that most JavaScript interop libraries use the async APIs to ensure the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="84925-143">Die Beispiel-app enthält eine Komponente, um die JS-Interop demonstrieren.</span><span class="sxs-lookup"><span data-stu-id="84925-143">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="84925-144">Die Komponente:</span><span class="sxs-lookup"><span data-stu-id="84925-144">The component:</span></span>

* <span data-ttu-id="84925-145">Erhält Benutzereingaben über eine Node.js-Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="84925-145">Receives user input via a JS prompt.</span></span>
* <span data-ttu-id="84925-146">Der Text wird an die Komponente für die Verarbeitung zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="84925-146">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="84925-147">Ruft eine zweite JS-Funktion, die mit dem DOM zum Anzeigen einer Begrüßungsnachricht interagiert.</span><span class="sxs-lookup"><span data-stu-id="84925-147">Calls a second JS function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="84925-148">*Pages/JSInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="84925-148">*Pages/JSInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=1&end=21&highlight=2-3,9-11,13,16-20)]

1. <span data-ttu-id="84925-149">Wenn `TriggerJsPrompt` durch Auswahl der Komponente ausgeführt wird **Trigger JavaScript Eingabeaufforderung** Schaltfläche der `ExampleJsInterop.Prompt` -Methode in der C# Code aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="84925-149">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the `ExampleJsInterop.Prompt` method in C# code is called.</span></span>
1. <span data-ttu-id="84925-150">Die `Prompt` Methode ausgeführt wird, den JavaScript-Code `showPrompt` Funktion aus der *wwwroot/exampleJsInterop.js* Datei.</span><span class="sxs-lookup"><span data-stu-id="84925-150">The `Prompt` method executes the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file.</span></span>
1. <span data-ttu-id="84925-151">Die `showPrompt` -Funktion akzeptiert Benutzereingaben (Name des Benutzers), die HTML-codiert und zurückgegeben, die `Prompt` Methode und schließlich an die Komponente zurück.</span><span class="sxs-lookup"><span data-stu-id="84925-151">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the `Prompt` method and ultimately back to the component.</span></span> <span data-ttu-id="84925-152">Die Komponente speichert den Namen des Benutzers in einer lokalen Variablen, `name`.</span><span class="sxs-lookup"><span data-stu-id="84925-152">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="84925-153">Die Zeichenfolge, die in gespeicherten `name` wird in eine Begrüßungsnachricht an, die an eine zweite integriert C# Methode `ExampleJsInterop.Display`.</span><span class="sxs-lookup"><span data-stu-id="84925-153">The string stored in `name` is incorporated into a welcome message, which is passed to a second C# method, `ExampleJsInterop.Display`.</span></span>
1. <span data-ttu-id="84925-154">`Display` Ruft eine JavaScript-Funktion `displayWelcome`, die Willkommensnachricht in der Überschrift-Tag gerendert.</span><span class="sxs-lookup"><span data-stu-id="84925-154">`Display` calls a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="capture-references-to-elements"></a><span data-ttu-id="84925-155">Erfassen Sie Verweise auf Elemente</span><span class="sxs-lookup"><span data-stu-id="84925-155">Capture references to elements</span></span>

<span data-ttu-id="84925-156">Einige [JavaScript-Interop](xref:razor-components/javascript-interop) Szenarien müssen Verweise auf HTML-Elemente.</span><span class="sxs-lookup"><span data-stu-id="84925-156">Some [JavaScript interop](xref:razor-components/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="84925-157">Beispielsweise kann eine UI-Bibliothek ein Elementverweises benötigen, für die Initialisierung, oder müssen Sie möglicherweise Befehl-ähnliche APIs aufrufen auf ein Element, z. B. `focus` oder `play`.</span><span class="sxs-lookup"><span data-stu-id="84925-157">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="84925-158">Sie können Verweise auf HTML-Elemente in einer Komponente erfassen, durch das Hinzufügen eine `ref` -Attribut auf das HTML-Element und das anschließende definieren ein Feld vom Typ `ElementRef` , deren Name entspricht dem Wert der `ref` Attribut.</span><span class="sxs-lookup"><span data-stu-id="84925-158">You can capture references to HTML elements in a component by adding a `ref` attribute to the HTML element and then defining a field of type `ElementRef` whose name matches the value of the `ref` attribute.</span></span>

<span data-ttu-id="84925-159">Das folgende Beispiel zeigt einen Verweis auf das Eingabeelement Benutzernamen erfassen:</span><span class="sxs-lookup"><span data-stu-id="84925-159">The following example shows capturing a reference to the username input element:</span></span>

```csharp
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> <span data-ttu-id="84925-160">Führen Sie **nicht** erfasste Elementverweise als eine Möglichkeit zum Auffüllen des DOM verwenden</span><span class="sxs-lookup"><span data-stu-id="84925-160">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="84925-161">Auf diese Weise kann die deklarative Renderingmodell beeinträchtigen.</span><span class="sxs-lookup"><span data-stu-id="84925-161">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="84925-162">Als .NET Code geht, eine `ElementRef` ist ein nicht transparentes Handle.</span><span class="sxs-lookup"><span data-stu-id="84925-162">As far as .NET code is concerned, an `ElementRef` is an opaque handle.</span></span> <span data-ttu-id="84925-163">Die *nur* für sie Verwendungsmöglichkeiten ist über über JavaScript-Interop an JavaScript-Code übergeben.</span><span class="sxs-lookup"><span data-stu-id="84925-163">The *only* thing you can do with it is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="84925-164">Wenn Sie dies tun, empfängt der JavaScript-Seite-Code ein `HTMLElement` -Instanz, die sie mit normalen DOM-APIs verwenden kann.</span><span class="sxs-lookup"><span data-stu-id="84925-164">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="84925-165">Der folgende Code definiert z. B. eine .NET-Erweiterungsmethode, die es ermöglicht, den Fokus auf ein Element festlegen:</span><span class="sxs-lookup"><span data-stu-id="84925-165">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="84925-166">*myLib.js*:</span><span class="sxs-lookup"><span data-stu-id="84925-166">*mylib.js*:</span></span>

```javascript
window.myLib = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="84925-167">*ElementRefExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="84925-167">*ElementRefExtensions.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Blazor;
using Microsoft.JSInterop;
using System.Threading.Tasks;

namespace MyLib
{
    public static class MyLibElementRefExtensions
    {
        public static Task Focus(this ElementRef elementRef)
        {
            return JSRuntime.Current.InvokeAsync<object>("myLib.focusElement", elementRef);
        }
    }
}
```

<span data-ttu-id="84925-168">Jetzt können Sie die Eingaben in einer Ihrer Komponenten konzentrieren:</span><span class="sxs-lookup"><span data-stu-id="84925-168">Now you can focus inputs in any of your components:</span></span>

```cshtml
@using MyLib

<input ref="username" />
<button onclick="@SetFocus">Set focus</button>

@functions {
    ElementRef username;

    void SetFocus()
    {
        username.Focus();
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="84925-169">Die `username` Variable wird nur aufgefüllt, nachdem die Komponente rendert und die Ausgabe enthält die `<input>` Element.</span><span class="sxs-lookup"><span data-stu-id="84925-169">The `username` variable is only populated after the component renders and its output includes the `<input>` element.</span></span> <span data-ttu-id="84925-170">Wenn Sie versuchen, eine nicht aufgefüllte übergeben `ElementRef` für JavaScript-Code, der JavaScript-Code empfängt `null`.</span><span class="sxs-lookup"><span data-stu-id="84925-170">If you try to pass an unpopulated `ElementRef` to JavaScript code, the JavaScript code receives `null`.</span></span> <span data-ttu-id="84925-171">Elementverweise zu ändern, nachdem die Komponente verwenden (um den Anfangsfokus für ein Element festgelegt) Rendern abgeschlossen ist die `OnAfterRenderAsync` oder `OnAfterRender` [Komponente Lebenszyklusmethoden](xref:razor-components/components#lifecycle-methods).</span><span class="sxs-lookup"><span data-stu-id="84925-171">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:razor-components/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="84925-172">Aufrufen von .NET Methoden von JavaScript-Funktionen</span><span class="sxs-lookup"><span data-stu-id="84925-172">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="84925-173">Statischen Methodenaufruf in .NET</span><span class="sxs-lookup"><span data-stu-id="84925-173">Static .NET method call</span></span>

<span data-ttu-id="84925-174">Um eine statische .NET-Methode aus JavaScript aufrufen zu können, verwenden Sie die `DotNet.invokeMethod` oder `DotNet.invokeMethodAsync` Funktionen.</span><span class="sxs-lookup"><span data-stu-id="84925-174">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="84925-175">Übergeben Sie den Bezeichner der statischen Methode, die Sie aufrufen, den Namen der Assembly mit der Funktion und alle Argumente möchten.</span><span class="sxs-lookup"><span data-stu-id="84925-175">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="84925-176">In diesem Fall wird die asynchrone Version bevorzugt, um serverseitige Szenarien zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="84925-176">Again, the async version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="84925-177">Um aus JavaScript aufgerufen werden, die .NET-Methode muss öffentliche, statische und durch `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="84925-177">To be invokable from JavaScript, the .NET method must be public, static, and decorated with `[JSInvokable]`.</span></span> <span data-ttu-id="84925-178">Der Methodenbezeichner wird standardmäßig der Name der Methode, aber Sie können angeben, einen anderen Bezeichner mit dem `JSInvokableAttribute` Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="84925-178">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="84925-179">Offene generische Methoden aufrufen wird derzeit nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="84925-179">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="84925-180">Die beispielanwendung enthält einen C# Methode, um ein Array von zurückzugeben `int`s.</span><span class="sxs-lookup"><span data-stu-id="84925-180">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="84925-181">Die Methode wird ergänzt, mit der `JSInvokable` Attribut.</span><span class="sxs-lookup"><span data-stu-id="84925-181">The method is decorated with the `JSInvokable` attribute.</span></span>

<span data-ttu-id="84925-182">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="84925-182">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=47&end=58&highlight=7-11)]

<span data-ttu-id="84925-183">JavaScript, die für den Client bereitgestellt Ruft die C# .NET-Methode übergeben.</span><span class="sxs-lookup"><span data-stu-id="84925-183">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="84925-184">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="84925-184">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-12)]

<span data-ttu-id="84925-185">Wenn die **Trigger .NET statische Methode ReturnArrayAsync** ausgewählt ist, überprüfen Sie die Konsolenausgabe in im Browser auf die Web Developer-Tools:</span><span class="sxs-lookup"><span data-stu-id="84925-185">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="84925-186">Der vierte Arraywert wird in das Array abgelegt (`data.push(4);`) vom `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="84925-186">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="84925-187">Instanz-Methodenaufruf</span><span class="sxs-lookup"><span data-stu-id="84925-187">Instance method call</span></span>

<span data-ttu-id="84925-188">Sie können auch Instanzmethoden für .NET aus JavaScript aufrufen.</span><span class="sxs-lookup"><span data-stu-id="84925-188">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="84925-189">Um eine Instanzmethode für .NET aus JavaScript aufrufen zu können, zuerst übergeben Sie die .NET Instanz in JavaScript durch wrapping in eine `DotNetObjectRef` Instanz.</span><span class="sxs-lookup"><span data-stu-id="84925-189">To invoke a .NET instance method from JavaScript, first pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectRef` instance.</span></span> <span data-ttu-id="84925-190">Die .NET-Instanz als Verweis an JavaScript übergeben wird, und Sie können .NET von Instanzmethoden für die Instanz mit Aufrufen der `invokeMethod` oder `invokeMethodAsync` Funktionen.</span><span class="sxs-lookup"><span data-stu-id="84925-190">The .NET instance is passed by reference to JavaScript, and you can invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="84925-191">Die Instanz der .NET kann auch als Argument übergeben werden, wenn andere .NET Methoden von JavaScript aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="84925-191">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="84925-192">Die Beispiel-app protokolliert Meldungen in die Client-Side-Konsole.</span><span class="sxs-lookup"><span data-stu-id="84925-192">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="84925-193">Überprüfen Sie für den folgenden Beispielen gezeigt, die von der Beispiel-app im Browser auf die Konsolenausgabe in das browserentwicklertools.</span><span class="sxs-lookup"><span data-stu-id="84925-193">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="84925-194">Wenn die **Trigger .NET Instanzmethode HelloHelper.SayHello** ausgewählt ist, `ExampleJsInterop.CallHelloHelperSayHello` aufgerufen wird, und übergeben Sie einen Namen, `Blazor`, an die Methode.</span><span class="sxs-lookup"><span data-stu-id="84925-194">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="84925-195">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="84925-195">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=60&end=69&highlight=8)]

<span data-ttu-id="84925-196">`CallHelloHelperSayHello` Ruft die JavaScript-Funktion `sayHello` durch eine neue Instanz der `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="84925-196">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="84925-197">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="84925-197">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=19-25)]

<span data-ttu-id="84925-198">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="84925-198">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-17)]

<span data-ttu-id="84925-199">Der Name wird zum übergeben `HelloHelper`Konstruktor, der legt der `HelloHelper.Name` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="84925-199">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="84925-200">Wenn die JavaScript-Funktion `sayHello` ausgeführt wird, `HelloHelper.SayHello` gibt die `Hello, {Name}!` -Nachricht, die von der JavaScript-Funktion in die Konsole geschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="84925-200">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="84925-201">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="84925-201">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="84925-202">Ausgabe im im Browser auf die Web-Entwicklertools-Konsole:</span><span class="sxs-lookup"><span data-stu-id="84925-202">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a><span data-ttu-id="84925-203">Freigeben von Interop-Code in einer Klassenbibliothek für die Razor-Komponente</span><span class="sxs-lookup"><span data-stu-id="84925-203">Share interop code in a Razor Component class library</span></span>

<span data-ttu-id="84925-204">Interop-JavaScript-Code in einer Klassenbibliothek für die Razor-Komponente enthalten sein kann (`dotnet new blazorlib`), können Sie den Code in einem NuGet-Paket verwenden.</span><span class="sxs-lookup"><span data-stu-id="84925-204">JavaScript interop code can be included in a Razor Component class library (`dotnet new blazorlib`), which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="84925-205">Die Komponente der Razor-Klassenbibliothek behandelt die Einbettung von JavaScript-Ressourcen in der erstellten Assembly.</span><span class="sxs-lookup"><span data-stu-id="84925-205">The Razor Component class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="84925-206">Die JavaScript-Dateien befinden sich der *"Wwwroot"* Ordner, und das Tool übernimmt die Einbettung der Ressourcen aus, wenn die Bibliothek erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="84925-206">The JavaScript files are placed in the *wwwroot* folder, and the tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="84925-207">Das erstellte NuGet-Paket wird in der Projektdatei der app verwiesen werden, genau wie normale NuGet-Pakets verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="84925-207">The built NuGet package is referenced in the project file of the app just as any normal NuGet package is referenced.</span></span> <span data-ttu-id="84925-208">Nachdem die app wiederhergestellt wurde, kann app-Code in JavaScript aufrufen, als wäre er C#.</span><span class="sxs-lookup"><span data-stu-id="84925-208">After the app has been restored, app code can call into JavaScript as if it were C#.</span></span>
