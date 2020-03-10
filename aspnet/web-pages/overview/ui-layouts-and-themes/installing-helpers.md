---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Installieren eines Hilfsobjekts in einer ASP.net Web Pages-(Razor-) Site | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Artikel wird beschrieben, wie ein Hilfsprogramm auf einer ASP.net Web Pages-Website (Razor) installiert wird. Ein Hilfsprogramm ist eine wiederverwendbare Komponente, die Code und Markup für pro...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 41e33c04a53a6ad257c3937cdadcec767e9217c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518643"
---
# <a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="0bd5b-104">Installieren eines Hilfsobjekts in einer ASP.net Web Pages-(Razor-) Site</span><span class="sxs-lookup"><span data-stu-id="0bd5b-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="0bd5b-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0bd5b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0bd5b-106">In diesem Artikel wird beschrieben, wie ein Hilfsprogramm auf einer ASP.net Web Pages-Website (Razor) installiert wird.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="0bd5b-107">Ein Hilfsprogramm ist eine wiederverwendbare Komponente, die Code und Markup enthält, *um eine Aufgabe* auszuführen, die vielleicht mühsam oder komplex ist.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="0bd5b-108">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0bd5b-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="0bd5b-109">Installieren eines Hilfsprogramms in einer mit webmatrix 3 erstellten Website.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0bd5b-110">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="0bd5b-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="0bd5b-111">Webmatrix 3</span><span class="sxs-lookup"><span data-stu-id="0bd5b-111">WebMatrix 3</span></span>

## <a name="overview-of-helpers"></a><span data-ttu-id="0bd5b-112">Übersicht über Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="0bd5b-112">Overview of Helpers</span></span>

<span data-ttu-id="0bd5b-113">Einige Aufgaben, die häufig auf Webseiten ausgeführt werden sollen, erfordern viel Code oder erfordern zusätzliches wissen.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="0bd5b-114">Beispiele hierfür sind das Anzeigen eines Diagramms für Daten. Verschieben einer Twitter-Schaltfläche "Follow" auf einer Seite Senden von e-Mails von Ihrer Website Zuschneiden oder Ändern der Größe von Bildern Verwenden von PayPal für Ihre Website.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="0bd5b-115">Um dies zu vereinfachen, können *Sie mit ASP.net Web Pages Hilfsprogramme verwenden.*</span><span class="sxs-lookup"><span data-stu-id="0bd5b-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="0bd5b-116">Hilfsprogramme sind Komponenten, die Sie für einen-Standort installieren und mit denen Sie typische Aufgaben durchführen können, indem Sie nur eine Zeile oder zwei Razor-Codes verwenden.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="0bd5b-117">In ASP.net Web Pages sind einige hilfshilfen integriert.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="0bd5b-118">Viele Hilfsprogramme sind jedoch in Paketen (Add-Ins) verfügbar, die mit dem nuget-Paket-Manager bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="0bd5b-119">Mit nuget können Sie ein Paket für die Installation auswählen. Anschließend werden alle Details der Installation behandelt.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="0bd5b-120">Installieren eines Hilfsprogramms in webmatrix 3</span><span class="sxs-lookup"><span data-stu-id="0bd5b-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="0bd5b-121">Klicken Sie in webmatrix 3 auf die Schaltfläche **nuget** .</span><span class="sxs-lookup"><span data-stu-id="0bd5b-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Dialogfeld "nuget-Katalog" in webmatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="0bd5b-123">Der nuget-Paket-Manager wird gestartet, und die verfügbaren Pakete werden angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="0bd5b-124">Geben Sie im Suchfeld ein Schlüsselwort für das Hilfsprogramm ein, das Sie installieren möchten.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Dialogfeld "nuget-Katalog" in webmatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="0bd5b-126">Wählen Sie das Paket aus, und klicken Sie auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="0bd5b-127">Klicken Sie bei der Frage, ob Sie das Paket installieren möchten, auf **Ja** , und geben Sie an, dass Sie die Bedingungen akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="0bd5b-128">Wenn Sie das Hilfsprogramm zum ersten Mal installiert haben, erstellt nuget für den Code, der das Hilfsprogramm bildet, Ordner auf Ihrer Website.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="0bd5b-129">Wenn Sie ein Hilfsprogramm deinstallieren möchten, klicken Sie auf die Schaltfläche **Galerie** , klicken Sie auf die Registerkarte **installiert** , und wählen Sie das zu deinstallierende</span><span class="sxs-lookup"><span data-stu-id="0bd5b-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="0bd5b-130">Installieren des Twitter-Hilfsprogramms</span><span class="sxs-lookup"><span data-stu-id="0bd5b-130">Installing the Twitter helper</span></span>

<span data-ttu-id="0bd5b-131">Die neueste Version der Twitter-API ist nicht mit dem Twitter-Hilfsprogramm kompatibel, das Sie über nuget installieren.</span><span class="sxs-lookup"><span data-stu-id="0bd5b-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="0bd5b-132">Informationen zum Einrichten des Twitter-Hilfsprogramms in Ihrem Projekt finden Sie stattdessen im Twitter-Hilfsprogramm [mit webmatrix](twitter-helper.md) .</span><span class="sxs-lookup"><span data-stu-id="0bd5b-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="0bd5b-133">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="0bd5b-133">Additional Resources</span></span>

[<span data-ttu-id="0bd5b-134">Einführung in die Grundlagen der ASP.net Web Pages 2-Programmierung</span><span class="sxs-lookup"><span data-stu-id="0bd5b-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="0bd5b-135">Twitter-Hilfsprogramm mit webmatrix</span><span class="sxs-lookup"><span data-stu-id="0bd5b-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
