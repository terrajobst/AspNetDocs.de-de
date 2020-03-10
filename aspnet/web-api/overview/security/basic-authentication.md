---
uid: web-api/overview/security/basic-authentication
title: Standard Authentifizierung in ASP.net-Web-API | Microsoft-Dokumentation
author: MikeWasson
description: Beschreibt die Verwendung der Standard Authentifizierung in ASP.net-Web-API.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1470bd4b5abd5199b9a5105973b053812d643351
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447633"
---
# <a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="b6a8c-103">Standard Authentifizierung in ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="b6a8c-103">Basic Authentication in ASP.NET Web API</span></span>

<span data-ttu-id="b6a8c-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b6a8c-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b6a8c-105">Die Standard Authentifizierung ist in [RFC 2617, http-Authentifizierung: Standard-und Digest-Zugriffs Authentifizierung](http://www.ietf.org/rfc/rfc2617.txt)definiert.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="b6a8c-106">Nachteile</span><span class="sxs-lookup"><span data-stu-id="b6a8c-106">Disadvantages</span></span>

- <span data-ttu-id="b6a8c-107">Benutzer Anmelde Informationen werden in der Anforderung gesendet.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="b6a8c-108">Anmelde Informationen werden als Klartext gesendet.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="b6a8c-109">Anmelde Informationen werden bei jeder Anforderung gesendet.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="b6a8c-110">Es gibt keine Möglichkeit, sich abzumelden, es sei denn, Sie beenden die Browsersitzung.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="b6a8c-111">Anfällig für Website übergreifende Anforderungs Fälschung (CSRF); erfordert Anti-CSRF-Measures.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="b6a8c-112">Vorteile</span><span class="sxs-lookup"><span data-stu-id="b6a8c-112">Advantages</span></span>

- <span data-ttu-id="b6a8c-113">Internet Standard.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-113">Internet standard.</span></span>
- <span data-ttu-id="b6a8c-114">Wird von allen wichtigen Browsern unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="b6a8c-115">Relativ einfaches Protokoll.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-115">Relatively simple protocol.</span></span>

<span data-ttu-id="b6a8c-116">Die Standard Authentifizierung funktioniert wie folgt:</span><span class="sxs-lookup"><span data-stu-id="b6a8c-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="b6a8c-117">Wenn eine Anforderung eine Authentifizierung erfordert, gibt der Server 401 (nicht autorisiert) zurück.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="b6a8c-118">Die Antwort enthält einen WWW-Authenticate-Header, der angibt, dass der Server die Standard Authentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="b6a8c-119">Der Client sendet eine weitere Anforderung mit den Client Anmelde Informationen im Autorisierungs Header.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="b6a8c-120">Die Anmelde Informationen werden als Zeichenfolge "Name: Password", Base64-codiert formatiert.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="b6a8c-121">Die Anmelde Informationen werden nicht verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="b6a8c-122">Die Standard Authentifizierung erfolgt im Kontext eines Bereichs.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="b6a8c-123">Der Server enthält den Namen des Bereichs im WWW-Authenticate-Header.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="b6a8c-124">Die Anmelde Informationen des Benutzers sind innerhalb dieses Bereichs gültig.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="b6a8c-125">Der genaue Bereich eines Bereichs wird vom Server definiert.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="b6a8c-126">Beispielsweise können Sie mehrere Bereiche definieren, um Ressourcen zu partitionieren.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="b6a8c-127">Da die Anmelde Informationen unverschlüsselt gesendet werden, ist die Standard Authentifizierung nur über HTTPS sicher.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="b6a8c-128">Siehe [Arbeiten mit SSL in der Web-API](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="b6a8c-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="b6a8c-129">Die Standard Authentifizierung ist auch anfällig für CSRF-Angriffe.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="b6a8c-130">Nachdem der Benutzer Anmelde Informationen eingegeben hat, sendet der Browser diese automatisch für die Dauer der Sitzung bei nachfolgenden Anforderungen an dieselbe Domäne.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="b6a8c-131">Dies schließt AJAX-Anforderungen ein.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-131">This includes AJAX requests.</span></span> <span data-ttu-id="b6a8c-132">Weitere Informationen finden Sie unter [verhindern von Angriffen für Website übergreifende Anforderungs Fälschung (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="b6a8c-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="b6a8c-133">Standard Authentifizierung mit IIS</span><span class="sxs-lookup"><span data-stu-id="b6a8c-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="b6a8c-134">IIS unterstützt die Standard Authentifizierung, aber es liegt ein Nachteil vor: der Benutzer wird anhand seiner Windows-Anmelde Informationen authentifiziert.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="b6a8c-135">Dies bedeutet, dass der Benutzer über ein Konto in der Domäne des Servers verfügen muss.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="b6a8c-136">Für eine öffentliche Website möchten Sie sich in der Regel bei einem ASP.net-Mitgliedschafts Anbieter authentifizieren.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="b6a8c-137">Legen Sie den Authentifizierungsmodus in der Datei "Web. config" Ihres ASP.NET-Projekts auf "Windows" fest, um die Standard Authentifizierung mit IIS zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="b6a8c-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="b6a8c-138">In diesem Modus verwendet IIS Windows-Anmelde Informationen, um sich zu authentifizieren.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="b6a8c-139">Außerdem müssen Sie die Standard Authentifizierung in IIS aktivieren.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="b6a8c-140">Wechseln Sie im IIS-Manager zur Ansicht Features, wählen Sie Authentifizierung aus, und aktivieren Sie die Standard Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="b6a8c-141">Fügen Sie in Ihrem Web-API-Projekt das `[Authorize]`-Attribut für alle Controller Aktionen hinzu, die eine Authentifizierung erfordern.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="b6a8c-142">Ein Client authentifiziert sich selbst, indem der Autorisierungs Header in der Anforderung festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="b6a8c-143">Dieser Schritt wird automatisch von Browser Clients durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="b6a8c-144">Nicht-Browser Clients müssen den-Header festlegen.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="b6a8c-145">Standard Authentifizierung mit benutzerdefinierter Mitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="b6a8c-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="b6a8c-146">Wie bereits erwähnt, verwendet die in IIS integrierte Standard Authentifizierung Windows-Anmelde Informationen.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="b6a8c-147">Dies bedeutet, dass Sie Konten für Ihre Benutzer auf dem Host Server erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="b6a8c-148">Allerdings werden Benutzerkonten bei einer Internetanwendung in der Regel in einer externen Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="b6a8c-149">Der folgende Code zeigt, wie ein HTTP-Modul, das die Standard Authentifizierung ausführt.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="b6a8c-150">Sie können problemlos einen ASP.net-Mitgliedschafts Anbieter einbinden, indem Sie die `CheckPassword`-Methode ersetzen, die in diesem Beispiel eine Dummy-Methode ist.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="b6a8c-151">In der Web-API 2 sollten Sie anstelle eines HTTP-Moduls einen [Authentifizierungs Filter](authentication-filters.md) oder eine [owin-Middleware](../../../aspnet/overview/owin-and-katana/index.md)schreiben.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="b6a8c-152">Fügen Sie der Datei "Web. config" im Abschnitt " **System. Webserver** " Folgendes hinzu, um das HTTP-Modul zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="b6a8c-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="b6a8c-153">Ersetzen Sie "yourassemblyname" durch den Namen der Assembly (ohne die DLL-Erweiterung).</span><span class="sxs-lookup"><span data-stu-id="b6a8c-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="b6a8c-154">Deaktivieren Sie andere Authentifizierungs Schemas, z. b. Formulare oder Windows-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="b6a8c-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
