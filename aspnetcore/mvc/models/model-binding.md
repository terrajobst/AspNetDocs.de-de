---
title: Modellbindung in ASP.NET Core
author: tdykstra
description: Erfahren Sie, wie die Modellbindung in ASP.NET Core MVC Aktionsmethodenparametern Daten aus HTTP-Anforderungen zuordnet.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: tdykstra
ms.date: 11/13/2018
uid: mvc/models/model-binding
ms.openlocfilehash: 1dc9b41328ed78440622acc1865b6f088d394403
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037047"
---
# <a name="model-binding-in-aspnet-core"></a>Modellbindung in ASP.NET Core

Von [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>Einführung in die Modellbindung

Die Modellbindung in ASP.NET Core MVC ordnet Daten aus HTTP-Anforderungen Aktionsmethodenparametern zu. Diese Parameter können einfache Typen wie Zeichenfolgen, ganze Zahlen, Gleitkommazahlen oder komplexe Typen sein. Dieses Feature von MVC ist sehr hilfreich, denn das Zuordnen von eingehenden Daten zu einem Gegenstück ist ein häufiges Szenario, unabhängig von der Größe und Komplexität der Daten. MVC löst dieses Problem, indem die Bindung abstrahiert wird, sodass Entwickler nicht denselben Code für jede App umschreiben müssen. Einen eigenen Text-in-Typ-Konverter zu schreiben ist mühsam und fehleranfällig.

## <a name="how-model-binding-works"></a>So funktioniert die Modellbindung

Wenn MVC eine HTTP-Anforderung empfängt, wird sie an eine bestimmte Aktionsmethode eines Controllers weitergeleitet. MVC bestimmt auf Grundlage der Routendaten, welche Aktionsmethode ausgeführt werden soll, und bindet dann Werte aus der HTTP-Anforderung an die Parameter dieser Aktionsmethode. Nehmen wir beispielsweise die folgende URL:

`http://contoso.com/movies/edit/2`

Da die Routenvorlage `{controller=Home}/{action=Index}/{id?}` entspricht, leitet `movies/edit/2` an den `Movies`-Controller und die zugehörige `Edit`-Aktionsmethode weiter. Diese akzeptiert auch einen optionalen Parameter namens `id`. Der Code für die Aktionsmethode sollte etwa wie folgt aussehen:

```csharp
public IActionResult Edit(int? id)
   ```

Hinweis: Die Zeichenfolgen in der URL-Route sind keine Groß-/Kleinschreibung beachtet.

MVC versucht, Anforderungsdaten anhand des Namens an die Aktionsparameter zu binden, und sucht mithilfe des Parameternamens und der Namen der öffentlichen festlegbaren Eigenschaften nach Werten für jeden Parameter. Im obigen Beispiel heißt der einzige Aktionsparameter `id`. Dieser wird von MVC an den Wert gebunden, der in den Routenwerten denselben Namen trägt. Zusätzlich zu den Routenwerten bindet MVC Daten aus verschiedenen Teilen der Anforderung, und zwar in einer festgelegten Reihenfolge. Bei der Modellbindung werden die folgenden Datenquellen in der folgenden Reihenfolge durchgegangen:

1. `Form values`: Dies sind Formularwerte, die in der HTTP-Anforderung mit der POST-Methode zu wechseln. (einschließlich jQuery-POST-Anforderungen).

2. `Route values`: Die Anzahl von bereitgestellten Routenwerte [Routing](xref:fundamentals/routing)

3. `Query strings`: Die Zeichenfolge Abfrageteil des URI.

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

Hinweis: Formularwerte, Routendaten und Abfrage an, dass Zeichenfolgen als Name / Wert-Paare gespeichert werden.

Da die Modellbindung einen Schlüssel namens `id` verlangt, `id` jedoch kein Teil der Formularwerte ist, wird in den Routenwerten nach diesem Schlüssel gesucht. In unserem Beispiel befindet sich der Schlüssel dort. Eine Bindung wird hergestellt, und der Wert wird in die ganze Zahl 2 konvertiert. Dieselbe Anforderung mit „Edit(Zeichenfolgen-ID)“ würde in die Zeichenfolge „2“ konvertiert werden.

Bisher wurden in diesem Beispiel einfache Typen verwendet. Einfache Typen bestehen in MVC aus allen primitiven .NET-Typen oder aus Typen mit einem Zeichenfolgentypkonverter. Falls der Aktionsmethodenparameter aus einer Klasse wie dem `Movie`-Typen besteht, der sowohl einfache als auch komplexe Typen als Eigenschaften enthält, kann die Modellbindung von MVC ihn trotzdem ordnungsgemäß verarbeiten. Sie nutzt Reflektion und Rekursion, um die Eigenschaften komplexer Typen auf der Suche nach Übereinstimmungen zu durchlaufen. Die Modellbindung sucht nach der Schreibweise *Parametername.Eigenschaftenname*, um Werte an Eigenschaften zu binden. Wenn keine übereinstimmenden Werte gefunden werden, versucht die Modellbindung, einfach anhand des Eigenschaftennamens Bindungen durchzuführen. Bei Typen wie `Collection` sucht die Modellbindung nach Übereinstimmungen mit *Parametername[index]* oder nur *[index]*. Bei der Modellbindung werden `Dictionary`-Typen ähnlich behandelt, da nur *parametername[key]* oder nur *[key]* verlangt wird, solange die Schlüssel aus einfachen Typen bestehen. Unterstützte Schlüssel stimmen mit der Feldnamen-HTML überein und markieren Hilfsprogramme, die für den gleichen Modelltyp generiert wurde. Dies ermöglicht die Erhaltung von Werten, sodass Formularfelder der Einfachheit halber mit der Benutzereingabe befüllt bleiben, z.B. wenn gebundene Daten aus einem Erstellungs- oder Bearbeitungsvorgang die Überprüfung nicht bestanden haben.

Damit die Modellbindung durchgeführt werden kann, muss die Klasse über einen öffentlichen Standardkonstruktor und öffentliche schreibbare Eigenschaften verfügen, die gebunden werden können. Wenn die Modellbindung erfolgt, wird die Klasse mit dem öffentlichen Standardkonstruktor instanziiert. Anschließend können die Eigenschaften festgelegt werden.

Wenn ein Parameter gebunden ist, beendet die Modellbindung die Suche nach Werten mit diesem Namen und fährt mit dem Binden des nächsten Parameters fort. Andernfalls legt das Verhalten der Standardmodellbindung Parameter in Abhängigkeit ihres Typs auf ihre jeweiligen Standardwerte fest:

* `T[]`: Mit Ausnahme von Arrays des Typs `byte[]`, Bindung legt die Parameter des Typs `T[]` zu `Array.Empty<T>()`. Arrays des Typs `byte[]` werden auf `null` festgelegt.

* Verweistypen: Bindung erstellt eine Instanz einer Klasse mit dem Standardkonstruktor, ohne Eigenschaften festzulegen. Die Modellbindung legt `string`-Parameter jedoch auf `null` fest.

* Nullable-Typen: Festgelegt auf NULL festlegbare Typen `null`. Im obigen Beispiel legt die Modellbindung `id` auf `null` fest, da dessen Typ `int?` ist.

* Werttypen: Nicht auf NULL festlegbare Typen `T` festgelegt `default(T)`. Beispielsweise legt die Modellbindung einen Parameters `int id` auf 0 (null) fest. Sie sollten statt Standardwerten die Modellvalidierung oder Nullable-Typen verwenden.

Wenn die Bindung fehlschlägt, gibt MVC keinen Fehler aus. Jede Aktion, die eine Benutzereingabe akzeptiert, sollte die Eigenschaft `ModelState.IsValid` prüfen.

Hinweis: Jeder Eintrag im des Controllers `ModelState` -Eigenschaft ist eine `ModelStateEntry` , enthält ein `Errors` Eigenschaft. Es ist nur selten notwendig, diese Auflistung selbst abzufragen. Verwenden Sie stattdessen `ModelState.IsValid`.

Darüber hinaus gibt es einige spezielle Datentypen, die MVC beim Ausführen der Modellbindung berücksichtigen muss:

* `IFormFile`, `IEnumerable<IFormFile>`: Eine oder mehrere hochgeladene Dateien, die Teil der HTTP-Anforderung sind.

* `CancellationToken`: Zum Abbrechen der Aktivität in asynchronen Controllern verwendet.

Diese Typen können an Aktionsparameter oder Eigenschaften eines Klassentyps gebunden werden.

Nach Abschluss der Modellbindung, wird die [Validierung](validation.md) ausgeführt. Die Standardmodellbindung funktioniert hervorragend für die meisten Entwicklungsszenarios. Sie ist außerdem erweiterbar. Wenn Sie also besondere Anforderungen haben, können Sie das integrierte Verhalten anpassen.

## <a name="customize-model-binding-behavior-with-attributes"></a>Anpassen des Modellbindungsverhaltens mit Attributen

MVC enthält mehrere Attribute, mit denen Sie das Standardverhalten der Modellbindung auf eine andere Datenquelle richten können. Sie können beispielsweise angeben, ob die Bindung für eine Eigenschaft erforderlich ist oder ob sie überhaupt niemals mithilfe der Attribute `[BindRequired]` oder `[BindNever]` erfolgen sollte. Alternativ können Sie die Standarddatenquelle überschreiben und die Datenquelle der Modellbindung angeben. Es folgt eine Liste der Modellbindungsattribute:

* `[BindRequired]`: Dieses Attribut fügt einen modellzustandsfehler hinzu, wenn die Bindung hergestellt werden kann.

* `[BindNever]`: Weist die modellbindung nie an diesen Parameter zu binden.

* `[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Verwenden Sie diese Option, um die genaue Bindungsquelle angeben, die Sie anwenden möchten.

* `[FromServices]`: Dieses Attribut verwendet [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md) um Parameter aus Diensten zu binden.

* `[FromBody]`: Mithilfe der konfigurierten Formatierer zum Binden von Daten aus dem Anforderungstext. Der Formatierer wird basierend auf dem Inhaltstyp der Anforderung ausgewählt.

* `[ModelBinder]`: Wird verwendet, um den Standardmodellbinder Bindungsquelle und Namen zu überschreiben.

Attribute sind sehr nützliche Tools, wenn Sie das Standardverhalten der Modellbindung überschreiben möchten.

## <a name="customize-model-binding-and-validation-globally"></a>Globales Anpassen der Modellbindung und der Überprüfung

Das Verhalten des Systems für die Modellbindung und Überprüfung wird durch die Klasse [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata) gesteuert, die Folgendes beschreibt:

* Wie ein Modell gebunden werden soll.
* Wie die Überprüfung für den Typ und dessen Eigenschaften erfolgt.

Die Aspekte des Systemverhaltens können durch Hinzufügen eines Detailanbieters zu [MvcOptions.ModelMetadataDetailsProviders](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions.modelmetadatadetailsproviders#Microsoft_AspNetCore_Mvc_MvcOptions_ModelMetadataDetailsProviders) global konfiguriert werden. MVC enthält einige integrierte Detailanbieter, die das Konfigurieren des Verhalten wie das Deaktivieren der Modellbindung oder der Überprüfung für bestimmte Typen ermöglichen.

Fügen Sie [ExcludeBindingMetadataProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.excludebindingmetadataprovider) in `Startup.ConfigureServices` hinzu, um die Modellbindung für alle Modelle eines bestimmten Typs zu deaktivieren. Beispielsweise können Sie die Modellbindung für alle Modelle vom Typ `System.Version` wie folgt deaktivieren:

```csharp
services.AddMvc().AddMvcOptions(options =>
    options.ModelMetadataDetailsProviders.Add(
        new ExcludeBindingMetadataProvider(typeof(System.Version))));
```

Fügen Sie [SuppressChildValidationMetadataProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.suppresschildvalidationmetadataprovider) in `Startup.ConfigureServices` hinzu, um die Überprüfung von Eigenschaften eines bestimmten Typs zu deaktivieren. Beispielsweise können Sie die Überprüfung von Eigenschaften vom Typ `System.Guid` wie folgt deaktivieren:

```csharp
services.AddMvc().AddMvcOptions(options =>
    options.ModelMetadataDetailsProviders.Add(
        new SuppressChildValidationMetadataProvider(typeof(System.Guid))));
```

## <a name="bind-formatted-data-from-the-request-body"></a>Binden formatierter Daten aus dem Anforderungstext

Anforderungsdaten können in einer Vielzahl von Formaten wie JSON und XML vorliegen. Wenn Sie mit dem Attribut [FromBody] angeben, dass Sie einen Parameter an Daten im Anforderungstext binden möchten, verwendet MVC einige konfigurierte Formatierer, um die Daten basierend auf deren Inhaltstyp zu verarbeiten. MVC umfasst standardmäßig eine `JsonInputFormatter`-Klasse für die Verarbeitung von JSON-Daten. Sie können jedoch zusätzliche Formatierer für die Verarbeitung von XML und anderen benutzerdefinierten Formaten hinzufügen.

> [!NOTE]
> Es kann höchstens ein Parameter pro Aktion um `[FromBody]` ergänzt werden. Die ASP.NET-MVC-Runtime delegiert die Verantwortung, den Anforderungsdatenstrom zu lesen, an den Formatierer. Sobald der Anforderungsdatenstrom auf einen Parameter gelesen wurde, kann der Anforderungsdatenstrom üblicherweise nicht erneut zum Binden von anderen `[FromBody]`-Parametern gelesen werden.

> [!NOTE]
> `JsonInputFormatter` ist der Standardformatierer und basiert auf [Json.NET](https://www.newtonsoft.com/json).

ASP.NET Core wählt Eingabeformatierer basierend auf dem Header [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) und dem Parametertyp aus, es sei denn, es wird ein Attribut darauf angewendet, das etwas anderes angibt. Wenn Sie XML-Code oder ein anderes Format verwenden möchten, müssen Sie es in der Datei *Startup.cs* konfigurieren. Zunächst müssen Sie jedoch ggf. einen Verweis auf `Microsoft.AspNetCore.Mvc.Formatters.Xml` mithilfe von NuGet abrufen. Der Startcode sollte in etwa folgendermaßen aussehen:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

Der Code in der Datei *Startup.cs* enthält eine `ConfigureServices`-Methode mit einem `services`-Argument, mit dem Sie Dienste für Ihre ASP.NET Core-App erstellen können. In diesem Beispiel werden wir einen XML-Formatierer als Dienst hinzufügen, den MVC für diese App bereitstellt. Mit dem an die `AddMvc`-Methode übergebenen `options`-Argument können Sie Filter, Formatierer und andere Systemoptionen von MVC beim Start der App hinzufügen und verwalten. Wenden Sie dann das `Consumes`-Attribut auf Controllerklassen oder Aktionsmethoden an, damit sie mit allen Formaten funktionieren.

### <a name="custom-model-binding"></a>Benutzerdefinierte Modellbindung

Sie können die Modellbindung erweitern, indem Sie Ihre eigene benutzerdefinierte Modellbindung schreiben. Erfahren Sie mehr über die [benutzerdefinierte Modellbindung](../advanced/custom-model-binding.md).
