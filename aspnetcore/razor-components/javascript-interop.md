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
# <a name="razor-components-javascript-interop"></a>Razor-Komponenten-JavaScript-interop

Durch [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), und [Luke Latham](https://github.com/guardrex)

Eine app für Razor-Komponenten kann JavaScript-Funktionen von .NET und .NET Aufrufen der Methoden von JavaScript-Code.

## <a name="invoke-javascript-functions-from-net-methods"></a>JavaScript-Funktionen von .NET Methoden aufrufen

Es gibt Zeiten, in denen .NET Code erforderlich, um eine JavaScript-Funktion aufrufen. Beispielsweise kann ein JavaScript-Aufruf Browserfunktionen oder Funktionen aus einer JavaScript-Bibliothek für die app verfügbar machen.

Verwenden Sie zum Aufrufen von .NET in JavaScript die `IJSRuntime` Abstraktion. Die `InvokeAsync<T>` Methode `IJSRuntime` akzeptiert einen Bezeichner für die JavaScript-Funktion, die Sie zusammen mit der eine beliebige Anzahl von JSON-serialisierbare Argumenten aufrufen möchten. Der Funktionsbezeichner ist relativ zu den globalen Bereich (`window`). Wenn Sie aufrufen möchten `window.someScope.someFunction`, der Bezeichner ist `someScope.someFunction`. Es ist nicht erforderlich, die die Funktion zu registrieren, bevor sie aufgerufen wird. Der Rückgabetyp `T` muss auch JSON serialisierbar sein.

Für serverseitige Komponenten von ASP.NET Core Razor-apps:

* Die serverseitige app werden mehrere benutzeranforderungen verarbeitet. Rufen Sie nicht `JSRuntime.Current` in einer Komponente zum Aufrufen von JavaScript-Funktionen.
* Einfügen der `IJSRuntime` Abstraktion und verwenden das eingefügte Objekt, das JavaScript-interop-Aufrufe ausgeben.

Das folgende Beispiel basiert auf [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), einen experimentellen JavaScript-basierten Decoder. Im Beispiel wird veranschaulicht, wie zum Aufrufen einer JavaScript-Funktion aus einem C# Methode. Die JavaScript-Funktion akzeptiert ein Bytearray aus einer C# Methode, die das Array decodiert und gibt den Text an die Komponente für die Anzeige zurück.

In der `<head>` Element *wwwroot/Index.HTML*, geben Sie eine Funktion, verwendet `TextDecoder` zu einem übergebenen Array zu decodierende:

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

JavaScript-Code, z. B. den Code im vorherigen Beispiel kann auch durch ein JavaScript-Datei mit einem Verweis auf die Skriptdatei im geladen werden die *wwwroot/Index.HTML* Datei.

Die folgende Komponente:

* Ruft die `ConvertArray` JavaScript-Funktion verwendet `JsRuntime` beim Klicken auf die Schaltfläche eine Komponente (**Array konvertieren**) ausgewählt ist.
* Nachdem die JavaScript-Funktion aufgerufen wurde, wird das übergebene Array in eine Zeichenfolge konvertiert. Die Zeichenfolge wird an die Komponente für die Anzeige zurückgegeben.

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

Für die clientseitige Blazor apps die `IJSRuntime` Abstraktion ist über `JSRuntime.Current`, die bezieht sich auf die aktuelle Anforderung des Benutzers. Da nur ein einziger Benutzer einer clientseitigen Blazor-App wird mithilfe von `JSRuntime.Current` aufzurufenden JavaScript Funktion arbeitet normal. Verwenden Sie nur `JSRuntime.Current` in der clientseitigen Blazor-apps.

In der Client-Side-Beispiel-app, die in diesem Thema begleitet, werden zwei JavaScript-Funktionen für die clientseitige Anwendung die Interaktion mit dem DOM zum Empfangen von Benutzereingaben und Anzeigen von eine Begrüßungsnachricht an verfügbar:

* `showPrompt` &ndash; Erzeugt eine Aufforderung zum Akzeptieren der Benutzereingabe (Name des Benutzers), und gibt den Namen an den Aufrufer zurück.
* `displayWelcome` &ndash; Weist eine Begrüßungsnachricht an vom Aufrufer ein DOM-Objekt mit einer `id` von `welcome`.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Ort der `<script>` Tag, das in der JavaScript-Datei verweist auf die *wwwroot/Index.HTML* Datei:

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=16)]

Ein Skripttag in einer Komponentendatei nicht platziert werden, da das Skripttag dynamisch aktualisiert werden kann.

.NET Methoden das Zusammenarbeiten mit der JavaScript-Funktionen, durch den Aufruf `InvokeAsync<T>` Methode `IJSRuntime`.

