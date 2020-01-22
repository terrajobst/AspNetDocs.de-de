---
title: Arbeiten mit SameSite-Cookies in ASP.net
author: rick-anderson
description: Erfahren Sie, wie Sie mit SameSite-Cookies in ASP.NET verwenden.
ms.author: riande
ms.date: 1/22/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: d2160bd9aeb93398b49b3a0e5e7a8a4404a5bc63
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519192"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Arbeiten mit SameSite-Cookies in ASP.net

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite ist ein [IETF](https://ietf.org/about/) -Entwurf, der einen Schutz vor Cross-Site Request fälschungstokenangriffen bietet. Der [Entwurf von SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):

* Behandelt Cookies standardmäßig als `SameSite=Lax`.
* Gibt Cookies an, die `SameSite=None` explizit bestätigen, um die standortübergreifende Übermittlung zu ermöglichen, muss als `Secure`gekennzeichnet werden.

`Lax` funktioniert für die meisten App-Cookies. Für einige Formen der Authentifizierung wie [OpenID Connect](https://openid.net/connect/) (oidc) und [WS-](https://auth0.com/docs/protocols/ws-fed) Verbund werden standardmäßig Post basierte Umleitungen bereitgestellt. Die Post basierten Umleitungen veranlassen den SameSite-Browserschutz, sodass SameSite für diese Komponenten deaktiviert ist. Die meisten [OAuth](https://oauth.net/) -Anmeldungen sind aufgrund von Unterschieden in der Art der Anforderungs Abläufe nicht betroffen.

Der `None`-Parameter verursacht Kompatibilitätsprobleme mit Clients, die den früheren [2016-Entwurfs Standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07) (z. b. IOS 12) implementiert haben. Siehe [Unterstützung älterer Browser](#sob) in diesem Dokument.

Jede ASP.NET-Komponente, die Cookies ausgibt, muss entscheiden, ob SameSite geeignet ist.

## <a name="api-usage-with-samesite"></a>API-Nutzung mit SameSite

Siehe [HttpCookie. SameSite-Eigenschaft](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite)

## <a name="history-and-changes"></a>Verlauf und Änderungen

Die SameSite-Unterstützung wurde zunächst in .NET 4.7.2 mithilfe des [Entwurfs Standards 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1)implementiert.

Die Updates vom 19. November 2019 für Windows aktualisierte .NET 4.7.2 + von der 2016-Standard-auf den 2019-Standard. Für andere Versionen von Windows werden weitere Updates demnächst durchlaufen. Weitere Informationen finden Sie unter <xref:samesite/kbs-samesite>.

 Der 2019-Entwurf der SameSite-Spezifikation:

* Ist **nicht** abwärts kompatibel mit dem 2016-Entwurf. Weitere Informationen finden Sie [unter unterstützen älterer Browser](#sob) in diesem Dokument.
* Gibt an, dass Cookies standardmäßig als `SameSite=Lax` behandelt werden.
* Gibt Cookies an, die `SameSite=None` explizit bestätigen, um die standortübergreifende Übermittlung zu ermöglichen, muss als `Secure`gekennzeichnet werden. `None` ist ein neuer Eintrag zum ablehnen.
* Wird von Patches unterstützt, die wie in den oben aufgeführten KB beschrieben ausgegeben werden.
* Ist für die standardmäßige Aktivierung durch [Chrome](https://chromestatus.com/feature/5088147346030592) in [Feb 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)vorgesehen. Die Umstellung auf diesen Standard in 2019 wurde gestartet.

### <a name="azure-app-servicesamesite-cookie-handling"></a>Azure App Service – SameSite-Cookie-Behandlung

Weitere Informationen finden Sie unter [Azure App Service – SameSite-Cookie-Behandlung und .NET Framework 4.7.2-Patch](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) .

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Unterstützung älterer Browser

Der 2016 SameSite-Standard hat festgestellt, dass unbekannte Werte als `SameSite=Strict` Werte behandelt werden müssen. Apps, auf die von älteren Browsern zugegriffen wird, die den 2016 SameSite-Standard unterstützen, können unterbrechen, wenn Sie eine SameSite-Eigenschaft mit dem Wert `None`erhalten. Web-Apps müssen die Browser Erkennung implementieren, wenn Sie ältere Browser unterstützen möchten. ASP.NET implementiert die Browser Erkennung nicht, da die Werte von Benutzer-Agents stark flüchtig sind und häufig geändert werden. Der folgende Code kann an der <xref:HTTP.HttpCookie> Aufruf Site aufgerufen werden:

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

Im vorangehenden Beispiel ist `MyUserAgentDetectionLib.DisallowsSameSiteNone` eine vom Benutzer bereitgestellte Bibliothek, die erkennt, ob der Benutzer-Agent SameSite `None`nicht unterstützt. Der folgende Code zeigt eine Beispiel `DisallowsSameSiteNone` Methode:

> [!WARNING]
> Der folgende Code dient nur zu Demonstrationszwecken:
> * Er sollte nicht als "Fertig" betrachtet werden.
> * Er wird nicht verwaltet oder unterstützt.

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet2)]

## <a name="test-apps-for-samesite-problems"></a>Testen von Apps für SameSite-Probleme

Apps, die mit Remote Standorten interagieren, z. b. durch die Anmeldung von Drittanbietern, benötigen Folgendes:

* Testen Sie die Interaktion in mehreren Browsern.
* Anwenden der in diesem Dokument beschriebenen [Browser Erkennung und-Entschärfung](#sob) .

Testen Sie Web-Apps mithilfe einer Client Version, die das neue SameSite-Verhalten abonnieren kann. Chrome, Firefox und Chromium Edge verfügen über neue Feature-Flags, die für Tests verwendet werden können. Nachdem Ihre APP die SameSite-Patches angewendet hat, testen Sie Sie mit älteren Client Versionen, insbesondere Safari. Weitere Informationen finden Sie [unter unterstützen älterer Browser](#sob) in diesem Dokument.

### <a name="test-with-chrome"></a>Testen mit Chrome

Chrome 78 + liefert irreführende Ergebnisse, da eine vorübergehende Entschärfung vorhanden ist. Die temporäre Chrome-Minderung von 78 und höher ermöglicht Cookies, die weniger als zwei Minuten alt sind. Chrome 76 oder 77 mit aktivierten geeigneten testflags bietet genauere Ergebnisse. Um das neue SameSite-Verhalten zu testen, `chrome://flags/#same-site-by-default-cookies` auf **aktiviert**. Ältere Versionen von Chrome (75 und niedriger) werden mit der neuen `None` Einstellung gemeldet. Siehe [Unterstützung älterer Browser](#sob) in diesem Dokument.

Google stellt keine älteren Chrome-Versionen zur Verfügung. Befolgen Sie die Anweisungen unter [Herunterladen von Chrom](https://www.chromium.org/getting-involved/download-chromium) , um ältere Versionen von Chrome zu testen. Laden Sie Chrome **nicht** von den von Ihnen bereitgestellten Links herunter, indem Sie nach älteren Versionen von Chrome suchen.

* [Chromium 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chromium 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a>Testen mit Safari

Safari 12 hat den vorherigen Entwurf streng implementiert und schlägt fehl, wenn der neue `None` Wert in einem Cookie liegt. `None` wird über den Browser Erkennungs Code vermieden, der ältere Browser in diesem Dokument [unterstützt](#sob) . Testen Sie Safari 12-, Safari 13-und WebKit-basierte Anmeldungen im Betriebssystem Format unter Verwendung von msal, Adal oder einer beliebigen Bibliothek, die Sie verwenden. Das Problem hängt von der zugrunde liegenden Betriebssystemversion ab. OSX-mujave (10,14) und IOS 12 haben bekanntermaßen Kompatibilitätsprobleme mit dem neuen SameSite-Verhalten. Durch ein Upgrade des Betriebssystems auf OSX Catalina (10,15) oder IOS 13 wird das Problem behoben. Safari verfügt derzeit nicht über ein Opt-in-Flag zum Testen des neuen Spezifikations Verhaltens.

### <a name="test-with-firefox"></a>Testen mit Firefox

Die Firefox-Unterstützung für den neuen Standard kann auf Version 68 + getestet werden, indem Sie sich auf der Seite "`about:config`" mit dem Feature-Flag `network.cookie.sameSite.laxByDefault`anmelden. Es gab keine Berichte über Kompatibilitätsprobleme mit älteren Versionen von Firefox.

### <a name="test-with-edge-browser"></a>Testen mit dem Edge-Browser

Edge unterstützt den alten SameSite-Standard. Edge Version 44 hat keine bekannten Kompatibilitätsprobleme mit dem neuen Standard.

### <a name="test-with-edge-chromium"></a>Testen mit Edge (Chrom)

Die SameSite-Flags werden auf der Seite "`edge://flags/#same-site-by-default-cookies`" festgelegt. Es wurden keine Kompatibilitätsprobleme mit Edge-Chrom erkannt.

### <a name="test-with-electron"></a>Testen mit einem Elektron

Zu den Electron-Versionen zählen ältere Versionen von Chromium. Beispielsweise ist die Version des von Teams verwendeten Elektrons Chrom 66, das das ältere Verhalten zeigt. Sie müssen ihre eigenen Kompatibilitätstests mit der Version des von Ihrem Produkt verwendeten-Elektronen durchführen. Weitere Informationen finden Sie [unter Unterstützung älterer Browser](#sob) im folgenden Abschnitt.

## <a name="additional-resources"></a>Weitere Ressourcen

* [Chromium-Blog: Entwickler: machen Sie sich bereit für neue SameSite = None; Sichere Cookie-Einstellungen](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Erläuterte SameSite-Cookies](https://web.dev/samesite-cookies-explained/)
