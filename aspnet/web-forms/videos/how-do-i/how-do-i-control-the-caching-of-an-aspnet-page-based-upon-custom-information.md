---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Gewusst wie:] Steuern der Zwischenspeicherung einer ASP.NET-Seite basierend auf benutzerdefinierten Informationen | Microsoft-Dokumentation'
author: rick-anderson
description: In diesem Video zeigt Chris Pels, wie die Kriterien zum Zwischenspeichern einer ASP.NET-Seite basierend auf benutzerdefinierten Informationen gesteuert werden. Es wird eine Beispielseite erstellt, und dann wird der O...
ms.author: riande
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: 676bc8f234a6e517104d07fd58a0ff14aec73e69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78506769"
---
# <a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="85e3d-104">[Gewusst wie:] Steuern der Zwischenspeicherung einer ASP.NET-Seite basierend auf benutzerdefinierten Informationen</span><span class="sxs-lookup"><span data-stu-id="85e3d-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>

<span data-ttu-id="85e3d-105">von [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="85e3d-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="85e3d-106">In diesem Video zeigt Chris Pels, wie die Kriterien zum Zwischenspeichern einer ASP.NET-Seite basierend auf benutzerdefinierten Informationen gesteuert werden.</span><span class="sxs-lookup"><span data-stu-id="85e3d-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="85e3d-107">Eine Beispielseite wird erstellt, und anschließend wird die OutputCache-Direktive mit dem VaryByCustom-Attribut verwendet, das einen benutzerdefinierten Wert enthält.</span><span class="sxs-lookup"><span data-stu-id="85e3d-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="85e3d-108">Im nächsten Schritt wird die getvarycustombystring ()-Methode im Global. asax-Modul überschrieben, das die Handhabung des benutzerdefinierten Attributs bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="85e3d-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="85e3d-109">In dieser Methode wird eine Zeichenfolge zurückgegeben, die die zwischengespeicherte Version der Seite eindeutig identifiziert.</span><span class="sxs-lookup"><span data-stu-id="85e3d-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="85e3d-110">Schließlich wird erläutert, wie die Zwischenspeicherung mithilfe eines benutzerdefinierten Werts auf verschiedene Weise für eine Website verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="85e3d-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="85e3d-111">&#9654;Video ansehen (12 Minuten)</span><span class="sxs-lookup"><span data-stu-id="85e3d-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
