---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Hinzufügen eines Controllers | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: eine aktualisierte Version dieses Tutorials ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, aber viel einfacher zu befolgen und zu demonstrieren...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f528c56435976c7f31fce453c834ef9eaebe6244
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456100"
---
# <a name="adding-a-controller"></a>Hinzufügen eines Controllers

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials ist [hier](../../getting-started/introduction/getting-started.md) verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, ist viel einfacher zu befolgen und zeigt mehr Features.

MVC steht für *Model-View-Controller*. MVC ist ein Muster zum Entwickeln von Anwendungen, die gut entworfen, testfähig und einfach zu verwalten sind. MVC-basierte Anwendungen enthalten Folgendes:

- **M** odels: Klassen, die die Daten der Anwendung darstellen und Validierungs Logik verwenden, um Geschäftsregeln für diese Daten zu erzwingen.
- **V** iews: Vorlagen Dateien, die Ihre Anwendung verwendet, um HTML-Antworten dynamisch zu generieren.
- **C** ontroller: Klassen, die eingehende Browser Anforderungen verarbeiten, Modelldaten abrufen und dann Ansichts Vorlagen angeben, die eine Antwort an den Browser zurückgeben.

Wir behandeln all diese Konzepte in dieser tutorialreihe und zeigen Ihnen, wie Sie Sie zum Erstellen einer Anwendung verwenden können.

Zunächst erstellen wir eine Controller Klasse. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Controllers* , und wählen Sie dann **Controller hinzufügen**aus.

![](adding-a-controller/_static/image1.png)

Benennen Sie den neuen Controller &quot;helloworldcontroller&quot;. Belassen Sie die Standardvorlage als **leeren MVC-Controller** , und klicken Sie auf **Hinzufügen**.

![Controller hinzufügen](adding-a-controller/_static/image2.png)

Beachten Sie in **Projektmappen-Explorer** , dass eine neue Datei mit dem Namen *HelloWorldController.cs*erstellt wurde. Die Datei ist in der IDE geöffnet.

![](adding-a-controller/_static/image3.png)

Ersetzen Sie den Inhalt der Datei durch den folgenden Code.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Die Controller Methoden geben als Beispiel eine HTML-Zeichenfolge zurück. Der Controller hat den Namen `HelloWorldController`, und die erste oben genannte Methode heißt `Index`. Wir rufen es in einem Browser auf. Führen Sie die Anwendung aus (drücken Sie F5 oder STRG + F5). Fügen Sie im Browser &quot;HelloWorld-&quot; an den Pfad in der Adressleiste an. (In der Abbildung unten ist es z. b. `http://localhost:1234/HelloWorld.`) Die Seite im Browser sieht wie der folgende Screenshot aus. In der obigen Methode gab der Code direkt eine Zeichenfolge zurück. Sie haben das System angewiesen, nur einige HTML-Code zurückzugeben.

![](adding-a-controller/_static/image4.png)

ASP.NET MVC ruft in Abhängigkeit von der eingehenden URL verschiedene Controller Klassen (und verschiedene Aktionsmethoden) auf. Die standardmäßige URL-Routing Logik, die von ASP.NET MVC verwendet wird, verwendet das folgende Format, um zu bestimmen, welcher Code aufgerufen werden soll

`/[Controller]/[ActionName]/[Parameters]`

Der erste Teil der URL bestimmt die auszuführende Controller Klasse. Daher wird */HelloWorld* der `HelloWorldController`-Klasse zugeordnet. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse, die ausgeführt werden soll. */HelloWorld/Index* würde also bewirken, dass die `Index`-Methode der `HelloWorldController` Klasse ausgeführt wird. Beachten Sie, dass wir nur zu */HelloWorld* navigieren mussten und die `Index`-Methode standardmäßig verwendet wurde. Dies liegt daran, dass eine Methode mit dem Namen `Index` die Standardmethode ist, die auf einem Controller aufgerufen wird, wenn Sie nicht explizit angegeben wird.

Navigieren Sie zu `http://localhost:xxxx/HelloWorld/Welcome`. Die `Welcome`-Methode wird ausgeführt und gibt die Zeichenfolge zurück &quot;dies die Willkommens Aktionsmethode...&quot;. Die MVC-Standard Zuordnung ist `/[Controller]/[ActionName]/[Parameters]`. Bei dieser URL ist `HelloWorld` der Controller und `Welcome` die Aktionsmethode. Sie haben den Teil `[Parameters]` der URL noch nicht verwendet.

![](adding-a-controller/_static/image5.png)

Ändern Sie das Beispiel so geringfügig, dass Sie einige Parameterinformationen von der URL an den Controller übergeben können (z. b. */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Ändern Sie die `Welcome`-Methode so, dass Sie zwei Parameter enthält, wie unten gezeigt. Beachten Sie, dass der Code C# die optionale-Parameter-Funktion verwendet, um anzugeben, dass der `numTimes`-Parameter den Standardwert 1 hat, wenn kein Wert für diesen Parameter übergeben wird.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Führen Sie die Anwendung aus, und navigieren Sie zur Beispiel-URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Sie können für `name` und `numtimes` in der URL verschiedene Werte ausprobieren. Das [ASP.NET MVC-Modell Bindungssystem](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) ordnet automatisch die benannten Parameter aus der Abfrage Zeichenfolge in der Adressleiste den Parametern in der-Methode zu.

![](adding-a-controller/_static/image6.png)

In beiden Beispielen hat der Controller den &quot;VC-&quot; Teil von MVC ausgeführt – das heißt, die Ansicht und der Controller funktionieren. Der Controller gibt direkt HTML zurück. Normalerweise möchten Sie nicht, dass Controller den HTML-Code direkt zurückgeben, da dieser Code sehr umständlich ist. Stattdessen verwenden wir in der Regel eine separate Ansichts Vorlagen Datei, um die HTML-Antwort zu generieren. Sehen wir uns nun an, wie wir dies tun können.

> [!div class="step-by-step"]
> [Zurück](intro-to-aspnet-mvc-4.md)
> [Weiter](adding-a-view.md)
