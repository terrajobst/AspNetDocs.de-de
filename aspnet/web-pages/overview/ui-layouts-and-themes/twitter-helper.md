---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Twitter-Hilfsprogramm mit ASP.net Web Pages | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Thema und dieser Anwendung wird gezeigt, wie Sie Ihrem webmatrix 3-Projekt ein Twitter-Hilfsprogramm hinzufügen. Sie enthält den Twitter-hilfsscode und zeigt, wie das Hilfsprogramm aufgerufen wird...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 76e32b7c808467a9a87c70017dac02bdb895e1df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518619"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="dfc3c-104">Twitter-Hilfsprogramm mit ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="dfc3c-104">Twitter Helper with ASP.NET Web Pages</span></span>

<span data-ttu-id="dfc3c-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dfc3c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dfc3c-106">Twitter-Hilfsprogramme sind veraltet.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="dfc3c-107">Informationen zu den neuesten Engagement-Tools für Twitter finden Sie unter [Übersicht über Twitter für Websites](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span><span class="sxs-lookup"><span data-stu-id="dfc3c-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="dfc3c-108">In diesem Thema und dieser Anwendung wird gezeigt, wie Sie Ihrem webmatrix 3-Projekt ein Twitter-Hilfsprogramm hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="dfc3c-109">Sie enthält den Twitter-hilfsscode und zeigt, wie die Hilfsmethoden aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="dfc3c-110">Dieser Code für die Twitter. cshtml-Datei wurde von der **Tian Pan** von Microsoft entwickelt.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="dfc3c-111">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="dfc3c-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="dfc3c-112">ASP.net Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="dfc3c-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="dfc3c-113">Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="introduction"></a><span data-ttu-id="dfc3c-114">Einführung</span><span class="sxs-lookup"><span data-stu-id="dfc3c-114">Introduction</span></span>

<span data-ttu-id="dfc3c-115">In diesem Thema wird veranschaulicht, wie Sie Ihrer Anwendung ein Twitter-Hilfsprogramm hinzufügen und Razor-Syntax verwenden, um die Hilfsmethoden aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="dfc3c-116">Mit dem Twitter-Hilfsprogramm können Sie ganz einfach Twitter-Schaltflächen und Widgets in Ihre Anwendung integrieren.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="dfc3c-117">Wenn Sie ein Twitter-Widget verwenden möchten, z. b. die Zeitachse eines Benutzers oder die Suchergebnisse für ein hashtag, müssen Sie zuerst das [Widget auf Twitter](https://twitter.com/settings/widgets)erstellen.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="dfc3c-118">Nachdem Sie Ihr Widget erstellt haben, erhalten Sie eine Widget-ID. Sie übergeben diese Widget-ID als Parameter, wenn Sie die Hilfsmethoden aufrufen, die das Widget anzeigen.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="dfc3c-119">Dieses Thema wurde für Version 1,1 der Twitter-API geschrieben.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="dfc3c-120">Wenn Sie den Twitter-Hilfscode direkt zu Ihrem Projekt hinzufügen, können Sie den hilfsscode aktualisieren, wenn sich die Twitter-API ändert.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="dfc3c-121">Weitere Informationen zum Installieren von webmatrix finden Sie unter [Introducing ASP.net Web Pages 2-Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="dfc3c-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="dfc3c-122">Twitter-Hilfsprogramm zu Ihrem Projekt hinzufügen</span><span class="sxs-lookup"><span data-stu-id="dfc3c-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="dfc3c-123">Fügen Sie zum Hinzufügen des Twitter-Hilfsprogramms zunächst dem Projekt einen Ordner mit dem Namen **App\_Code** hinzu.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="dfc3c-124">Erstellen Sie dann eine Datei mit dem Namen **Twitter. cshtml**.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-124">Then, create a file named **Twitter.cshtml**.</span></span>

![App_Code Ordner](twitter-helper/_static/image1.png)

<span data-ttu-id="dfc3c-126">Ersetzen Sie den Standard Code in Twitter. cshtml durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="dfc3c-127">Twitter-Methoden von ihren Webseiten aus anrufen</span><span class="sxs-lookup"><span data-stu-id="dfc3c-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="dfc3c-128">Im folgenden Beispiel wird gezeigt, wie Sie die Twitter-Hilfsmethoden von einer Seite in Ihrem Projekt verwenden können.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="dfc3c-129">In Ihrem Projekt möchten Sie die Parameterwerte durch Werte ersetzen, die für Ihre Anforderungen relevant sind.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="dfc3c-130">Sie können die bereitgestellten Widget-IDs verwenden, um zu untersuchen, wie die Methoden funktionieren, aber Sie möchten Ihre eigenen Widgets für Ihr Projekt generieren.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="dfc3c-131">Nicht alle unten gezeigten Parameter sind erforderlich.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="dfc3c-132">Die optionalen Parameter werden verwendet, um anzupassen, wie die Schaltfläche oder das Widget angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="dfc3c-133">Beispielsweise muss auf die Schaltfläche "nachverfolgen" nur der Benutzername befolgt werden, aber im Beispiel wird gezeigt, wie Sie die Anzahl von Followern einschließen und wie Sie die Größe der Schaltfläche und der Sprache angeben.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="dfc3c-134">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="dfc3c-134">See the results</span></span>

<span data-ttu-id="dfc3c-135">Der obige Code erzeugt die folgenden Schaltflächen und Widgets.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="dfc3c-136">Diese Schaltflächen und Widgets sind voll funktionsfähig, keine Screenshots.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="dfc3c-137">Die Schaltfläche Nachverfolgung wird auf Spanisch angezeigt, da der sprach Parameter auf **es**festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="dfc3c-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="dfc3c-138">Schaltfläche "folgen</span><span class="sxs-lookup"><span data-stu-id="dfc3c-138">Follow Button</span></span>

<span data-ttu-id="dfc3c-139">[@aspnetfolgen)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="dfc3c-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="dfc3c-140">Tweet-Schaltfläche</span><span class="sxs-lookup"><span data-stu-id="dfc3c-140">Tweet Button</span></span>

<span data-ttu-id="dfc3c-141">[Tweet](https://twitter.com/share) -`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="dfc3c-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="dfc3c-142">Benutzer Zeitachse (Profil)</span><span class="sxs-lookup"><span data-stu-id="dfc3c-142">User Timeline (Profile)</span></span>

<span data-ttu-id="dfc3c-143">[Tweets nach @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="dfc3c-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="dfc3c-144">Favoriten</span><span class="sxs-lookup"><span data-stu-id="dfc3c-144">Favorites</span></span>

<span data-ttu-id="dfc3c-145">Die [bevorzugten tweets nach @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="dfc3c-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="dfc3c-146">Liste</span><span class="sxs-lookup"><span data-stu-id="dfc3c-146">List</span></span>

<span data-ttu-id="dfc3c-147">[Tweets von @Microsoft/MS\_Consumer\_Bänder](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="dfc3c-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="dfc3c-148">Suche</span><span class="sxs-lookup"><span data-stu-id="dfc3c-148">Search</span></span>

<span data-ttu-id="dfc3c-149">[Tweets zu &quot;#ASP .net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="dfc3c-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
