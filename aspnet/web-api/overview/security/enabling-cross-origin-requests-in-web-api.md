---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Aktivieren von Ursprungs übergreifenden Anforderungen in ASP.net-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Zeigt, wie Cross-Origin Resource Sharing (cors) in ASP.net-Web-API unterstützt wird.
ms.author: riande
ms.date: 01/29/2019
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d3016d98fa6c3a55359c6dab0737407b29925f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447615"
---
# <a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="7a1ca-103">Aktivieren von Ursprungs übergreifenden Anforderungen in ASP.net-Web-API 2</span><span class="sxs-lookup"><span data-stu-id="7a1ca-103">Enable cross-origin requests in ASP.NET Web API 2</span></span>

<span data-ttu-id="7a1ca-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7a1ca-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="7a1ca-105">Die Browsersicherheit verhindert, dass eine Webseite AJAX-Anforderungen an eine andere Domäne richtet.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="7a1ca-106">Diese Einschränkung wird als *Richtlinie des gleichen Ursprungs* bezeichnet und verhindert, dass eine schädliche Website sensible Daten von einer anderen Website liest.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="7a1ca-107">Manchmal sollen andere Websites jedoch unter Umständen Ihre Web-API aufrufen können.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-107">However, sometimes you might want to let other sites call your web API.</span></span>
>
> <span data-ttu-id="7a1ca-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (cors) ist ein W3C-Standard, der es einem Server ermöglicht, die Richtlinie für denselben Ursprung zu lockern.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="7a1ca-109">Mit CORS kann ein Server explizit einige ursprungsübergreifende Anforderungen zulassen und andere ablehnen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="7a1ca-110">Cors ist sicherer und flexibler als frühere Techniken wie [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="7a1ca-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="7a1ca-111">In diesem Tutorial wird gezeigt, wie Sie cors in Ihrer Web-API-Anwendung aktivieren.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
>
> ## <a name="software-used-in-the-tutorial"></a><span data-ttu-id="7a1ca-112">Im Tutorial verwendete Software</span><span class="sxs-lookup"><span data-stu-id="7a1ca-112">Software used in the tutorial</span></span>
>
> - [<span data-ttu-id="7a1ca-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7a1ca-113">Visual Studio</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="7a1ca-114">Web-API 2,2</span><span class="sxs-lookup"><span data-stu-id="7a1ca-114">Web API 2.2</span></span>

## <a name="introduction"></a><span data-ttu-id="7a1ca-115">Einführung</span><span class="sxs-lookup"><span data-stu-id="7a1ca-115">Introduction</span></span>

<span data-ttu-id="7a1ca-116">In diesem Tutorial wird die cors-Unterstützung in ASP.net-Web-API veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="7a1ca-117">Zunächst erstellen wir zwei ASP.net-Projekte – eine mit dem Namen "Webservice", die einen Web-API-Controller hostet, und die andere mit dem Namen "Webclient", die Webservice aufruft.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="7a1ca-118">Da die beiden Anwendungen in verschiedenen Domänen gehostet werden, ist eine AJAX-Anforderung von WebClient an Webservice eine Ursprungs übergreifende Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="7a1ca-119">Was ist "derselbe Ursprung"?</span><span class="sxs-lookup"><span data-stu-id="7a1ca-119">What is "same origin"?</span></span>

<span data-ttu-id="7a1ca-120">Zwei URLs haben denselben Ursprung, wenn Sie identische Schemas, Hosts und Ports aufweisen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="7a1ca-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="7a1ca-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="7a1ca-122">Diese beiden URLs haben denselben Ursprung:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="7a1ca-123">Diese URLs haben andere Ursprünge als die vorherigen beiden:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="7a1ca-124">`http://example.net`-andere Domäne</span><span class="sxs-lookup"><span data-stu-id="7a1ca-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="7a1ca-125">`http://example.com:9000/foo.html`-anderer Port</span><span class="sxs-lookup"><span data-stu-id="7a1ca-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="7a1ca-126">`https://example.com/foo.html`-anderes Schema</span><span class="sxs-lookup"><span data-stu-id="7a1ca-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="7a1ca-127">`http://www.example.com/foo.html`-Unterdomäne</span><span class="sxs-lookup"><span data-stu-id="7a1ca-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="7a1ca-128">Internet Explorer berücksichtigt den Port beim Vergleich von Ursprüngen nicht.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-128">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="create-the-webservice-project"></a><span data-ttu-id="7a1ca-129">Erstellen des Webservice-Projekts</span><span class="sxs-lookup"><span data-stu-id="7a1ca-129">Create the WebService project</span></span>

> [!NOTE]
> <span data-ttu-id="7a1ca-130">In diesem Abschnitt wird davon ausgegangen, dass Sie bereits über das Erstellen von Web-API-Projekten</span><span class="sxs-lookup"><span data-stu-id="7a1ca-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="7a1ca-131">Falls nicht, finden Sie weitere Informationen unter [Getting Started with ASP.net-Web-API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="7a1ca-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>

1. <span data-ttu-id="7a1ca-132">Starten Sie Visual Studio, und erstellen Sie ein neues Projekt der **ASP.NET-Webanwendung (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="7a1ca-132">Start Visual Studio and create a new **ASP.NET Web Application (.NET Framework)** project.</span></span>
2. <span data-ttu-id="7a1ca-133">Wählen Sie im Dialogfeld **neue ASP.NET-Webanwendung** die Vorlage **leeres** Projekt aus.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-133">In the **New ASP.NET Web Application** dialog box, select the **Empty** project template.</span></span> <span data-ttu-id="7a1ca-134">Wählen Sie unter **Ordner und Kern Verweise hinzufügen für**das Kontrollkästchen **Web-API** aus.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-134">Under **Add folders and core references for**, select the **Web API** checkbox.</span></span>

   ![Dialogfeld "neues ASP.net Projekt" in Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. <span data-ttu-id="7a1ca-136">Fügen Sie einen Web-API-Controller mit dem Namen `TestController` mit folgendem Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-136">Add a Web API controller named `TestController` with the following code:</span></span>

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. <span data-ttu-id="7a1ca-137">Sie können die Anwendung lokal ausführen oder in Azure bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-137">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="7a1ca-138">(Für die Screenshots in diesem Tutorial wird die APP für Azure App Service Web-Apps bereitgestellt.) Um zu überprüfen, ob die Web-API funktioniert, navigieren Sie zu `http://hostname/api/test/`, wobei *Hostname* die Domäne ist, in der Sie die Anwendung bereitgestellt haben.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-138">(For the screenshots in this tutorial, the app deploys to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="7a1ca-139">Der Antworttext sollte angezeigt werden, &quot;Get: Test Message&quot;.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-139">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

   ![Webbrowser zeigt eine Test Meldung an](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a><span data-ttu-id="7a1ca-141">Erstellen des WebClient-Projekts</span><span class="sxs-lookup"><span data-stu-id="7a1ca-141">Create the WebClient project</span></span>

1. <span data-ttu-id="7a1ca-142">Erstellen Sie eine weitere **ASP.NET-Webanwendung (.NET Framework)** , und wählen Sie die **MVC** -Projektvorlage aus.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-142">Create another **ASP.NET Web Application (.NET Framework)** project and select the **MVC** project template.</span></span> <span data-ttu-id="7a1ca-143">Wählen Sie optional **Authentifizierung ändern** > **keine Authentifizierung**aus.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="7a1ca-144">Für dieses Tutorial ist keine Authentifizierung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-144">You don't need authentication for this tutorial.</span></span>

   ![MVC-Vorlage im Dialogfeld "neues ASP.net Projekt" in Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. <span data-ttu-id="7a1ca-146">Öffnen Sie in **Projektmappen-Explorer**die Datei *views/Home/Index. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-146">In **Solution Explorer**, open the file *Views/Home/Index.cshtml*.</span></span> <span data-ttu-id="7a1ca-147">Ersetzen Sie den Code in dieser Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-147">Replace the code in this file with the following:</span></span>

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   <span data-ttu-id="7a1ca-148">Verwenden Sie für die *serviceUrl* -Variable den URI der Webservice-app.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-148">For the *serviceUrl* variable, use the URI of the WebService app.</span></span>

3. <span data-ttu-id="7a1ca-149">Führen Sie die WebClient-App lokal aus, oder veröffentlichen Sie Sie auf einer anderen Website.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-149">Run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="7a1ca-150">Wenn Sie auf die Schaltfläche "ausprobieren" klicken, wird eine AJAX-Anforderung an die Webdienst-App übermittelt, die die im Dropdown Feld (Get, Post, Put) aufgeführte HTTP-Methode verwendet.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-150">When you click the "Try It" button, an AJAX request is submitted to the WebService app using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="7a1ca-151">Auf diese Weise können Sie verschiedene Ursprungs übergreifende Anforderungen untersuchen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-151">This lets you examine different cross-origin requests.</span></span> <span data-ttu-id="7a1ca-152">Zurzeit unterstützt die Webservice-App keine cors. Wenn Sie also auf die Schaltfläche klicken, wird eine Fehlermeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-152">Currently, the WebService app does not support CORS, so if you click the button you'll get an error.</span></span>

![Fehler "try it" im Browser](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="7a1ca-154">Wenn Sie den HTTP-Datenverkehr in einem Tool wie z. b. " [fddler](https://www.telerik.com/fiddler)" beobachten, werden Sie feststellen, dass der Browser die Get-Anforderung sendet und die Anforderung erfolgreich ist, aber der AJAX-Befehl gibt einen Fehler zurück.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-154">If you watch the HTTP traffic in a tool like [Fiddler](https://www.telerik.com/fiddler), you'll see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="7a1ca-155">Es ist wichtig zu verstehen, dass die Richtlinie für denselben Ursprung nicht verhindert, dass der Browser die Anforderung *sendet* .</span><span class="sxs-lookup"><span data-stu-id="7a1ca-155">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="7a1ca-156">Stattdessen wird verhindert, dass die Anwendung die *Antwort*sieht.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-156">Instead, it prevents the application from seeing the *response*.</span></span>

![Webdebugger für den webdebugger, der Webanforderungen anzeigt](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a><span data-ttu-id="7a1ca-158">Aktivieren von CORS</span><span class="sxs-lookup"><span data-stu-id="7a1ca-158">Enable CORS</span></span>

<span data-ttu-id="7a1ca-159">Nun aktivieren wir cors in der Webdienst-app.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-159">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="7a1ca-160">Fügen Sie zunächst das cors-nuget-Paket hinzu.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-160">First, add the CORS NuGet package.</span></span> <span data-ttu-id="7a1ca-161">Klicken Sie in Visual Studio im **Menü Extras** auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-161">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="7a1ca-162">Geben Sie im Fenster der Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-162">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="7a1ca-163">Mit diesem Befehl wird das neueste Paket installiert, und alle Abhängigkeiten, einschließlich der Web-API-Kernbibliotheken, werden aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-163">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="7a1ca-164">Verwenden Sie das `-Version` Flag, um eine bestimmte Version als Ziel zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-164">Use the `-Version` flag to target a specific version.</span></span> <span data-ttu-id="7a1ca-165">Für das cors-Paket ist die Web-API 2,0 oder höher erforderlich.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-165">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="7a1ca-166">Öffnen Sie die Datei *-App\_Start/webapiconfig. cs*.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-166">Open the file *App\_Start/WebApiConfig.cs*.</span></span> <span data-ttu-id="7a1ca-167">Fügen Sie der **webapiconfig. Register** -Methode den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-167">Add the following code to the **WebApiConfig.Register** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="7a1ca-168">Fügen Sie als nächstes das **[enablecors]** -Attribut zur `TestController`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-168">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="7a1ca-169">Verwenden Sie für den *Ursprungs* Parameter den URI, in dem Sie die WebClient Anwendung bereitgestellt haben.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-169">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="7a1ca-170">Dies ermöglicht Ursprungs übergreifende Anforderungen von WebClient, während alle anderen Domänen übergreifenden Anforderungen immer noch nicht zugelassen werden.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-170">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="7a1ca-171">Später beschreibe ich die Parameter für **[enablecors]** ausführlicher.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-171">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="7a1ca-172">Fügen Sie am Ende der *Ursprungs* -URL keinen Schrägstrich ein.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-172">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="7a1ca-173">Stellen Sie die aktualisierte Webservice-Anwendung erneut bereit.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-173">Redeploy the updated WebService application.</span></span> <span data-ttu-id="7a1ca-174">Sie müssen WebClient nicht aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-174">You don't need to update WebClient.</span></span> <span data-ttu-id="7a1ca-175">Die AJAX-Anforderung von WebClient sollte jetzt erfolgreich sein.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-175">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="7a1ca-176">Die Methoden get, Put und Post sind zulässig.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-176">The GET, PUT, and POST methods are all allowed.</span></span>

![Webbrowser zeigt eine erfolgreiche Testnachricht an](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a><span data-ttu-id="7a1ca-178">Funktionsweise von cors</span><span class="sxs-lookup"><span data-stu-id="7a1ca-178">How CORS Works</span></span>

<span data-ttu-id="7a1ca-179">In diesem Abschnitt wird beschrieben, was in einer cors-Anforderung auf der Ebene der HTTP-Nachrichten geschieht.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-179">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="7a1ca-180">Es ist wichtig, die Funktionsweise von cors zu verstehen, sodass Sie das Attribut **[enablecors]** ordnungsgemäß konfigurieren und beheben können, wenn die Dinge nicht erwartungsgemäß funktionieren.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-180">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="7a1ca-181">In der cors-Spezifikation werden mehrere neue HTTP-Header eingeführt, die Ursprungs übergreifende Anforderungen ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-181">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="7a1ca-182">Wenn ein Browser cors unterstützt, werden diese Header automatisch für Ursprungs übergreifende Anforderungen festgelegt. Sie müssen in Ihrem JavaScript-Code nichts Besonderes tun.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-182">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="7a1ca-183">Im folgenden finden Sie ein Beispiel für eine Ursprungs übergreifende Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-183">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="7a1ca-184">Der "Origin"-Header gibt die Domäne der Website an, von der die Anforderung stammt.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-184">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="7a1ca-185">Wenn der Server die Anforderung zulässt, wird der Access-Control-Allow-Origin-Header festgelegt.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-185">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="7a1ca-186">Der Wert dieses Headers entspricht entweder dem Ursprungs Header oder ist der Platzhalter Wert "\*", was bedeutet, dass ein beliebiger Ursprung zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-186">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="7a1ca-187">Wenn die Antwort den Access-Control-Allow-Origin-Header nicht einschließt, schlägt die AJAX-Anforderung fehl.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-187">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="7a1ca-188">Der Browser lässt die Anforderung insbesondere nicht zu.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-188">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="7a1ca-189">Auch wenn der Server eine erfolgreiche Antwort zurückgibt, stellt der Browser die Antwort nicht für die Client Anwendung zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-189">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="7a1ca-190">**Preflight-Anforderungen**</span><span class="sxs-lookup"><span data-stu-id="7a1ca-190">**Preflight Requests**</span></span>

<span data-ttu-id="7a1ca-191">Bei einigen cors-Anforderungen sendet der Browser eine zusätzliche Anforderung, die als "Preflight-Anforderung" bezeichnet wird, bevor die tatsächliche Anforderung für die Ressource gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-191">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="7a1ca-192">Der Browser kann die Preflight-Anforderung überspringen, wenn die folgenden Bedingungen zutreffen:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-192">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="7a1ca-193">Die Anforderungs Methode ist *Get, Head* oder Post.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-193">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="7a1ca-194">Die Anwendung legt keine anderen Anforderungs Header als "Accept", "Accept-Language", "Content-Language", "Content-Type" oder " Last-Event-ID" fest.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-194">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="7a1ca-195">Der Content-Type-Header (falls festgelegt) ist einer der folgenden:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-195">The Content-Type header (if set) is one of the following:</span></span>

    - <span data-ttu-id="7a1ca-196">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="7a1ca-196">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="7a1ca-197">Multipart/Form-Data</span><span class="sxs-lookup"><span data-stu-id="7a1ca-197">multipart/form-data</span></span>
    - <span data-ttu-id="7a1ca-198">text/plain</span><span class="sxs-lookup"><span data-stu-id="7a1ca-198">text/plain</span></span>

<span data-ttu-id="7a1ca-199">Die Regel für Anforderungs Header gilt für Header, die von der Anwendung festgelegt werden, indem **setrequestheader** für das **XMLHttpRequest** -Objekt aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-199">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="7a1ca-200">(In der cors-Spezifikation werden diese "Autoren Anforderungs Header" aufgerufen.) Die Regel gilt nicht für Header, die vom *Browser* festgelegt werden können, z. b. Benutzer-Agent, Host oder Inhalts Länge.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-200">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="7a1ca-201">Im folgenden finden Sie ein Beispiel für eine Preflight-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-201">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="7a1ca-202">Die Pre-Flight-Anforderung verwendet die HTTP OPTIONS-Methode.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-202">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="7a1ca-203">Es enthält zwei spezielle Header:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-203">It includes two special headers:</span></span>

- <span data-ttu-id="7a1ca-204">Access-Control-Request-Method: die HTTP-Methode, die für die tatsächliche Anforderung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-204">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="7a1ca-205">Access-Control-Request-Headers: eine Liste mit Anforderungs Headern, die von der *Anwendung* für die tatsächliche Anforderung festgelegt wurden.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-205">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="7a1ca-206">(Auch hier sind keine Header enthalten, die vom Browser festgelegt werden.)</span><span class="sxs-lookup"><span data-stu-id="7a1ca-206">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="7a1ca-207">Hier sehen Sie eine Beispiel Antwort, in der angenommen wird, dass der Server die Anforderung zulässt:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-207">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="7a1ca-208">Die Antwort enthält einen Access-Control-Allow-Methods-Header, der die zulässigen Methoden auflistet, und optional einen Access-Control-Allow-Headers-Header, der die zulässigen Header auflistet.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-208">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="7a1ca-209">Wenn die Preflight-Anforderung erfolgreich ist, sendet der Browser die tatsächliche Anforderung, wie zuvor beschrieben.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-209">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<span data-ttu-id="7a1ca-210">Tools, die häufig zum Testen von Endpunkten mit Preflight-Options Anforderungen (z. b. " [fddler](https://www.telerik.com/fiddler) " und [Postman](https://www.getpostman.com/)) verwendet werden, senden standardmäßig nicht die erforderlichen Options Header.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-210">Tools commonly used to test endpoints with preflight OPTIONS requests (for example, [Fiddler](https://www.telerik.com/fiddler) and [Postman](https://www.getpostman.com/)) don't send the required OPTIONS headers by default.</span></span> <span data-ttu-id="7a1ca-211">Vergewissern Sie sich, dass die Header "`Access-Control-Request-Method`" und "`Access-Control-Request-Headers`" mit der Anforderung gesendet werden und dass die Options Header die APP über IIS erreichen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-211">Confirm that the `Access-Control-Request-Method` and `Access-Control-Request-Headers` headers are sent with the request and that OPTIONS headers reach the app through IIS.</span></span>

<span data-ttu-id="7a1ca-212">Um IIS so zu konfigurieren, dass eine ASP.net-App Options Anforderungen empfangen und verarbeiten kann, fügen Sie der Datei *Web. config* der APP im Abschnitt `<system.webServer><handlers>` die folgende Konfiguration hinzu:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-212">To configure IIS to allow an ASP.NET app to receive and handle OPTION requests, add the following configuration to the app's *web.config* file in the `<system.webServer><handlers>` section:</span></span>

```xml
<system.webServer>
  <handlers>
    <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
    <remove name="OPTIONSVerbHandler" />
    <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
  </handlers>
</system.webServer>
```

<span data-ttu-id="7a1ca-213">Durch das Entfernen von `OPTIONSVerbHandler` wird verhindert, dass IIS Options Anforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-213">The removal of `OPTIONSVerbHandler` prevents IIS from handling OPTIONS requests.</span></span> <span data-ttu-id="7a1ca-214">Durch die Ersetzung von `ExtensionlessUrlHandler-Integrated-4.0` können Options Anforderungen die APP erreichen, da die Standardmodul Registrierung nur Get-, Head-, Post-und Debug-Anforderungen mit Erweiterungs losen URLs zulässt.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-214">The replacement of `ExtensionlessUrlHandler-Integrated-4.0` allows OPTIONS requests to reach the app because the default module registration only allows GET, HEAD, POST, and DEBUG requests with extensionless URLs.</span></span>

## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="7a1ca-215">Bereichs Regeln für [enablecors]</span><span class="sxs-lookup"><span data-stu-id="7a1ca-215">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="7a1ca-216">Sie können cors pro Aktion, pro Controller oder global für alle Web-API-Controller in der Anwendung aktivieren.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-216">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="7a1ca-217">**Pro Aktion**</span><span class="sxs-lookup"><span data-stu-id="7a1ca-217">**Per Action**</span></span>

<span data-ttu-id="7a1ca-218">Um cors für eine einzelne Aktion zu aktivieren, legen Sie das **[enablecors]** -Attribut für die Aktionsmethode fest.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-218">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="7a1ca-219">Im folgenden Beispiel werden cors nur für die `GetItem`-Methode aktiviert.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-219">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="7a1ca-220">**Pro Controller**</span><span class="sxs-lookup"><span data-stu-id="7a1ca-220">**Per Controller**</span></span>

<span data-ttu-id="7a1ca-221">Wenn Sie **[enablecors]** für die Controller Klasse festlegen, gilt dies für alle Aktionen auf dem Controller.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-221">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="7a1ca-222">Wenn Sie cors für eine Aktion deaktivieren möchten, fügen Sie der Aktion das Attribut **[disablecors]** hinzu.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-222">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="7a1ca-223">Im folgenden Beispiel werden cors für jede Methode außer `PutItem`aktiviert.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-223">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="7a1ca-224">**Global**</span><span class="sxs-lookup"><span data-stu-id="7a1ca-224">**Globally**</span></span>

<span data-ttu-id="7a1ca-225">Wenn Sie cors für alle Web-API-Controller in der Anwendung aktivieren möchten, übergeben Sie eine **enablecorsattribute** -Instanz an die **enablecors** -Methode:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-225">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="7a1ca-226">Wenn Sie das Attribut auf mehr als einen Bereich festlegen, ist die Rangfolge wie folgt:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-226">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="7a1ca-227">Aktion</span><span class="sxs-lookup"><span data-stu-id="7a1ca-227">Action</span></span>
2. <span data-ttu-id="7a1ca-228">Controller</span><span class="sxs-lookup"><span data-stu-id="7a1ca-228">Controller</span></span>
3. <span data-ttu-id="7a1ca-229">Global</span><span class="sxs-lookup"><span data-stu-id="7a1ca-229">Global</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="7a1ca-230">Festlegen der zulässigen Ursprünge</span><span class="sxs-lookup"><span data-stu-id="7a1ca-230">Set the allowed origins</span></span>

<span data-ttu-id="7a1ca-231">Der *Ursprungs* Parameter des **[enablecors]** -Attributs gibt an, welche Ursprünge auf die Ressource zugreifen dürfen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-231">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="7a1ca-232">Der Wert ist eine durch Trennzeichen getrennte Liste der zulässigen Ursprünge.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-232">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="7a1ca-233">Sie können auch den Platzhalter Wert "\*" verwenden, um Anforderungen von beliebigen Ursprüngen zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-233">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="7a1ca-234">Berücksichtigen Sie sorgfältig, bevor Sie Anforderungen von einem beliebigen Ursprung zulassen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-234">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="7a1ca-235">Dies bedeutet, dass jede Website eine AJAX-Aufrufe an Ihre Web-API durchführen kann.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-235">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="7a1ca-236">Festlegen der zulässigen HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="7a1ca-236">Set the allowed HTTP methods</span></span>

<span data-ttu-id="7a1ca-237">Der *Methods* -Parameter des **[enablecors]** -Attributs gibt an, welche HTTP-Methoden für den Zugriff auf die Ressource zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-237">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="7a1ca-238">Um alle Methoden zuzulassen, verwenden Sie den Platzhalter Wert "\*".</span><span class="sxs-lookup"><span data-stu-id="7a1ca-238">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="7a1ca-239">Im folgenden Beispiel werden nur Get-und Post-Anforderungen ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-239">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="7a1ca-240">Festlegen der zulässigen Anforderungs Header</span><span class="sxs-lookup"><span data-stu-id="7a1ca-240">Set the allowed request headers</span></span>

<span data-ttu-id="7a1ca-241">In diesem Artikel wurde beschrieben, wie eine Preflight-Anforderung einen Access-Control-Request-Headers-Header enthalten kann, der die von der Anwendung festgelegten HTTP-Header auflistet (die so genannte "Autor Request Headers").</span><span class="sxs-lookup"><span data-stu-id="7a1ca-241">This article described earlier how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="7a1ca-242">Der *Headers* -Parameter des **[enablecors]** -Attributs gibt an, welche Autoren Anforderungs Header zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-242">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="7a1ca-243">Um Header zuzulassen, legen Sie *Header* auf "\*" fest.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-243">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="7a1ca-244">Um bestimmte Header in eine Whitelist aufzunehmen, legen Sie *Header* auf eine durch Trennzeichen getrennte Liste der zulässigen Header fest:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-244">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="7a1ca-245">Allerdings sind Browser nicht vollständig konsistent, wenn Sie Access-Control-Request-Headers festlegen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-245">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="7a1ca-246">Chrome enthält z. b. aktuell "Origin".</span><span class="sxs-lookup"><span data-stu-id="7a1ca-246">For example, Chrome currently includes "origin".</span></span> <span data-ttu-id="7a1ca-247">Firefox umfasst keine Standard Header wie "Accept", auch wenn Sie von der Anwendung im Skript festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-247">FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="7a1ca-248">Wenn Sie *Header* auf einen anderen Wert als "\*" festlegen, sollten Sie mindestens "Accept", "Content-Type" und "Origin" sowie alle benutzerdefinierten Header einschließen, die Sie unterstützen möchten.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-248">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="7a1ca-249">Festlegen der zulässigen Antwortheader</span><span class="sxs-lookup"><span data-stu-id="7a1ca-249">Set the allowed response headers</span></span>

<span data-ttu-id="7a1ca-250">Standardmäßig macht der Browser nicht alle Antwortheader für die Anwendung verfügbar.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-250">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="7a1ca-251">Die standardmäßig verfügbaren Antwortheader lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-251">The response headers that are available by default are:</span></span>

- <span data-ttu-id="7a1ca-252">Cachesteuerung</span><span class="sxs-lookup"><span data-stu-id="7a1ca-252">Cache-Control</span></span>
- <span data-ttu-id="7a1ca-253">Inhaltssprache</span><span class="sxs-lookup"><span data-stu-id="7a1ca-253">Content-Language</span></span>
- <span data-ttu-id="7a1ca-254">Content-Type</span><span class="sxs-lookup"><span data-stu-id="7a1ca-254">Content-Type</span></span>
- <span data-ttu-id="7a1ca-255">Läuft ab</span><span class="sxs-lookup"><span data-stu-id="7a1ca-255">Expires</span></span>
- <span data-ttu-id="7a1ca-256">Zuletzt geändert</span><span class="sxs-lookup"><span data-stu-id="7a1ca-256">Last-Modified</span></span>
- <span data-ttu-id="7a1ca-257">Pragma</span><span class="sxs-lookup"><span data-stu-id="7a1ca-257">Pragma</span></span>

<span data-ttu-id="7a1ca-258">In der cors-Spezifikation werden diese [einfachen Antwortheader](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header)aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-258">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="7a1ca-259">Um andere Header der Anwendung zur Verfügung zu stellen, legen Sie den *exposedheaders* -Parameter von **[enablecors]** fest.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-259">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="7a1ca-260">Im folgenden Beispiel legt die `Get`-Methode des Controllers einen benutzerdefinierten Header mit dem Namen "X-Custom-Header" fest.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-260">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="7a1ca-261">Standardmäßig macht der Browser diesen Header nicht in einer Ursprungs übergreifenden Anforderung verfügbar.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-261">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="7a1ca-262">Um den Header verfügbar zu machen, schließen Sie "X-Custom-Header" in *exposedheaders*ein.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-262">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a><span data-ttu-id="7a1ca-263">Übergeben von Anmelde Informationen in Ursprungs übergreifenden Anforderungen</span><span class="sxs-lookup"><span data-stu-id="7a1ca-263">Pass credentials in cross-origin requests</span></span>

<span data-ttu-id="7a1ca-264">Anmelde Informationen erfordern eine spezielle Behandlung in einer cors-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-264">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="7a1ca-265">Standardmäßig sendet der Browser keine Anmelde Informationen mit einer Ursprungs übergreifenden Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-265">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="7a1ca-266">Anmelde Informationen enthalten Cookies und http-Authentifizierungs Schemas.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-266">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="7a1ca-267">Um Anmelde Informationen mit einer Ursprungs übergreifenden Anforderung zu senden, muss der Client **XMLHttpRequest. withanmelde** Informationen auf "true" festlegen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-267">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="7a1ca-268">Direktes Verwenden von " **XMLHttpRequest** ":</span><span class="sxs-lookup"><span data-stu-id="7a1ca-268">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="7a1ca-269">In jQuery:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-269">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="7a1ca-270">Außerdem muss der Server die Anmelde Informationen zulassen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-270">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="7a1ca-271">Um Ursprungs übergreifende Anmelde Informationen in der Web-API zuzulassen, legen Sie für das Attribut **[enablecors]** die **supportscredenatyp** -Eigenschaft auf true fest:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-271">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="7a1ca-272">Wenn diese Eigenschaft true ist, enthält die HTTP-Antwort einen Access-Control-Allow-Anmelde Header.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-272">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="7a1ca-273">Dieser Header weist den Browser an, dass der Server Anmelde Informationen für eine Ursprungs übergreifende Anforderung zulässt.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-273">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="7a1ca-274">Wenn der Browser Anmelde Informationen sendet, die Antwort jedoch keinen gültigen Header "Access-Control-Allow-Anmelde Informationen" enthält, macht der Browser die Antwort an die Anwendung nicht verfügbar, und die AJAX-Anforderung schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-274">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="7a1ca-275">Beachten Sie, dass **supportscredenatyp** auf true festgelegt wird, da es eine Website in einer anderen Domäne bedeutet, dass die Anmelde Informationen des angemeldeten Benutzers im Auftrag des Benutzers an Ihre Web-API gesendet werden können, ohne dass der Benutzer sich bewusst ist.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-275">Be careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="7a1ca-276">In der cors-Spezifikation wird auch festgelegt, dass das Festlegen von *Ursprüngen* auf &quot;\*&quot; ungültig ist, wenn **supportscredenseins** true ist.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-276">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

## <a name="custom-cors-policy-providers"></a><span data-ttu-id="7a1ca-277">Benutzerdefinierte cors-Richtlinien Anbieter</span><span class="sxs-lookup"><span data-stu-id="7a1ca-277">Custom CORS policy providers</span></span>

<span data-ttu-id="7a1ca-278">Das **[enablecors]** -Attribut implementiert die **icorspolicyprovider** -Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-278">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="7a1ca-279">Sie können Ihre eigene Implementierung bereitstellen, indem Sie eine Klasse erstellen, die vom- **Attribut** abgeleitet ist und **icorspolicyprovider**implementiert.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-279">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsPolicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="7a1ca-280">Nun können Sie das Attribut an jedem Ort anwenden, den Sie **[enablecors]** einfügen würden.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-280">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="7a1ca-281">Ein benutzerdefinierter cors-Richtlinien Anbieter könnte z. b. die Einstellungen aus einer Konfigurationsdatei lesen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-281">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="7a1ca-282">Als Alternative zur Verwendung von Attributen können Sie ein **icorspolicyproviderfactory** -Objekt registrieren, das **icorspolicyprovider** -Objekte erstellt.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-282">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="7a1ca-283">Um **icorspolicyproviderfactory**festzulegen, nennen Sie die **setcorspolicyproviderfactory** -Erweiterungsmethode beim Start wie folgt:</span><span class="sxs-lookup"><span data-stu-id="7a1ca-283">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a><span data-ttu-id="7a1ca-284">Browserunterstützung</span><span class="sxs-lookup"><span data-stu-id="7a1ca-284">Browser support</span></span>

<span data-ttu-id="7a1ca-285">Das Web-API-cors-Paket ist eine serverseitige Technologie.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-285">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="7a1ca-286">Der Browser des Benutzers muss auch cors unterstützen.</span><span class="sxs-lookup"><span data-stu-id="7a1ca-286">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="7a1ca-287">Glücklicherweise beinhalten die aktuellen Versionen aller wichtigen Browser [Unterstützung für cors](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="7a1ca-287">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>
