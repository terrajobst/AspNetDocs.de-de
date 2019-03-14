---
ms.openlocfilehash: 4a4ad30846d1993b3e561c96c00fbf9a0c7f3929
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57040807"
---
# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a>Beispiel für das Umschreiben der URL in ASP.NET Core (ASP.NET Core 2.x)

Dieses Beispiel veranschaulicht die Verwendung der Middleware für das Umschreiben der URL von ASP.NET Core 2.x. In der App werden die Optionen zum Umleiten und Neuschreiben von URLs demonstriert.

Beim Ausführen des Beispiels wird die neu geschriebene oder umgeleitete URL von Antworten ohne Datei zurückgegeben, wenn eine der Regeln auf eine Anforderungs-URL angewendet wird. In den Beispielen für XML- und Textdateien, wird die Datei von der Middleware für statische Dateien verarbeitet, nachdem die Anforderungs-URL von der Middleware umgeschrieben wurde.

## <a name="examples-in-this-sample"></a>Beispiele

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Statuscode für Erfolg: 302 (Found)
  - Beispiel (Umleitung): **/redirect-rule/{capture_group}** zu **/redirected/{capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Statuscode für Erfolg: 200 (OK)
  - Beispiel (Umschreibung): **/rewrite-rule/{capture_group_1}/{capture_group_2}** in **/rewritten?var1={capture_group_1}&var2={capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Statuscode für Erfolg: 302 (Found)
  - Beispiel (Umleitung): **/apache-mod-rules-redirect/{capture_group}** zu **/redirected?id={capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Statuscode für Erfolg: 200 (OK)
  - Beispiel (Umschreibung): **/iis-rules-rewrite/{capture_group}** in **/rewritten?id={capture_group}**
* `Add(RedirectXmlFileRequests)`
  - Statuscode für Erfolg: 301 (Permanent verschoben)
  - Beispiel (Umleitung): **/file.xml** zu **/xmlfiles/file.xml**
* `Add(RewriteTextFileRequests)`
  - Statuscode für Erfolg: 200 (OK)
  - Beispiel (Neuschreibung): **/some_file.txt** zu **/file.txt**
* `Add(new RedirectImageRequests(".png", "/png-images")))`<br>`Add(new RedirectImageRequests(".jpg", "/jpg-images")))`
  - Statuscode für Erfolg: 301 (Permanent verschoben)
  - Beispiel (Umleitung): **/image.png** zu **/png-images/image.png**
  - Beispiel (Umleitung): **/image.jpg** zu **/jpg-images/image.jpg**

## <a name="use-a-physicalfileprovider"></a>PhysicalFileProvider

Sie können auch ein `IFileProvider`-Objekt abrufen, indem Sie ein `PhysicalFileProvider`-Objekt erstellen, das an die Methoden `AddApacheModRewrite()` und `AddIISUrlRewrite()` übergeben wird:

```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```

## <a name="secure-redirection-extensions"></a>Sichere Umleitungserweiterungen

Dieses Beispiel enthält eine `WebHostBuilder`-Konfiguration für die App zur Verwendung von URLs (`https://localhost:5001`, `https://localhost`) und ein Testzertifikat (*testCert.pfx*) als Unterstützung beim Erkunden der sicheren Umleitungsmethoden. Wenn dem Server Port 443 bereits zugewiesen wurde oder dieser verwendet wird, funktioniert das `https://localhost`-Beispiel nicht. &mdash; Entfernen Sie `ListenOptions` für Port 443 in der `CreateWebHostBuilder`-Methode der *Program.cs*-Datei oder lösen Sie Port 443 aus dem Server heraus, sodass der Port von Kestrel verwendet werden kann.

| Methode                           | Statuscode |    Port    |
| -------------------------------- | :---------: | :--------: |
| `.AddRedirectToHttpsPermanent()` |     301     | NULL (465) |
| `.AddRedirectToHttps()`          |     302     | NULL (465) |
| `.AddRedirectToHttps(301)`       |     301     | NULL (465) |
| `.AddRedirectToHttps(301, 5001)` |     301     |    5001    |
