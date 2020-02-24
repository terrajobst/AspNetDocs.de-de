---
title: Arbeiten mit SameSite-Cookies in ASP.net
author: rick-anderson
description: Erfahren Sie, wie Sie mit SameSite-Cookies in ASP.NET verwenden.
ms.author: riande
ms.date: 2/15/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: edb368910b24be2d042afe3c19ffa1fb23245443
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455703"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Arbeiten mit SameSite-Cookies in ASP.net

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite ist ein [IETF](https://ietf.org/about/) -Entwurfs Standard, der Schutz vor Cross-Site Request fälschungstoken (CSRF) bietet. Ursprünglich in [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07)entworfen wurde, wurde der Entwurfs Standard in [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00)aktualisiert. Der aktualisierte Standard ist nicht abwärts kompatibel mit dem vorherigen Standard, und es gibt folgende Unterschiede:

* Cookies ohne SameSite-Header werden standardmäßig als `SameSite=Lax` behandelt.
* `SameSite=None` müssen verwendet werden, um eine standortübergreifende Cookie-Verwendung zuzulassen.
* Cookies, die `SameSite=None` bestätigen, müssen ebenfalls als `Secure`gekennzeichnet werden.
* Bei Anwendungen, die [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) verwenden, treten möglicherweise Probleme mit `sameSite=Lax` oder `sameSite=Strict` Cookies auf, da `<iframe>` als standortübergreifende Szenarios behandelt werden.
* Der Wert `SameSite=None` ist vom [2016-Standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07) nicht zulässig und bewirkt, dass einige Implementierungen solche Cookies wie `SameSite=Strict`behandeln. Siehe [Unterstützung älterer Browser](#sob) in diesem Dokument.

Die `SameSite=Lax` Einstellung funktioniert für die meisten Anwendungs Cookies. Für einige Formen der Authentifizierung wie [OpenID Connect](https://openid.net/connect/) (oidc) und [WS-](https://auth0.com/docs/protocols/ws-fed) Verbund werden standardmäßig Post basierte Umleitungen bereitgestellt. Die Post basierten Umleitungen veranlassen den SameSite-Browserschutz, sodass SameSite für diese Komponenten deaktiviert ist. Die meisten [OAuth](https://oauth.net/) -Anmeldungen sind aufgrund von Unterschieden in der Art der Anforderungs Abläufe nicht betroffen.

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

[!INCLUDE[](~/includes/MTcomments.md)]

<a name="retargeting"></a>

### <a name="retarget-net-apps"></a>Neuzuweisen von .net-apps

Ziel für .NET 4.7.2 oder höher:

* Stellen Sie sicher, dass *Web. config* Folgendes enthält:  <!-- review, I removed `debug="true"` -->

  ```xml
  <system.web>
    <compilation targetFramework="4.7.2"/>
    <httpRuntime targetFramework="4.7.2"/>
  </system.web>

* Verify the project file contains the correct [TargetFrameworkVersion](/visualstudio/msbuild/msbuild-target-framework-and-target-platform?view=vs-2019):

  ```xml
  <TargetFrameworkVersion>v4.7.2</TargetFrameworkVersion>
  ```

  Im [.net-Migrations Handbuch](/dotnet/framework/migration-guide/) finden Sie weitere Details.

* Überprüfen Sie, ob nuget-Pakete im Projekt auf die richtige Framework-Version abzielen. Sie können die richtige Frameworkversion überprüfen, indem Sie die Datei " *Packages. config* " untersuchen, beispielsweise:

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <packages>
    <package id="Microsoft.AspNet.Mvc" version="5.2.7" targetFramework="net472" />
    <package id="Microsoft.ApplicationInsights" version="2.4.0" targetFramework="net451" />
  </packages>
  ```

  In der vorherigen Datei " *Packages. config* " das `Microsoft.ApplicationInsights` Paket:
    * Ist für .NET 4.5.1 konzipiert.
    * Sollte das `targetFramework`-Attribut auf `net472` aktualisiert werden, wenn ein aktualisiertes Paket für das Framework-Ziel vorhanden ist.

<a name="nope"></a>

### <a name="net-versions-earlier-than-472"></a>.NET-Versionen vor 4.7.2

Microsoft bietet keine Unterstützung für .NET-Versionen, die 4.7.2 zum Schreiben desselben-Site-Cookie-Attributs verwendet werden. Wir haben keine zuverlässige Methode für Folgendes gefunden:

* Stellen Sie sicher, dass das Attribut basierend auf der Browserversion richtig geschrieben wurde.
* Abfangen und Anpassen von Authentifizierungs-und Sitzungs Cookies in älteren Framework-Versionen.

### <a name="december-patch-behavior-changes"></a>Änderungen im Dezember-patchverhalten

Die spezifische Behavior Change für .NET Framework gibt an, wie die `SameSite`-Eigenschaft den `None` Wert interpretiert:

* Vor dem Patch hat der Wert `None` gemeint:
  * Geben Sie das Attribut überhaupt nicht aus.
* Nach dem Patch:
  * Der Wert `None`bedeutet, dass das Attribut mit dem Wert `None`ausgegeben wird.
  * Ein `SameSite` Wert `(SameSiteMode)(-1)` bewirkt, dass das Attribut nicht ausgegeben wird.

Der Standardwert von SameSite für die Formular Authentifizierung und Sitzungs Status Cookies wurde von `None` in `Lax`geändert.

### <a name="summary-of-change-impact-on-browsers"></a>Zusammenfassung der Auswirkungen von Änderungen auf Browser

Wenn Sie den Patch installieren und ein Cookie mit `SameSite.None`ausgeben, geschieht eins von zwei Dingen:
* Chrome V80 behandelt dieses Cookie gemäß der neuen Implementierung und erzwingt nicht die gleichen Website Einschränkungen für das Cookie.
* Alle Browser, die nicht zur Unterstützung der neuen Implementierung aktualisiert wurden, folgen der alten Implementierung. Die alte Implementierung besagt Folgendes:
  * Wenn Sie einen Wert sehen, den Sie nicht verstehen, ignorieren Sie ihn, und wechseln Sie zu strikt denselben Website Einschränkungen.

Entweder wird die app in Chrome unterbrochen, oder Sie unterbrechen an zahlreichen anderen stellen.

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

Der 2016 SameSite-Standard hat festgestellt, dass unbekannte Werte als `SameSite=Strict` Werte behandelt werden müssen. Apps, auf die von älteren Browsern zugegriffen wird, die den 2016 SameSite-Standard unterstützen, können unterbrechen, wenn Sie eine SameSite-Eigenschaft mit dem Wert `None`erhalten. Web-Apps müssen die Browser Erkennung implementieren, wenn Sie ältere Browser unterstützen möchten. ASP.NET implementiert die Browser Erkennung nicht, da die Werte von Benutzer-Agents stark flüchtig sind und häufig geändert werden.

Der Ansatz von Microsoft, das Problem zu beheben, besteht darin, Sie bei der Implementierung von Browser Erkennungs Komponenten zu unterstützen, um das `sameSite=None` Attribut aus Cookies zu entfernen, wenn es von einem Browser bekannt ist, dass Die Ratschläge von Google bestand darin, doppelte Cookies auszugeben, eine mit dem neuen Attribut und eine ohne das-Attribut. Wir werden jedoch die Ratschläge von Google in Erwägung gezogen. Einige Browser, insbesondere Mobile Browser, haben sehr geringe Beschränkungen hinsichtlich der Anzahl von Cookies, die von einer Website oder von einem Domänen Namen gesendet werden können. Das Senden mehrerer Cookies, insbesondere von großen Cookies wie Authentifizierungs Cookies, kann das Mobile Browser Limit sehr schnell erreichen, was zu app-Fehlern führt, die schwer zu diagnostizieren und zu beheben sind. Außerdem gibt es als Framework ein großes Ökosystem aus Code und Komponenten von Drittanbietern, die möglicherweise nicht aktualisiert werden, um einen doppelten Cookie-Ansatz zu verwenden.

Der Browser Erkennungs Code, der in den Beispielprojekten in [diesem GitHub-Repository]() verwendet wird, ist in zwei Dateien enthalten.

* [C#SameSiteSupport.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.cs)
* [VB samesitesupport. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.vb)

Diese Erkennungen sind die am häufigsten verwendeten Browser-Agents, die den 2016-Standard unterstützen und für die das Attribut vollständig entfernt werden muss. Es ist nicht als umfassende Implementierung gemeint:

* In Ihrer APP werden möglicherweise Browser angezeigt, die für unsere Test Websites nicht angezeigt werden.
* Sie sollten darauf vorbereitet sein, Erkennungen nach Bedarf für Ihre Umgebung hinzuzufügen.

Die Art und Weise, wie Sie die Erkennung verknüpfen, variiert entsprechend der von Ihnen verwendeten .NET-Version und des verwendeten Webframe Works. Der folgende Code kann an der <xref:HTTP.HttpCookie> Aufruf Site aufgerufen werden:

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

Weitere Informationen finden Sie in den folgenden ASP.NET 4.7.2 SameSite Cookie-Themen:

* [C#MVC](xref:samesite/csMVC)
* [C#WebForms](xref:samesite/CSharpWebForms)
* [VB-WebForms](xref:samesite/vbWF)
* [VB-MVC](xref:samesite/vbMVC)
<!--
* <xref:samesite/csMVC>
* <xref:samesite/CSharpWebForms>
* <xref:samesite/vbWF>
* <xref:samesite/vbMVC>
-->

### <a name="ensuring-your-site-redirects-to-https"></a>Sicherstellen, dass Ihre Website an https umgeleitet wird

Für ASP.NET 4. x, WebForms und MVC kann [das URL-Rewrite-Feature von IIS](/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module) verwendet werden, um alle Anforderungen an HTTPS umzuleiten. Der folgende XML-Code zeigt eine Beispiel Regel:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Redirect to https" stopProcessing="true">
          <match url="(.*)"/>
          <conditions>
            <add input="{HTTPS}" pattern="Off"/>
            <add input="{REQUEST_METHOD}" pattern="^get$|^head$" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" redirectType="Permanent"/>
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

Bei lokalen Installationen von IIS- [URL-Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) ist ein optionales Feature, das möglicherweise installiert werden muss.

## <a name="test-apps-for-samesite-problems"></a>Testen von Apps für SameSite-Probleme

Sie müssen Ihre APP mit den von Ihnen unterstützten Browsern testen und die Szenarien durchlaufen, in denen Cookies beteiligt sind. Cookie-Szenarios in der Regel

* Anmeldeformulare
* Externe Anmelde Mechanismen, z. b. Facebook, Azure AD, OAuth und oidc
* Seiten, die Anforderungen von anderen Websites annehmen
* Seiten in Ihrer APP, die in iframes eingebettet werden sollen

Sie sollten überprüfen, ob Cookies in Ihrer APP ordnungsgemäß erstellt, persistent gespeichert und gelöscht werden.

Apps, die mit Remote Standorten interagieren, z. b. durch die Anmeldung von Drittanbietern, benötigen Folgendes:

* Testen Sie die Interaktion in mehreren Browsern.
* Anwenden der in diesem Dokument beschriebenen [Browser Erkennung und-Entschärfung](#sob) .

Testen Sie Web-Apps mithilfe einer Client Version, die das neue SameSite-Verhalten abonnieren kann. Chrome, Firefox und Chromium Edge verfügen über neue Feature-Flags, die für Tests verwendet werden können. Nachdem Ihre APP die SameSite-Patches angewendet hat, testen Sie Sie mit älteren Client Versionen, insbesondere Safari. Weitere Informationen finden Sie [unter unterstützen älterer Browser](#sob) in diesem Dokument.

### <a name="test-with-chrome"></a>Testen mit Chrome

Chrome 78 + liefert irreführende Ergebnisse, da eine vorübergehende Entschärfung vorhanden ist. Die temporäre Chrome-Minderung von 78 und höher ermöglicht Cookies, die weniger als zwei Minuten alt sind. Chrome 76 oder 77 mit aktivierten geeigneten testflags bietet genauere Ergebnisse. Um das neue SameSite-Verhalten zu testen, `chrome://flags/#same-site-by-default-cookies` auf **aktiviert**. Ältere Versionen von Chrome (75 und niedriger) werden mit der neuen `None` Einstellung gemeldet. Siehe [Unterstützung älterer Browser](#sob) in diesem Dokument.

Google stellt keine älteren Chrome-Versionen zur Verfügung. Befolgen Sie die Anweisungen unter [Herunterladen von Chrom](https://www.chromium.org/getting-involved/download-chromium) , um ältere Versionen von Chrome zu testen. Laden Sie Chrome **nicht** von den von Ihnen bereitgestellten Links herunter, indem Sie nach älteren Versionen von Chrome suchen.

* [Chromium 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chromium 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)
* Wenn Sie keine 64-Bit-Version von Windows verwenden, können Sie den [omahaproxy-Viewer](https://omahaproxy.appspot.com/) verwenden, um zu überprüfen, welcher Chrom Branch Chrome 74 (v 74.0.3729.108) entspricht, indem Sie die [von Chrom bereitgestellten Anweisungen](https://www.chromium.org/getting-involved/download-chromium)verwenden.

#### <a name="test-with-chrome-80"></a>Test mit Chrome 80 und höher

[Laden Sie](https://www.google.com/chrome/) eine Version von Chrome herunter, die Ihr neues Attribut unterstützt. Zum Zeitpunkt des Schreibens ist die aktuelle Version Chrome 80. Chrome 80 benötigt das Flag, das für die Verwendung des neuen Verhaltens aktiviert `chrome://flags/#same-site-by-default-cookies`. Sie sollten auch (`chrome://flags/#cookies-without-same-site-must-be-secure`) aktivieren, um das bevorstehende Verhalten für Cookies zu testen, für die kein SameSite-Attribut aktiviert ist. Chrome 80 ist als Ziel festzulegen, damit der Schalter Cookies ohne das-Attribut als `SameSite=Lax`behandelt, auch wenn für bestimmte Anforderungen eine Zeitüberschreitung aufgetreten ist. Zum Deaktivieren der Zeitüberschreitung für die Zeitüberschreitung kann Chrome 80 mit dem folgenden Befehlszeilenargument gestartet werden:

`--enable-features=SameSiteDefaultChecksMethodRigorously`

Chrome 80 enthält Warnmeldungen in der Browser Konsole über fehlende SameSite-Attribute. Verwenden Sie F12, um die Browser Konsole zu öffnen.

### <a name="test-with-safari"></a>Testen mit Safari

Safari 12 hat den vorherigen Entwurf streng implementiert und schlägt fehl, wenn der neue `None` Wert in einem Cookie liegt. `None` wird über den Browser Erkennungs Code vermieden, der ältere Browser in diesem Dokument [unterstützt](#sob) . Testen Sie Safari 12-, Safari 13-und WebKit-basierte Anmeldungen im Betriebssystem Format unter Verwendung von msal, Adal oder einer beliebigen Bibliothek, die Sie verwenden. Das Problem hängt von der zugrunde liegenden Betriebssystemversion ab. OSX-mujave (10,14) und IOS 12 haben bekanntermaßen Kompatibilitätsprobleme mit dem neuen SameSite-Verhalten. Durch ein Upgrade des Betriebssystems auf OSX Catalina (10,15) oder IOS 13 wird das Problem behoben. Safari verfügt derzeit nicht über ein Opt-in-Flag zum Testen des neuen Spezifikations Verhaltens.

### <a name="test-with-firefox"></a>Testen mit Firefox

Die Firefox-Unterstützung für den neuen Standard kann auf Version 68 + getestet werden, indem Sie sich auf der Seite "`about:config`" mit dem Feature-Flag `network.cookie.sameSite.laxByDefault`anmelden. Es gab keine Berichte über Kompatibilitätsprobleme mit älteren Versionen von Firefox.

### <a name="test-with-edge-legacy-browser"></a>Test mit Microsoft Edge-Browser (Legacy)

Microsoft Edge unterstützt den alten SameSite-Standard. Microsoft Edge Version 44 + hat keine bekannten Kompatibilitätsprobleme mit dem neuen Standard.

### <a name="test-with-edge-chromium"></a>Testen mit Microsoft Edge (Chrom)

Die SameSite-Flags werden auf der Seite "`edge://flags/#same-site-by-default-cookies`" festgelegt. Es wurden keine Kompatibilitätsprobleme mit Microsoft Edge Chromium erkannt.

### <a name="test-with-electron"></a>Testen mit einem Elektron

Zu den Electron-Versionen zählen ältere Versionen von Chromium. Beispielsweise ist die Version des von Teams verwendeten Elektrons Chrom 66, das das ältere Verhalten zeigt. Sie müssen ihre eigenen Kompatibilitätstests mit der Version des von Ihrem Produkt verwendeten-Elektronen durchführen. Siehe [Unterstützung älterer Browser](#sob).

## <a name="reverting-samesite-patches"></a>Wiederherstellen von SameSite-Patches

Sie können das aktualisierte SameSite-Verhalten in .NET Framework-apps auf das vorherige Verhalten zurücksetzen, bei dem das SameSite-Attribut für den Wert `None`nicht ausgegeben wird, und die Authentifizierungs-und Sitzungs Cookies zurücksetzen, um den Wert nicht auszugeben. Dies sollte als *äußerst vorübergehende Korrektur*angesehen werden, da die Chrome-Änderungen externe Post-Anforderungen oder die Authentifizierung für Benutzer mit Browsern unterstützen, die die Änderungen am Standard unterstützen.

### <a name="reverting-net-472-behavior"></a>Zurücksetzen des .NET 4.7.2-Verhaltens

Aktualisieren Sie *Web. config* so, dass die folgenden Konfigurationseinstellungen enthalten sind:

```xml
<configuration> 
  <appSettings>
    <add key="aspnet:SuppressSameSiteNone" value="true" />
  </appSettings>
 
  <system.web> 
    <authentication> 
      <forms cookieSameSite="None" /> 
    </authentication> 
    <sessionState cookieSameSite="None" /> 
  </system.web> 
</configuration>
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Anstehende SameSite-Cookie-Änderungen in ASP.net und ASP.net Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [Chromium-Blog: Entwickler: machen Sie sich bereit für neue SameSite = None; Sichere Cookie-Einstellungen](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Erläuterte SameSite-Cookies](https://web.dev/samesite-cookies-explained/)
* [Chrome-Updates](https://www.chromium.org/updates/same-site)
* [.Net SameSite-Patches](/aspnet/samesite/kbs-samesite)
* [Azure-Webanwendungen Website Informationen](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/)
* [Informationen zu gleichen Website Informationen zu Azure ActiveDirectory](/azure/active-directory/develop/howto-handle-samesite-cookie-changes-chrome-browser)
