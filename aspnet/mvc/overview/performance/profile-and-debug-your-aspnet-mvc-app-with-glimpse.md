---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilerstellung und Debuggen Ihrer ASP.NET MVC-App mit Blick auf | Microsoft-Dokumentation
author: Rick-Anderson
description: Der Einblick ist eine wachsende und wachsende Familie von Open Source-nuget-Paketen, die ausführliche Informationen zu Leistung, Debugging und Diagnose für ASP.net a...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: d3689147a3bc3aa1f4180c377d2483a94bdd95a9
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457660"
---
# <a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="87cfe-103">Erstellen von Profilen und Debuggen der ASP.NET MVC-App mit Glimpse</span><span class="sxs-lookup"><span data-stu-id="87cfe-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>

<span data-ttu-id="87cfe-104">von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="87cfe-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="87cfe-105">Der Einblick ist eine wachsende und wachsende Familie von Open Source-nuget-Paketen, die ausführliche Leistungs-, Debug-und Diagnoseinformationen für ASP.net-apps bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="87cfe-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="87cfe-106">Es ist trivial, einfach zu installieren, einfach und extrem schnell und zeigt wichtige Leistungsmetriken unten auf jeder Seite an.</span><span class="sxs-lookup"><span data-stu-id="87cfe-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="87cfe-107">Sie können einen Drilldown in Ihre APP ausführen, wenn Sie herausfinden möchten, was auf dem Server läuft.</span><span class="sxs-lookup"><span data-stu-id="87cfe-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="87cfe-108">Der Einblick bietet so viel nützliche Informationen, die Sie im gesamten Entwicklungszeitraum, einschließlich ihrer Azure-Testumgebung, verwenden sollten.</span><span class="sxs-lookup"><span data-stu-id="87cfe-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="87cfe-109">[Wenngleich die](http://www.telerik.com/fiddler) Tools und die [F-12-Entwicklungs Tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) eine Client seitige Ansicht bereitstellen, bietet der Einblick eine detaillierte Ansicht des Servers.</span><span class="sxs-lookup"><span data-stu-id="87cfe-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="87cfe-110">Dieses Tutorial konzentriert sich auf die Verwendung von "Glimpse ASP.NET MVC-und EF-Paketen", aber es sind viele weitere Pakete verfügbar.</span><span class="sxs-lookup"><span data-stu-id="87cfe-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="87cfe-111">Wenn möglich, verwende ich eine Verknüpfung zu [den entsprechenden Informationen](http://getglimpse.com/Docs/) , die ich bei der Pflege unterstützen werde.</span><span class="sxs-lookup"><span data-stu-id="87cfe-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="87cfe-112">Der Einblick ist ein Open-Source-Projekt. Sie können auch zum Quellcode und zu den Dokumenten beitragen.</span><span class="sxs-lookup"><span data-stu-id="87cfe-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>

