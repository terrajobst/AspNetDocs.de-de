---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Was ist in ASP.net nicht zu tun? Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Thema werden einige häufige Fehler beschrieben, die Benutzer innerhalb von ASP.NET-Webprojekten treffen. Hier finden Sie Empfehlungen dazu, was Sie tun sollten, um diese Kommas zu vermeiden...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 980d3544df70643043391e6573803ce21b3a824f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500133"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="ec00a-104">Häufige Fehler bei ASP.NET und empfohlene Vorgehensweisen</span><span class="sxs-lookup"><span data-stu-id="ec00a-104">What not to do in ASP.NET, and what to do instead</span></span>

> <span data-ttu-id="ec00a-105">In diesem Thema werden einige häufige Fehler beschrieben, die Benutzer innerhalb von ASP.NET-Webprojekten treffen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-105">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="ec00a-106">Hier finden Sie Empfehlungen dazu, was Sie tun sollten, um diese häufigen Fehler zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-106">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="ec00a-107">Es basiert auf einer [Präsentation](http://vimeo.com/68390507) von **Damian Edwards** auf der norwegischen Entwicklerkonferenz.</span><span class="sxs-lookup"><span data-stu-id="ec00a-107">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>

## <a name="disclaimer"></a><span data-ttu-id="ec00a-108">Haftungsausschluss</span><span class="sxs-lookup"><span data-stu-id="ec00a-108">Disclaimer</span></span>

<span data-ttu-id="ec00a-109">Dieses Thema ist nicht als kompletter Leitfaden gedacht, um sicherzustellen, dass Ihre Anwendung sicher und effizient ist.</span><span class="sxs-lookup"><span data-stu-id="ec00a-109">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="ec00a-110">Sie müssen weiterhin bewährte Methoden für Sicherheit und Leistung befolgen, die in diesem Thema nicht beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-110">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="ec00a-111">Es wird nur vorgeschlagen, häufige Fehler in Bezug auf .NET-Klassen und-Prozesse zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-111">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="ec00a-112">Übersicht</span><span class="sxs-lookup"><span data-stu-id="ec00a-112">Overview</span></span>

<span data-ttu-id="ec00a-113">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="ec00a-113">This topic contains the following sections:</span></span>

- [<span data-ttu-id="ec00a-114">Einhaltung von Standards</span><span class="sxs-lookup"><span data-stu-id="ec00a-114">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="ec00a-115">Steuerelement Adapter</span><span class="sxs-lookup"><span data-stu-id="ec00a-115">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="ec00a-116">Stileigenschaften für Steuerelemente</span><span class="sxs-lookup"><span data-stu-id="ec00a-116">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="ec00a-117">Seiten-und Steuerelement Rückrufe</span><span class="sxs-lookup"><span data-stu-id="ec00a-117">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="ec00a-118">Erkennung von Browser Funktionen</span><span class="sxs-lookup"><span data-stu-id="ec00a-118">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="ec00a-119">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="ec00a-119">Security</span></span>](#security)

    - [<span data-ttu-id="ec00a-120">Anforderungs Validierung</span><span class="sxs-lookup"><span data-stu-id="ec00a-120">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="ec00a-121">Formular Authentifizierung und Sitzung ohne cookielose Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="ec00a-121">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="ec00a-122">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="ec00a-122">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="ec00a-123">Mittlere Vertrauensstellung</span><span class="sxs-lookup"><span data-stu-id="ec00a-123">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="ec00a-124">&lt;appSettings-&gt;</span><span class="sxs-lookup"><span data-stu-id="ec00a-124">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="ec00a-125">Urlpathcodieren</span><span class="sxs-lookup"><span data-stu-id="ec00a-125">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="ec00a-126">Zuverlässigkeit und Leistung</span><span class="sxs-lookup"><span data-stu-id="ec00a-126">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="ec00a-127">PreSendRequestHeaders und PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="ec00a-127">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="ec00a-128">Asynchrone Seiten Ereignisse mit Web Forms</span><span class="sxs-lookup"><span data-stu-id="ec00a-128">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="ec00a-129">Fire-and-Forget-Arbeit</span><span class="sxs-lookup"><span data-stu-id="ec00a-129">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="ec00a-130">Anforderungs Entitäts Text</span><span class="sxs-lookup"><span data-stu-id="ec00a-130">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="ec00a-131">Response. Redirect und Response. End</span><span class="sxs-lookup"><span data-stu-id="ec00a-131">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="ec00a-132">EnableViewState und ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="ec00a-132">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="ec00a-133">Sqlmitgliedshipprovider</span><span class="sxs-lookup"><span data-stu-id="ec00a-133">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="ec00a-134">Anforderungen mit langer Ausführungszeit (> 110 Sekunden)</span><span class="sxs-lookup"><span data-stu-id="ec00a-134">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="ec00a-135">Einhaltung von Standards</span><span class="sxs-lookup"><span data-stu-id="ec00a-135">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="ec00a-136">Steuerelement Adapter</span><span class="sxs-lookup"><span data-stu-id="ec00a-136">Control adapters</span></span>

<span data-ttu-id="ec00a-137">Empfehlung: Verwenden Sie für das adaptive Rendering nicht die Verwendung von Steuerelement Adaptern, und verwenden Sie stattdessen CSS-Medien Abfragen und standardkonforme HTML.</span><span class="sxs-lookup"><span data-stu-id="ec00a-137">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="ec00a-138">Steuerelement Adapter wurden in .NET 2,0 eingeführt, um Präsentationscode zu rendieren, der für verschiedene Geräte und Umgebungen angepasst wurde.</span><span class="sxs-lookup"><span data-stu-id="ec00a-138">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="ec00a-139">Jetzt kann dieses adaptive Rendering mit CSS und HTML erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-139">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="ec00a-140">Sie sollten die Verwendung von Steuerelement Adaptern nicht mehr unterbinden und vorhandene Adapter in CSS und HTML konvertieren.</span><span class="sxs-lookup"><span data-stu-id="ec00a-140">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="ec00a-141">Weitere Informationen finden Sie unter [Medien Abfragen](http://www.w3.org/TR/css3-mediaqueries/) und Gewusst [wie: Hinzufügen von mobilen Seiten zu Ihrer ASP.net Web Forms/MVC-Anwendung](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="ec00a-141">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="ec00a-142">Stileigenschaften für Steuerelemente</span><span class="sxs-lookup"><span data-stu-id="ec00a-142">Style properties on controls</span></span>

<span data-ttu-id="ec00a-143">Empfehlung: setzen Sie das Festlegen von Stilwerten im Steuerelement Markup aus, und legen Sie stattdessen Formatierungs Werte in CSS-Stylesheets fest.</span><span class="sxs-lookup"><span data-stu-id="ec00a-143">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="ec00a-144">Webserver Steuerelemente enthalten Dutzende von Eigenschaften, die verwendet werden können, um Eigenschaften im Linienstil festzulegen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-144">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="ec00a-145">Beispielsweise legt die ForeColor-Eigenschaft die Textfarbe für ein Steuerelement fest.</span><span class="sxs-lookup"><span data-stu-id="ec00a-145">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="ec00a-146">Sie können diese Wirkung effizienter über CSS-Stylesheets erreichen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-146">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="ec00a-147">Stylesheets ermöglichen es Ihnen, Stilwerte zu zentralisieren und diese Werte nicht in der gesamten Anwendung festzulegen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-147">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="ec00a-148">Das folgende Beispiel zeigt eine CSS-Klasse, die den Text auf "rot" setzt.</span><span class="sxs-lookup"><span data-stu-id="ec00a-148">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="ec00a-149">Im nächsten Beispiel wird gezeigt, wie die CSS-Klasse dynamisch angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="ec00a-149">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="ec00a-150">Seiten-und Steuerelement Rückrufe</span><span class="sxs-lookup"><span data-stu-id="ec00a-150">Page and control callbacks</span></span>

<span data-ttu-id="ec00a-151">Empfehlung: Verwenden Sie die Seiten-und Steuerelement Rückrufe nicht mehr, und verwenden Sie stattdessen eine der folgenden Methoden: AJAX, UpdatePanel, MVC-Aktionsmethoden, Web-API oder signalr.</span><span class="sxs-lookup"><span data-stu-id="ec00a-151">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="ec00a-152">In früheren Versionen von ASP.net ermöglichten die Rückruf Methoden "page" und "Control" das Aktualisieren eines Teils der Webseite, ohne eine ganze Seite zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="ec00a-152">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="ec00a-153">Sie können jetzt partielle Seiten Aktualisierungen über [AJAX](../../../ajax/index.md), [Update Panel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web-API](../../../web-api/index.md) oder [signalr](../../../signalr/index.md)ausführen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-153">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="ec00a-154">Sie sollten die Rückruf Methoden nicht mehr verwenden, da Sie Probleme mit freundlichen URLs und Routing verursachen können.</span><span class="sxs-lookup"><span data-stu-id="ec00a-154">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="ec00a-155">Standardmäßig aktivieren Steuerelemente keine Rückruf Methoden, aber wenn Sie diese Funktion in einem-Steuerelement aktiviert haben, sollten Sie Sie deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="ec00a-155">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="ec00a-156">Erkennung von Browser Funktionen</span><span class="sxs-lookup"><span data-stu-id="ec00a-156">Browser capability detection</span></span>

<span data-ttu-id="ec00a-157">Empfehlung: Verwenden Sie die statische Erkennung von Browserfunktionen nicht mehr, sondern verwenden Sie stattdessen die Erkennung dynamischer Features.</span><span class="sxs-lookup"><span data-stu-id="ec00a-157">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="ec00a-158">In früheren Versionen von ASP.net wurden die unterstützten Funktionen für jeden Browser in einer XML-Datei gespeichert.</span><span class="sxs-lookup"><span data-stu-id="ec00a-158">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="ec00a-159">Das Erkennen von Funktions Unterstützung durch eine statische Suche ist nicht der beste Ansatz.</span><span class="sxs-lookup"><span data-stu-id="ec00a-159">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="ec00a-160">Nun können Sie die unterstützten Funktionen eines Browsers dynamisch mithilfe eines Features zur Funktionserkennung wie [modernizr](http://modernizr.com/)erkennen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-160">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="ec00a-161">Die Funktionserkennung bestimmt die Unterstützung, indem versucht wird, eine Methode oder Eigenschaft zu verwenden und dann zu überprüfen, ob der Browser das gewünschte Ergebnis erzeugt hat.</span><span class="sxs-lookup"><span data-stu-id="ec00a-161">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="ec00a-162">Modernizr ist standardmäßig in den Webanwendungs Vorlagen enthalten.</span><span class="sxs-lookup"><span data-stu-id="ec00a-162">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="ec00a-163">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="ec00a-163">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="ec00a-164">Anforderungsvalidierung</span><span class="sxs-lookup"><span data-stu-id="ec00a-164">Request validation</span></span>

<span data-ttu-id="ec00a-165">Empfehlung: Überprüfen Sie die Benutzereingaben, und codieren Sie die Ausgabe von Benutzern.</span><span class="sxs-lookup"><span data-stu-id="ec00a-165">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="ec00a-166">Die Anforderungs Validierung ist eine Funktion von ASP.net, die jede Anforderung überprüft und die Anforderung beendet, wenn eine erkannte Bedrohung gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="ec00a-166">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="ec00a-167">Richten Sie sich nicht nach der Anforderungs Überprüfung, um Ihre Anwendung vor Site übergreifenden Skript Angriffen zu schützen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-167">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="ec00a-168">Überprüfen Sie stattdessen alle Eingaben von Benutzern, und codieren Sie die Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="ec00a-168">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="ec00a-169">In einigen eingeschränkten Fällen können Sie reguläre Ausdrücke verwenden, um die Eingabe zu validieren, aber in komplizierteren Fällen sollten Sie die Benutzereingabe mithilfe von .NET-Klassen validieren, die bestimmen, ob der Wert mit zulässigen Werten übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="ec00a-169">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="ec00a-170">Im folgenden Beispiel wird gezeigt, wie eine statische Methode in der URI-Klasse verwendet wird, um zu bestimmen, ob der von einem Benutzer bereitgestellte URI gültig ist.</span><span class="sxs-lookup"><span data-stu-id="ec00a-170">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="ec00a-171">Um jedoch den URI ausreichend zu überprüfen, sollten Sie auch überprüfen, ob `http` oder `https`angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="ec00a-171">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="ec00a-172">Im folgenden Beispiel wird Instanzmethoden verwendet, um zu überprüfen, ob der URI gültig ist.</span><span class="sxs-lookup"><span data-stu-id="ec00a-172">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="ec00a-173">Bevor Sie Benutzereingaben als HTML-Code oder Benutzereingaben in eine SQL-Abfrage einschließen, codieren Sie die Werte, um sicherzustellen, dass bösartiger Code nicht eingeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="ec00a-173">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="ec00a-174">Sie können den Wert im Markup mit der &lt;%:%&gt; Syntax in HTML codieren, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="ec00a-174">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="ec00a-175">In Razor-Syntax können Sie die Codierung auch mit @ codieren, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="ec00a-175">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="ec00a-176">Im nächsten Beispiel wird gezeigt, wie Sie einen Wert im Code Behind in HTML codieren.</span><span class="sxs-lookup"><span data-stu-id="ec00a-176">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="ec00a-177">Verwenden Sie Befehlsparameter wie [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx), um einen Wert für SQL-Befehle sicher zu codieren.</span><span class="sxs-lookup"><span data-stu-id="ec00a-177">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="ec00a-178">Formular Authentifizierung und Sitzung ohne cookielose Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="ec00a-178">Cookieless forms authentication and session</span></span>

<span data-ttu-id="ec00a-179">Empfehlung: Cookies erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ec00a-179">Recommendation: Require cookies.</span></span>

<span data-ttu-id="ec00a-180">Das übergeben von Authentifizierungsinformationen in der Abfrage Zeichenfolge ist nicht sicher.</span><span class="sxs-lookup"><span data-stu-id="ec00a-180">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="ec00a-181">Daher sind Cookies erforderlich, wenn Ihre Anwendung die Authentifizierung einschließt.</span><span class="sxs-lookup"><span data-stu-id="ec00a-181">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="ec00a-182">Wenn Ihr Cookie vertrauliche Informationen speichert, sollten Sie SSL für das Cookie benötigen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-182">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="ec00a-183">Im folgenden Beispiel wird gezeigt, wie in der Datei "Web. config" angegeben wird, dass die Formular Authentifizierung ein Cookie erfordert, das über SSL übertragen wird.</span><span class="sxs-lookup"><span data-stu-id="ec00a-183">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="ec00a-184">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="ec00a-184">EnableViewStateMac</span></span>

<span data-ttu-id="ec00a-185">Empfehlung: nie auf false festgelegt.</span><span class="sxs-lookup"><span data-stu-id="ec00a-185">Recommendation: Never set to false.</span></span>

<span data-ttu-id="ec00a-186">Standardmäßig ist enableviewstatuemac auf true festgelegt.</span><span class="sxs-lookup"><span data-stu-id="ec00a-186">By default, EnableViewStateMac is set to true.</span></span> <span data-ttu-id="ec00a-187">Auch wenn Ihre Anwendung keinen Ansichts Zustand verwendet, legen Sie enableViewStateMac nicht auf false fest.</span><span class="sxs-lookup"><span data-stu-id="ec00a-187">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="ec00a-188">Wenn Sie diesen Wert auf false festlegen, wird Ihre Anwendung anfällig für Website übergreifende Skripts.</span><span class="sxs-lookup"><span data-stu-id="ec00a-188">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="ec00a-189">Beginnend mit ASP.NET 4.5.2 erzwingt die Runtime **enableviewstatuemac = true**.</span><span class="sxs-lookup"><span data-stu-id="ec00a-189">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="ec00a-190">Auch wenn Sie den Wert auf false festlegen, wird dieser Wert von der Laufzeit ignoriert, und der Wert wird auf true festgelegt.</span><span class="sxs-lookup"><span data-stu-id="ec00a-190">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="ec00a-191">Weitere Informationen finden Sie unter [ASP.NET 4.5.2 and enableviewstatuemac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec00a-191">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="ec00a-192">Im folgenden Beispiel wird gezeigt, wie "enableviewstatuemac" auf "true" festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="ec00a-192">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="ec00a-193">Sie müssen diesen Wert nicht auf "true" festlegen, da er standardmäßig "true" ist.</span><span class="sxs-lookup"><span data-stu-id="ec00a-193">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="ec00a-194">Wenn Sie jedoch auf einer beliebigen Seite in der Anwendung auf false festgelegt haben, müssen Sie diesen Wert sofort korrigieren.</span><span class="sxs-lookup"><span data-stu-id="ec00a-194">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="ec00a-195">Mittlere Vertrauensstellung</span><span class="sxs-lookup"><span data-stu-id="ec00a-195">Medium trust</span></span>

<span data-ttu-id="ec00a-196">Empfehlung: Sie sind nicht von mittlerer Vertrauensstellung (oder einer anderen Vertrauens Ebene) als Sicherheitsgrenze abhängig.</span><span class="sxs-lookup"><span data-stu-id="ec00a-196">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="ec00a-197">Teilweise Vertrauenswürdigkeit schützt Ihre Anwendung nicht angemessen und sollte nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-197">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="ec00a-198">Verwenden Sie stattdessen die volle Vertrauenswürdigkeit, und isolieren Sie nicht vertrauenswürdige Anwendungen in separaten Anwendungs Pools.</span><span class="sxs-lookup"><span data-stu-id="ec00a-198">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="ec00a-199">Führen Sie auch jeden Anwendungs Pool unter einer eindeutigen Identität aus.</span><span class="sxs-lookup"><span data-stu-id="ec00a-199">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="ec00a-200">Weitere Informationen finden Sie unter [ASP.net partielle Vertrauenswürdigkeit garantiert keine Anwendungs Isolation](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="ec00a-200">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="ec00a-201">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="ec00a-201">&lt;appSettings&gt;</span></span>

<span data-ttu-id="ec00a-202">Empfehlung: Deaktivieren Sie die Sicherheitseinstellungen in &lt;appSettings-&gt; Element nicht.</span><span class="sxs-lookup"><span data-stu-id="ec00a-202">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="ec00a-203">Das appSettings-Element enthält viele Werte, die für Sicherheitsupdates erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="ec00a-203">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="ec00a-204">Diese Werte sollten nicht geändert oder deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-204">You should not change or disable these values.</span></span> <span data-ttu-id="ec00a-205">Wenn Sie diese Werte beim Bereitstellen eines Updates deaktivieren müssen, aktivieren Sie Sie nach Abschluss der Bereitstellung sofort erneut.</span><span class="sxs-lookup"><span data-stu-id="ec00a-205">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="ec00a-206">Weitere Informationen finden Sie unter [ASP.net appSettings-Element](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec00a-206">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="ec00a-207">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="ec00a-207">UrlPathEncode</span></span>

<span data-ttu-id="ec00a-208">Empfehlung: Verwenden Sie stattdessen [urlencode](https://msdn.microsoft.com/library/zttxte6w.aspx) .</span><span class="sxs-lookup"><span data-stu-id="ec00a-208">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="ec00a-209">Die urlpathencode-Methode wurde der .NET Framework hinzugefügt, um ein sehr spezifisches Browser Kompatibilitätsproblem zu beheben.</span><span class="sxs-lookup"><span data-stu-id="ec00a-209">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="ec00a-210">Sie codiert eine URL nicht ordnungsgemäß und schützt Ihre Anwendung nicht vor Cross-Site Scripting.</span><span class="sxs-lookup"><span data-stu-id="ec00a-210">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="ec00a-211">Sie sollten Sie niemals in Ihrer Anwendung verwenden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-211">You should never use it in your application.</span></span> <span data-ttu-id="ec00a-212">Verwenden Sie stattdessen [urlencode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec00a-212">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="ec00a-213">Im folgenden Beispiel wird gezeigt, wie eine codierte URL als Abfrage Zeichenfolgen-Parameter für ein Hyperlink-Steuerelement übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="ec00a-213">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="ec00a-214">Zuverlässigkeit und Leistung</span><span class="sxs-lookup"><span data-stu-id="ec00a-214">Reliability and performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="ec00a-215">PreSendRequestHeaders und PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="ec00a-215">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="ec00a-216">Empfehlung: Verwenden Sie diese Ereignisse nicht mit verwalteten Modulen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-216">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="ec00a-217">Schreiben Sie stattdessen ein natives IIS-Modul, um die erforderliche Aufgabe auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-217">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="ec00a-218">Siehe [Erstellen von HTTP-Modulen](https://msdn.microsoft.com/library/ms693629.aspx)mit System eigenem Code.</span><span class="sxs-lookup"><span data-stu-id="ec00a-218">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="ec00a-219">Sie können die Ereignisse " [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) " und " [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) " mit nativen IIS-Modulen verwenden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-219">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="ec00a-220">Verwenden Sie `PreSendRequestHeaders` und `PreSendRequestContent` nicht mit verwalteten Modulen, die `IHttpModule`implementieren.</span><span class="sxs-lookup"><span data-stu-id="ec00a-220">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="ec00a-221">Durch Festlegen dieser Eigenschaften können Probleme mit asynchronen Anforderungen verursacht werden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-221">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="ec00a-222">Die Kombination aus dem Anwendungs angeforderten Routing (arr) und websockets kann zu Zugriffs Verletzungs Ausnahmen führen, die zu einem Absturz der w3wp führen können.</span><span class="sxs-lookup"><span data-stu-id="ec00a-222">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="ec00a-223">Beispiel: iiscore! W3_CONTEXT_BASE:: getislastnotification + 68 in "iiscore. dll" hat eine Zugriffs Verletzungs Ausnahme verursacht (0xc0000005).</span><span class="sxs-lookup"><span data-stu-id="ec00a-223">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="ec00a-224">Asynchrone Seiten Ereignisse mit Web Forms</span><span class="sxs-lookup"><span data-stu-id="ec00a-224">Asynchronous page events with web forms</span></span>

<span data-ttu-id="ec00a-225">Empfehlung: vermeiden Sie in Web Forms das Schreiben von asynchronen void-Methoden für Seiten Lebenszyklus Ereignisse, und verwenden Sie stattdessen [Page. RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) für asynchronen Code.</span><span class="sxs-lookup"><span data-stu-id="ec00a-225">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="ec00a-226">Wenn Sie ein Seiten Ereignis mit " **Async** " und " **void**" markieren, können Sie nicht ermitteln, wann der asynchrone Code abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="ec00a-226">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="ec00a-227">Verwenden Sie stattdessen Page. RegisterAsyncTask, um den asynchronen Code so auszuführen, dass Sie den Abschluss nachverfolgen können.</span><span class="sxs-lookup"><span data-stu-id="ec00a-227">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="ec00a-228">Das folgende Beispiel zeigt einen Click-Handler für Schaltflächen, der asynchronen Code enthält.</span><span class="sxs-lookup"><span data-stu-id="ec00a-228">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="ec00a-229">Dieses Beispiel umfasst das asynchrone Lesen eines Zeichen folgen Werts, der nur als vereinfachtes Beispiel für eine asynchrone Aufgabe und nicht als empfohlene Vorgehensweise bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="ec00a-229">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="ec00a-230">Wenn Sie asynchrone Aufgaben verwenden, legen Sie das Ziel Framework für die HTTP-Laufzeit in der Datei "Web. config" auf 4,5 (oder höher) fest.</span><span class="sxs-lookup"><span data-stu-id="ec00a-230">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 (or later) in the Web.config file.</span></span> <span data-ttu-id="ec00a-231">Wenn Sie das Ziel Framework auf 4,5 festlegen, wird der neue Synchronisierungs Kontext eingeschaltet, der in .NET 4,5 hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="ec00a-231">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="ec00a-232">Dieser Wert wird standardmäßig in neuen Projekten in Visual Studio festgelegt, aber nicht festgelegt, wenn Sie mit einem vorhandenen Projekt arbeiten.</span><span class="sxs-lookup"><span data-stu-id="ec00a-232">This value is set by default in new projects in Visual Studio, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="ec00a-233">Fire-and-Forget-Arbeit</span><span class="sxs-lookup"><span data-stu-id="ec00a-233">Fire-and-forget work</span></span>

<span data-ttu-id="ec00a-234">Empfehlung: vermeiden Sie bei der Verarbeitung einer Anforderung in ASP.NET das Starten von Fire-and-Forget-Arbeit (z. b. das Aufrufen der Thread Pool. QueueUserWorkItem-Methode oder das Erstellen eines Timers, der wiederholt einen Delegaten aufruft)</span><span class="sxs-lookup"><span data-stu-id="ec00a-234">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="ec00a-235">Wenn Ihre Anwendung Fire-and-Forget-Arbeit in ASP.NET ausführt, kann die Anwendung nicht synchronisiert werden. Die APP-Domäne kann jederzeit zerstört werden. Dies bedeutet, dass der laufende Prozess möglicherweise nicht mehr mit dem aktuellen Status der Anwendung identisch ist.</span><span class="sxs-lookup"><span data-stu-id="ec00a-235">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="ec00a-236">Sie sollten diese Art von Arbeit außerhalb von ASP.net verschieben.</span><span class="sxs-lookup"><span data-stu-id="ec00a-236">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="ec00a-237">Sie können einen Webauftrag, einen Windows-Dienst oder eine workerrolle in Azure verwenden, um laufende Aufgaben auszuführen und den Code aus einem anderen Prozess auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-237">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="ec00a-238">Wenn Sie diese Arbeit in ASP.net ausführen müssen, können Sie das nuget-Paket namens [webbackgroat](http://www.nuget.org/packages/webbackgrounder) hinzufügen, um den Code auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-238">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="ec00a-239">Anforderungs Entitäts Text</span><span class="sxs-lookup"><span data-stu-id="ec00a-239">Request entity body</span></span>

<span data-ttu-id="ec00a-240">Empfehlung: vermeiden Sie das Lesen von "Request. Form" oder "Request. InputStream" vor dem Ausführungs Ereignis des Handlers.</span><span class="sxs-lookup"><span data-stu-id="ec00a-240">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="ec00a-241">Der früheste, den Sie aus "Request. Form" oder "Request. InputStream" lesen sollten, ist während des Execute-Ereignisses des Handlers.</span><span class="sxs-lookup"><span data-stu-id="ec00a-241">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="ec00a-242">In MVC ist der Controller der-Handler, und das Execute-Ereignis wird ausgeführt, wenn die Aktionsmethode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="ec00a-242">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="ec00a-243">In Web Forms ist die Seite der-Handler, und das Execute-Ereignis ist, wenn das Page. Init-Ereignis ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="ec00a-243">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="ec00a-244">Wenn Sie den Anforderungs Entitäts Text vor dem Execute-Ereignis lesen, stören Sie die Verarbeitung der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="ec00a-244">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="ec00a-245">Wenn Sie den Anforderungs Entitäts Text vor dem Execute-Ereignis lesen müssen, verwenden Sie entweder [Request. getbufferlessinputstream](https://msdn.microsoft.com/library/ff406798.aspx) oder [Request. getbufferedinputstream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec00a-245">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="ec00a-246">Wenn Sie getbufferlessinputstream verwenden, erhalten Sie den unformatierten Datenstrom aus der Anforderung und übernehmen die Verantwortung für die Verarbeitung der gesamten Anforderung.</span><span class="sxs-lookup"><span data-stu-id="ec00a-246">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="ec00a-247">Nach dem Aufrufen von getbufferlessinputstream sind Request. Form und Request. InputStream nicht verfügbar, da Sie nicht von ASP.net aufgefüllt wurden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-247">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="ec00a-248">Wenn Sie getbufferedinputstream verwenden, erhalten Sie eine Kopie des Streams aus der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="ec00a-248">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="ec00a-249">Request. Form und Request. InputStream sind später in der Anforderung weiterhin verfügbar, da ASP.net die andere Kopie auffüllt.</span><span class="sxs-lookup"><span data-stu-id="ec00a-249">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="ec00a-250">Response. Redirect und Response. End</span><span class="sxs-lookup"><span data-stu-id="ec00a-250">Response.Redirect and Response.End</span></span>

<span data-ttu-id="ec00a-251">Empfehlung: Beachten Sie die Unterschiede in der Behandlung von Threads nach dem Aufrufen von [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec00a-251">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="ec00a-252">Die [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) -Methode ruft die Response. End-Methode auf.</span><span class="sxs-lookup"><span data-stu-id="ec00a-252">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="ec00a-253">Bei einem synchronen Prozess bewirkt das Aufrufen von Request. Redirect, dass der aktuelle Thread sofort abgebrochen wird.</span><span class="sxs-lookup"><span data-stu-id="ec00a-253">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="ec00a-254">Bei einem asynchronen Prozess bricht das Aufrufen von Response. Redirect den aktuellen Thread jedoch nicht ab, sodass die Codeausführung für die Anforderung fortgesetzt wird.</span><span class="sxs-lookup"><span data-stu-id="ec00a-254">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="ec00a-255">In einem asynchronen Prozess müssen Sie die-Aufgabe von der-Methode zurückgeben, um die Codeausführung zu beenden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-255">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="ec00a-256">In einem MVC-Projekt sollte "Response. Redirect" nicht aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-256">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="ec00a-257">Stattdessen wird ein redirectresult zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="ec00a-257">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="ec00a-258">EnableViewState und ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="ec00a-258">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="ec00a-259">Empfehlung: Verwenden Sie ViewStateMode anstelle von EnableViewState, um genau zu steuern, welche Steuerelemente den Ansichts Zustand verwenden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-259">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="ec00a-260">Wenn Sie in der Page-Direktive EnableViewState auf false festlegen, wird der Ansichts Zustand für alle Steuerelemente innerhalb der Seite deaktiviert und kann nicht aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-260">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="ec00a-261">Wenn Sie den Ansichts Zustand nur für bestimmte Steuerelemente in der Seite aktivieren möchten, legen Sie für die Seite ViewStateMode auf deaktiviert fest.</span><span class="sxs-lookup"><span data-stu-id="ec00a-261">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="ec00a-262">Legen Sie dann ViewStateMode nur für die Steuerelemente, die tatsächlich den Ansichts Zustand benötigen, auf aktiviert fest.</span><span class="sxs-lookup"><span data-stu-id="ec00a-262">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="ec00a-263">Wenn Sie den Ansichts Zustand nur für die Steuerelemente aktivieren, die ihn benötigen, können Sie die Größe des Ansichts Zustands für Ihre Webseiten verkleinern.</span><span class="sxs-lookup"><span data-stu-id="ec00a-263">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="ec00a-264">Sqlmitgliedshipprovider</span><span class="sxs-lookup"><span data-stu-id="ec00a-264">SqlMembershipProvider</span></span>

<span data-ttu-id="ec00a-265">Empfehlung: Verwenden Sie universelle Anbieter.</span><span class="sxs-lookup"><span data-stu-id="ec00a-265">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="ec00a-266">In den aktuellen Projektvorlagen wurde sqlmembership shipprovider durch [ASP.net-universelle Anbieter](http://www.nuget.org/packages/Microsoft.AspNet.Providers)ersetzt, das als nuget-Paket verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="ec00a-266">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="ec00a-267">Wenn Sie sqlmembership shipprovider in einem Projekt verwenden, das mit einer früheren Version der Vorlagen erstellt wurde, sollten Sie zu universelle Anbieter wechseln.</span><span class="sxs-lookup"><span data-stu-id="ec00a-267">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="ec00a-268">Die universelle Anbieter arbeiten mit allen Datenbanken, die von Entity Framework unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-268">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="ec00a-269">Weitere Informationen finden Sie unter [Einführung in ASP.net-universelle Anbieter](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec00a-269">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="ec00a-270">Lange Ausführungsanforderungen (> 110 Sekunden)</span><span class="sxs-lookup"><span data-stu-id="ec00a-270">Long-running requests (>110 seconds)</span></span>

<span data-ttu-id="ec00a-271">Empfehlung: Verwenden Sie [websockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) oder [signalr](../../../signalr/index.md) für verbundene Clients, und verwenden Sie asynchrone e/a-Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="ec00a-271">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="ec00a-272">Anforderungen mit langer Ausführungszeit können unvorhersehbare Ergebnisse und eine schlechte Leistung in Ihrer Webanwendung verursachen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-272">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="ec00a-273">Die standardmäßige Timeout Einstellung für eine Anforderung beträgt 110 Sekunden.</span><span class="sxs-lookup"><span data-stu-id="ec00a-273">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="ec00a-274">Wenn Sie den Sitzungszustand mit einer Anforderung mit langer Ausführungszeit verwenden, gibt ASP.net die Sperre für das Sitzungs Objekt nach 110 Sekunden frei.</span><span class="sxs-lookup"><span data-stu-id="ec00a-274">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="ec00a-275">Die Anwendung befindet sich jedoch möglicherweise in der Mitte eines Vorgangs für das Sitzungs Objekt, wenn die Sperre aufgehoben wird, und der Vorgang wird möglicherweise nicht erfolgreich abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-275">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="ec00a-276">Wenn eine zweite Anforderung vom Benutzer blockiert wird, während die erste Anforderung ausgeführt wird, kann die zweite Anforderung auf das Sitzungs Objekt in einem inkonsistenten Zustand zugreifen.</span><span class="sxs-lookup"><span data-stu-id="ec00a-276">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="ec00a-277">Wenn Ihre Anwendung blockierende (oder synchrone) e/a-Vorgänge umfasst, reagiert die Anwendung nicht mehr.</span><span class="sxs-lookup"><span data-stu-id="ec00a-277">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="ec00a-278">Verwenden Sie die asynchronen e/a-Vorgänge in der .NET Framework, um die Leistung zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="ec00a-278">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="ec00a-279">Verwenden Sie auch websockets oder signalr zum Verbinden von Clients mit dem Server.</span><span class="sxs-lookup"><span data-stu-id="ec00a-279">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="ec00a-280">Diese Features sind für die effiziente Verarbeitung von Anforderungen mit langer Ausführungszeit konzipiert.</span><span class="sxs-lookup"><span data-stu-id="ec00a-280">These features are designed to efficiently handle long-running requests.</span></span>
