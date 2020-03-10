---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
title: Übergeben von Daten an Ansichts MasterC#Seiten () | Microsoft-Dokumentation
author: microsoft
description: In diesem Tutorial wird erläutert, wie Sie Daten von einem Controller an eine Ansichts Master Seite übergeben können. Wir untersuchen zwei Strategien zum Übergeben von Daten an eine Sicht m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 5fee879b-8bde-42a9-a434-60ba6b1cf747
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1492175812b0a092cd1594a770e348efe9b4122b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78485703"
---
# <a name="passing-data-to-view-master-pages-c"></a>Übergeben von Daten an Ansichtsmasterseiten (C#)

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_CS.pdf)

> In diesem Tutorial wird erläutert, wie Sie Daten von einem Controller an eine Ansichts Master Seite übergeben können. Wir untersuchen zwei Strategien zum Übergeben von Daten an eine Ansichts Master Seite. Zuerst wird eine einfache Lösung erläutert, die zu einer Anwendung führt, die schwer zu verwalten ist. Als nächstes untersuchen wir eine viel bessere Lösung, die etwas mehr anfängliche Arbeit erfordert, aber zu einer viel besser verwaltbaren Anwendung führt.

## <a name="passing-data-to-view-master-pages"></a>Übergeben von Daten an Ansichts Master Seiten

In diesem Tutorial wird erläutert, wie Sie Daten von einem Controller an eine Ansichts Master Seite übergeben können. Wir untersuchen zwei Strategien zum Übergeben von Daten an eine Ansichts Master Seite. Zuerst wird eine einfache Lösung erläutert, die zu einer Anwendung führt, die schwer zu verwalten ist. Als nächstes untersuchen wir eine viel bessere Lösung, die etwas mehr anfängliche Arbeit erfordert, aber zu einer viel besser verwaltbaren Anwendung führt.

### <a name="the-problem"></a>Problemstellung

Stellen Sie sich vor, dass Sie eine Movie Database-Anwendung entwickeln und die Liste der Film Kategorien auf jeder Seite in der Anwendung anzeigen möchten (siehe Abbildung 1). Stellen Sie sich außerdem vor, dass die Liste der Film Kategorien in einer Datenbanktabelle gespeichert wird. In diesem Fall wäre es sinnvoll, die Kategorien aus der Datenbank abzurufen und die Liste der Film Kategorien innerhalb einer Ansichts Master Seite zu erstellen.

[![Anzeigen von Film Kategorien auf einer Ansichts Master Seite](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)

**Abbildung 01**: Anzeigen von Film Kategorien in einer Ansichts Master Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](passing-data-to-view-master-pages-cs/_static/image3.png))

Hier ist das Problem. Wie rufen Sie die Liste der Film Kategorien auf der Master Seite ab? Es ist verlockend, Methoden ihrer Modellklassen direkt auf der Master Seite aufzurufen. Anders ausgedrückt: Es ist verlockend, den Code zum Abrufen der Daten aus der Datenbank direkt auf der Master Seite einzuschließen. Die Umgehung ihrer MVC-Controller für den Zugriff auf die Datenbank würde jedoch gegen eine saubere Trennung der Belange verstoßen, die einer der Hauptvorteile des Aufbaus einer MVC-Anwendung ist.

In einer MVC-Anwendung möchten Sie, dass alle Interaktionen zwischen ihren MVC-Ansichten und Ihrem MVC-Modell von ihren MVC-Controllern verarbeitet werden. Diese Trennung von Anliegen führt zu einer besser verwaltbaren, anpassbaren und Test fähigen Anwendung.

In einer MVC-Anwendung sollten alle Daten, die an eine Sicht – einschließlich einer Ansichts Master Seite –, an eine Ansicht durch eine Controller Aktion übermittelt werden. Außerdem sollten die Daten weitergegeben werden, indem Sie die Vorteile von Ansichts Daten nutzen. Im restlichen Teil dieses Tutorials werden zwei Methoden zum Übergeben von Ansichts Daten an eine Ansichts Master Seite untersucht.

### <a name="the-simple-solution"></a>Die einfache Lösung

Wir beginnen mit der einfachsten Lösung, Ansichts Daten von einem Controller an eine Ansichts Master Seite zu übergeben. Die einfachste Lösung besteht darin, die Ansichts Daten für die Master Seite in den einzelnen Controller Aktionen zu übergeben.

Beachten Sie den Controller in der Liste 1. Es macht zwei Aktionen mit dem Namen `Index()` und `Details()`verfügbar. Die `Index()`-Aktionsmethode gibt jeden Film in der Movies-Datenbanktabelle zurück. Die `Details()` Aktionsmethode gibt jeden Film in einer bestimmten Filmkategorie zurück.

**Codebeispiel 1 – `Controllers\HomeController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample1.cs)]

Beachten Sie, dass die Aktionen Index () und Details () zwei Elemente zum Anzeigen von Daten hinzufügen. Die Aktion Index () fügt zwei Schlüssel hinzu: Kategorien und Filme. Der kategorieschlüssel stellt die Liste der Film Kategorien dar, die auf der Seite Master anzeigen angezeigt werden. Der Schlüssel Movies stellt die Liste der Filme dar, die auf der Seite Index Ansicht angezeigt werden.

Mit der Aktion Details () werden auch zwei Schlüssel namens Kategorien und Filme hinzugefügt. Die kategorietaste stellt wiederum die Liste der Film Kategorien dar, die von der Ansichts Master Seite angezeigt werden. Der Schlüssel Movies stellt die Liste der Filme in einer bestimmten Kategorie dar, die auf der Seite Detailansicht angezeigt wird (siehe Abbildung 2).

[![der Detailansicht](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)

**Abbildung 02**: Ansicht "Details" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](passing-data-to-view-master-pages-cs/_static/image6.png))

Die Index Sicht ist in der Liste 2 enthalten. Sie durchläuft einfach die Liste der Filme, die durch das "Movies"-Element in "Daten anzeigen" dargestellt werden.

**Codebeispiel 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample2.aspx)]

Die Seite Master anzeigen ist in der Liste 3 enthalten. Die Seite Master anzeigen durchläuft alle Film Kategorien, die durch das Kategorieelement dargestellt werden, aus den Ansichts Daten.

**Codebeispiel 3 – `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample3.aspx)]

