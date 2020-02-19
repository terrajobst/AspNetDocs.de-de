---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Hinzufügen eines Controllers (VB) | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 2e77f62a9796211b0e59a99c71bc532659b7cb92
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457399"
---
# <a name="adding-a-controller-vb"></a>Hinzufügen eines Controllers (VB)

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mit Microsoft Visual Web Developer 2010 Express Service Pack 1, einer kostenlosen Version von Microsoft Visual Studio. Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen. Sie können alle Komponenten installieren, indem Sie auf den folgenden Link klicken: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten einzeln mithilfe der folgenden Links installieren:
> 
> - [Erforderliche Komponenten für Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3-Tools aktualisieren](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Laufzeit + Tool Unterstützung)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, installieren Sie die erforderlichen Komponenten, indem Sie auf den folgenden Link klicken: [Visual Studio 2010 Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit VB.NET-Quellcode ist für dieses Thema verfügbar. [Laden Sie die VB.NET-Version herunter](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wechseln Sie C#ggf. zur [ C# Version](../cs/adding-a-controller.md) dieses Tutorials.

MVC steht für *Model-View-Controller*. MVC ist ein Muster zum Entwickeln von Anwendungen, in denen jedes Teil über eine separate Verantwortung verfügt:

- Model: die Daten für Ihre Anwendung.
- Sichten: die Vorlagen Dateien, die von Ihrer Anwendung verwendet werden, um HTML-Antworten dynamisch zu generieren.
- Controller: Klassen, die eingehende URL-Anforderungen an die Anwendung verarbeiten, Modelldaten abrufen und dann Ansichts Vorlagen angeben, die eine Antwort an den Client Rendering.

Wir werden alle diese Konzepte in diesem Tutorial abdecken und Ihnen zeigen, wie Sie Sie zum Erstellen einer Anwendung verwenden können.

Erstellen Sie einen neuen Controller, indem Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner *Controller* klicken und dann **Controller hinzufügen**auswählen.

[![Addcontroller](adding-a-controller/_static/image2.png "Addcontroller")](adding-a-controller/_static/image1.png)

Benennen Sie den neuen Controller &quot;helloworldcontroller&quot;, und klicken Sie auf **Hinzufügen**.

[![2addemptycontroller](adding-a-controller/_static/image4.png "2addemptycontroller")](adding-a-controller/_static/image3.png)

Beachten Sie in **Projektmappen-Explorer** auf der rechten Seite, dass eine neue Datei mit dem Namen *HelloWorldController.cs* erstellt wurde und dass die Datei in der IDE geöffnet ist.

Erstellen Sie im neuen `public class HelloWorldController`-Block zwei neue Methoden, die wie der folgende Code aussehen. Als Beispiel wird eine HTML-Zeichenfolge direkt vom Controller zurückgegeben.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Der Controller hat den Namen `HelloWorldController`, und die neue Methode heißt `Index`. Führen Sie die Anwendung aus (drücken Sie F5 oder STRG + F5). Wenn Ihr Browser gestartet wurde, fügen Sie &quot;HelloWorld-&quot; an den Pfad in der Adressleiste an. (Auf meinem Computer ist es `http://localhost:43246/HelloWorld`) Ihr Browser wird wie im folgenden Screenshot aussehen. In der obigen Methode gab der Code direkt eine Zeichenfolge zurück. Wir haben das System angewiesen, nur HTML zurückzugeben.

![](adding-a-controller/_static/image5.png)

ASP.NET MVC ruft in Abhängigkeit von der eingehenden URL verschiedene Controller Klassen (und verschiedene Aktionsmethoden) auf. Die von ASP.NET MVC verwendete standardmäßige Mapping-Logik verwendet ein Format wie dieses, um zu steuern, welcher Code aufgerufen wird:

`/[Controller]/[ActionName]/[Parameters]`

Der erste Teil der URL bestimmt die auszuführende Controller Klasse. Daher wird */HelloWorld* der `HelloWorldController`-Klasse zugeordnet. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse, die ausgeführt werden soll. */HelloWorld/Index* würde also bewirken, dass die `Index`-Methode der `HelloWorldController` Klasse ausgeführt wird. Beachten Sie, dass wir nur */HelloWorld* aufrufen mussten und die `Index`-Methode standardmäßig verwendet wurde. Dies liegt daran, dass eine Methode mit dem Namen `Index` die Standardmethode ist, die auf einem Controller aufgerufen wird, wenn Sie nicht explizit angegeben wird.

Navigieren Sie zu `http://localhost:xxxx/HelloWorld/Welcome`. Die `Welcome`-Methode wird ausgeführt und gibt die Zeichenfolge zurück &quot;dies die Willkommens Aktionsmethode...&quot;. Die MVC-Standard Zuordnung ist `/[Controller]/[ActionName]/[Parameters]`. Für diese URL ist der Controller `HelloWorld`, und `Welcome` ist die-Methode. Wir haben den `[Parameters]` Teil der URL noch nicht verwendet.

![](adding-a-controller/_static/image6.png)

Ändern wir das Beispiel leicht, damit wir einige Parameterinformationen aus der URL an den Controller übergeben können (z. b. */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Ändern Sie die `Welcome`-Methode so, dass Sie zwei Parameter enthält, wie unten gezeigt. Beachten Sie, dass wir die optionale VB-Parameter Funktion verwendet haben, um anzugeben, dass der `numTimes`-Parameter den Standardwert 1 hat, wenn kein Wert für diesen Parameter übergeben wird.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Führen Sie die Anwendung aus, und navigieren Sie zu `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Sie können verschiedene Werte für `name` und `numtimes`ausprobieren. Das System ordnet automatisch die benannten Parameter aus der Abfrage Zeichenfolge in der Adressleiste den Parametern in der-Methode zu.

![](adding-a-controller/_static/image7.png)

In beiden Beispielen hat der Controller den VC-Teil von MVC ausgeführt – das ist die Ansicht und die Controller Arbeit. Der Controller gibt direkt HTML zurück. Normalerweise möchten wir nicht, dass Controller den HTML-Code direkt zurückgeben, da dies für Code sehr umständlich wird. Stattdessen verwenden wir in der Regel eine separate Ansichts Vorlagen Datei, um die HTML-Antwort zu generieren. Sehen wir uns an, wie wir dies tun können.

> [!div class="step-by-step"]
> [Zurück](intro-to-aspnet-mvc-3.md)
> [Weiter](adding-a-view.md)
