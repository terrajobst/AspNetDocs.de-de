---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Nachverfolgen von Besucher Informationen (Analytics) für eine ASP.net Web Pages (Razor-) Site | Microsoft-Dokumentation
author: Rick-Anderson
description: Nachdem Sie Ihre Website erhalten haben, möchten Sie möglicherweise den Datenverkehr Ihrer Website analysieren.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 095a5572c755446e0661c052ca9de82d636429fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421455"
---
# <a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="53f1b-103">Nachverfolgen von Besucher Informationen (Analytics) für eine ASP.net Web Pages (Razor-) Site</span><span class="sxs-lookup"><span data-stu-id="53f1b-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="53f1b-104">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="53f1b-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="53f1b-105">In diesem Artikel wird beschrieben, wie Sie mit einem Hilfsprogramm Website Analysen zu Seiten auf einer ASP.net Web Pages-Website (Razor) hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="53f1b-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="53f1b-106">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="53f1b-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="53f1b-107">Informationen zum Senden von Informationen über den Datenverkehr Ihrer Website an einen Analyse Anbieter.</span><span class="sxs-lookup"><span data-stu-id="53f1b-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="53f1b-108">Dies sind die ASP.net-Programmierfunktionen, die im Artikel eingeführt wurden:</span><span class="sxs-lookup"><span data-stu-id="53f1b-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="53f1b-109">Das `Analytics`-Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="53f1b-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="53f1b-110">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="53f1b-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="53f1b-111">ASP.net Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="53f1b-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="53f1b-112">ASP.net-webhilfsprogramme-Bibliothek (nuget-Paket)</span><span class="sxs-lookup"><span data-stu-id="53f1b-112">ASP.NET Web Helpers Library (NuGet package)</span></span>

