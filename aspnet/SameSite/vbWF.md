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
# <a name="samesite-cookie-sample-for-aspnet-472-vb-webforms"></a><span data-ttu-id="f4599-103">SameSite-Cookie-Beispiel für ASP.NET 4.7.2 VB WebForms</span><span class="sxs-lookup"><span data-stu-id="f4599-103">SameSite cookie sample for ASP.NET 4.7.2 VB WebForms</span></span>
<span data-ttu-id="f4599-104">.NET Framework 4,7 verfügt über eine integrierte Unterstützung für das [SameSite](https://www.owasp.org/index.php/SameSite) -Attribut, entspricht jedoch dem ursprünglichen Standard.</span><span class="sxs-lookup"><span data-stu-id="f4599-104">.NET Framework 4.7 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it adheres to the original standard.</span></span>
<span data-ttu-id="f4599-105">Das gepatchte Verhalten änderte die Bedeutung von `SameSite.None`, um das Attribut mit dem Wert `None`auszugeben, anstatt den Wert überhaupt auszugeben.</span><span class="sxs-lookup"><span data-stu-id="f4599-105">The patched behavior changed the meaning of `SameSite.None` to emit the attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="f4599-106">Wenn Sie den Wert nicht ausgeben möchten, können Sie die `SameSite`-Eigenschaft für ein Cookie auf-1 festlegen.</span><span class="sxs-lookup"><span data-stu-id="f4599-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="f4599-107">Schreiben des SameSite-Attributs</span><span class="sxs-lookup"><span data-stu-id="f4599-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="f4599-108">Im folgenden finden Sie ein Beispiel für das Schreiben eines SameSite-Attributs für ein Cookie.</span><span class="sxs-lookup"><span data-stu-id="f4599-108">Following is an example of how to write a SameSite attribute on a cookie;</span></span>

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

<span data-ttu-id="f4599-109">Das standardmäßige SameSite-Attribut für ein Formular Authentifizierungs Cookie wird im `cookieSameSite`-Parameter der Formular Authentifizierungs Einstellungen in festgelegt `web.config`</span><span class="sxs-lookup"><span data-stu-id="f4599-109">The default sameSite attribute for a forms authentication cookie is set in the `cookieSameSite` parameter of the forms authentication settings in `web.config`</span></span> 

```xml
<system.web>
  <authentication mode="Forms">
    <forms name=".ASPXAUTH" loginUrl="~/" cookieSameSite="None" requireSSL="true">
    </forms>
  </authentication>
</system.web>
```

<span data-ttu-id="f4599-110">Das standardmäßige SameSite-Attribut für den Sitzungszustand wird auch im Parameter "cookiesamesite" der Sitzungs Einstellungen in festgelegt `web.config`</span><span class="sxs-lookup"><span data-stu-id="f4599-110">The default sameSite attribute for session state is also set in the 'cookieSameSite' parameter of the session settings in `web.config`</span></span>

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

<span data-ttu-id="f4599-111">Bei der Aktualisierung von .net vom November 2019 wurden die Standardeinstellungen für die Formular Authentifizierung und die Sitzung auf `lax` geändert, da die kompatibelste Einstellung ist. Wenn Sie jedoch Seiten in iframes einbetten, müssen Sie diese Einstellung möglicherweise auf "keine" zurücksetzen und dann den unten gezeigten [Abfang](#interception) Code hinzufügen, um das `none` Verhalten je nach Browserfunktion anzupassen.</span><span class="sxs-lookup"><span data-stu-id="f4599-111">The November 2019 update to .NET changed the default settings for Forms Authentication and Session to `lax` as is the most compatible setting, however if you embed pages into iframes you may need to revert this setting to None, and then add the [interception](#interception) code shown below to adjust the `none` behavior depending on browser capability.</span></span>

### <a name="running-the-sample"></a><span data-ttu-id="f4599-112">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="f4599-112">Running the sample</span></span>

<span data-ttu-id="f4599-113">Wenn Sie das Beispiel Projekt ausführen, laden Sie Ihren Browser Debugger auf der ersten Seite, und verwenden Sie ihn, um die Cookie-Sammlung für die Website anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f4599-113">If you run the sample project  load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span>
<span data-ttu-id="f4599-114">Wenn Sie dies in Edge und Chrome tun möchten `F12` klicken Sie dann auf die Registerkarte `Application`, und klicken Sie im Abschnitt `Storage` unter der Option `Cookies` auf die Website-URL.</span><span class="sxs-lookup"><span data-stu-id="f4599-114">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Liste der Browser Debugger-Cookies](sample/img/BrowserDebugger.png)

<span data-ttu-id="f4599-116">In der obigen Abbildung sehen Sie, dass das Cookie, das durch das Beispiel erstellt wird, wenn Sie auf die Schaltfläche "Cookies erstellen" klicken, einen SameSite-Attribut Wert `Lax`aufweist, der mit dem im [Beispielcode](#sampleCode)festgelegten Wert übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="f4599-116">You can see from the image above that the cookie created by the sample when you click the "Create Cookies" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="f4599-117">Abfangen von Cookies, die Sie nicht steuern</span><span class="sxs-lookup"><span data-stu-id="f4599-117">Intercepting cookies you do not control</span></span>

<span data-ttu-id="f4599-118">.NET 4.5.2 hat ein neues Ereignis für das Abfangen des Schreibens von Headern, `Response.AddOnSendingHeaders`, eingeführt.</span><span class="sxs-lookup"><span data-stu-id="f4599-118">.NET 4.5.2 introduced a new event for intercepting the writing of headers, `Response.AddOnSendingHeaders`.</span></span> <span data-ttu-id="f4599-119">Dies kann verwendet werden, um Cookies abzufangen, bevor Sie an den Client Computer zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="f4599-119">This can be used to intercept cookies before they are returned to the client machine.</span></span> <span data-ttu-id="f4599-120">Im Beispiel verknüpfen wir das Ereignis mit einer statischen Methode, die überprüft, ob der Browser die neuen SameSite-Änderungen unterstützt, und wenn nicht, ändert die Cookies so, dass das Attribut nicht ausgegeben wird, wenn der neue `None` Wert festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="f4599-120">In the sample we wire up the event to a static method which checks whether the browser supports the new sameSite changes, and if not, changes the cookies to not emit the attribute if the new `None` value has been set.</span></span>

<span data-ttu-id="f4599-121">In " [Global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/Global.asax.vb) " finden Sie ein Beispiel für das Einbinden des Ereignisses und [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/SameSiteCookieRewriter.vb) für ein Beispiel für die Behandlung des Ereignisses und das Anpassen des Cookies `sameSite` Attributs.</span><span class="sxs-lookup"><span data-stu-id="f4599-121">See [global.asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/Global.asax.vb) for an example of hooking up the event and [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/SameSiteCookieRewriter.vb) for an example of handling the event and adjusting the cookie `sameSite` attribute.</span></span>


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

<span data-ttu-id="f4599-122">Sie können das Verhalten bestimmter benannter Cookies auf die gleiche Weise ändern: Im folgenden Beispiel wird das Standard Authentifizierungs Cookie von `Lax` auf `None` in Browsern, die den `None`-Wert unterstützen, und das SameSite-Attribut in Browsern, die `None`nicht unterstützen, angepasst.</span><span class="sxs-lookup"><span data-stu-id="f4599-122">You can change specific named cookie behavior in much the same way; the sample below adjust the default authentication cookie from `Lax` to `None` on browsers which support the `None` value, or removes the sameSite attribute on browsers which do not support `None`.</span></span>

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

## <a name="more-information"></a><span data-ttu-id="f4599-123">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="f4599-123">More Information</span></span>

[<span data-ttu-id="f4599-124">Chrome-Updates</span><span class="sxs-lookup"><span data-stu-id="f4599-124">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)

[<span data-ttu-id="f4599-125">ASP.NET-Dokumentation</span><span class="sxs-lookup"><span data-stu-id="f4599-125">ASP.NET Documentation</span></span>](/aspnet/samesite/system-web-samesite)

[<span data-ttu-id="f4599-126">.Net SameSite-Patches</span><span class="sxs-lookup"><span data-stu-id="f4599-126">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)