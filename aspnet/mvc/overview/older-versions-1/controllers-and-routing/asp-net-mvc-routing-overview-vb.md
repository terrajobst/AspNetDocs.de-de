---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: Übersicht über ASP.NET MVC-Routing (VB) | Microsoft-Dokumentation
author: StephenWalther
description: In diesem Tutorial zeigt Stephen Walther, wie das ASP.NET MVC-Framework Browser Anforderungen Controller Aktionen zuordnet.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: ed043d76b89ce31945cf3423b0c5afca9383cc21
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486897"
---
# <a name="aspnet-mvc-routing-overview-vb"></a>ASP.NET MVC-Routing – Übersicht (VB)

von [Stephen Walther](https://github.com/StephenWalther)

> In diesem Tutorial zeigt Stephen Walther, wie das ASP.NET MVC-Framework Browser Anforderungen Controller Aktionen zuordnet.

In diesem Tutorial haben Sie eine wichtige Funktion jeder ASP.NET MVC-Anwendung mit dem Namen " *ASP.NET Routing*" eingeführt. Das ASP.NET-Routing Modul ist für die Zuordnung eingehender Browser Anforderungen zu bestimmten MVC-Controller Aktionen verantwortlich. Am Ende dieses Tutorials erfahren Sie, wie die Standardrouten Tabelle Anforderungen Controller Aktionen zuordnet.

## <a name="using-the-default-route-table"></a>Verwenden der Standardrouten Tabelle

Wenn Sie eine neue ASP.NET MVC-Anwendung erstellen, ist die Anwendung bereits für die Verwendung des ASP.NET-Routings konfiguriert. ASP.NET Routing ist an zwei Stellen eingerichtet.

Zuerst ist ASP.NET Routing in der Webkonfigurationsdatei der Anwendung (Web. config-Datei) aktiviert. Die Konfigurationsdatei enthält vier Abschnitte, die für das Routing relevant sind: den Abschnitt "System. Web. HttpModules", den Abschnitt "System. Web. httpHandlers", den Abschnitt "System. Webserver. modules" und den Abschnitt "System. Webserver. Handlers". Achten Sie darauf, diese Abschnitte nicht zu löschen, da das Routing ohne diese Abschnitte nicht mehr funktioniert.

Zweitens und noch wichtiger ist, dass eine Routing Tabelle in der Datei Global. asax der Anwendung erstellt wird. Die Datei "Global. asax" ist eine spezielle Datei, die Ereignishandler für ASP.net-Anwendungslebenszyklus-Ereignisse enthält. Die Routing Tabelle wird während des Anwendungs Start Ereignisses erstellt.

Die Datei in "Listing 1" enthält die Standarddatei "Global. asax" für eine ASP.NET MVC-Anwendung.

**Codebeispiel 1: Global. asax. vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

Wenn eine MVC-Anwendung zum ersten Mal gestartet wird, wird die Anwendung\_Start ()-Methode aufgerufen. Diese Methode ruft wiederum die RegisterRoutes ()-Methode auf. Die Methode "RegisterRoutes ()" erstellt die Routing Tabelle.

Die Standardrouten Tabelle enthält eine einzelne Route (mit dem Namen Standard). Die Standardroute ordnet das erste Segment einer URL einem Controller Namen, das zweite Segment einer URL zu einer Controller Aktion und das dritte Segment einem Parameter mit dem Namen " **ID**" zu.

Stellen Sie sich vor, dass Sie die folgende URL in die Adressleiste Ihres Webbrowsers eingeben:

/Home/Index/3

Die Standardroute ordnet diese URL den folgenden Parametern zu:

- Controller = Startseite

- Action = Index

- id = 3

Wenn Sie die URL/Home/Index/3 anfordern, wird der folgende Code ausgeführt:

HomeController. Index (3)

Die Standardroute enthält Standardwerte für alle drei Parameter. Wenn Sie keinen Controller angeben, wird der Controller Parameter standardmäßig auf den Wert **Home**eingestellt. Wenn Sie keine Aktion angeben, wird der Aktionsparameter standardmäßig auf den Wert **Index**eingestellt. Wenn Sie keine ID angeben, wird der ID-Parameter standardmäßig auf eine leere Zeichenfolge zurückgelegt.

Betrachten wir einige Beispiele dafür, wie die Standardroute URLs zu Controller Aktionen zuordnet. Stellen Sie sich vor, dass Sie die folgende URL in die Adressleiste Ihres Browsers eingeben:

/Home

Aufgrund der standardmäßigen Routen Parameter-Standardwerte führt die Eingabe dieser URL dazu, dass die Index ()-Methode der HomeController-Klasse in der Liste 2 aufgerufen wird.

**Codebeispiel 2: HomeController. vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

In der Liste 2 enthält die HomeController-Klasse eine Methode mit dem Namen Index (), die einen einzelnen Parameter mit dem Namen ID akzeptiert. Die URL/Home bewirkt, dass die Index ()-Methode mit dem Wert Nothing als Wert des ID-Parameters aufgerufen wird.

Aufgrund der Art und Weise, wie das MVC-Framework Controller Aktionen aufruft, stimmt die URL/Home auch mit der Index ()-Methode der HomeController-Klasse in der Liste 3 überein.

**Codebeispiel 3: HomeController. vb (Index Aktion ohne Parameter)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

Die Index ()-Methode in der Liste 3 akzeptiert keine Parameter. Die URL/Home bewirkt, dass diese Index ()-Methode aufgerufen wird. Die URL/Home/Index/3 ruft auch diese Methode auf (die ID wird ignoriert).

Die URL/Home entspricht auch der Index ()-Methode der HomeController-Klasse in der Liste 4.

**Codebeispiel 4: HomeController. vb (Index Aktion mit Parametern, die NULL-Werte zulassen)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

In der Liste 4 weist die Index ()-Methode einen ganzzahligen Parameter auf. Da der-Parameter ein Parameter ist, der NULL-Werte zulässt (kann den Wert Nothing aufweisen), kann der Index () aufgerufen werden, ohne dass ein Fehler ausgegeben wird.

Zum Schluss verursacht der Aufruf der Index ()-Methode in der Liste 5 mit der URL/Home eine Ausnahme, da der ID-Parameter *kein Parameter ist* , der NULL-Werte zulässt. Wenn Sie versuchen, die Index ()-Methode aufzurufen, erhalten Sie den in Abbildung 1 angezeigten Fehler.

**Auflisten 5-HomeController. vb (Index Aktion mit ID-Parameter)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]

[![Aufrufen einer Controller Aktion, die einen Parameterwert erwartet](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)

**Abbildung 01**: Aufrufen einer Controller Aktion, die einen Parameterwert erwartet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](asp-net-mvc-routing-overview-vb/_static/image2.png))

Die URL/Home/Index/3 hingegen funktioniert problemlos mit der Index Controller Aktion in der Liste 5. Die Anforderung/Home/Index/3 bewirkt, dass die Index ()-Methode mit einem ID-Parameter mit dem Wert 3 aufgerufen wird.

## <a name="summary"></a>Zusammenfassung

Ziel dieses Tutorials war es, Ihnen eine kurze Einführung in das ASP.NET-Routing zu bieten. Wir haben die Standardrouten Tabelle untersucht, die Sie mit einer neuen ASP.NET MVC-Anwendung erhalten. Sie haben gelernt, wie die Standardroute URLs Controller Aktionen zuordnet.

> [!div class="step-by-step"]
> [Zurück](creating-an-action-cs.md)
> [Weiter](understanding-action-filters-vb.md)
