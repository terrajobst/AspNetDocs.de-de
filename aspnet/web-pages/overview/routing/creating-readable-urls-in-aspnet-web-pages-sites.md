---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Erstellen lesbarer URLs in ASP.net Web Pages Websites (Razor) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Artikel wird das Routing auf einer ASP.net Web Pages-Website (Razor) beschrieben, und Sie erfahren, wie Sie URLs verwenden können, die besser lesbar und besser für SEO geeignet sind. Was Sie tun...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 832db8e144cab730f16c78f67c12feb9b7c92c7c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509907"
---
# <a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="fee31-104">Erstellen lesbarer URLs in ASP.net Web Pages Websites (Razor)</span><span class="sxs-lookup"><span data-stu-id="fee31-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>

<span data-ttu-id="fee31-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fee31-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fee31-106">In diesem Artikel wird das Routing auf einer ASP.net Web Pages-Website (Razor) beschrieben, und Sie erfahren, wie Sie URLs verwenden können, die besser lesbar und besser für SEO geeignet sind.</span><span class="sxs-lookup"><span data-stu-id="fee31-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="fee31-107">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="fee31-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="fee31-108">Wie ASP.NET das Routing verwendet, damit Sie besser lesbare und durchsuchbare URLs verwenden können.</span><span class="sxs-lookup"><span data-stu-id="fee31-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="fee31-109">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="fee31-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="fee31-110">ASP.net Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="fee31-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="fee31-111">Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="fee31-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="about-routing"></a><span data-ttu-id="fee31-112">Informationen zum Routing</span><span class="sxs-lookup"><span data-stu-id="fee31-112">About Routing</span></span>

<span data-ttu-id="fee31-113">Die URLs für die Seiten in Ihrer Website können sich darauf auswirken, wie gut die Site funktioniert.</span><span class="sxs-lookup"><span data-stu-id="fee31-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="fee31-114">Eine URL, die &quot;Friendly&quot; ist, kann es Benutzern erleichtern, die Website zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="fee31-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="fee31-115">Sie kann auch bei der Suche-Engine-Optimierung (SEO) für die Site helfen.</span><span class="sxs-lookup"><span data-stu-id="fee31-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="fee31-116">ASP.NET Websites bieten die Möglichkeit, freundliche URLs automatisch zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="fee31-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="fee31-117">Mit ASP.net können Sie sinnvolle URLs erstellen, die Benutzeraktionen beschreiben, anstatt nur auf eine Datei auf dem Server zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="fee31-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="fee31-118">Beachten Sie diese URLs für einen fiktiven Blog:</span><span class="sxs-lookup"><span data-stu-id="fee31-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="fee31-119">Vergleichen Sie diese URLs mit den folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="fee31-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="fee31-120">Im ersten paar muss ein Benutzer wissen, dass der Blog mithilfe der Seite " *Blog. cshtml* " angezeigt wird, und dann eine Abfrage Zeichenfolge erstellen, die die richtige Kategorie oder den richtigen Datumsbereich abruft.</span><span class="sxs-lookup"><span data-stu-id="fee31-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="fee31-121">Der zweite Satz von Beispielen ist viel leichter zu verstehen und zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fee31-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="fee31-122">Die URLs für das erste Beispiel zeigen auch direkt auf eine bestimmte Datei (*Blog. cshtml*).</span><span class="sxs-lookup"><span data-stu-id="fee31-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="fee31-123">Wenn der Blog aus irgendeinem Grund in einen anderen Ordner auf dem Server verschoben wurde, oder wenn der Blog so umgeschrieben wurde, dass eine andere Seite verwendet wird, sind die Links falsch.</span><span class="sxs-lookup"><span data-stu-id="fee31-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="fee31-124">Der zweite Satz von URLs verweist nicht auf eine bestimmte Seite. auch wenn sich die Blog Implementierung oder der Speicherort ändert, sind die URLs weiterhin gültig.</span><span class="sxs-lookup"><span data-stu-id="fee31-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="fee31-125">In ASP.net Web Pages können Sie benutzerfreundlicheren-URLs wie in den obigen Beispielen erstellen, da ASP.NET das *Routing*verwendet.</span><span class="sxs-lookup"><span data-stu-id="fee31-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="fee31-126">Routing erstellt eine logische Zuordnung von einer URL zu einer Seite (oder Seiten), die die Anforderung erfüllen kann.</span><span class="sxs-lookup"><span data-stu-id="fee31-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="fee31-127">Da die Zuordnung für eine bestimmte Datei logisch (nicht physisch) ist, bietet das Routing eine hohe Flexibilität beim Definieren der URLs für Ihre Website.</span><span class="sxs-lookup"><span data-stu-id="fee31-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="fee31-128">Funktionsweise des Routings</span><span class="sxs-lookup"><span data-stu-id="fee31-128">How Routing Works</span></span>