Die Beispiel-app verwendet ein Paar von C# Methoden `Prompt` und `Display`, zum Aufrufen der `showPrompt` und `displayWelcome` JavaScript-Funktionen:

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=6-8,14-16)]

Die `IJSRuntime` Abstraktion ist asynchron, um die serverseitigen Szenarios zu ermöglichen. Wenn die app ausgeführt, die clientseitige wird und eine JavaScript-Funktion synchron aufgerufen werden soll, Typumwandlung `IJSInProcessRuntime` , und rufen Sie `Invoke<T>` stattdessen. Es wird empfohlen, dass die meisten JavaScript-Interop-Bibliotheken mithilfe die asynchronen APIs, um sicherzustellen, dass die Bibliotheken in allen Szenarien, die clientseitige oder serverseitige verfügbar sind.

Die Beispiel-app enthält eine Komponente, um die JS-Interop demonstrieren. Die Komponente:

* Erhält Benutzereingaben über eine Node.js-Eingabeaufforderung.
* Der Text wird an die Komponente für die Verarbeitung zurückgegeben.
* Ruft eine zweite JS-Funktion, die mit dem DOM zum Anzeigen einer Begrüßungsnachricht interagiert.

*Pages/JSInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=1&end=21&highlight=2-3,9-11,13,16-20)]

1. Wenn `TriggerJsPrompt` durch Auswahl der Komponente ausgeführt wird **Trigger JavaScript Eingabeaufforderung** Schaltfläche der `ExampleJsInterop.Prompt` -Methode in der C# Code aufgerufen wird.
1. Die `Prompt` Methode ausgeführt wird, den JavaScript-Code `showPrompt` Funktion aus der *wwwroot/exampleJsInterop.js* Datei.
1. Die `showPrompt` -Funktion akzeptiert Benutzereingaben (Name des Benutzers), die HTML-codiert und zurückgegeben, die `Prompt` Methode und schließlich an die Komponente zurück. Die Komponente speichert den Namen des Benutzers in einer lokalen Variablen, `name`.
1. Die Zeichenfolge, die in gespeicherten `name` wird in eine Begrüßungsnachricht an, die an eine zweite integriert C# Methode `ExampleJsInterop.Display`.
1. `Display` Ruft eine JavaScript-Funktion `displayWelcome`, die Willkommensnachricht in der Überschrift-Tag gerendert.

## <a name="capture-references-to-elements"></a>Erfassen Sie Verweise auf Elemente

Einige [JavaScript-Interop](xref:razor-components/javascript-interop) Szenarien müssen Verweise auf HTML-Elemente. Beispielsweise kann eine UI-Bibliothek ein Elementverweises benötigen, für die Initialisierung, oder müssen Sie möglicherweise Befehl-ähnliche APIs aufrufen auf ein Element, z. B. `focus` oder `play`.

Sie können Verweise auf HTML-Elemente in einer Komponente erfassen, durch das Hinzufügen eine `ref` -Attribut auf das HTML-Element und das anschließende definieren ein Feld vom Typ `ElementRef` , deren Name entspricht dem Wert der `ref` Attribut.

Das folgende Beispiel zeigt einen Verweis auf das Eingabeelement Benutzernamen erfassen:

```csharp
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> Führen Sie **nicht** erfasste Elementverweise als eine Möglichkeit zum Auffüllen des DOM verwenden Auf diese Weise kann die deklarative Renderingmodell beeinträchtigen.

Als .NET Code geht, eine `ElementRef` ist ein nicht transparentes Handle. Die *nur* für sie Verwendungsmöglichkeiten ist über über JavaScript-Interop an JavaScript-Code übergeben. Wenn Sie dies tun, empfängt der JavaScript-Seite-Code ein `HTMLElement` -Instanz, die sie mit normalen DOM-APIs verwenden kann.

Der folgende Code definiert z. B. eine .NET-Erweiterungsmethode, die es ermöglicht, den Fokus auf ein Element festlegen:

*myLib.js*:

```javascript
window.myLib = {
  focusElement : function (element) {
    element.focus();
  }
}
```

*ElementRefExtensions.cs*:

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

Jetzt können Sie die Eingaben in einer Ihrer Komponenten konzentrieren:

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
> Die `username` Variable wird nur aufgefüllt, nachdem die Komponente rendert und die Ausgabe enthält die `<input>` Element. Wenn Sie versuchen, eine nicht aufgefüllte übergeben `ElementRef` für JavaScript-Code, der JavaScript-Code empfängt `null`. Elementverweise zu ändern, nachdem die Komponente verwenden (um den Anfangsfokus für ein Element festgelegt) Rendern abgeschlossen ist die `OnAfterRenderAsync` oder `OnAfterRender` [Komponente Lebenszyklusmethoden](xref:razor-components/components#lifecycle-methods).

## <a name="invoke-net-methods-from-javascript-functions"></a>Aufrufen von .NET Methoden von JavaScript-Funktionen

### <a name="static-net-method-call"></a>Statischen Methodenaufruf in .NET

Um eine statische .NET-Methode aus JavaScript aufrufen zu können, verwenden Sie die `DotNet.invokeMethod` oder `DotNet.invokeMethodAsync` Funktionen. Übergeben Sie den Bezeichner der statischen Methode, die Sie aufrufen, den Namen der Assembly mit der Funktion und alle Argumente möchten. In diesem Fall wird die asynchrone Version bevorzugt, um serverseitige Szenarien zu unterstützen. Um aus JavaScript aufgerufen werden, die .NET-Methode muss öffentliche, statische und durch `[JSInvokable]`. Der Methodenbezeichner wird standardmäßig der Name der Methode, aber Sie können angeben, einen anderen Bezeichner mit dem `JSInvokableAttribute` Konstruktor. Offene generische Methoden aufrufen wird derzeit nicht unterstützt.

Die beispielanwendung enthält einen C# Methode, um ein Array von zurückzugeben `int`s. Die Methode wird ergänzt, mit der `JSInvokable` Attribut.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=47&end=58&highlight=7-11)]

JavaScript, die für den Client bereitgestellt Ruft die C# .NET-Methode übergeben.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-12)]

Wenn die **Trigger .NET statische Methode ReturnArrayAsync** ausgewählt ist, überprüfen Sie die Konsolenausgabe in im Browser auf die Web Developer-Tools:

```console
Array(4) [ 1, 2, 3, 4 ]
```

Der vierte Arraywert wird in das Array abgelegt (`data.push(4);`) vom `ReturnArrayAsync`.

### <a name="instance-method-call"></a>Instanz-Methodenaufruf

Sie können auch Instanzmethoden für .NET aus JavaScript aufrufen. Um eine Instanzmethode für .NET aus JavaScript aufrufen zu können, zuerst übergeben Sie die .NET Instanz in JavaScript durch wrapping in eine `DotNetObjectRef` Instanz. Die .NET-Instanz als Verweis an JavaScript übergeben wird, und Sie können .NET von Instanzmethoden für die Instanz mit Aufrufen der `invokeMethod` oder `invokeMethodAsync` Funktionen. Die Instanz der .NET kann auch als Argument übergeben werden, wenn andere .NET Methoden von JavaScript aufgerufen.

> [!NOTE]
> Die Beispiel-app protokolliert Meldungen in die Client-Side-Konsole. Überprüfen Sie für den folgenden Beispielen gezeigt, die von der Beispiel-app im Browser auf die Konsolenausgabe in das browserentwicklertools.

Wenn die **Trigger .NET Instanzmethode HelloHelper.SayHello** ausgewählt ist, `ExampleJsInterop.CallHelloHelperSayHello` aufgerufen wird, und übergeben Sie einen Namen, `Blazor`, an die Methode.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=60&end=69&highlight=8)]

`CallHelloHelperSayHello` Ruft die JavaScript-Funktion `sayHello` durch eine neue Instanz der `HelloHelper`.

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=19-25)]

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-17)]

Der Name wird zum übergeben `HelloHelper`Konstruktor, der legt der `HelloHelper.Name` Eigenschaft. Wenn die JavaScript-Funktion `sayHello` ausgeführt wird, `HelloHelper.SayHello` gibt die `Hello, {Name}!` -Nachricht, die von der JavaScript-Funktion in die Konsole geschrieben wird.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Ausgabe im im Browser auf die Web-Entwicklertools-Konsole:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a>Freigeben von Interop-Code in einer Klassenbibliothek für die Razor-Komponente

Interop-JavaScript-Code in einer Klassenbibliothek für die Razor-Komponente enthalten sein kann (`dotnet new blazorlib`), können Sie den Code in einem NuGet-Paket verwenden.

Die Komponente der Razor-Klassenbibliothek behandelt die Einbettung von JavaScript-Ressourcen in der erstellten Assembly. Die JavaScript-Dateien befinden sich der *"Wwwroot"* Ordner, und das Tool übernimmt die Einbettung der Ressourcen aus, wenn die Bibliothek erstellt wird.

Das erstellte NuGet-Paket wird in der Projektdatei der app verwiesen werden, genau wie normale NuGet-Pakets verwiesen wird. Nachdem die app wiederhergestellt wurde, kann app-Code in JavaScript aufrufen, als wäre er C#.
