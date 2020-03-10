---
title: SameSite-Cookie-Beispiel für C# ASP.NET 4.7.2 MVC
author: blowdart
description: SameSite-Cookie-Beispiel für C# ASP.NET 4.7.2 MVC
ms.author: riande
ms.date: 2/15/2019
uid: samesite/csMVC
ms.openlocfilehash: dcbd0bee009669fb747d74e6ccef07fbae70a236
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78438201"
---
# <a name="samesite-cookie-sample-for-aspnet-472-c-mvc"></a><span data-ttu-id="b1f14-103">SameSite-Cookie-Beispiel für C# ASP.NET 4.7.2 MVC</span><span class="sxs-lookup"><span data-stu-id="b1f14-103">SameSite cookie sample for ASP.NET 4.7.2 C# MVC</span></span>

<span data-ttu-id="b1f14-104">.NET Framework 4,7 verfügt über eine integrierte Unterstützung für das [SameSite](https://www.owasp.org/index.php/SameSite) -Attribut, entspricht jedoch dem ursprünglichen Standard.</span><span class="sxs-lookup"><span data-stu-id="b1f14-104">.NET Framework 4.7 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it adheres to the original standard.</span></span>
<span data-ttu-id="b1f14-105">Das gepatchte Verhalten änderte die Bedeutung von `SameSite.None`, um das Attribut mit dem Wert `None`auszugeben, anstatt den Wert überhaupt auszugeben.</span><span class="sxs-lookup"><span data-stu-id="b1f14-105">The patched behavior changed the meaning of `SameSite.None` to emit the attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="b1f14-106">Wenn Sie den Wert nicht ausgeben möchten, können Sie die `SameSite`-Eigenschaft für ein Cookie auf-1 festlegen.</span><span class="sxs-lookup"><span data-stu-id="b1f14-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="b1f14-107">Schreiben des SameSite-Attributs</span><span class="sxs-lookup"><span data-stu-id="b1f14-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="b1f14-108">Im folgenden finden Sie ein Beispiel für das Schreiben eines SameSite-Attributs für ein Cookie.</span><span class="sxs-lookup"><span data-stu-id="b1f14-108">Following is an example of how to write a SameSite attribute on a cookie;</span></span>

```c#
// Create the cookie
HttpCookie sameSiteCookie = new HttpCookie("SameSiteSample");

// Set a value for the cookieSite none.
// Note this will also require you to be running on HTTPS
sameSiteCookie.Value = "sample";

// Set the secure flag, which Chrome's changes will require for Same
sameSiteCookie.Secure = true;

// Set the cookie to HTTP only which is good practice unless you really do need
// to access it client side in scripts.
sameSiteCookie.HttpOnly = true;

// Add the SameSite attribute, this will emit the attribute with a value of none.
// To not emit the attribute at all set the SameSite property to -1.
sameSiteCookie.SameSite = SameSiteMode.None;

// Add the cookie to the response cookie collection
Response.Cookies.Add(sameSiteCookie);
```

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="b1f14-109">Das standardmäßige SameSite-Attribut für den Sitzungszustand wird im Parameter "cookiesamesite" der Sitzungs Einstellungen in festgelegt `web.config`</span><span class="sxs-lookup"><span data-stu-id="b1f14-109">The default sameSite attribute for session state is set in the 'cookieSameSite' parameter of the session settings in `web.config`</span></span>

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a><span data-ttu-id="b1f14-110">MVC-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="b1f14-110">MVC Authentication</span></span>

<span data-ttu-id="b1f14-111">Die MVC-cookiebasierte Authentifizierung verwendet einen cookiemanager, um das Ändern von cookieattributen zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="b1f14-111">OWIN MVC cookie based authentication uses a cookie manager to enable the changing of cookie attributes.</span></span> <span data-ttu-id="b1f14-112">[SameSiteCookieManager.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieManager.cs) ist eine Implementierung einer solchen Klasse, die Sie in Ihre eigenen Projekte kopieren können.</span><span class="sxs-lookup"><span data-stu-id="b1f14-112">The [SameSiteCookieManager.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieManager.cs) is an implementation of such a class which you can copy into your own projects.</span></span> 

<span data-ttu-id="b1f14-113">Sie müssen sicherstellen, dass Ihre Microsoft. owin-Komponenten auf Version 4.1.0 oder höher aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="b1f14-113">You must ensure your Microsoft.Owin components are all upgraded to version 4.1.0 or greater.</span></span> <span data-ttu-id="b1f14-114">Überprüfen Sie Ihre `packages.config` Datei, um sicherzustellen, dass alle Versionsnummern identisch sind, z.b.</span><span class="sxs-lookup"><span data-stu-id="b1f14-114">Check your `packages.config` file to ensure all the version numbers match, for example.</span></span>

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

<span data-ttu-id="b1f14-115">Die Authentifizierungs Komponenten müssen dann so konfiguriert werden, dass Sie den cookiemanager in der Startup-Klasse verwenden.</span><span class="sxs-lookup"><span data-stu-id="b1f14-115">The authentication components must then be configured to use the CookieManager in your startup class;</span></span>

```c#
public void Configuration(IAppBuilder app)
{
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        CookieSameSite = SameSiteMode.None,
        CookieHttpOnly = true,
        CookieSecure = CookieSecureOption.Always,
        CookieManager = new SameSiteCookieManager(new SystemWebCookieManager())
    });
}
```

<span data-ttu-id="b1f14-116">Für *jede* Komponente, die diese unterstützt, muss ein cookiemanager festgelegt werden. dazu gehören cookieauthentication und openidconnectauthentication.</span><span class="sxs-lookup"><span data-stu-id="b1f14-116">A cookie manager must be set on *each* component that supports it, this includes CookieAuthentication and OpenIdConnectAuthentication.</span></span>

<span data-ttu-id="b1f14-117">Der systemwebcookiemanager wird verwendet, um [bekannte Probleme](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) bei der Integration von Antwort Cookies zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="b1f14-117">The SystemWebCookieManager is used to avoid [known issues](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) with response cookie integration.</span></span>

### <a name="running-the-sample"></a><span data-ttu-id="b1f14-118">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="b1f14-118">Running the sample</span></span>

<span data-ttu-id="b1f14-119">Wenn Sie das Beispiel Projekt ausführen, laden Sie Ihren Browser Debugger auf der ersten Seite, und verwenden Sie ihn, um die Cookie-Sammlung für die Website anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b1f14-119">If you run the sample project  load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span>
<span data-ttu-id="b1f14-120">Wenn Sie dies in Edge und Chrome tun möchten `F12` klicken Sie dann auf die Registerkarte `Application`, und klicken Sie im Abschnitt `Storage` unter der Option `Cookies` auf die Website-URL.</span><span class="sxs-lookup"><span data-stu-id="b1f14-120">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Liste der Browser Debugger-Cookies](sample/img/BrowserDebugger.png)

