---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Hinzufügen eines ControllersC#() | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 959116ff773f4ef466cda6b172e8321590b50e5b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457828"
---
# <a name="adding-a-controller-c"></a>Hinzufügen eines Controllers (C#)

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials ist [hier](../../../getting-started/introduction/getting-started.md) verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, ist viel einfacher zu befolgen und zeigt mehr Features.
> 
> 
> Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mit Microsoft Visual Web Developer 2010 Express Service Pack 1, einer kostenlosen Version von Microsoft Visual Studio. Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen. Sie können alle Komponenten installieren, indem Sie auf den folgenden Link klicken: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten einzeln mithilfe der folgenden Links installieren:
> 
> - [Erforderliche Komponenten für Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3-Tools aktualisieren](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Laufzeit + Tool Unterstützung)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, installieren Sie die erforderlichen Komponenten, indem Sie auf den folgenden Link klicken: [Visual Studio 2010 Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Für dieses Thema steht ein Visual C# Web Developer-Projekt mit Quellcode zur Verfügung. [Laden Sie C# die Version herunter](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie Visual Basic bevorzugen, wechseln Sie zur [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) dieses Tutorials.

MVC steht für *Model-View-Controller*. MVC ist ein Muster zum Entwickeln von Anwendungen, die gut strukturiert und leicht zu verwalten sind. MVC-basierte Anwendungen enthalten Folgendes:

- Controller: Klassen, die eingehende Anforderungen an die Anwendung verarbeiten, Modelldaten abrufen und dann Ansichts Vorlagen angeben, die eine Antwort an den Client zurückgeben.
- Models: Klassen, die die Daten der Anwendung darstellen und Validierungs Logik verwenden, um Geschäftsregeln für diese Daten zu erzwingen.
- Sichten: Vorlagen Dateien, die Ihre Anwendung verwendet, um HTML-Antworten dynamisch zu generieren.

Wir behandeln all diese Konzepte in dieser tutorialreihe und zeigen Ihnen, wie Sie Sie zum Erstellen einer Anwendung verwenden können.

Zunächst erstellen wir eine Controller Klasse. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Controllers* , und wählen Sie dann **Controller hinzufügen**aus.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Nennen Sie den neuen Controller "helloworldcontroller". Belassen Sie die Standardvorlage als **leerer Controller** , und klicken Sie auf **Hinzufügen**.

[![addhelloworldcontroller](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Beachten Sie in **Projektmappen-Explorer** , dass eine neue Datei mit dem Namen *HelloWorldController.cs*erstellt wurde. Die Datei ist in der IDE geöffnet.

![](adding-a-controller/_static/image5.png)

Erstellen Sie im `public class HelloWorldController`-Block zwei Methoden, die wie der folgende Code aussehen. Der Controller gibt als Beispiel eine HTML-Zeichenfolge zurück.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Der Controller hat den Namen `HelloWorldController`, und die erste oben genannte Methode heißt `Index`. Wir rufen es in einem Browser auf. Führen Sie die Anwendung aus (drücken Sie F5 oder STRG + F5). Fügen Sie im Browser "HelloWorld" an den Pfad in der Adressleiste an. (In der Abbildung unten ist es z. b. `http://localhost:43246/HelloWorld.`) Die Seite im Browser sieht wie der folgende Screenshot aus. In der obigen Methode gab der Code direkt eine Zeichenfolge zurück. Sie haben das System angewiesen, nur einige HTML-Code zurückzugeben.

![](adding-a-controller/_static/image6.png)

ASP.NET MVC ruft in Abhängigkeit von der eingehenden URL verschiedene Controller Klassen (und verschiedene Aktionsmethoden) auf. Die von ASP.NET MVC verwendete standardmäßige Mapping-Logik verwendet ein Format wie dieses, um zu bestimmen, welcher Code aufgerufen werden soll:

`/[Controller]/[ActionName]/[Parameters]`

Der erste Teil der URL bestimmt die auszuführende Controller Klasse. Daher wird */HelloWorld* der `HelloWorldController`-Klasse zugeordnet. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse, die ausgeführt werden soll. */HelloWorld/Index* würde also bewirken, dass die `Index`-Methode der `HelloWorldController` Klasse ausgeführt wird. Beachten Sie, dass wir nur zu */HelloWorld* navigieren mussten und die `Index`-Methode standardmäßig verwendet wurde. Dies liegt daran, dass eine Methode mit dem Namen `Index` die Standardmethode ist, die auf einem Controller aufgerufen wird, wenn Sie nicht explizit angegeben wird.

Navigieren Sie zu `http://localhost:xxxx/HelloWorld/Welcome`. Die `Welcome`-Methode wird ausgeführt und gibt die Zeichenfolge „This is the Welcome action method...“ zurück. Die MVC-Standard Zuordnung ist `/[Controller]/[ActionName]/[Parameters]`. Bei dieser URL ist `HelloWorld` der Controller und `Welcome` die Aktionsmethode. Sie haben den Teil `[Parameters]` der URL noch nicht verwendet.

![](adding-a-controller/_static/image7.png)

Ändern Sie das Beispiel so geringfügig, dass Sie einige Parameterinformationen von der URL an den Controller übergeben können (z. b. */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Ändern Sie die `Welcome`-Methode so, dass Sie zwei Parameter enthält, wie unten gezeigt. Beachten Sie, dass der Code C# die optionale-Parameter-Funktion verwendet, um anzugeben, dass der `numTimes`-Parameter den Standardwert 1 hat, wenn kein Wert für diesen Parameter übergeben wird.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Führen Sie die Anwendung aus, und navigieren Sie zur Beispiel-URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Sie können für `name` und `numtimes` in der URL verschiedene Werte ausprobieren. Das System ordnet automatisch die benannten Parameter aus der Abfrage Zeichenfolge in der Adressleiste den Parametern in der-Methode zu.

![](adding-a-controller/_static/image8.png)

In beiden Beispielen hat der Controller den "VC"-Teil von MVC ausgeführt – das heißt, die Ansicht und der Controller funktionieren. Der Controller gibt direkt HTML zurück. Normalerweise möchten Sie nicht, dass Controller den HTML-Code direkt zurückgeben, da dieser Code sehr umständlich ist. Stattdessen verwenden wir in der Regel eine separate Ansichts Vorlagen Datei, um die HTML-Antwort zu generieren. Sehen wir uns nun an, wie wir dies tun können.

> [!div class="step-by-step"]
> [Zurück](intro-to-aspnet-mvc-3.md)
> [Weiter](adding-a-view.md)
