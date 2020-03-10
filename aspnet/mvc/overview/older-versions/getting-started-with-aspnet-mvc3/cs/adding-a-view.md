---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Hinzufügen einer AnsichtC#() | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: b4a1316feb8d9b7f3ef5ca4755bf1cc5b23e6ef9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498843"
---
# <a name="adding-a-view-c"></a>Hinzufügen einer Ansicht (C#)

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

In diesem Abschnitt ändern Sie die `HelloWorldController`-Klasse so, dass Sie Vorlagen Dateien anzeigen verwendet, um den Prozess der Erstellung von HTML-Antworten für einen Client zu bereinigen.

Sie erstellen eine Ansichts Vorlagen Datei mithilfe der neuen [Razor-Ansichts-Engine](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) , die mit ASP.NET MVC 3 eingeführt wurde. Razor-basierte Ansichts Vorlagen verfügen über die Dateierweiterung " *. cshtml* " und bieten eine elegante Möglichkeit zum Erstellen C#einer HTML-Ausgabe mithilfe von. Razor minimiert die Anzahl der zum Schreiben einer Ansichts Vorlage erforderlichen Zeichen und Tastatureingaben und ermöglicht einen schnellen, fließenden Codierungs Workflow.

Beginnen Sie mit der Verwendung einer Ansichts Vorlage mit der `Index`-Methode in der `HelloWorldController`-Klasse. Derzeit gibt die `Index`-Methode eine Zeichenfolge mit der Meldung zurück, die in der Controllerklasse hartcodiert ist. Ändern Sie die `Index`-Methode, um ein `View` Objekt zurückzugeben, wie im folgenden dargestellt:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

In diesem Code wird eine Ansichts Vorlage verwendet, um eine HTML-Antwort an den Browser zu generieren. Fügen Sie im Projekt eine Ansichts Vorlage hinzu, die Sie mit der `Index`-Methode verwenden können. Klicken Sie dazu mit der rechten Maustaste in die `Index`-Methode, und klicken Sie auf **Ansicht hinzufügen**.

![](adding-a-view/_static/image1.png)

Das Dialogfeld **Ansicht hinzufügen** wird angezeigt. Überlassen Sie die Standardeinstellungen, und klicken Sie auf die Schaltfläche **Hinzufügen** :

![](adding-a-view/_static/image2.png)

Der Ordner " *mvcmuvie\views\helloworld* " und die Datei " *mvcmuvie\views\helloworld\index.cshtml* " werden erstellt. Sie können Sie in **Projektmappen-Explorer**anzeigen:

![](adding-a-view/_static/image3.png)

Das folgende Beispiel zeigt die Datei " *Index. cshtml* ", die erstellt wurde:

