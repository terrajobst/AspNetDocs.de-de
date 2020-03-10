---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Rendern von ASP.net Web Pages Websites (Razor) für mobile Geräte | Microsoft-Dokumentation
author: Rick-Anderson
description: 'In diesem Artikel wird beschrieben, wie Sie Seiten auf einer ASP.net Web Pages-Website (Razor-Website) erstellen, die auf mobilen Geräten entsprechend dargestellt wird. Was Sie lernen werden: wie...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: c012348d65e48a275cb0e4808fef2a7f31e5fb33
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454353"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="eb419-104">Rendern von ASP.net Web Pages-Websites (Razor) für mobile Geräte</span><span class="sxs-lookup"><span data-stu-id="eb419-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>

<span data-ttu-id="eb419-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="eb419-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="eb419-106">In diesem Artikel wird beschrieben, wie Sie Seiten auf einer ASP.net Web Pages-Website (Razor-Website) erstellen, die auf mobilen Geräten entsprechend dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="eb419-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="eb419-107">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="eb419-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="eb419-108">Verwenden einer Benennungs Konvention, um anzugeben, dass eine Seite speziell für mobile Geräte entworfen wurde.</span><span class="sxs-lookup"><span data-stu-id="eb419-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="eb419-109">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="eb419-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="eb419-110">ASP.net Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="eb419-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="eb419-111">Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="eb419-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="eb419-112">Mit ASP.net Web Pages können Sie benutzerdefinierte anzeigen zum Rendern von Inhalten auf mobilen oder anderen Geräten erstellen.</span><span class="sxs-lookup"><span data-stu-id="eb419-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="eb419-113">Die einfachste Möglichkeit zum Erstellen einer gerätespezifischen Seite in einer ASP.net Web Pages Site ist die Verwendung eines Datei Benennungs Musters wie dem folgenden: *filename. Mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="eb419-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.Mobile.cshtml*.</span></span> <span data-ttu-id="eb419-114">Sie können zwei Versionen einer Seite erstellen (z. b. " *MyFile. cshtml* " und " *MyFile. Mobile. cshtml*").</span><span class="sxs-lookup"><span data-stu-id="eb419-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="eb419-115">Wenn ein mobiles Gerät zur Laufzeit *MyFile. cshtml*anfordert, rendert ASP.NET den Inhalt aus *MyFile. Mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="eb419-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="eb419-116">Andernfalls wird *MyFile. cshtml* gerendert.</span><span class="sxs-lookup"><span data-stu-id="eb419-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="eb419-117">Im folgenden Beispiel wird gezeigt, wie das mobile Rendering durch Hinzufügen einer Inhaltsseite für mobile Geräte aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="eb419-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="eb419-118">*"Page1. cshtml* enthält Inhalt und eine Navigations Rand Leiste.</span><span class="sxs-lookup"><span data-stu-id="eb419-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="eb419-119">*"Page1. Mobile. cshtml* enthält denselben Inhalt, lässt jedoch die Rand Leiste aus.</span><span class="sxs-lookup"><span data-stu-id="eb419-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="eb419-120">Erstellen Sie an einer ASP.net Web Pages Website eine Datei mit dem Namen *"Page1. cshtml* , und ersetzen Sie den aktuellen Inhalt durch das folgende Markup.</span><span class="sxs-lookup"><span data-stu-id="eb419-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="eb419-121">Erstellen Sie eine Datei mit dem Namen *"Page1. Mobile. cshtml* , und ersetzen Sie den vorhandenen Inhalt durch das folgende Markup.</span><span class="sxs-lookup"><span data-stu-id="eb419-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="eb419-122">Beachten Sie, dass die Mobile Version der Seite den Navigationsbereich zum besseren Rendering auf einem kleineren Bildschirm auslässt.</span><span class="sxs-lookup"><span data-stu-id="eb419-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="eb419-123">Führen Sie einen Desktop Browser aus, und navigieren Sie zu *"Page1. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="eb419-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="eb419-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eb419-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="eb419-125">Führen Sie einen mobilen Browser (oder einen Emulator für mobile Geräte) aus, und navigieren Sie zu *"Page1. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="eb419-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="eb419-126">(Beachten Sie, dass Sie nicht " *. Mobile* " einschließen.</span><span class="sxs-lookup"><span data-stu-id="eb419-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="eb419-127">als Teil der URL.) Obwohl es sich bei der Anforderung um *"Page1. cshtml*handelt, rendert ASP.net *" Page1. Mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="eb419-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="eb419-129">Zum Testen von mobilen Seiten können Sie einen Simulator für mobile Geräte verwenden, der auf einem Desktop Computer ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="eb419-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="eb419-130">Mit diesem Tool können Sie Webseiten so testen, wie Sie auf mobilen Geräten aussehen würden (in der Regel mit einem viel geringeren Anzeigebereich).</span><span class="sxs-lookup"><span data-stu-id="eb419-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="eb419-131">Ein Beispiel für einen Simulator ist das [Benutzer-Agent-Switcher-Add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) für Mozilla Firefox, mit dem Sie verschiedene Mobile Browser von einer Desktop Version von Firefox emulieren können.</span><span class="sxs-lookup"><span data-stu-id="eb419-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="eb419-132">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="eb419-132">Additional Resources</span></span>

<span data-ttu-id="eb419-133">[Windows Phone-Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="eb419-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
