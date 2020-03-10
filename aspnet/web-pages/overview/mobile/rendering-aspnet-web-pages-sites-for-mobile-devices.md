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
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Rendern von ASP.net Web Pages-Websites (Razor) für mobile Geräte

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird beschrieben, wie Sie Seiten auf einer ASP.net Web Pages-Website (Razor-Website) erstellen, die auf mobilen Geräten entsprechend dargestellt wird.
> 
> Sie lernen Folgendes:
> 
> - Verwenden einer Benennungs Konvention, um anzugeben, dass eine Seite speziell für mobile Geräte entworfen wurde.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 3
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.

Mit ASP.net Web Pages können Sie benutzerdefinierte anzeigen zum Rendern von Inhalten auf mobilen oder anderen Geräten erstellen.

Die einfachste Möglichkeit zum Erstellen einer gerätespezifischen Seite in einer ASP.net Web Pages Site ist die Verwendung eines Datei Benennungs Musters wie dem folgenden: *filename. Mobile. cshtml*. Sie können zwei Versionen einer Seite erstellen (z. b. " *MyFile. cshtml* " und " *MyFile. Mobile. cshtml*"). Wenn ein mobiles Gerät zur Laufzeit *MyFile. cshtml*anfordert, rendert ASP.NET den Inhalt aus *MyFile. Mobile. cshtml*. Andernfalls wird *MyFile. cshtml* gerendert.

Im folgenden Beispiel wird gezeigt, wie das mobile Rendering durch Hinzufügen einer Inhaltsseite für mobile Geräte aktiviert wird. *"Page1. cshtml* enthält Inhalt und eine Navigations Rand Leiste. *"Page1. Mobile. cshtml* enthält denselben Inhalt, lässt jedoch die Rand Leiste aus.

1. Erstellen Sie an einer ASP.net Web Pages Website eine Datei mit dem Namen *"Page1. cshtml* , und ersetzen Sie den aktuellen Inhalt durch das folgende Markup.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Erstellen Sie eine Datei mit dem Namen *"Page1. Mobile. cshtml* , und ersetzen Sie den vorhandenen Inhalt durch das folgende Markup. Beachten Sie, dass die Mobile Version der Seite den Navigationsbereich zum besseren Rendering auf einem kleineren Bildschirm auslässt.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Führen Sie einen Desktop Browser aus, und navigieren Sie zu *"Page1. cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Führen Sie einen mobilen Browser (oder einen Emulator für mobile Geräte) aus, und navigieren Sie zu *"Page1. cshtml*. (Beachten Sie, dass Sie nicht " *. Mobile* " einschließen. als Teil der URL.) Obwohl es sich bei der Anforderung um *"Page1. cshtml*handelt, rendert ASP.net *" Page1. Mobile. cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Zum Testen von mobilen Seiten können Sie einen Simulator für mobile Geräte verwenden, der auf einem Desktop Computer ausgeführt wird. Mit diesem Tool können Sie Webseiten so testen, wie Sie auf mobilen Geräten aussehen würden (in der Regel mit einem viel geringeren Anzeigebereich). Ein Beispiel für einen Simulator ist das [Benutzer-Agent-Switcher-Add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) für Mozilla Firefox, mit dem Sie verschiedene Mobile Browser von einer Desktop Version von Firefox emulieren können.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Windows Phone-Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
