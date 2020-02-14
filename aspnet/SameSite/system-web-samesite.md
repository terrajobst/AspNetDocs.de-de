---
title: Arbeiten mit SameSite-Cookies in ASP.net
author: rick-anderson
description: Erfahren Sie, wie Sie mit SameSite-Cookies in ASP.NET verwenden.
ms.author: riande
ms.date: 1/22/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: c262e300361f33621e8bd126a34b251c23f56e1a
ms.sourcegitcommit: 6bd0d7581ec36dc32cb85d0d5fc0e51068dd4423
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2020
ms.locfileid: "77234761"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Arbeiten mit SameSite-Cookies in ASP.net

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite ist ein [IETF](https://ietf.org/about/) -Entwurfs Standard, der Schutz vor Cross-Site Request fälschungstoken (CSRF) bietet. Ursprünglich in [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07)entworfen wurde, wurde der Entwurfs Standard in [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00)aktualisiert. Der aktualisierte Standard ist nicht abwärts kompatibel mit dem vorherigen Standard, und es gibt folgende Unterschiede:

* Cookies ohne SameSite-Header werden standardmäßig als `SameSite=Lax` behandelt.
* `SameSite=None` müssen verwendet werden, um eine standortübergreifende Cookie-Verwendung zuzulassen.
* Cookies, die `SameSite=None` bestätigen, müssen ebenfalls als `Secure`gekennzeichnet werden.
* Der Wert SameSite = None ist vom [2016-Standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07) nicht zulässig und bewirkt, dass einige Implementierungen solche Cookies wie SameSite = Strict behandeln. Siehe [Unterstützung älterer Browser](#sob) in diesem Dokument.

Die `SameSite=Lax` Einstellung funktioniert für die meisten Anwendungs Cookies. Für einige Formen der Authentifizierung wie [OpenID Connect](https://openid.net/connect/) (oidc) und [WS-](https://auth0.com/docs/protocols/ws-fed) Verbund werden standardmäßig Post basierte Umleitungen bereitgestellt. Die Post basierten Umleitungen veranlassen den SameSite-Browserschutz, sodass SameSite für diese Komponenten deaktiviert ist. Die meisten [OAuth](https://oauth.net/) -Anmeldungen sind aufgrund von Unterschieden in der Art der Anforderungs Abläufe nicht betroffen.

Bei Anwendungen, die `iframe` verwenden, treten möglicherweise Probleme mit `SameSite=Lax` oder `SameSite=Strict` Cookies auf, da iframes als standortübergreifende Szenarios behandelt werden.

Jede ASP.NET-Komponente, die Cookies ausgibt, muss entscheiden, ob SameSite geeignet ist.

Weitere Informationen finden Sie unter [bekannte Probleme](#known) bei Anwendungen nach Installation der 2019 .net SameSite-Updates.

## <a name="using-samesite-in-aspnet-472-and-48"></a>Verwenden von SameSite in ASP.NET 4.7.2 und 4,8

.NET 4.7.2 und 4,8 unterstützen [2019 den Entwurf Standard](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) für SameSite seit der Veröffentlichung von Updates im Dezember 2019. Entwickler können den Wert des SameSite-Headers mithilfe der [HttpCookie. SameSite-Eigenschaft](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite)Programm gesteuert steuern. Wenn Sie die `SameSite`-Eigenschaft auf `Strict`, `Lax`oder `None` festlegen, werden diese Werte mit dem Cookie im Netzwerk geschrieben. Das Festlegen des Werts auf `(SameSiteMode)(-1)` gibt an, dass keine SameSite-Kopfzeile mit dem Cookie im Netzwerk enthalten sein soll. Die [HttpCookie. Secure-Eigenschaft](/dotnet/api/system.web.httpcookie.secure)oder ' Requirements SSL ' in Konfigurationsdateien kann verwendet werden, um das Cookie als `Secure` zu kennzeichnen.

Neue `HttpCookie` Instanzen werden standardmäßig `SameSite=(SameSiteMode)(-1)` und `Secure=false`. Diese Standardwerte können im Abschnitt `system.web/httpCookies`-Konfiguration außer Kraft gesetzt werden, in dem die Zeichenfolge `"Unspecified"` eine reine Konfigurations Syntax für `(SameSiteMode)(-1)`ist:

```xml
<configuration>
 <system.web>
  <httpCookies sameSite="[Strict|Lax|None|Unspecified]" requireSSL="[true|false]" />
 <system.web>
<configuration>
```

ASP.net gibt auch vier spezifische Cookies für diese Features aus: anonyme Authentifizierung, Formular Authentifizierung, Sitzungs Status und Rollen Verwaltung. Instanzen dieser Cookies, die in der Laufzeit abgerufen werden, können mit den Eigenschaften `SameSite` und `Secure` wie jede andere HttpCookie-Instanz manipuliert werden. Aufgrund der patchentstehung des SameSite-Standards sind die Konfigurationsoptionen für diese vier Features jedoch inkonsistent. Die relevanten Konfigurations Abschnitte und-Attribute mit Standardwerten sind unten dargestellt. Wenn keine `SameSite` oder `Secure` Verknüpftes Attribut für eine Funktion vorhanden ist, wird die Funktion auf die im oben beschriebenen `system.web/httpCookies` Abschnitt konfigurierten Standardwerte zurückgreifen.

```xml
<configuration>
 <system.web>
  <anonymousIdentification cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
  <authentication>
   <forms cookieSameSite="Lax" requireSSL="false" />
  </authentication>
  <sessionState cookieSameSite="Lax" /> <!-- No config attribute for Secure -->
  <roleManager cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
 <system.web>
<configuration>
```  

**Hinweis**: "nicht angegeben" ist zurzeit nur für die `system.web/httpCookies@sameSite` verfügbar. Wir hoffen, dass Sie den zuvor gezeigten cookiesamesite-Attributen in zukünftigen Updates eine ähnliche Syntax hinzufügen. Das Festlegen von `(SameSiteMode)(-1)` im Code funktioniert weiterhin auf Instanzen dieser Cookies. *

## <a name="history-and-changes"></a>Verlauf und Änderungen

Die SameSite-Unterstützung wurde zunächst in .NET 4.7.2 mithilfe des [Entwurfs Standards 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1)implementiert.

Die Updates vom 19. November 2019 für Windows aktualisierte .NET 4.7.2 + von der 2016-Standard-auf den 2019-Standard. Für andere Versionen von Windows werden weitere Updates demnächst durchlaufen. Weitere Informationen finden Sie unter <xref:samesite/kbs-samesite>.

 Der 2019-Entwurf der SameSite-Spezifikation:

* Ist **nicht** abwärts kompatibel mit dem 2016-Entwurf. Weitere Informationen finden Sie [unter unterstützen älterer Browser](#sob) in diesem Dokument.
* Gibt an, dass Cookies standardmäßig als `SameSite=Lax` behandelt werden.
* Gibt Cookies an, die `SameSite=None` explizit bestätigen, um die standortübergreifende Übermittlung zu aktivieren, sollte ebenfalls als `Secure`gekennzeichnet werden.
* Wird von Patches unterstützt, die wie in den oben aufgeführten KB beschrieben ausgegeben werden.
* Ist für die standardmäßige Aktivierung durch [Chrome](https://chromestatus.com/feature/5088147346030592) in [Feb 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)vorgesehen. Die Umstellung auf diesen Standard in 2019 wurde gestartet.

<a name="known"><a/>

## <a name="known-issues"></a>Bekannte Probleme

Da die Entwurfs Spezifikationen 2016 und 2019 nicht kompatibel sind, führt das .NET Framework-Update von November 2019 einige Änderungen ein, die möglicherweise unterbrochen werden.

* Sitzungs Status-und Formular Authentifizierungs Cookies werden nun als `Lax` in das Netzwerk geschrieben, anstatt nicht angegeben.
  * Obwohl die meisten apps mit `SameSite=Lax` Cookies arbeiten, werden apps, die über Websites oder Anwendungen bereitstellen, die `iframe` verwenden, möglicherweise feststellen, dass Ihre Sitzungs Status-oder Formular Autorisierungs Cookies nicht wie erwartet verwendet werden. Um dieses Problem zu beheben, ändern Sie den `cookieSameSite` Wert im entsprechenden Konfigurations Abschnitt, wie bereits erläutert.
* Bei httpCookies, die `SameSite=None` im Code oder in der Konfiguration explizit festlegen, wird dieser Wert jetzt mit dem Cookie geschrieben, während er zuvor ausgelassen wurde. Dies kann zu Problemen mit älteren Browsern führen, die nur den 2016 Draft-Standard unterstützen.
  * Wenn Sie auf Browser abzielen, die den 2019-Entwurfs Standard mit `SameSite=None` Cookies unterstützen, achten Sie darauf, Sie auch `Secure` zu markieren, oder Sie werden nicht erkannt.
  * Verwenden Sie die app-Einstellung `aspnet:SupressSameSiteNone=true`, um das 2016-Verhalten beim Schreiben von `SameSite=None`wiederherzustellen. Beachten Sie, dass dies für alle httpCookies in der APP gilt.

### <a name="azure-app-servicesamesite-cookie-handling"></a>Azure App Service – SameSite-Cookie-Behandlung

Informationen dazu, wie Azure App Service SameSite-Verhalten in .NET 4.7.2-apps konfiguriert, finden Sie unter [Azure App Service – SameSite Cookie Handling und .NET Framework 4.7.2-Patch](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) .

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

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Anstehende SameSite-Cookie-Änderungen in ASP.net und ASP.net Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [Chromium-Blog: Entwickler: machen Sie sich bereit für neue SameSite = None; Sichere Cookie-Einstellungen](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Erläuterte SameSite-Cookies](https://web.dev/samesite-cookies-explained/)