- [<span data-ttu-id="87cfe-113">Installieren von Glimpse</span><span class="sxs-lookup"><span data-stu-id="87cfe-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="87cfe-114">Aktivieren Sie den Einblick für localhost.</span><span class="sxs-lookup"><span data-stu-id="87cfe-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="87cfe-115">Registerkarte Zeitachse</span><span class="sxs-lookup"><span data-stu-id="87cfe-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="87cfe-116">Modellbindung</span><span class="sxs-lookup"><span data-stu-id="87cfe-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="87cfe-117">Routen</span><span class="sxs-lookup"><span data-stu-id="87cfe-117">Routes</span></span>](#route)
- [<span data-ttu-id="87cfe-118">Verwenden von Glimpse in Azure</span><span class="sxs-lookup"><span data-stu-id="87cfe-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="87cfe-119">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="87cfe-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="87cfe-120">Installieren von Glimpse</span><span class="sxs-lookup"><span data-stu-id="87cfe-120">Installing Glimpse</span></span>

<span data-ttu-id="87cfe-121">Sie können den Einblick über die nuget-Paket-Manager-Konsole oder über die Konsole **nuget-Pakete verwalten** installieren.</span><span class="sxs-lookup"><span data-stu-id="87cfe-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="87cfe-122">Für diese Demo werden die Mvc5-und EF6-Pakete installiert:</span><span class="sxs-lookup"><span data-stu-id="87cfe-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![Installieren Sie den Einblick von nuget Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="87cfe-124">Suchen Sie nach " *Glimpse. EF* "</span><span class="sxs-lookup"><span data-stu-id="87cfe-124">Search for *Glimpse.EF*</span></span>

!["Glimpse. EF" aus der nuget-Installation Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="87cfe-126">Wenn Sie **installierte Pakete**auswählen, sehen Sie, dass die glimabhängigen Module installiert sind:</span><span class="sxs-lookup"><span data-stu-id="87cfe-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Installierte Glimpse-Pakete von Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="87cfe-128">Mit den folgenden Befehlen werden die Module "Glimpse MVC5" und "EF6" von der Paket-Manager-Konsole</span><span class="sxs-lookup"><span data-stu-id="87cfe-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="87cfe-129">Aktivieren Sie den Einblick für localhost.</span><span class="sxs-lookup"><span data-stu-id="87cfe-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="87cfe-130">Navigieren Sie zu http://localhost:&lt;p Ort #&gt;/Glimpse.axd, und klicken Sie auf die Schaltfläche " <strong>Glimpse</strong> ein".</span><span class="sxs-lookup"><span data-stu-id="87cfe-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Seite "Blick auf axd"](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="87cfe-132">Wenn Sie die Favoritenleiste angezeigt haben, können Sie die Schaltflächen mit dem Drag & Drop ablegen und als Bookmarklets hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="87cfe-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![IE mit Glimpse-Bookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="87cfe-134">Sie können jetzt in der APP navigieren, und die **Heads-Up-Anzeige** (HUD) wird unten auf der Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="87cfe-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Kontakt-Manager-Seite mit HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="87cfe-136">Auf der [Seite "Glid](http://getglimpse.com/Docs/Heads-up-Display) " finden Sie Informationen zu den oben gezeigten Zeit Steuerungsinformationen.</span><span class="sxs-lookup"><span data-stu-id="87cfe-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="87cfe-137">Die unaufdringlichen Leistungsdaten, die in der HUD angezeigt werden, können Sie sofort über ein Problem benachrichtigen, bevor Sie zum Testzeitraum gelangen.</span><span class="sxs-lookup"><span data-stu-id="87cfe-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="87cfe-138">Wenn Sie in der unteren rechten Ecke auf die &quot;g-&quot; klicken, wird der Bereich "Glimpse" angezeigt:</span><span class="sxs-lookup"><span data-stu-id="87cfe-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Bereich mit Blick](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="87cfe-140">In der obigen Abbildung ist die [Registerkarte Ausführung](http://getglimpse.com/Docs/Execution-Tab) ausgewählt, mit der Details zur zeitlichen Steuerung der Aktionen und Filter in der Pipeline angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="87cfe-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="87cfe-141">Der [Zeitgeber für den halte Filter](http://www.nuget.org/packages/StopWatch/) Start in Phase 6 der Pipeline wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="87cfe-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="87cfe-142">Während mein Lightweight-Timer nützliche Profil-und Zeit Steuerungsdaten bereitstellen kann, gibt er die Zeit, die für die Autorisierung und das Rendering der Ansicht aufgewendet wird</span><span class="sxs-lookup"><span data-stu-id="87cfe-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="87cfe-143">Weitere Informationen finden Sie unter [Profil und Zeit Ihrer ASP.NET MVC-app in Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="87cfe-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="87cfe-144">Die [Registerkarten](http://getglimpse.com/Docs/Tabs) Seite enthält Links zu detaillierten Informationen zu den einzelnen Registerkarten.</span><span class="sxs-lookup"><span data-stu-id="87cfe-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="87cfe-145">Registerkarte Zeitachse</span><span class="sxs-lookup"><span data-stu-id="87cfe-145">The Timeline tab</span></span>

<span data-ttu-id="87cfe-146">Ich habe das ausstehende [EF 6/MVC 5-Tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) von Tom Dykstra geändert, mit der folgenden Codeänderung am Dozenten Controller:</span><span class="sxs-lookup"><span data-stu-id="87cfe-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="87cfe-147">Der obige Code ermöglicht es mir, die Abfrage Zeichenfolge (`eager`) zu übergeben, um das sorgfältige oder explizite Laden von Daten zu steuern.</span><span class="sxs-lookup"><span data-stu-id="87cfe-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="87cfe-148">In der folgenden Abbildung wird Explizites Laden verwendet, und auf der Seite für die zeitliche Steuerung werden alle Anmeldungen angezeigt, die in der `Index`-Aktionsmethode geladen werden</span><span class="sxs-lookup"><span data-stu-id="87cfe-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![Explizites Laden](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="87cfe-150">Im folgenden Code wird "eifrig" angegeben, und jede Registrierung wird abgerufen, nachdem die `Index` Ansicht aufgerufen wurde:</span><span class="sxs-lookup"><span data-stu-id="87cfe-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

!["eifrig" ist angegeben](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="87cfe-152">Sie können auf ein Zeitsegment zeigen, um ausführliche Informationen zur zeitlichen Steuerung zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="87cfe-152">You can hover over a time segment to get detailed timing information:</span></span>

![zeigen Sie auf eine ausführliche zeitliche Steuerung](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="87cfe-154">Modellbindung</span><span class="sxs-lookup"><span data-stu-id="87cfe-154">Model Binding</span></span>

<span data-ttu-id="87cfe-155">Auf der [Registerkarte Modell Bindung](http://getglimpse.com/Docs/Model-Binding-Tab) finden Sie eine Fülle von Informationen, die Ihnen helfen zu verstehen, wie Ihre Formularvariablen gebunden werden und warum einige nicht erwartungsgemäß gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="87cfe-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="87cfe-156">Die Abbildung unten zeigt den **?**</span><span class="sxs-lookup"><span data-stu-id="87cfe-156">The image below shows the **?**</span></span> <span data-ttu-id="87cfe-157">ein Symbol, auf das Sie klicken können, um die Hilfeseite für das entsprechende Feature aufzurufenden.</span><span class="sxs-lookup"><span data-stu-id="87cfe-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![Ansicht für Modell Bindung](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="87cfe-159">Routen</span><span class="sxs-lookup"><span data-stu-id="87cfe-159">Routes</span></span>

 <span data-ttu-id="87cfe-160">Auf der Registerkarte "Glimpse-Routen" können Sie das Routing und das Routing verstehen.</span><span class="sxs-lookup"><span data-stu-id="87cfe-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="87cfe-161">In der folgenden Abbildung ist die Produkt Route ausgewählt (und wird in grün, eine Glimpse-Konvention) angezeigt.</span><span class="sxs-lookup"><span data-stu-id="87cfe-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="87cfe-162">![Produktname](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Routen Einschränkungen ausgewählt werden, werden Bereiche und Daten Token ebenfalls angezeigt.</span><span class="sxs-lookup"><span data-stu-id="87cfe-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="87cfe-163">Weitere Informationen finden Sie unter [Einblicke in Routen](http://getglimpse.com/Docs/Routes-Tab) und [Attribut Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) .</span><span class="sxs-lookup"><span data-stu-id="87cfe-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="87cfe-164">Verwenden von Glimpse in Azure</span><span class="sxs-lookup"><span data-stu-id="87cfe-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="87cfe-165">Mit der Standard Sicherheitsrichtlinie für einen Blick können nur Daten vom lokalen Host angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="87cfe-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="87cfe-166">Sie können diese Sicherheitsrichtlinie ändern, sodass Sie diese Daten auf einem Remote Server anzeigen können (z. b. eine Web-App in Azure).</span><span class="sxs-lookup"><span data-stu-id="87cfe-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="87cfe-167">Fügen Sie für Testumgebungen in Azure die markierte Markierung am Ende der Datei " *Web. config* " hinzu, um den Einblick zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="87cfe-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.config* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="87cfe-168">Mit dieser Änderung können alle Benutzer Ihre Daten auf einer Remote Website einsehen.</span><span class="sxs-lookup"><span data-stu-id="87cfe-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="87cfe-169">Sie sollten das obige Markup einem Veröffentlichungs Profil hinzufügen, damit es nur bei Verwendung dieses Veröffentlichungs Profils (z. b. Ihres Azure-Test Profils) bereitgestellt wird. Um die Anzeige von Daten zu beschränken, fügen wir die `canViewGlimpseData` Rolle hinzu und gestatten Benutzern in dieser Rolle nur die Anzeige von Daten.</span><span class="sxs-lookup"><span data-stu-id="87cfe-169">Consider adding the markup above to a publish profile so it's only deployed an applied when you use that publish profile (for example, your Azure test profile.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="87cfe-170">Entfernen Sie die Kommentare aus der Datei *GlimpseSecurityPolicy.cs* , und ändern Sie den [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) -Aufrufen von `Administrator` in die `canViewGlimpseData` Rolle:</span><span class="sxs-lookup"><span data-stu-id="87cfe-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="87cfe-171">Sicherheit: die umfangreichen Daten, die von Glimpse bereitgestellt werden, können die Sicherheit Ihrer app verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="87cfe-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="87cfe-172">Microsoft hat keine Sicherheitsüberprüfung für die Verwendung in den Produktions-apps durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="87cfe-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>

<span data-ttu-id="87cfe-173">Weitere Informationen zum Hinzufügen von Rollen finden Sie im Tutorial bereitstellen [einer Secure ASP.NET MVC 5-Web-App mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) .</span><span class="sxs-lookup"><span data-stu-id="87cfe-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="87cfe-174">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="87cfe-174">Additional Resources</span></span>

- [<span data-ttu-id="87cfe-175">Stellen Sie eine sichere ASP.NET MVC 5-App mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure bereit.</span><span class="sxs-lookup"><span data-stu-id="87cfe-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="87cfe-176">[Glimpse-Konfiguration](http://getglimpse.com/Docs/Configuration) : Seite "doc" zum Konfigurieren von Registerkarten, Lauf Zeit Richtlinie, Protokollierung und mehr.</span><span class="sxs-lookup"><span data-stu-id="87cfe-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