<span data-ttu-id="53f1b-113">Analytics ist ein allgemeiner Begriff für Technologie, die den Datenverkehr auf Ihrer Website misst, damit Sie verstehen können, wie Benutzer die Website verwenden.</span><span class="sxs-lookup"><span data-stu-id="53f1b-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="53f1b-114">Viele Analysedienste sind verfügbar, darunter Dienste von Google, Yahoo, Status Counter und anderen.</span><span class="sxs-lookup"><span data-stu-id="53f1b-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="53f1b-115">Die Vorgehensweise bei der Analyse besteht darin, dass Sie sich für ein Konto mit dem Analyse Anbieter registrieren, in dem Sie die Website registrieren, die Sie nachverfolgen möchten. Der Anbieter sendet Ihnen einen Ausschnitt von JavaScript-Code, der eine ID oder einen nach Verfolgungs Code für Ihr Konto enthält.</span><span class="sxs-lookup"><span data-stu-id="53f1b-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="53f1b-116">Sie fügen den JavaScript-Code Ausschnitt den Webseiten auf der Website hinzu, die Sie nachverfolgen möchten. (Sie fügen den Analyse Ausschnitt in der Regel zu einer Fußzeile oder Layoutseite oder zu einem anderen HTML-Markup hinzu, das auf jeder Seite der Website angezeigt wird.) Wenn Benutzer eine Seite anfordern, die einen dieser JavaScript-Ausschnitte enthält, sendet der Ausschnitt Informationen über die aktuelle Seite an den Analyse Anbieter, der verschiedene Details zur Seite aufzeichnet.</span><span class="sxs-lookup"><span data-stu-id="53f1b-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="53f1b-117">Wenn Sie sich Ihre Site Statistik ansehen möchten, melden Sie sich bei der Website des Analytics-Anbieters an.</span><span class="sxs-lookup"><span data-stu-id="53f1b-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="53f1b-118">Sie können dann alle Arten von Berichten zu Ihrer Website anzeigen, wie z. b.:</span><span class="sxs-lookup"><span data-stu-id="53f1b-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="53f1b-119">Die Anzahl der Seitenaufrufe für einzelne Seiten.</span><span class="sxs-lookup"><span data-stu-id="53f1b-119">The number of page views for individual pages.</span></span> <span data-ttu-id="53f1b-120">Dies teilt Ihnen (ungefähr), wie viele Personen die Website besuchen und welche Seiten auf Ihrer Website am beliebtesten sind.</span><span class="sxs-lookup"><span data-stu-id="53f1b-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="53f1b-121">Gibt an, wie lange die Benutzer für bestimmte Seiten aufwenden.</span><span class="sxs-lookup"><span data-stu-id="53f1b-121">How long people spend on specific pages.</span></span> <span data-ttu-id="53f1b-122">So können Sie erkennen, ob Ihre Startseite die Interessen der Menschen beibehält.</span><span class="sxs-lookup"><span data-stu-id="53f1b-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="53f1b-123">Auf welchen Websites sich die Mitarbeiter befanden, bevor Sie Ihre Website besucht haben.</span><span class="sxs-lookup"><span data-stu-id="53f1b-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="53f1b-124">Dies hilft Ihnen zu verstehen, ob der Datenverkehr von Verknüpfungen, von Such Vorgängen usw. stammt.</span><span class="sxs-lookup"><span data-stu-id="53f1b-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="53f1b-125">Wenn die Benutzer Ihre Website besuchen und wie lange Sie verbringen.</span><span class="sxs-lookup"><span data-stu-id="53f1b-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="53f1b-126">Aus welchen Ländern die Besucher stammen.</span><span class="sxs-lookup"><span data-stu-id="53f1b-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="53f1b-127">Welche Browser und Betriebssysteme von den Besuchern verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="53f1b-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="53f1b-129">Verwenden eines Hilfsobjekts zum Hinzufügen von Analysen zu einer Seite</span><span class="sxs-lookup"><span data-stu-id="53f1b-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="53f1b-130">ASP.net Web Pages umfasst mehrere Analyse Hilfen (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`und `Analytics.GetStatCounterHtml`), die die Verwaltung der für die Analyse verwendeten JavaScript-Ausschnitte erleichtern.</span><span class="sxs-lookup"><span data-stu-id="53f1b-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="53f1b-131">Anstatt herauszufinden, wie und wo der JavaScript-Code abgelegt werden soll, müssen Sie nur das Hilfsprogramm zu einer Seite hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="53f1b-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="53f1b-132">Die einzigen Informationen, die Sie bereitstellen müssen, sind Ihr Konto Name, Ihre ID oder der nach Verfolgungs Code.</span><span class="sxs-lookup"><span data-stu-id="53f1b-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="53f1b-133">(Für den Status Counter müssen Sie auch einige zusätzliche Werte angeben.)</span><span class="sxs-lookup"><span data-stu-id="53f1b-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="53f1b-134">In diesem Verfahren erstellen Sie eine Layoutseite, die das `GetGoogleHtml`-Hilfsprogramm verwendet.</span><span class="sxs-lookup"><span data-stu-id="53f1b-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="53f1b-135">Wenn Sie bereits über ein Konto mit einem der anderen Analyse Anbieter verfügen, können Sie stattdessen dieses Konto verwenden und bei Bedarf geringfügige Anpassungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="53f1b-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="53f1b-136">Wenn Sie ein Analytics-Konto erstellen, registrieren Sie die URL der Website, die Sie nachverfolgen möchten.</span><span class="sxs-lookup"><span data-stu-id="53f1b-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="53f1b-137">Wenn Sie alles auf dem lokalen Computer testen, wird der tatsächliche Datenverkehr (nur der Datenverkehr) nachverfolgt, sodass Sie keine Website Statistiken aufzeichnen und anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="53f1b-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="53f1b-138">Dieses Verfahren zeigt jedoch, wie Sie einer Seite ein Analytics-Hilfsprogramm hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="53f1b-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="53f1b-139">Wenn Sie Ihre Website veröffentlichen, sendet die Live Site Informationen an ihren Analyse Anbieter.</span><span class="sxs-lookup"><span data-stu-id="53f1b-139">When you publish your site, the live site will send information to your analytics provider.</span></span>

1. <span data-ttu-id="53f1b-140">Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, wenn Sie Sie noch nicht hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="53f1b-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="53f1b-141">Erstellen Sie ein Konto mit Google Analytics, und notieren Sie sich den Kontonamen.</span><span class="sxs-lookup"><span data-stu-id="53f1b-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="53f1b-142">Erstellen Sie eine Layoutseite mit dem Namen *Analytics. cshtml* , und fügen Sie das folgende Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="53f1b-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="53f1b-143">Sie müssen den aufzurufenden `Analytics`-Hilfsprogramms im Text der Webseite (vor dem `</body>`-Tag) platzieren.</span><span class="sxs-lookup"><span data-stu-id="53f1b-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="53f1b-144">Andernfalls führt der Browser das Skript nicht aus.</span><span class="sxs-lookup"><span data-stu-id="53f1b-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="53f1b-145">Wenn Sie einen anderen Analyse Anbieter verwenden, verwenden Sie stattdessen eines der folgenden Hilfsprogramme:</span><span class="sxs-lookup"><span data-stu-id="53f1b-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="53f1b-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="53f1b-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="53f1b-147">(Status Counter) `@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="53f1b-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="53f1b-148">Ersetzen Sie `myaccount` durch den Namen des Kontos, der ID oder des nach Verfolgungs Codes, den Sie in Schritt 1 erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="53f1b-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="53f1b-149">Führen Sie die Seite im Browser aus.</span><span class="sxs-lookup"><span data-stu-id="53f1b-149">Run the page in the browser.</span></span> <span data-ttu-id="53f1b-150">(Stellen Sie sicher, dass die Seite im Arbeitsbereich " **Dateien** " ausgewählt ist, bevor Sie Sie ausführen.)</span><span class="sxs-lookup"><span data-stu-id="53f1b-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="53f1b-151">Zeigen Sie im Browser die Seitenquelle an.</span><span class="sxs-lookup"><span data-stu-id="53f1b-151">In the browser, view the page source.</span></span> <span data-ttu-id="53f1b-152">Der gerenderte Analyse Code kann angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="53f1b-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="53f1b-153">Melden Sie sich bei der Google Analytics-Website an, und überprüfen Sie die Statistiken für Ihre Website</span><span class="sxs-lookup"><span data-stu-id="53f1b-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="53f1b-154">Wenn Sie die Seite auf einer Live Website ausführen, wird ein Eintrag angezeigt, der den Besuch auf Ihrer Seite protokolliert.</span><span class="sxs-lookup"><span data-stu-id="53f1b-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="53f1b-155">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="53f1b-155">Additional Resources</span></span>

- [<span data-ttu-id="53f1b-156">Google Analytics-Website</span><span class="sxs-lookup"><span data-stu-id="53f1b-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="53f1b-157">Yahoo! Web Analytics-Website</span><span class="sxs-lookup"><span data-stu-id="53f1b-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="53f1b-158">Status Counter-Website</span><span class="sxs-lookup"><span data-stu-id="53f1b-158">StatCounter site</span></span>](http://statcounter.com/)
