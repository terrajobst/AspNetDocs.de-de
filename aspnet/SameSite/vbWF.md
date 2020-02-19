---
title: SameSite-Cookie-Beispiel für ASP.NET 4.7.2 VB WebForms
author: blowdart
description: SameSite-Cookie-Beispiel für ASP.NET 4.7.2 VB WebForms
ms.author: riande
ms.date: 2/15/2019
uid: samesite/vbWF
ms.openlocfilehash: 8979edecc5acf7dac81b9f53d31af00389f4727c
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458384"
---
# <a name="samesite-cookie-sample-for-aspnet-472-vb-webforms"></a>SameSite-Cookie-Beispiel für ASP.NET 4.7.2 VB WebForms
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

Das standardmäßige SameSite-Attribut für ein Formular Authentifizierungs Cookie wird im `cookieSameSite`-Parameter der Formular Authentifizierungs Einstellungen in festgelegt `web.config` 

```xml
<system.web>
  <authentication mode="Forms">
    <forms name=".ASPXAUTH" loginUrl="~/" cookieSameSite="None" requireSSL="true">
    </forms>
  </authentication>
</system.web>
```

Das standardmäßige SameSite-Attribut für den Sitzungszustand wird auch im Parameter "cookiesamesite" der Sitzungs Einstellungen in festgelegt `web.config`

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

Bei der Aktualisierung von .net vom November 2019 wurden die Standardeinstellungen für die Formular Authentifizierung und die Sitzung auf `lax` geändert, da die kompatibelste Einstellung ist. Wenn Sie jedoch Seiten in iframes einbetten, müssen Sie diese Einstellung möglicherweise auf "keine" zurücksetzen und dann den unten gezeigten [Abfang](#interception) Code hinzufügen, um das `none` Verhalten je nach Browserfunktion anzupassen.

### <a name="running-the-sample"></a>Ausführen des Beispiels

Wenn Sie das Beispiel Projekt ausführen, laden Sie Ihren Browser Debugger auf der ersten Seite, und verwenden Sie ihn, um die Cookie-Sammlung für die Website anzuzeigen.
Wenn Sie dies in Edge und Chrome tun möchten `F12` klicken Sie dann auf die Registerkarte `Application`, und klicken Sie im Abschnitt `Storage` unter der Option `Cookies` auf die Website-URL.

![Liste der Browser Debugger-Cookies](sample/img/BrowserDebugger.png)

In der obigen Abbildung sehen Sie, dass das Cookie, das durch das Beispiel erstellt wird, wenn Sie auf die Schaltfläche "Cookies erstellen" klicken, einen SameSite-Attribut Wert `Lax`aufweist, der mit dem im [Beispielcode](#sampleCode)festgelegten Wert übereinstimmt.

## <a name="interception"></a>Abfangen von Cookies, die Sie nicht steuern

.NET 4.5.2 hat ein neues Ereignis für das Abfangen des Schreibens von Headern, `Response.AddOnSendingHeaders`, eingeführt. Dies kann verwendet werden, um Cookies abzufangen, bevor Sie an den Client Computer zurückgegeben werden. Im Beispiel verknüpfen wir das Ereignis mit einer statischen Methode, die überprüft, ob der Browser die neuen SameSite-Änderungen unterstützt, und wenn nicht, ändert die Cookies so, dass das Attribut nicht ausgegeben wird, wenn der neue `None` Wert festgelegt wurde.

In " [Global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/Global.asax.vb) " finden Sie ein Beispiel für das Einbinden des Ereignisses und [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/SameSiteCookieRewriter.vb) für ein Beispiel für die Behandlung des Ereignisses und das Anpassen des Cookies `sameSite` Attributs.


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

[ASP.NET-Dokumentation](/aspnet/samesite/system-web-samesite)

[.Net SameSite-Patches](/aspnet/samesite/kbs-samesite)