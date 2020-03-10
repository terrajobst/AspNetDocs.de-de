---
title: SameSite-Cookie-Beispiel für ASP.NET 4.7.2 VB MVC
author: blowdart
description: SameSite-Cookie-Beispiel für ASP.NET 4.7.2 VB MVC
ms.author: riande
ms.date: 2/15/2019
uid: samesite/vbMVC
ms.openlocfilehash: f6effce6075f94fb58ce10ec08bf010fab8b4b56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440847"
---
# <a name="samesite-cookie-sample-for-aspnet-472-vb-mvc"></a>SameSite-Cookie-Beispiel für ASP.NET 4.7.2 VB MVC

.NET Framework 4,7 verfügt über eine integrierte Unterstützung für das [SameSite](https://www.owasp.org/index.php/SameSite) -Attribut, entspricht jedoch dem ursprünglichen Standard.
Das gepatchte Verhalten änderte die Bedeutung von `SameSite.None`, um das Attribut mit dem Wert `None`auszugeben, anstatt den Wert überhaupt auszugeben. Wenn Sie den Wert nicht ausgeben möchten, können Sie die `SameSite`-Eigenschaft für ein Cookie auf-1 festlegen.

## <a name="sampleCode"></a>Schreiben des SameSite-Attributs

Im folgenden finden Sie ein Beispiel für das Schreiben eines SameSite-Attributs für ein Cookie.

```vb
' Create the cookie
Dim sameSiteCookie As New HttpCookie("sameSiteSample")

' Set a value for the cookie
sameSiteCookie.Value = "sample"

' Set the secure flag, which Chrome's changes will require for SameSite none.
' Note this will also require you to be running on HTTPS
sameSiteCookie.Secure = True

' Set the cookie to HTTP only which is good practice unless you really do need
' to access it client side in scripts.
sameSiteCookie.HttpOnly = True

' Expire the cookie in 1 minute
sameSiteCookie.Expires = Date.Now.AddMinutes(1)

' Add the SameSite attribute, this will emit the attribute with a value of none.
' To Not emit the attribute at all set the SameSite property to -1.
sameSiteCookie.SameSite = SameSiteMode.None

' Add the cookie to the response cookie collection
Response.Cookies.Add(sameSiteCookie)
```

[!INCLUDE[](~/includes/MTcomments.md)]

Das standardmäßige SameSite-Attribut für den Sitzungszustand wird im Parameter "cookiesamesite" der Sitzungs Einstellungen in festgelegt `web.config`

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a>MVC-Authentifizierung

Die MVC-cookiebasierte Authentifizierung verwendet einen cookiemanager, um das Ändern von cookieattributen zu aktivieren. Die [samesitecookiemanager. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieManager.vb) -Klasse ist eine Implementierung einer solchen Klasse, die Sie in Ihre eigenen Projekte kopieren können. 

Sie müssen sicherstellen, dass Ihre Microsoft. owin-Komponenten auf Version 4.1.0 oder höher aktualisiert werden. Überprüfen Sie Ihre `packages.config` Datei, um sicherzustellen, dass alle Versionsnummern identisch sind, z.b.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <!-- other packages -->
  <package id="Microsoft.Owin.Host.SystemWeb" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Owin.Security" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Owin.Security.Cookies" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Web.Infrastructure" version="1.0.0.0" targetFramework="net472" />
  <package id="Owin" version="1.0" targetFramework="net472" />
</packages>
```

Die Authentifizierungs Komponenten müssen so konfiguriert werden, dass Sie den cookiemanager in der Startup-Klasse verwenden.

```vb
Public Sub Configuration(app As IAppBuilder)
    app.UseCookieAuthentication(New CookieAuthenticationOptions() With {
        .CookieSameSite = SameSiteMode.None,
        .CookieHttpOnly = True,
        .CookieSecure = CookieSecureOption.Always,
        .CookieManager = New SameSiteCookieManager(New SystemWebCookieManager())
    })
End Sub
```

Für *jede* Komponente, die diese unterstützt, muss ein cookiemanager festgelegt werden. dazu gehören cookieauthentication und openidconnectauthentication.

Der systemwebcookiemanager wird verwendet, um [bekannte Probleme](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) bei der Integration von Antwort Cookies zu vermeiden.

### <a name="running-the-sample"></a>Ausführen des Beispiels

Wenn Sie das Beispiel Projekt ausführen, laden Sie den Browser Debugger auf der Anfangs Seite, und verwenden Sie ihn, um die Cookie-Sammlung für die Website anzuzeigen.
Wenn Sie dies in Edge und Chrome tun möchten `F12` klicken Sie dann auf die Registerkarte `Application`, und klicken Sie im Abschnitt `Storage` unter der Option `Cookies` auf die Website-URL.

![Liste der Browser Debugger-Cookies](sample/img/BrowserDebugger.png)

In der obigen Abbildung sehen Sie, dass das Cookie, das durch das Beispiel erstellt wird, wenn Sie auf die Schaltfläche "SameSite Cookie erstellen" klicken, einen SameSite-Attribut Wert `Lax`hat, der mit dem im [Beispielcode](#sampleCode)festgelegten Wert übereinstimmt.

## <a name="interception"></a>Abfangen von Cookies, die Sie nicht steuern

.NET 4.5.2 hat ein neues Ereignis für das Abfangen des Schreibens von Headern, `Response.AddOnSendingHeaders`, eingeführt. Dies kann verwendet werden, um Cookies abzufangen, bevor Sie an den Client Computer zurückgegeben werden. Im Beispiel verknüpfen wir das Ereignis mit einer statischen Methode, die überprüft, ob der Browser die neuen SameSite-Änderungen unterstützt, und wenn nicht, ändert die Cookies so, dass das Attribut nicht ausgegeben wird, wenn der neue `None` Wert festgelegt wurde.

Ein Beispiel für das Einbinden des Ereignisses und " [samesitecookierewriter. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieRewriter.vb) " finden Sie unter " [Global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/Global.asax.vb) ". hier finden Sie ein Beispiel für die Behandlung des Ereignisses und das Anpassen des Cookie`sameSite` Attributs, das Sie in ihren eigenen Code kopieren können.

```vb
Sub FilterSameSiteNoneForIncompatibleUserAgents(ByVal sender As Object)
    Dim application As HttpApplication = TryCast(sender, HttpApplication)

    If application IsNot Nothing Then
        Dim userAgent = application.Context.Request.UserAgent

        If SameSite.DisallowsSameSiteNone(userAgent) Then
            application.Response.AddOnSendingHeaders(
                Function(context)
                    Dim cookies = context.Response.Cookies

                    For i = 0 To cookies.Count - 1
                        Dim cookie = cookies(i)

                        If cookie.SameSite = SameSiteMode.None Then
                            cookie.SameSite = CType((-1), SameSiteMode)
                        End If
                    Next
                End Function)
        End If
    End If
End Sub
```

Sie können das Verhalten bestimmter benannter Cookies auf die gleiche Weise ändern: Im folgenden Beispiel wird das Standard Authentifizierungs Cookie von `Lax` auf `None` in Browsern, die den `None`-Wert unterstützen, und das SameSite-Attribut in Browsern, die `None`nicht unterstützen, angepasst.

```vb
Public Shared Sub AdjustSpecificCookieSettings()
    HttpContext.Current.Response.AddOnSendingHeaders(Function(context)
            Dim cookies = context.Response.Cookies

            For i = 0 To cookies.Count - 1
            Dim cookie = cookies(i)

            If String.Equals(".ASPXAUTH", cookie.Name, StringComparison.Ordinal) Then

                If SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent) Then
                    cookie.SameSite = -1
                Else
                    cookie.SameSite = SameSiteMode.None
                End If

                cookie.Secure = True
            End If
            Next
        End Function)
End Sub
```

## <a name="more-information"></a>Weitere Informationen
 
[Chrome-Updates](https://www.chromium.org/updates/same-site)

[Dokumentation zu owin SameSite](/aspnet/samesite/owin-samesite)

[ASP.NET-Dokumentation](/aspnet/samesite/system-web-samesite)

[.Net SameSite-Patches](/aspnet/samesite/kbs-samesite)