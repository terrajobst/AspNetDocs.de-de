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
# <a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Erstellen lesbarer URLs in ASP.net Web Pages Websites (Razor)

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird das Routing auf einer ASP.net Web Pages-Website (Razor) beschrieben, und Sie erfahren, wie Sie URLs verwenden können, die besser lesbar und besser für SEO geeignet sind.
> 
> Sie lernen Folgendes:
> 
> - Wie ASP.NET das Routing verwendet, damit Sie besser lesbare und durchsuchbare URLs verwenden können.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 3
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.

## <a name="about-routing"></a>Informationen zum Routing

Die URLs für die Seiten in Ihrer Website können sich darauf auswirken, wie gut die Site funktioniert. Eine URL, die &quot;Friendly&quot; ist, kann es Benutzern erleichtern, die Website zu verwenden. Sie kann auch bei der Suche-Engine-Optimierung (SEO) für die Site helfen. ASP.NET Websites bieten die Möglichkeit, freundliche URLs automatisch zu verwenden.

Mit ASP.net können Sie sinnvolle URLs erstellen, die Benutzeraktionen beschreiben, anstatt nur auf eine Datei auf dem Server zu verweisen. Beachten Sie diese URLs für einen fiktiven Blog:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Vergleichen Sie diese URLs mit den folgenden URLs:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

Im ersten paar muss ein Benutzer wissen, dass der Blog mithilfe der Seite " *Blog. cshtml* " angezeigt wird, und dann eine Abfrage Zeichenfolge erstellen, die die richtige Kategorie oder den richtigen Datumsbereich abruft. Der zweite Satz von Beispielen ist viel leichter zu verstehen und zu erstellen.

Die URLs für das erste Beispiel zeigen auch direkt auf eine bestimmte Datei (*Blog. cshtml*). Wenn der Blog aus irgendeinem Grund in einen anderen Ordner auf dem Server verschoben wurde, oder wenn der Blog so umgeschrieben wurde, dass eine andere Seite verwendet wird, sind die Links falsch. Der zweite Satz von URLs verweist nicht auf eine bestimmte Seite. auch wenn sich die Blog Implementierung oder der Speicherort ändert, sind die URLs weiterhin gültig.

In ASP.net Web Pages können Sie benutzerfreundlicheren-URLs wie in den obigen Beispielen erstellen, da ASP.NET das *Routing*verwendet. Routing erstellt eine logische Zuordnung von einer URL zu einer Seite (oder Seiten), die die Anforderung erfüllen kann. Da die Zuordnung für eine bestimmte Datei logisch (nicht physisch) ist, bietet das Routing eine hohe Flexibilität beim Definieren der URLs für Ihre Website.

## <a name="how-routing-works"></a>Funktionsweise des Routings

Wenn ASP.net eine Anforderung verarbeitet, liest Sie die URL, um zu bestimmen, wie Sie weitergeleitet werden soll. ASP.net versucht, einzelne Segmente der URL zu Dateien auf dem Datenträger abzugleichen, von links nach rechts. Wenn eine Entsprechung vorliegt, wird alles, was in der URL verbleiben, als *Pfadinformationen*an die Seite übermittelt.

Stellen Sie sich vor, dass jemand eine Anforderung mit dieser URL sendet:

`http://www.contoso.com/a/b/c`

Die Suche verläuft wie folgt:

1. Gibt es eine Datei mit dem Pfad und dem Namen */a/b/c.cshtml*? Wenn dies der Fall ist, führen Sie diese Seite aus und übergeben keine Informationen an Sie. Andernfalls...
2. Gibt es eine Datei mit dem Pfad und dem Namen */a/b.cshtml*? Wenn dies der Fall ist, führen Sie diese Seite aus, und übergeben Sie den Wert `c`. Andernfalls...
3. Gibt es eine Datei mit dem Pfad und dem Namen */a.cshtml*? Wenn dies der Fall ist, führen Sie diese Seite aus, und übergeben Sie den Wert `b/c`.

Wenn bei der Suche keine genauen Übereinstimmungen für *cshtml* -Dateien in den angegebenen Ordnern gefunden werden, sucht ASP.net weiterhin nach diesen Dateien:

1. */a/b/c/default.cshtml* (keine Pfadinformationen).
2. */a/b/c/Index.cshtml* (keine Pfadinformationen).

> [!NOTE]
> Um klar zu sein, funktionieren Anforderungen für bestimmte Seiten (d. h. Anforderungen, die die Dateinamenerweiterung *. cshtml* enthalten) genauso wie erwartet. Bei einer Anforderung wie `http://www.contoso.com/a/b.cshtml` wird die Seite *b. cshtml* problemlos ausgeführt.

Innerhalb einer Seite können Sie die Pfadinformationen über die `UrlData`-Eigenschaft der Seite, die ein Wörterbuch ist, erhalten. Stellen Sie sich vor, dass Sie eine Datei mit dem Namen " *viewcustomers. cshtml* " haben und Ihre Website diese Anforderung erhält:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Wie in den obigen Regeln beschrieben, wird die Anforderung an Ihre Seite weitergeleitet. Auf der Seite können Sie Code wie den folgenden verwenden, um die Pfadinformationen (in diesem Fall den Wert &quot;1000&quot;) zu erhalten und anzuzeigen:

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Da das Routing keine vollständigen Dateinamen umfasst, kann Mehrdeutigkeit auftreten, wenn Sie über Seiten mit dem gleichen Namen, aber mit unterschiedlichen Dateinamen Erweiterungen verfügen (z. b. *MyPage. cshtml* und *MyPage. html*). Um Probleme mit dem Routing zu vermeiden, sollten Sie sicherstellen, dass keine Seiten auf Ihrer Website vorhanden sind, deren Namen sich nur in ihrer Erweiterung unterscheiden.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Webmatrix-URLs, UrlData und Routing für SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). In diesem Blogeintrag von Mike Brind finden Sie weitere Informationen zur Funktionsweise des Routings in ASP.net Web Pages.
