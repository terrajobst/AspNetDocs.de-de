---
title: Arbeiten mit SameSite-Cookies und der Open Web Interface for .net (owin)
author: rick-anderson
description: Arbeiten mit SameSite-Cookies und der Open Web Interface for .net (owin)
ms.author: riande
ms.date: 12/6/2019
uid: owin-samesite
ms.openlocfilehash: ac5ae24eeb9e8e1cc6296667a4bebef72c3eb62c
ms.sourcegitcommit: 7b1e1784213dd4c301635f9e181764f3e2f94162
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74993079"
---
# <a name="samesite-cookies-and-the-open-web-interface-for-net-owin"></a>SameSite-Cookies und die Open Web Interface für .net (owin)

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

`SameSite` ist ein [IETF](https://ietf.org/about/) -Entwurf, der einen Schutz vor Cross-Site Request fälschungstokenangriffen bietet. Der [Entwurf von SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):

* Behandelt Cookies standardmäßig als `SameSite=Lax`.
* Gibt Cookies an, die `SameSite=None` explizit bestätigen, um die standortübergreifende Übermittlung zu ermöglichen, muss als `Secure`gekennzeichnet werden.

`Lax` funktioniert für die meisten App-Cookies. Für einige Formen der Authentifizierung wie [OpenID Connect](https://openid.net/connect/) (oidc) und [WS-](https://auth0.com/docs/protocols/ws-fed) Verbund werden standardmäßig Post basierte Umleitungen bereitgestellt. Die Post basierten Umleitungen löst die `SameSite` Browserschutz aus, sodass `SameSite` für diese Komponenten deaktiviert ist. Die meisten [OAuth](https://oauth.net/) -Anmeldungen sind aufgrund von Unterschieden in der Art der Anforderungs Abläufe nicht betroffen. Alle anderen Komponenten werden standardmäßig **nicht** `SameSite` festgelegt und verwenden das Standardverhalten der Clients (alt oder neu).

Der `None`-Parameter verursacht Kompatibilitätsprobleme mit Clients, die den früheren [2016-Entwurfs Standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07) (z. b. IOS 12) implementiert haben. Siehe [Unterstützung älterer Browser](#sob) in diesem Dokument.

Jede owin-Komponente, die Cookies ausgibt, muss entscheiden, ob `SameSite` geeignet ist.

Die ASP.NET 4. x-Version dieses Artikels finden Sie unter <xref:samesite/system-web-samesite>.

## <a name="api-usage-with-samesite"></a>API-Nutzung mit SameSite

`Microsoft.Owin` verfügt über eine eigene `SameSite` Implementierung:

* Das ist nicht direkt von dem in `System.Web`abhängig.
* `SameSite` funktioniert an allen Versionen, die von den `Microsoft.Owin` Paketen, .NET 4,5 und höher, als Ziel verwendet werden können.
* Nur die [systemwebcookiemanager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebCookieManager.cs) -Komponente interagiert direkt mit der `System.Web` `HttpCookie`-Klasse.

`SystemWebCookieManager` ist abhängig von den .NET 4.7.2-`System.Web`-APIs, um die `SameSite` Unterstützung und die Patches zum Ändern des Verhaltens zu aktivieren.

Die Gründe für die Verwendung von `SystemWebCookieManager` werden in [owin-und System. Web-Antwort-Cookie-Integrationsproblemen](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues)beschrieben. `SystemWebCookieManager` wird empfohlen, wenn Sie auf `System.Web`ausführen.

Der folgende Code legt `SameSite` auf `Lax`fest:

```csharp
owinContext.Response.Cookies.Append("My Key", "My Value", new CookieOptions()
{
    SameSite = SameSiteMode.Lax
});
```

Die folgenden APIs verwenden `SameSite`:

* [Microsoft. owin. samesitemode](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin/SameSiteMode.cs)
* [Cookieoptions. SameSite](xref:Microsoft.AspNetCore.Http.CookieOptions.SameSite)
* [Cookieauthenticationoptions-Klasse](/previous-versions/aspnet/dn385599(v%3Dvs.113)) <!-- CookieAuthenticationOptions.CookieSameSite not published -->
* [Cookieauthenticationoptions. cookiesamesite](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.Cookies/CookieAuthenticationOptions.cs#L68-#L72)
* [Icookiemanager](/previous-versions/aspnet/dn800238(v%3Dvs.113))
* [Systemwebcookiemanager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebCookieManager.cs)
* [Systemwebchunkingcookiemanager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebChunkingCookieManager.cs)
* [Cookieauthenticationoptions. cookiemanager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.Cookies/CookieAuthenticationOptions.cs#L143-#AL148)
* [Openidconnectauthenticationoptions. cookiemanager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.OpenIdConnect/OpenIdConnectAuthenticationOptions.cs#L315-#L318)

## <a name="history-and-changes"></a>Verlauf und Änderungen

[Microsoft. owin](https://www.nuget.org/packages/Microsoft.Owin/) hat den [Standard Entwurf von`SameSite` 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1)nie unterstützt.

Die Unterstützung für den [Entwurf von SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) ist nur in `Microsoft.Owin` 4.1.0 und höher verfügbar. Es gibt keine Patches für frühere Versionen.

Der 2019-Entwurf der `SameSite` Spezifikation:

* Ist **nicht** abwärts kompatibel mit dem 2016-Entwurf. Weitere Informationen finden Sie [unter unterstützen älterer Browser](#sob) in diesem Dokument.
* Gibt an, dass Cookies standardmäßig als `SameSite=Lax` behandelt werden.
* Gibt Cookies an, die `SameSite=None` explizit bestätigen, um die standortübergreifende Übermittlung zu ermöglichen, muss als `Secure`gekennzeichnet werden. `None` ist ein neuer Eintrag zum ablehnen.
* Ist für die standardmäßige Aktivierung durch [Chrome](https://chromestatus.com/feature/5088147346030592) in [Feb 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)vorgesehen. Die Umstellung auf diesen Standard in 2019 wurde gestartet.
* Wird von Patches unterstützt, die wie in KB-Artikeln beschrieben ausgegeben werden. Weitere Informationen finden Sie unter <xref:samesite/kbs-samesite>.

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Unterstützung älterer Browser

Der 2016-`SameSite` Standard hat vorgeschrieben, dass unbekannte Werte als `SameSite=Strict` Werte behandelt werden müssen. Apps, auf die von älteren Browsern zugegriffen wird, die den 2016-`SameSite` Standard unterstützen, können unterbrechen, wenn Sie eine `SameSite`-Eigenschaft mit dem Wert `None`erhalten Web-Apps müssen die Browser Erkennung implementieren, wenn Sie ältere Browser unterstützen möchten. ASP.NET implementiert die Browser Erkennung nicht, da die Werte von Benutzer-Agents stark flüchtig sind und häufig geändert werden. Ein Erweiterungs Punkt in [icookiemanager](/previous-versions/aspnet/dn800238(v%3Dvs.113)) ermöglicht das Plug in Benutzer-Agent-spezifischer Logik.
<!-- https://docs.microsoft.com/en-us/previous-versions/aspnet/dn800238(v%3Dvs.113) -->

Fügen Sie in `Startup.Configuration`Code hinzu, der in etwa wie folgt aussieht:

[!code-csharp[](sample/Startup1.cs?name=snippet)]

Der vorangehende Code erfordert den Patch für .NET 4.7.2 oder höher `SameSite`.

Der folgende Code zeigt eine Beispiel Implementierung von `SameSiteCookieManager`:

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet)]

Im vorangehenden Beispiel wird `DisallowsSameSiteNone` in der `CheckSameSite`-Methode aufgerufen. `DisallowsSameSiteNone` ist eine Benutzer Methode, die erkennt, ob der Benutzer-Agent `SameSite` `None`nicht unterstützt:

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet3&highlight=4)]

Der folgende Code zeigt eine Beispiel `DisallowsSameSiteNone` Methode:

> [!WARNING]
> Der folgende Code dient nur zu Demonstrationszwecken:
> * Er sollte nicht als "Fertig" betrachtet werden.
> * Er wird nicht verwaltet oder unterstützt.

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet2)]

## <a name="test-apps-for-samesite-problems"></a>Testen von Apps für SameSite-Probleme

Apps, die mit Remote Standorten interagieren, z. b. durch die Anmeldung von Drittanbietern, benötigen Folgendes:

* Testen Sie die Interaktion in mehreren Browsern.
* Anwenden der in diesem Dokument beschriebenen [Browser Erkennung und-Entschärfung](#sob) .

Testen Sie Web-Apps mithilfe einer Client Version, die für das neue `SameSite` Verhalten verwendet werden kann. Chrome, Firefox und Chromium Edge verfügen über neue Feature-Flags, die für Tests verwendet werden können. Nachdem Ihre APP die `SameSite` Patches angewendet hat, testen Sie Sie mit älteren Client Versionen, insbesondere Safari. Weitere Informationen finden Sie [unter unterstützen älterer Browser](#sob) in diesem Dokument.

### <a name="test-with-chrome"></a>Testen mit Chrome

Chrome 78 + liefert irreführende Ergebnisse, da eine vorübergehende Entschärfung vorhanden ist. Die temporäre Chrome-Minderung von 78 und höher ermöglicht Cookies, die weniger als zwei Minuten alt sind. Chrome 76 oder 77 mit aktivierten geeigneten testflags bietet genauere Ergebnisse. Um das neue `SameSite` Verhalten zu testen, wechseln Sie `chrome://flags/#same-site-by-default-cookies` in **aktiviert**. Ältere Versionen von Chrome (75 und niedriger) werden mit der neuen `None` Einstellung gemeldet. Siehe [Unterstützung älterer Browser](#sob) in diesem Dokument.

Google stellt keine älteren Chrome-Versionen zur Verfügung. Befolgen Sie die Anweisungen unter [Herunterladen von Chrom](https://www.chromium.org/getting-involved/download-chromium) , um ältere Versionen von Chrome zu testen. Laden Sie Chrome **nicht** von den von Ihnen bereitgestellten Links herunter, indem Sie nach älteren Versionen von Chrome suchen.

* [Chromium 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chromium 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a>Testen mit Safari

Safari 12 hat den vorherigen Entwurf streng implementiert und schlägt fehl, wenn der neue `None` Wert in einem Cookie liegt. `None` wird über den Browser Erkennungs Code vermieden, der ältere Browser in diesem Dokument [unterstützt](#sob) . Testen Sie Safari 12-, Safari 13-und WebKit-basierte Anmeldungen im Betriebssystem Format unter Verwendung von msal, Adal oder einer beliebigen Bibliothek, die Sie verwenden. Das Problem hängt von der zugrunde liegenden Betriebssystemversion ab. OSX-mujave (10,14) und IOS 12 haben bekanntermaßen Kompatibilitätsprobleme mit dem neuen `SameSite` Verhalten. Durch ein Upgrade des Betriebssystems auf OSX Catalina (10,15) oder IOS 13 wird das Problem behoben. Safari verfügt derzeit nicht über ein Opt-in-Flag zum Testen des neuen Spezifikations Verhaltens.

### <a name="test-with-firefox"></a>Testen mit Firefox

Die Firefox-Unterstützung für den neuen Standard kann auf Version 68 + getestet werden, indem Sie sich auf der Seite "`about:config`" mit dem Feature-Flag `network.cookie.sameSite.laxByDefault`anmelden. Es gab keine Berichte über Kompatibilitätsprobleme mit älteren Versionen von Firefox.

### <a name="test-with-edge-browser"></a>Testen mit dem Edge-Browser

Edge unterstützt den alten `SameSite` Standard. Edge Version 44 hat keine bekannten Kompatibilitätsprobleme mit dem neuen Standard.

### <a name="test-with-edge-chromium"></a>Testen mit Edge (Chrom)

`SameSite` Flags werden auf der `edge://flags/#same-site-by-default-cookies` Seite festgelegt. Es wurden keine Kompatibilitätsprobleme mit Edge-Chrom erkannt.

### <a name="test-with-electron"></a>Testen mit einem Elektron

Zu den Electron-Versionen zählen ältere Versionen von Chromium. Beispielsweise ist die Version des von Teams verwendeten Elektrons Chrom 66, das das ältere Verhalten zeigt. Sie müssen ihre eigenen Kompatibilitätstests mit der Version des von Ihrem Produkt verwendeten-Elektronen durchführen. Weitere Informationen finden Sie [unter Unterstützung älterer Browser](#sob) im folgenden Abschnitt.

## <a name="additional-resources"></a>Weitere Ressourcen

* [Chromium-Blog: Entwickler: machen Sie sich bereit für neue SameSite = None; Sichere Cookie-Einstellungen](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Erläuterte SameSite-Cookies](https://web.dev/samesite-cookies-explained/)
* [Integrationsprobleme bei der owin-und System. Web-Antwort Cookie](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues)