<span data-ttu-id="fee31-129">Wenn ASP.net eine Anforderung verarbeitet, liest Sie die URL, um zu bestimmen, wie Sie weitergeleitet werden soll.</span><span class="sxs-lookup"><span data-stu-id="fee31-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="fee31-130">ASP.net versucht, einzelne Segmente der URL zu Dateien auf dem Datenträger abzugleichen, von links nach rechts.</span><span class="sxs-lookup"><span data-stu-id="fee31-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="fee31-131">Wenn eine Entsprechung vorliegt, wird alles, was in der URL verbleiben, als *Pfadinformationen*an die Seite übermittelt.</span><span class="sxs-lookup"><span data-stu-id="fee31-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="fee31-132">Stellen Sie sich vor, dass jemand eine Anforderung mit dieser URL sendet:</span><span class="sxs-lookup"><span data-stu-id="fee31-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="fee31-133">Die Suche verläuft wie folgt:</span><span class="sxs-lookup"><span data-stu-id="fee31-133">The search goes like this:</span></span>

1. <span data-ttu-id="fee31-134">Gibt es eine Datei mit dem Pfad und dem Namen */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="fee31-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="fee31-135">Wenn dies der Fall ist, führen Sie diese Seite aus und übergeben keine Informationen an Sie.</span><span class="sxs-lookup"><span data-stu-id="fee31-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="fee31-136">Andernfalls...</span><span class="sxs-lookup"><span data-stu-id="fee31-136">Otherwise ...</span></span>
2. <span data-ttu-id="fee31-137">Gibt es eine Datei mit dem Pfad und dem Namen */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="fee31-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="fee31-138">Wenn dies der Fall ist, führen Sie diese Seite aus, und übergeben Sie den Wert `c`.</span><span class="sxs-lookup"><span data-stu-id="fee31-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="fee31-139">Andernfalls...</span><span class="sxs-lookup"><span data-stu-id="fee31-139">Otherwise …</span></span>
3. <span data-ttu-id="fee31-140">Gibt es eine Datei mit dem Pfad und dem Namen */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="fee31-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="fee31-141">Wenn dies der Fall ist, führen Sie diese Seite aus, und übergeben Sie den Wert `b/c`.</span><span class="sxs-lookup"><span data-stu-id="fee31-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="fee31-142">Wenn bei der Suche keine genauen Übereinstimmungen für *cshtml* -Dateien in den angegebenen Ordnern gefunden werden, sucht ASP.net weiterhin nach diesen Dateien:</span><span class="sxs-lookup"><span data-stu-id="fee31-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="fee31-143">*/a/b/c/default.cshtml* (keine Pfadinformationen).</span><span class="sxs-lookup"><span data-stu-id="fee31-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="fee31-144">*/a/b/c/Index.cshtml* (keine Pfadinformationen).</span><span class="sxs-lookup"><span data-stu-id="fee31-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="fee31-145">Um klar zu sein, funktionieren Anforderungen für bestimmte Seiten (d. h. Anforderungen, die die Dateinamenerweiterung *. cshtml* enthalten) genauso wie erwartet.</span><span class="sxs-lookup"><span data-stu-id="fee31-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="fee31-146">Bei einer Anforderung wie `http://www.contoso.com/a/b.cshtml` wird die Seite *b. cshtml* problemlos ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="fee31-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>

<span data-ttu-id="fee31-147">Innerhalb einer Seite können Sie die Pfadinformationen über die `UrlData`-Eigenschaft der Seite, die ein Wörterbuch ist, erhalten.</span><span class="sxs-lookup"><span data-stu-id="fee31-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="fee31-148">Stellen Sie sich vor, dass Sie eine Datei mit dem Namen " *viewcustomers. cshtml* " haben und Ihre Website diese Anforderung erhält:</span><span class="sxs-lookup"><span data-stu-id="fee31-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="fee31-149">Wie in den obigen Regeln beschrieben, wird die Anforderung an Ihre Seite weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="fee31-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="fee31-150">Auf der Seite können Sie Code wie den folgenden verwenden, um die Pfadinformationen (in diesem Fall den Wert &quot;1000&quot;) zu erhalten und anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="fee31-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="fee31-151">Da das Routing keine vollständigen Dateinamen umfasst, kann Mehrdeutigkeit auftreten, wenn Sie über Seiten mit dem gleichen Namen, aber mit unterschiedlichen Dateinamen Erweiterungen verfügen (z. b. *MyPage. cshtml* und *MyPage. html*).</span><span class="sxs-lookup"><span data-stu-id="fee31-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="fee31-152">Um Probleme mit dem Routing zu vermeiden, sollten Sie sicherstellen, dass keine Seiten auf Ihrer Website vorhanden sind, deren Namen sich nur in ihrer Erweiterung unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="fee31-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="fee31-153">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="fee31-153">Additional Resources</span></span>

<span data-ttu-id="fee31-154">[Webmatrix-URLs, UrlData und Routing für SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="fee31-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="fee31-155">In diesem Blogeintrag von Mike Brind finden Sie weitere Informationen zur Funktionsweise des Routings in ASP.net Web Pages.</span><span class="sxs-lookup"><span data-stu-id="fee31-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
