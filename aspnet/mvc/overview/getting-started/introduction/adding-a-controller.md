---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Hinzufügen eines Controllers | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 6b38d757d37374b14979f8a079a46158ff64f9c3
ms.sourcegitcommit: f774732a3960fca079438a88a5472c37cf7be08a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68810766"
---
# <a name="adding-a-controller"></a>Hinzufügen eines Controllers

von [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC steht für *Model-View-Controller*. MVC ist ein Muster zum Entwickeln von Anwendungen, die gut entworfen, testfähig und einfach zu verwalten sind. MVC-basierte Anwendungen enthalten Folgendes:

- **M** odels: Klassen, die die Daten der Anwendung darstellen und Validierungs Logik verwenden, um Geschäftsregeln für diese Daten zu erzwingen.
- **V** iews: Vorlagen Dateien, die Ihre Anwendung verwendet, um HTML-Antworten dynamisch zu generieren.
- **C** -ontroller: Klassen, die eingehende Browser Anforderungen verarbeiten, Modelldaten abrufen und dann Ansichts Vorlagen angeben, die eine Antwort an den Browser zurückgeben.

Wir behandeln all diese Konzepte in dieser tutorialreihe und zeigen Ihnen, wie Sie Sie zum Erstellen einer Anwendung verwenden können.

Zunächst erstellen wir eine Controller Klasse. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Controller* , und klicken Sie dann auf **Hinzufügen**und dann auf **Controller**

![](adding-a-controller/_static/image1.png)

Klicken Sie im Dialogfeld **Gerüst hinzufügen** auf **MVC 5 Controller-leer**, und klicken Sie dann auf **Hinzufügen**.

![](adding-a-controller/_static/image2.png)  

Benennen Sie den neuen Controller "helloworldcontroller", und klicken Sie auf **Hinzufügen**.

![Controller hinzufügen](adding-a-controller/_static/image3.png)

Beachten Sie in **Projektmappen-Explorer** , dass eine neue Datei mit dem Namen *HelloWorldController.cs* und einem neuen Ordner " *views\helloworld*" erstellt wurde. Der Controller ist in der IDE geöffnet.

![](adding-a-controller/_static/image4.png)

Ersetzen Sie den Inhalt der Datei durch den folgenden Code.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Die Controller Methoden geben als Beispiel eine HTML-Zeichenfolge zurück. Der Controller hat den `HelloWorldController` Namen, und die erste Methode `Index`heißt. Wir rufen es in einem Browser auf. Führen Sie die Anwendung aus (drücken Sie F5 oder STRG + F5). Fügen &quot;Sie im Browser HelloWorld&quot; an den Pfad in der Adressleiste an. (Z. b. in der Abbildung unten `http://localhost:1234/HelloWorld.`), sieht die Seite im Browser wie der folgende Screenshot aus. In der obigen Methode gab der Code direkt eine Zeichenfolge zurück. Sie haben das System angewiesen, nur einige HTML-Code zurückzugeben.

![](adding-a-controller/_static/image5.png)

ASP.NET MVC ruft in Abhängigkeit von der eingehenden URL verschiedene Controller Klassen (und verschiedene Aktionsmethoden) auf. Die standardmäßige URL-Routing Logik, die von ASP.NET MVC verwendet wird, verwendet das folgende Format, um zu bestimmen, welcher Code aufgerufen werden soll

`/[Controller]/[ActionName]/[Parameters]`

Sie legen das Format für das Routing in der Datei " *\_App Start/routeconfig. cs* " fest.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Wenn Sie die Anwendung ausführen und keine URL-Segmente angeben, wird standardmäßig der "Home"-Controller und die Aktionsmethode "index" verwendet, die im Abschnitt "Standard" des obigen Codes angegeben ist.

Der erste Teil der URL bestimmt die auszuführende Controller Klasse. Daher wird */HelloWorld* der `HelloWorldController` -Klasse zugeordnet. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse, die ausgeführt werden soll. */HelloWorld/Index* würde also bewirken, `Index` dass die- `HelloWorldController` Methode der-Klasse ausgeführt wird. Beachten Sie, dass wir nur zu */HelloWorld* navigieren mussten und `Index` die-Methode standardmäßig verwendet wurde. Dies liegt daran, dass es `Index` sich bei einer Methode mit dem Namen um die Standardmethode handelt, die auf einem Controller aufgerufen wird, wenn eine nicht explizit angegeben ist. Der dritte Teil des URL-Segments (`Parameters`) ist für Routendaten. Weiter unten in diesem Tutorial werden die Weiterleitungs Daten angezeigt.

Wechseln Sie zu `http://localhost:xxxx/HelloWorld/Welcome`. Die `Welcome` -Methode wird ausgeführt und gibt &quot;die Zeichenfolge zurück, die die Willkommens Aktionsmethode ist... &quot;. Die MVC-Standard Zuordnung `/[Controller]/[ActionName]/[Parameters]`ist. Bei dieser URL ist `HelloWorld` der Controller und `Welcome` die Aktionsmethode. Sie haben den Teil `[Parameters]` der URL noch nicht verwendet.

![](adding-a-controller/_static/image6.png)

Ändern Sie das Beispiel so geringfügig, dass Sie einige Parameterinformationen von der URL an den Controller übergeben können (z. b. */HelloWorld/Welcome? Name&amp;= Scott numtimes = 4*). Ändern Sie `Welcome` die-Methode so, dass Sie zwei Parameter wie unten dargestellt enthält. Beachten Sie, dass der Code C# die optionale-Parameter-Funktion verwendet, `numTimes` um anzugeben, dass der-Parameter den Standardwert 1 hat, wenn kein Wert für diesen Parameter übergeben wird.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Sicherheitshinweis: Der obige Code verwendet [HttpUtility. HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) , um die Anwendung vor böswilliger Eingabe (JavaScript) zu schützen. Weitere Informationen finden Sie unter [Vorgehensweise: Schutz vor Skript Exploits in einer Webanwendung durch Anwenden der HTML-](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)Codierung auf Zeichen folgen.

 Führen Sie die Anwendung aus, und navigieren Sie zur`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`Beispiel-URL (). Sie können für `name` und `numtimes` in der URL verschiedene Werte ausprobieren. Das [ASP.NET MVC-Modell Bindungssystem](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) ordnet automatisch die benannten Parameter aus der Abfrage Zeichenfolge in der Adressleiste den Parametern in der-Methode zu.

![](adding-a-controller/_static/image7.png)

Im obigen Beispiel wird das URL-Segment ( `Parameters`) nicht verwendet, und die `name` Parameter `numTimes` und werden als [Abfrage](http://en.wikipedia.org/wiki/Query_string)Zeichenfolgen weitergegeben. Das Platzhalterzeichen ? (Fragezeichen) in der obigen URL ist ein Trennzeichen, und die Abfrage Zeichenfolgen folgen. Das Zeichen &amp; trennt Abfragezeichenfolgen.

Ersetzen Sie die Willkommens Methode durch den folgenden Code:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Führen Sie die Anwendung aus, und geben Sie die folgende URL ein:`http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Diesmal stimmte das dritte URL-Segment mit dem Routen `ID.` Parameter `Welcome` überein. die Aktionsmethode enthält`ID`einen Parameter (), der mit der `RegisterRoutes` URL-Spezifikation in der Methode übereinstimmt.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

In ASP.NET MVC-Anwendungen ist es typischer, Parameter als Routendaten zu übergeben (wie wir mit der ID oben), als Abfrage Zeichenfolgen zu übergeben. Sie können auch eine Route hinzufügen, um die `name` Parameter `numtimes` und in den Parametern als Routendaten in der URL zu übergeben. Fügen Sie in der Datei *\_App start\routeconfig.cs* die "Hello"-Route hinzu:

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Führen Sie die Anwendung aus, `/localhost:XXX/HelloWorld/Welcome/Scott/3`und navigieren Sie zu.

![](adding-a-controller/_static/image9.png)

Bei vielen MVC-Anwendungen funktioniert die Standardroute problemlos. Sie werden später in diesem Tutorial erfahren, wie Sie Daten mithilfe der Modell Bindung übergeben, und Sie müssen die Standardroute für diese nicht ändern.

In diesen Beispielen hat der Controller den &quot;VC&quot; -Teil von MVC ausgeführt – das heißt, die Ansicht und der Controller funktionieren. Der Controller gibt direkt HTML zurück. Normalerweise möchten Sie nicht, dass Controller den HTML-Code direkt zurückgeben, da dieser Code sehr umständlich ist. Stattdessen verwenden wir in der Regel eine separate Ansichts Vorlagen Datei, um die HTML-Antwort zu generieren. Sehen wir uns nun an, wie wir dies tun können.

> [!div class="step-by-step"]
> [Zurück](getting-started.md)
> [Weiter](adding-a-view.md)
