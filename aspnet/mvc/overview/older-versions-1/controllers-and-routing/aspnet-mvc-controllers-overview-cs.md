---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: ASP.NET MVC-Controller ÜbersichtC#() | Microsoft-Dokumentation
author: StephenWalther
description: In diesem Tutorial führt Stephen Walther Sie in ASP.NET MVC-Controller ein. Sie erfahren, wie Sie neue Controller erstellen und verschiedene Arten von Aktionen zurückgeben...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a287b37742400a17c2ed53cfd00bfb053b4f3d2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437679"
---
# <a name="aspnet-mvc-controller-overview-c"></a>ASP.NET MVC-Controller – Übersicht (C#)

von [Stephen Walther](https://github.com/StephenWalther)

> In diesem Tutorial führt Stephen Walther Sie in ASP.NET MVC-Controller ein. Sie erfahren, wie Sie neue Controller erstellen und unterschiedliche Aktions Ergebnistypen zurückgeben.

In diesem Tutorial wird das Thema ASP.NET MVC-Controller, Controller Aktionen und Aktions Ergebnisse erläutert. Nach Abschluss dieses Tutorials erfahren Sie, wie Controller verwendet werden, um zu steuern, wie ein Besucher mit einer ASP.NET MVC-Website interagiert.

## <a name="understanding-controllers"></a>Grundlagen der Controller

MVC-Controller sind dafür verantwortlich, auf Anforderungen zu reagieren, die für eine ASP.NET MVC-Website gestellt werden. Jede Browser Anforderung wird einem bestimmten Controller zugeordnet. Stellen Sie sich beispielsweise vor, dass Sie die folgende URL in die Adressleiste Ihres Browsers eingeben:

`http://localhost/Product/Index/3`

In diesem Fall wird ein Controller mit dem Namen ProductController aufgerufen. Der ProductController ist dafür verantwortlich, die Antwort auf die Browser Anforderung zu erstellen. Beispielsweise kann der Controller eine bestimmte Ansicht zurück an den Browser zurückgeben oder der Controller kann den Benutzer an einen anderen Controller umleiten.

Die Auflistung 1 enthält einen einfachen Controller mit dem Namen ProductController.

**Listing1-controllers\productcontroller.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

Wie Sie in der Liste 1 sehen können, ist ein Controller nur eine Klasse (ein Visual Basic .net C# oder eine Klasse). Ein Controller ist eine Klasse, die von der System. Web. MVC. Controller-Basisklasse abgeleitet wird. Da ein Controller von dieser Basisklasse erbt, erbt ein Controller mehrere nützliche Methoden kostenlos (diese Methoden werden in Kürze besprochen).

## <a name="understanding-controller-actions"></a>Grundlegendes zu Controller Aktionen

Controller Aktionen werden von einem Controller bereitstellt. Eine Aktion ist eine Methode auf einem Controller, die aufgerufen wird, wenn Sie eine bestimmte URL in der Adressleiste Ihres Browsers eingeben. Stellen Sie sich beispielsweise vor, dass Sie eine Anforderung für die folgende URL senden:

`http://localhost/Product/Index/3`

In diesem Fall wird die Index ()-Methode für die ProductController-Klasse aufgerufen. Die Index ()-Methode ist ein Beispiel für eine Controller Aktion.

Eine Controller Aktion muss eine öffentliche Methode einer Controller Klasse sein. C#Standardmäßig sind Methoden private Methoden. Beachten Sie, dass jede öffentliche Methode, die Sie einer Controller Klasse hinzufügen, automatisch als Controller Aktion verfügbar gemacht wird (Sie müssen darauf achten, da eine Controller Aktion von beliebigen Personen im Universum aufgerufen werden kann, indem Sie einfach die richtige URL in eine Adressleiste des Browsers eingeben).

Es gibt einige zusätzliche Anforderungen, die durch eine Controller Aktion erfüllt werden müssen. Eine Methode, die als Controller Aktion verwendet wird, kann nicht überladen werden. Außerdem kann eine Controller Aktion keine statische Methode sein. Abgesehen davon können Sie fast jede Methode als Controller Aktion verwenden.

## <a name="understanding-action-results"></a>Grundlegendes zu Aktions Ergebnissen

Eine Controller Aktion gibt etwas zurück, das als *Aktions Ergebnis*bezeichnet wird. Ein Aktions Ergebnis ist, was eine Controller Aktion als Reaktion auf eine Browser Anforderung zurückgibt.

Das ASP.NET-MVC-Framework unterstützt verschiedene Arten von Aktions Ergebnissen, einschließlich:

1. ViewResult-stellt HTML und Markup dar.
2. EmptyResult-stellt kein Ergebnis dar.
3. Redirectresult: stellt eine Umleitung zu einer neuen URL dar.
4. Jsonresult: stellt ein JavaScript Object Notation Ergebnis dar, das in einer AJAX-Anwendung verwendet werden kann.
5. Javascriptresult: stellt ein JavaScript-Skript dar.
6. ContentResult-stellt ein Text Ergebnis dar.
7. Filecontentresult: stellt eine herunterladbare Datei dar (mit dem binären Inhalt).
8. Fileblothresult: stellt eine herunterladbare Datei (mit einem Pfad) dar.
9. Filestreamresult: stellt eine herunterladbare Datei (mit einem Dateistream) dar.

Alle diese Aktions Ergebnisse erben von der Basisklasse "action result".

In den meisten Fällen gibt eine Controller Aktion ein ViewResult zurück. Die Index Controller Aktion in der Liste 2 gibt beispielsweise ein ViewResult zurück.

**Codebeispiel 2: controllers\bookcontroller.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

Wenn eine Aktion ein ViewResult zurückgibt, wird HTML an den Browser zurückgegeben. Die Index ()-Methode in der Liste 2 gibt eine Sicht mit dem Namen Index für den Browser zurück.

Beachten Sie, dass die Index ()-Aktion in der Liste 2 kein ViewResult () zurückgibt. Stattdessen wird die View ()-Methode der Controller-Basisklasse aufgerufen. Normalerweise wird ein Aktions Ergebnis nicht direkt zurückgegeben. Stattdessen wird eine der folgenden Methoden der Controller-Basisklasse aufgerufen:

1. View: gibt ein Ergebnis der ViewResult-Aktion zurück.
2. Redirect-gibt ein redirectresult-Aktions Ergebnis zurück.
3. Redirectdeaction: gibt ein redirecttorouteresult-Aktions Ergebnis zurück.
4. RedirectToRoute-gibt ein redirecttorouteresult-Aktions Ergebnis zurück.
5. JSON: gibt ein jsonresult-Aktions Ergebnis zurück.
6. Javascriptresult: gibt einen javascriptresult zurück.
7. Content: gibt ein Ergebnis der ContentResult-Aktion zurück.
8. File: gibt ein filecontentresult, filepthresult oder filestreamresult zurück, abhängig von den Parametern, die an die Methode weitergegeben werden.

Wenn Sie also eine Ansicht an den Browser zurückgeben möchten, wird die View ()-Methode aufgerufen. Wenn Sie den Benutzer von einer Controller Aktion zu einer anderen umleiten möchten, können Sie die redirectdeaction ()-Methode aufzurufen. Beispielsweise wird in der Aktion Details () in der Liste 3 entweder eine Ansicht angezeigt, oder der Benutzer wird zur Index ()-Aktion umgeleitet, je nachdem, ob der ID-Parameter einen Wert aufweist.

**Codebeispiel 3-CustomerController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

Das Ergebnis der ContentResult-Aktion ist ein besonderes Ergebnis. Sie können das Ergebnis der ContentResult-Aktion verwenden, um ein Aktions Ergebnis als nur-Text zurückzugeben. Beispielsweise gibt die Index ()-Methode in der Auflistung 4 eine Meldung als nur-Text und nicht als HTML zurück.

**Codebeispiel 4-controllers\status Controller .cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

Wenn die Status Controller. Index ()-Aktion aufgerufen wird, wird eine Sicht nicht zurückgegeben. Stattdessen wird der Rohtext "Hallo Welt!" wird an den Browser zurückgegeben.

Wenn eine Controller Aktion ein Ergebnis zurückgibt, das kein Aktions Ergebnis ist (z. b. ein Datum oder eine ganze Zahl), wird das Ergebnis automatisch in ein ContentResult umschließt. Wenn z. b. die Index ()-Aktion des Arbeits Controllers in der Auflistung 5 aufgerufen wird, wird das Datum automatisch als ContentResult zurückgegeben.

**Auflistung 5-WorkController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

Die Index ()-Aktion in Auflistung 5 gibt ein DateTime-Objekt zurück. Das ASP.NET-MVC-Framework konvertiert das DateTime-Objekt in eine Zeichenfolge und umschließt den DateTime-Wert in einem ContentResult automatisch. Der Browser empfängt das Datum und die Uhrzeit als Klartext.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wurden die Konzepte der ASP.NET MVC-Controller, Controller Aktionen und Controller Aktions Ergebnisse vorgestellt. Im ersten Abschnitt haben Sie erfahren, wie Sie einem ASP.NET MVC-Projekt neue Controller hinzufügen. Als nächstes haben Sie gelernt, wie öffentliche Methoden eines Controllers als Controller Aktionen für das Universum verfügbar gemacht werden. Schließlich haben wir die unterschiedlichen Arten von Aktions Ergebnissen erläutert, die von einer Controller Aktion zurückgegeben werden können. Insbesondere haben wir erläutert, wie ViewResult, redirectdeaktionresult und ContentResult aus einer Controller Aktion zurückgegeben werden.

> [!div class="step-by-step"]
> [Zurück](creating-an-action-vb.md)
> [Weiter](creating-custom-routes-cs.md)