[![helloworldindex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Fügen Sie unter dem `<h2>`-Tag HTML hinzu. Die geänderte Datei *mvcmuvie\views\helloworld\index.cshtml* ist unten dargestellt.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Führen Sie die Anwendung aus, und navigieren Sie zum `HelloWorld` Controller (`http://localhost:xxxx/HelloWorld`). Die `Index`-Methode in Ihrem Controller hat nicht viel funktioniert. Es wurde einfach die-Anweisung `return View()`ausgeführt, mit der angegeben wurde, dass die Methode eine Ansichts Vorlagen Datei verwenden soll, um eine Antwort an den Browser auszugeben. Da Sie den Namen der zu verwendenden Ansichts Vorlagen Datei nicht explizit angegeben haben, verwendet ASP.NET MVC standardmäßig die Ansichts Datei " *Index. cshtml* " im Ordner " *\views\helloworld* ". Die folgende Abbildung zeigt die Zeichenfolge, die in der Ansicht hart codiert ist.

![](adding-a-view/_static/image6.png)

Sieht ziemlich gut aus. Beachten Sie jedoch, dass in der Titelleiste des Browsers "index" und der große Titel auf der Seite "meine MVC-Anwendung" lautet. Ändern wir diese Änderungen.

## <a name="changing-views-and-layout-pages"></a>Ändern von Ansichten und Layoutseiten

Zuerst möchten Sie den Titel "meine MVC-Anwendung" am oberen Rand der Seite ändern. Dieser Text ist für jede Seite üblich. Sie wird tatsächlich nur an einer Stelle im Projekt implementiert, auch wenn Sie auf jeder Seite in der Anwendung angezeigt wird. Wechseln Sie in **Projektmappen-Explorer** zum Ordner */views/Shared* , und öffnen Sie die Datei *\_Layout. cshtml* . Diese Datei wird als *Layoutseite* bezeichnet und ist die freigegebene "Shell", die von allen anderen Seiten verwendet wird.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Mit Layoutvorlagen können Sie das HTML-Container Layout Ihrer Website an einem Ort angeben und dann auf mehrere Seiten auf Ihrer Website anwenden. Beachten Sie die `@RenderBody()` Zeile am Ende der Datei. `RenderBody` ist ein Platzhalter, bei dem alle von Ihnen erstellten Ansichts spezifischen Seiten auf der Layoutseite "umschließen" angezeigt werden. Ändern Sie die Titel Überschrift in der Layoutvorlage von "meine MVC-Anwendung" in "MVC Movie App".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Führen Sie die Anwendung aus, und beachten Sie, dass Sie jetzt "MVC Movie App" heißt. Klicken Sie **auf den Link** Info, und Sie sehen, dass auf dieser Seite auch "MVC Movie App" angezeigt wird. Wir konnten die Änderung einmal in der Layoutvorlage vornehmen, sodass alle Seiten auf der Website den neuen Titel widerspiegeln.

![](adding-a-view/_static/image9.png)

Die vollständige *\_Layout. cshtml* -Datei ist unten dargestellt:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Nun ändern wir den Titel der Index Seite (Sicht).

Öffnen Sie *mvcmuvie\views\helloworld\index.cshtml*. Es gibt zwei Möglichkeiten, eine Änderung vorzunehmen: zuerst den Text, der im Titel des Browsers angezeigt wird, und dann im sekundären Header (das `<h2>`-Element). Sie nehmen diese geringfügig anders vor, damit Sie erkennen können, welcher Teil der App mithilfe einer Codebearbeitung geändert wird.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Um den HTML-Titel anzugeben, der angezeigt werden soll, legt der obige Code eine `Title` Eigenschaft des `ViewBag` Objekts fest (in der Ansichts Vorlage *Index. cshtml* ). Wenn Sie sich den Quellcode der Layoutvorlage ansehen, werden Sie bemerken, dass die Vorlage diesen Wert im `<title>`-Element als Teil des `<head>` Abschnitts des HTML-Codes verwendet. Mit diesem Ansatz können Sie problemlos andere Parameter zwischen der Ansichts Vorlage und der Layoutdatei übergeben.

Führen Sie die Anwendung aus, und navigieren Sie zu `http://localhost:xx/HelloWorld`. Sie sehen, dass sich der Browsertitel, die primäre Überschrift und die sekundären Überschriften geändert haben. (Wenn die Änderungen nicht im Browser angezeigt werden, zeigen Sie ggf. zwischengespeicherten Inhalt an. Drücken Sie in Ihrem Browser STRG+F5, um das Laden der Antwort vom Server zu erzwingen.)

Beachten Sie auch, wie der Inhalt in der Ansichts Vorlage *Index. cshtml* mit der Ansichts Vorlage *\_Layout. cshtml* zusammengeführt und eine einzelne HTML-Antwort an den Browser gesendet wurde. Layoutvorlagen erleichtern ungemein das Vornehmen von Änderungen an allen Seiten in Ihrer Anwendung.

![](adding-a-view/_static/image10.png)

Unser kleiner Beitrag zu den „Daten“ (in diesem Fall die Meldung "Hello from our View Template!") ist jedoch hartcodiert. Die MVC-Anwendung weist ein „V“ (View [Ansicht]) auf, und Sie verfügen über ein „C“ (Controller), aber noch nicht über ein „M“ (Modell). In Kürze werden die Schritte zum Erstellen einer Datenbank und zum Abrufen von Modelldaten erläutert.

## <a name="passing-data-from-the-controller-to-the-view"></a>Übergeben von Daten vom Controller an die Ansicht

Bevor wir zu einer Datenbank gehen und über Modelle sprechen, sollten wir uns jedoch zunächst mit der Übergabe von Informationen vom Controller an eine Ansicht beschäftigen. Controller Klassen werden als Antwort auf eine eingehende URL-Anforderung aufgerufen. Eine Controller Klasse ist der Ort, an dem Sie den Code schreiben, der die eingehenden Parameter verarbeitet, Daten aus einer Datenbank abruft und letztendlich entscheidet, welche Art von Antwort an den Browser zurückgesendet werden soll. Ansichts Vorlagen können dann von einem Controller verwendet werden, um eine HTML-Antwort an den Browser zu generieren und zu formatieren.

Controller sind dafür verantwortlich, welche Daten oder Objekte erforderlich sind, damit eine Ansichts Vorlage eine Antwort an den Browser Rendering. Eine Ansichts Vorlage sollte nie eine Geschäftslogik durchführen oder direkt mit einer Datenbank interagieren. Stattdessen sollte Sie nur mit den Daten arbeiten, die für den Controller bereitgestellt werden. Wenn Sie diese "Trennung von Anliegen" beibehalten, können Sie Ihren Code bereinigen und besser verwalten.

Derzeit nimmt die `Welcome` Aktionsmethode in der `HelloWorldController`-Klasse eine `name` und einen `numTimes` Parameter an und gibt die Werte dann direkt an den Browser aus. Anstatt den Controller diese Antwort als Zeichenfolge zu erzeugen, ändern wir den Controller so, dass stattdessen eine Ansichts Vorlage verwendet wird. Die Ansichtsvorlage generiert eine dynamische Antwort. Das bedeutet, dass Sie die entsprechenden Datenelemente vom Controller an die Ansicht übergeben müssen, um die Antwort zu generieren. Dazu muss der Controller die dynamischen Daten, die die Ansichts Vorlage benötigt, in einem `ViewBag` Objekt ablegen, auf das die Ansichts Vorlage zugreifen kann.

Kehren Sie zur Datei *HelloWorldController.cs* zurück, und ändern Sie die `Welcome`-Methode, um dem `ViewBag` Objekt eine `Message` und `NumTimes` Wert hinzuzufügen. `ViewBag` ist ein dynamisches Objekt, d. h., Sie können den gewünschten Wert für die Anwendung einfügen. Das `ViewBag`-Objekt hat keine definierten Eigenschaften, bis Sie etwas darin ablegen. Die vollständige Datei *HelloWorldController.cs* sieht wie folgt aus:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Nun enthält das `ViewBag` Objektdaten, die automatisch an die Ansicht übermittelt werden.

Als nächstes benötigen Sie eine Willkommens Ansichts Vorlage. Wählen Sie im Menü **Debuggen** die Option **mvcmovie erstellen** aus, um sicherzustellen, dass das Projekt kompiliert wird.

[![buildhelloworld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Klicken Sie dann mit der rechten Maustaste in die `Welcome` Methode, und klicken Sie auf **Ansicht hinzufügen** Das Dialogfeld **Ansicht hinzufügen** sieht wie folgt aus:

![](adding-a-view/_static/image13.png)

Klicken Sie auf **Hinzufügen**, und fügen Sie dann den folgenden Code unter dem `<h2>`-Element in der neuen Datei " *Welcome. cshtml* " hinzu. Sie erstellen eine Schleife, die "Hello" so oft wie der Benutzer besagt, dass dies der Fall sein sollte. Die vollständige Datei " *Welcome. cshtml* " ist unten dargestellt.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Führen Sie die Anwendung aus, und navigieren Sie zur folgenden URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Nun werden die Daten aus der URL entnommen und automatisch an den Controller übermittelt. Der Controller packt die Daten in ein `ViewBag` Objekt und übergibt dieses Objekt an die Ansicht. Die Ansicht zeigt dann die Daten für den Benutzer als HTML an.

![](adding-a-view/_static/image14.png)

Das war also eine Art eines „M“ für Modell, jedoch nicht der Art Datenbank. Lassen Sie uns das Gelernte umsetzen und eine Filmdatenbank erstellen.

> [!div class="step-by-step"]
> [Zurück](adding-a-controller.md)
> [Weiter](adding-a-model.md)
