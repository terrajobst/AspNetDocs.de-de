---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Web-API über einen .NET-Client (C#)-ASP.NET 4. x abrufen
author: MikeWasson
description: In diesem Tutorial wird gezeigt, wie Sie eine Web-API aus einer .NET 4. x-Anwendung abrufen.
ms.author: riande
ms.date: 11/24/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: ab3ba71839123e848dffaa59871f9dac8c1a88d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504957"
---
# <a name="call-a-web-api-from-a-net-client-c"></a>Web-API über einen .NET-Client (C#) aufgerufen

von [Mike Wasson](https://github.com/MikeWasson) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Das [abgeschlossene Projekt herunterladen](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample). [Anweisungen zum Download.](/aspnet/core/tutorials/#how-to-download-a-sample) 

In diesem Tutorial wird gezeigt, wie Sie eine Web-API mithilfe von [System .net. http. HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) aus einer .NET-Anwendung abrufen.

In diesem Tutorial wird eine Client-App geschrieben, die die folgende Web-API nutzt:

| Aktion | HTTP-Methode | Relativer URI |
| --- | --- | --- |
| Produkt nach ID erhalten | GET | /API/Products/-*ID* |
| Erstellen eines neuen Produkts | POST | /api/products |
| Aktualisieren eines Produkts | PUT | /API/Products/-*ID* |
| Löschen eines Produkts | Delete | /API/Products/-*ID* |

Informationen dazu, wie Sie diese API mit ASP.net-Web-API implementieren, finden Sie unter [Erstellen einer Web-API, die CRUD-Vorgänge unterstützt](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Der Einfachheit halber handelt es sich bei der Client Anwendung in diesem Tutorial um eine Windows-Konsolenanwendung. **HttpClient** wird auch für Windows Phone-und Windows Store-Apps unterstützt. Weitere Informationen finden Sie unter [Schreiben von Web-API-Client Code für mehrere Plattformen mit portablen Bibliotheken](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Erstellen der Konsolenanwendung

Erstellen Sie in Visual Studio eine neue Windows-Konsolen-App mit dem Namen " **httpclientsample** ", und fügen Sie den folgenden Code ein:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Der vorangehende Code ist die komplette Client-App.

`RunAsync` wird ausgeführt, bis der Vorgang abgeschlossen ist. Die meisten **HttpClient** -Methoden sind asynchron, da Sie Netzwerk-e/a-Vorgänge durchführen. Alle asynchronen Aufgaben werden innerhalb `RunAsync`ausgeführt. Normalerweise blockiert eine APP den Haupt Thread nicht, aber diese APP lässt keine Interaktion zu.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Installieren der Web-API-Client Bibliotheken

Verwenden Sie den nuget-Paket-Manager zum Installieren des Web-API-Client Bibliotheks Pakets.

Öffnen Sie das Menü **Extras**, und wählen Sie **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus. Geben Sie in der Paket-Manager-Konsole (PMC) den folgenden Befehl ein:

`Install-Package Microsoft.AspNet.WebApi.Client`

Mit dem vorangehenden Befehl werden dem Projekt die folgenden nuget-Pakete hinzugefügt:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Netwonsoft. JSON (auch als JSON.net bezeichnet) ist ein gängiges hochleistungsfähiges JSON-Framework für .net.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Hinzufügen einer Modell Klasse

Überprüfen Sie die `Product`-Klasse:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Diese Klasse entspricht dem Datenmodell, das von der Web-API verwendet wird. Eine APP kann **HttpClient** verwenden, um eine `Product` Instanz aus einer HTTP-Antwort zu lesen. Die APP muss keinen Deserialisierungscode schreiben.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Erstellen und Initialisieren von httpclient

Überprüfen Sie die statische **HttpClient** -Eigenschaft:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** soll einmal instanziiert und während der gesamten Lebensdauer einer Anwendung wieder verwendet werden. Die folgenden Bedingungen können zu **SocketException** -Fehlern führen:

* Erstellen einer neuen **HttpClient** -Instanz pro Anforderung.
* Server mit starker Auslastung.

Durch das Erstellen einer neuen **HttpClient** -Instanz pro Anforderung können die verfügbaren Sockets erschöpft werden.

Der folgende Code initialisiert die **HttpClient** -Instanz:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Der vorangehende Code:

* Legt den Basis-URI für HTTP-Anforderungen fest. Ändern Sie die Portnummer in den Port, der in der Server-App verwendet wird. Die APP funktioniert nur, wenn für die Server-App ein Port verwendet wird.
* Legt den Accept-Header auf "application/json" fest. Das Festlegen dieses Headers weist den Server an, Daten im JSON-Format zu senden.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Senden einer GET-Anforderung zum Abrufen einer Ressource

Der folgende Code sendet eine GET-Anforderung für ein Produkt:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

Die **getasync** -Methode sendet die HTTP GET-Anforderung. Wenn die Methode abgeschlossen ist, gibt Sie eine **httpresponanmessage** zurück, die die HTTP-Antwort enthält. Wenn der Statuscode in der Antwort ein Erfolgs Code ist, enthält der Antworttext die JSON-Darstellung eines Produkts. Aufrufen von "read **asasync** ", um die JSON-Nutzlast in eine `Product` Instanz zu deserialisieren. Die "read **asasync** "-Methode ist asynchron, da der Antworttext beliebig groß sein kann.

**HttpClient** löst keine Ausnahme aus, wenn die HTTP-Antwort einen Fehlercode enthält. Stattdessen ist die **issuccess Statuscode** -Eigenschaft **false** , wenn der Status ein Fehlercode ist. Wenn Sie HTTP-Fehlercodes als Ausnahmen behandeln möchten, rufen Sie [HttpResponseMessage. ensuresuccmestatuscode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) für das Response-Objekt auf. `EnsureSuccessStatusCode` löst eine Ausnahme aus, wenn der Statuscode außerhalb des Bereichs 200&ndash;299 liegt. Beachten Sie, dass **HttpClient** Ausnahmen aus anderen Gründen auslösen kann &mdash; z. b. bei einem Timeout der Anforderung.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Zu deserialisierende Medientyp-Formatierer

Wenn " **leseasasync** " ohne Parameter aufgerufen wird, wird ein Standardsatz von *Medien Formatierern* verwendet, um den Antworttext zu lesen. Die Standardformatierer unterstützen JSON-, XML-und Formular-URL-codierte Daten.

Anstatt die Standard Formatierer zu verwenden, können Sie eine Liste von Formatierern für die Methode "read **asasync** " bereitstellen.  Die Verwendung einer Liste von Formatierern ist nützlich, wenn Sie über einen benutzerdefinierten Medientyp Formatierer verfügen:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Weitere Informationen finden Sie unter [Media Formatters in ASP.net-Web-API 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Senden einer POST-Anforderung zum Erstellen einer Ressource

Der folgende Code sendet eine Post-Anforderung, die eine `Product` Instanz im JSON-Format enthält:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

Die **postasjsonasync** -Methode:

* Serialisiert ein Objekt in JSON.
* Sendet die JSON-Nutzlast in einer POST-Anforderung.

Wenn die Anforderung erfolgreich ausgeführt wird:

* Es sollte eine 201-Antwort (created) zurückgegeben werden.
* Die Antwort sollte die URL der erstellten Ressourcen im Location-Header enthalten.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Senden einer Put-Anforderung zum Aktualisieren einer Ressource

Der folgende Code sendet eine PUT-Anforderung zum Aktualisieren eines Produkts:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

Die **putasjsonasync** -Methode funktioniert wie **postasjsonasync**, mit dem Unterschied, dass Sie eine PUT-Anforderung anstelle von Post sendet.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Hiermit wird eine DELETE-Anforderung zum Löschen einer Ressource gesendet.

Der folgende Code sendet eine DELETE-Anforderung zum Löschen eines Produkts:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Wie bei Get hat eine Löschanforderung keinen Anforderungs Text. Sie müssen das JSON-oder XML-Format nicht mit DELETE angeben.

## <a name="test-the-sample"></a>Testen des Beispiels

So testen Sie die Client-App:

1. [Herunterladen](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) und Ausführen der Server-app. [Anweisungen zum Download.](/aspnet/core/#how-to-download-a-sample) Überprüfen Sie, ob die Server-APP funktioniert. Beispielsweise sollte `http://localhost:64195/api/products` eine Liste der Produkte zurückgeben.
2. Legen Sie den Basis-URI für HTTP-Anforderungen fest. Ändern Sie die Portnummer in den Port, der in der Server-App verwendet wird.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Führen Sie die Client-App aus. Es wird die folgende Ausgabe generiert:

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
