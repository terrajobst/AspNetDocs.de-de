---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Bündeln und minimieren von Assets in einer ASP.net Web Pages-(Razor-) Site | Microsoft-Dokumentation
author: microsoft
description: Bündelung und Minimierung sind Möglichkeiten, Ihre Website schneller zu gestalten. Mithilfe der Bündelung können Sie mehrere JavaScript-Dateien (. js) oder mehrere Cascading Stylesheets (...
ms.author: riande
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 5e42111ad71ec65581e56c73822e23ecd5fcbd58
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78516177"
---
# <a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="42fd0-104">Bündeln und Minimieren der Objekte in einer ASP.NET Web Pages-Website (Razor)</span><span class="sxs-lookup"><span data-stu-id="42fd0-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="42fd0-105">von [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="42fd0-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="42fd0-106">Bündelung und Minimierung sind Möglichkeiten, Ihre Website schneller zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="42fd0-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="42fd0-107">Mithilfe der Bündelung können Sie mehrere JavaScript-Dateien ( *. js*) oder mehrere Cascading Stylesheet-Dateien (*CSS*-Dateien) kombinieren, sodass Sie als Einheit anstatt einzeln heruntergeladen werden können.</span><span class="sxs-lookup"><span data-stu-id="42fd0-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="42fd0-108">Bei der Minimierung werden Leerräume gequetscht und andere Komprimierungs Typen durchführt, um die heruntergeladenen Dateien so klein wie möglich zu machen.</span><span class="sxs-lookup"><span data-stu-id="42fd0-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="42fd0-109">Die RC-Version von ASP.net Web Pages 2 unterstützt das bündeln und minimieren nicht, da das Paket, das die erforderlichen Elemente enthält, in Microsoft webmatrix noch nicht verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="42fd0-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="42fd0-110">Wir entschuldigen uns für die Unannehmlichkeiten.</span><span class="sxs-lookup"><span data-stu-id="42fd0-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="42fd0-111">Es wird erwartet, dass das Paket in der endgültigen Version von ASP.net Web Pages 2 und webmatrix 2 verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="42fd0-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