Alle Daten werden über die Ansichts Daten an die Ansicht und die Master Seite der Ansicht übermittelt. Das ist die korrekte Methode, Daten an die Master Seite zu übergeben.

Was ist also mit dieser Lösung falsch? Das Problem besteht darin, dass diese Lösung gegen das trockene Prinzip (Don't repeat yourself) verstößt. Jede Controller Aktion muss dieselbe Liste von Film Kategorien hinzufügen, um Daten anzuzeigen. Durch Duplizieren von Code in der Anwendung wird die Wartung, Anpassung und Änderung Ihrer Anwendung erheblich erschwert.

### <a name="the-good-solution"></a>Die gute Lösung

In diesem Abschnitt untersuchen wir eine Alternative und bessere Lösung für das Übergeben von Daten aus einer Controller Aktion an eine Ansichts Master Seite. Anstatt die Film Kategorien für die Master Seite in den einzelnen Controller Aktionen hinzuzufügen, fügen wir die Film Kategorien nur einmal zu den Ansichts Daten hinzu. Alle Ansichts Daten, die von der Ansichts Master Seite verwendet werden, werden in einem Anwendungs Controller hinzugefügt.

Die ApplicationController-Klasse ist in der Liste 4 enthalten.

**Codebeispiel 4 – `Controllers\ApplicationController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample4.cs)]

Es gibt drei Dinge, die Sie über den Anwendungs Controller in der Liste 4 bemerken sollten. Beachten Sie zunächst, dass die-Klasse von der System. Web. MVC. Controller-Basisklasse erbt. Der Anwendungs Controller ist eine Controller Klasse.

Beachten Sie, dass die Application Controller-Klasse eine abstrakte Klasse ist. Eine abstrakte Klasse ist eine Klasse, die von einer konkreten Klasse implementiert werden muss. Da der Anwendungs Controller eine abstrakte Klasse ist, können Sie keine in der-Klasse definierten Methoden direkt aufrufen. Wenn Sie versuchen, die Anwendungsklasse direkt aufzurufen, erhalten Sie die Fehlermeldung "Ressource wurde nicht gefunden".

Beachten Sie, dass der Anwendungs Controller einen Konstruktor enthält, mit dem die Liste der Film Kategorien zum Anzeigen von Daten hinzugefügt wird. Jede Controller Klasse, die vom Anwendungs Controller erbt, Ruft den Konstruktor des Anwendungs Controllers automatisch auf. Jedes Mal, wenn Sie eine Aktion auf einem Controller, der vom Anwendungs Controller erbt, aufgerufen wird, sind die Film Kategorien automatisch in den Ansichts Daten enthalten.

Der Filme Controller in der Liste 5 erbt vom Anwendungs Controller.

**Codebeispiel 5 – `Controllers\MoviesController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample5.cs)]

Der Filme Controller macht genau wie der Home Controller, der im vorherigen Abschnitt erläutert wurde, zwei Aktionsmethoden mit dem Namen `Index()` und `Details()`verfügbar. Beachten Sie, dass die Liste der Film Kategorien, die auf der Seite Master anzeigen angezeigt werden, nicht zum Anzeigen von Daten in der `Index()`-oder `Details()`-Methode hinzugefügt wird. Da der Filme Controller vom Anwendungs Controller erbt, wird die Liste der Film Kategorien hinzugefügt, um Daten automatisch anzuzeigen.

Beachten Sie, dass diese Lösung zum Hinzufügen von Ansichts Daten für eine Ansichts Master Seite nicht gegen das Dry-Prinzip (Don't repeat yourself) verstößt. Der Code zum Hinzufügen der Liste der Film Kategorien zum Anzeigen von Daten ist nur an einem Speicherort enthalten: dem Konstruktor für den Anwendungs Controller.

### <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben wir zwei Ansätze erläutert, um Ansichts Daten von einem Controller an eine Ansichts Master Seite zu übergeben. Zuerst haben wir eine einfache, aber schwer zu verwaltende Methode untersucht. Im ersten Abschnitt wurde erläutert, wie Sie Ansichts Daten für eine Ansichts Master Seite in jeder Controller Aktion in Ihrer Anwendung hinzufügen können. Wir haben festgestellt, dass dies ein fehlerhafter Ansatz ist, weil er gegen das trockene (Don't repeat yourself) Prinzip verstößt.

Als nächstes haben wir eine viel bessere Strategie zum Hinzufügen von Daten untersucht, die für eine Ansichts Master Seite erforderlich sind, um Daten anzuzeigen Anstatt die Ansichts Daten in den einzelnen Controller Aktionen hinzuzufügen, haben wir die Ansichts Daten nur einmal innerhalb eines Anwendungs Controllers hinzugefügt. Auf diese Weise können Sie doppelten Code vermeiden, wenn Sie Daten an eine Ansichts Master Seite in einer ASP.NET MVC-Anwendung übergeben.

> [!div class="step-by-step"]
> [Zurück](creating-page-layouts-with-view-master-pages-cs.md)
> [Weiter](asp-net-mvc-views-overview-vb.md)