<span data-ttu-id="b1f14-122">In der obigen Abbildung sehen Sie, dass das Cookie, das durch das Beispiel erstellt wird, wenn Sie auf die Schaltfläche "Cookies erstellen" klicken, einen SameSite-Attribut Wert `Lax`aufweist, der mit dem im [Beispielcode](#sampleCode)festgelegten Wert übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="b1f14-122">You can see from the image above that the cookie created by the sample when you click the "Create Cookies" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="b1f14-123">Abfangen von Cookies, die Sie nicht steuern</span><span class="sxs-lookup"><span data-stu-id="b1f14-123">Intercepting cookies you do not control</span></span>

<span data-ttu-id="b1f14-124">.NET 4.5.2 hat ein neues Ereignis für das Abfangen des Schreibens von Headern, `Response.AddOnSendingHeaders`, eingeführt.</span><span class="sxs-lookup"><span data-stu-id="b1f14-124">.NET 4.5.2 introduced a new event for intercepting the writing of headers, `Response.AddOnSendingHeaders`.</span></span> <span data-ttu-id="b1f14-125">Dies kann verwendet werden, um Cookies abzufangen, bevor Sie an den Client Computer zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b1f14-125">This can be used to intercept cookies before they are returned to the client machine.</span></span> <span data-ttu-id="b1f14-126">Im Beispiel verknüpfen wir das Ereignis mit einer statischen Methode, die überprüft, ob der Browser die neuen SameSite-Änderungen unterstützt, und wenn nicht, ändert die Cookies so, dass das Attribut nicht ausgegeben wird, wenn der neue `None` Wert festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="b1f14-126">In the sample we wire up the event to a static method which checks whether the browser supports the new sameSite changes, and if not, changes the cookies to not emit the attribute if the new `None` value has been set.</span></span>

<span data-ttu-id="b1f14-127">In " [Global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/Global.asax.cs) " finden Sie ein Beispiel für das Einbinden des Ereignisses und [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieRewriter.cs) für ein Beispiel für die Behandlung des Ereignisses und das Anpassen des Cookies `sameSite` Attributs, das Sie in ihren eigenen Code kopieren können.</span><span class="sxs-lookup"><span data-stu-id="b1f14-127">See [global.asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/Global.asax.cs) for an example of hooking up the event and [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieRewriter.cs) for an example of handling the event and adjusting the cookie `sameSite` attribute which you can copy into your own code.</span></span>

```c#
public static void FilterSameSiteNoneForIncompatibleUserAgents(object sender)
{
    HttpApplication application = sender as HttpApplication;
    if (application != null)
    {
        var userAgent = application.Context.Request.UserAgent;
        if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
        {
            HttpContext.Current.Response.AddOnSendingHeaders(context =>
            {
                var cookies = context.Response.Cookies;
                for (var i = 0; i < cookies.Count; i++)
                {
                    var cookie = cookies[i];
                    if (cookie.SameSite == SameSiteMode.None)
                    {
                        cookie.SameSite = (SameSiteMode)(-1); // Unspecified
                    }
                }
            });
        }
    }
}
```

<span data-ttu-id="b1f14-128">Sie können das Verhalten bestimmter benannter Cookies auf die gleiche Weise ändern: Im folgenden Beispiel wird das Standard Authentifizierungs Cookie von `Lax` auf `None` in Browsern, die den `None`-Wert unterstützen, und das SameSite-Attribut in Browsern, die `None`nicht unterstützen, angepasst.</span><span class="sxs-lookup"><span data-stu-id="b1f14-128">You can change specific named cookie behavior in much the same way; the sample below adjust the default authentication cookie from `Lax` to `None` on browsers which support the `None` value, or removes the sameSite attribute on browsers which do not support `None`.</span></span>

```c#
public static void AdjustSpecificCookieSettings()
{
    HttpContext.Current.Response.AddOnSendingHeaders(context =>
    {
        var cookies = context.Response.Cookies;
        for (var i = 0; i < cookies.Count; i++)
        {
            var cookie = cookies[i]; 
            // Forms auth: ".ASPXAUTH"
            // Session: "ASP.NET_SessionId"
            if (string.Equals(".ASPXAUTH", cookie.Name, StringComparison.Ordinal))
            { 
                if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
                {
                    cookie.SameSite = -1;
                }
                else
                {
                    cookie.SameSite = SameSiteMode.None;
                }
                cookie.Secure = true;
            }
        }
    });
}
```

### <a name="more-information"></a><span data-ttu-id="b1f14-129">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="b1f14-129">More Information</span></span>
 
[<span data-ttu-id="b1f14-130">Chrome-Updates</span><span class="sxs-lookup"><span data-stu-id="b1f14-130">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)

[<span data-ttu-id="b1f14-131">Dokumentation zu owin SameSite</span><span class="sxs-lookup"><span data-stu-id="b1f14-131">OWIN SameSite Documentation</span></span>](/aspnet/samesite/owin-samesite)

[<span data-ttu-id="b1f14-132">ASP.NET-Dokumentation</span><span class="sxs-lookup"><span data-stu-id="b1f14-132">ASP.NET Documentation</span></span>](/aspnet/samesite/system-web-samesite)

[<span data-ttu-id="b1f14-133">.Net SameSite-Patches</span><span class="sxs-lookup"><span data-stu-id="b1f14-133">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)