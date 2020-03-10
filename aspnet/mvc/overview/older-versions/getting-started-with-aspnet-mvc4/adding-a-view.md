---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Hinzufügen einer Ansicht | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: eine aktualisierte Version dieses Tutorials ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, aber viel einfacher zu befolgen und zu demonstrieren...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 81c2e1f46b08cbc9b5aa5d6c1b36d9d8dc2ba581
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78485367"
---
# <a name="adding-a-view"></a>Hinzufügen einer Ansicht

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials ist [hier](../../getting-started/introduction/getting-started.md) verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, ist viel einfacher zu befolgen und zeigt mehr Features.

In diesem Abschnitt ändern Sie die `HelloWorldController`-Klasse so, dass Sie Vorlagen Dateien anzeigen verwendet, um den Prozess der Erstellung von HTML-Antworten für einen Client zu bereinigen.

Sie erstellen eine Ansichts Vorlagen Datei mithilfe der [Razor-Ansichts-Engine](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) , die mit ASP.NET MVC 3 eingeführt wurde. Razor-basierte Ansichts Vorlagen verfügen über die Dateierweiterung " *. cshtml* " und bieten eine elegante Möglichkeit zum Erstellen C#einer HTML-Ausgabe mithilfe von. Razor minimiert die Anzahl der zum Schreiben einer Ansichts Vorlage erforderlichen Zeichen und Tastatureingaben und ermöglicht einen schnellen, fließenden Codierungs Workflow.

Derzeit gibt die `Index`-Methode eine Zeichenfolge mit der Meldung zurück, die in der Controllerklasse hartcodiert ist. Ändern Sie die `Index`-Methode, um ein `View` Objekt zurückzugeben, wie im folgenden Code gezeigt:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Die oben beschriebene `Index` Methode verwendet eine Ansichts Vorlage, um eine HTML-Antwort an den Browser zu generieren. Controller Methoden (auch [Aktionsmethoden](http://rachelappel.com/asp.net-mvc-actionresults-explained)genannt), wie z. b. die oben genannte `Index` Methode, geben im Allgemeinen ein [Action result](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (oder eine von " [Action result](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)" abgeleitete Klasse) und keine primitiven Typen wie String zurück.

Fügen Sie im Projekt eine Ansichts Vorlage hinzu, die Sie mit der `Index`-Methode verwenden können. Klicken Sie dazu mit der rechten Maustaste in die `Index`-Methode, und klicken Sie auf **Ansicht hinzufügen**.

![](adding-a-view/_static/image1.png)

Das Dialogfeld **Ansicht hinzufügen** wird angezeigt. Überlassen Sie die Standardeinstellungen, und klicken Sie auf die Schaltfläche **Hinzufügen** :

![](adding-a-view/_static/image2.png)

Der Ordner " *mvcmuvie\views\helloworld* " und die Datei " *mvcmuvie\views\helloworld\index.cshtml* " werden erstellt. Sie können Sie in **Projektmappen-Explorer**anzeigen:

![](adding-a-view/_static/image3.png)

Das folgende Beispiel zeigt die Datei " *Index. cshtml* ", die erstellt wurde:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Fügen Sie den folgenden HTML-Code unter dem `<h2>`-Tag hinzu.

[!code-html[Main](adding-a-view/samples/sample2.html)]

Die vollständige Datei " *mvcmuvie\views\helloworld\index.cshtml* " ist unten dargestellt.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Wenn Sie Visual Studio 2012 verwenden, klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Datei *Index. cshtml* , und wählen Sie **in Seitenprüfung anzeigen aus**.

![PI](adding-a-view/_static/image5.png)

Das [Seitenprüfung-Tutorial](../../views/using-page-inspector-in-aspnet-mvc.md) enthält weitere Informationen zu diesem neuen Tool.

Alternativ können Sie die Anwendung ausführen und zum `HelloWorld` Controller (`http://localhost:xxxx/HelloWorld`) navigieren. Die `Index`-Methode in Ihrem Controller hat nicht viel funktioniert. Es wurde einfach die-Anweisung `return View()`ausgeführt, mit der angegeben wurde, dass die Methode eine Ansichts Vorlagen Datei verwenden soll, um eine Antwort an den Browser auszugeben. Da Sie den Namen der zu verwendenden Ansichts Vorlagen Datei nicht explizit angegeben haben, verwendet ASP.NET MVC standardmäßig die Ansichts Datei " *Index. cshtml* " im Ordner " *\views\helloworld* ". Die folgende Abbildung zeigt die Zeichenfolge &quot;Hello aus unserer Ansichts Vorlage.&quot; in der-Sicht hart codiert.

![](adding-a-view/_static/image6.png)

Sieht ziemlich gut aus. Beachten Sie jedoch, dass in der Titelleiste des Browsers &quot;Index My ASP.net A&quot; und der große Link oben auf der Seite &quot;Ihr Logo hier angezeigt wird.&quot; unter dem &quot;Ihr Logo.&quot; Link sind die Links "Registrierung" und "Anmelden", und darunter finden Sie Links zu Homepage-, Info-und Kontaktseiten. Ändern wir einige dieser Änderungen.

## <a name="changing-views-and-layout-pages"></a>Ändern von Ansichten und Layoutseiten

Zuerst möchten Sie den &quot;Ihr Logo hier ändern.&quot; Titel am oberen Rand der Seite. Dieser Text ist für jede Seite üblich. Es wird tatsächlich nur an einer Stelle im Projekt implementiert, auch wenn es auf jeder Seite in der Anwendung angezeigt wird. Wechseln Sie in **Projektmappen-Explorer** zum Ordner */views/Shared* , und öffnen Sie die Datei *\_Layout. cshtml* . Diese Datei wird als *Layoutseite* bezeichnet und ist die freigegebene &quot;Shell&quot;, die von allen anderen Seiten verwendet wird.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Mit Layoutvorlagen können Sie das HTML-Container Layout Ihrer Website an einem Ort angeben und dann auf mehrere Seiten auf Ihrer Website anwenden. Suchen Sie die Zeile `@RenderBody()`. `RenderBody` ist ein Platzhalter, bei dem alle ansichtsspezifischen Seiten, die Sie erstellen, von der Layoutseite &quot;umschlossen&quot; angezeigt werden. Wenn Sie z. b. den Link Info auswählen, wird die Ansicht *views\home\about.cshtml* innerhalb der `RenderBody`-Methode gerendert.

Ändern Sie die Überschrift Standort Titel in der Layoutvorlage von &quot;Ihr Logo hier&quot; in &quot;MVC Movie&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Ersetzen Sie den Inhalt des title-Elements durch das folgende Markup:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Führen Sie die Anwendung aus, und beachten Sie, dass Sie jetzt &quot;MVC Movie &quot;heißt. Klicken Sie auf **den Link info** , und Sie sehen, dass auf dieser Seite auch &quot;MVC Movie&quot;angezeigt wird. Wir konnten die Änderung einmal in der Layoutvorlage vornehmen, sodass alle Seiten auf der Website den neuen Titel widerspiegeln.

![](adding-a-view/_static/image8.png)

Nun ändern wir den Titel der Index Sicht.

Öffnen Sie *mvcmuvie\views\helloworld\index.cshtml*. Es gibt zwei Möglichkeiten, eine Änderung vorzunehmen: zuerst den Text, der im Titel des Browsers angezeigt wird, und dann im sekundären Header (das `<h2>`-Element). Sie nehmen diese geringfügig anders vor, damit Sie erkennen können, welcher Teil der App mithilfe einer Codebearbeitung geändert wird.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

Um den HTML-Titel anzugeben, der angezeigt werden soll, legt der obige Code eine `Title` Eigenschaft des `ViewBag` Objekts fest (in der Ansichts Vorlage *Index. cshtml* ). Wenn Sie sich den Quellcode der Layoutvorlage ansehen, werden Sie bemerken, dass die Vorlage diesen Wert im `<title>`-Element als Teil des `<head>` Abschnitts des HTML-Codes verwendet, den wir zuvor geändert haben. Mithilfe dieses `ViewBag` Ansatzes können Sie problemlos andere Parameter zwischen der Ansichts Vorlage und der Layoutdatei übergeben.

Führen Sie die Anwendung aus, und navigieren Sie zu `http://localhost:xx/HelloWorld`. Sie sehen, dass sich der Browsertitel, die primäre Überschrift und die sekundären Überschriften geändert haben. (Wenn die Änderungen nicht im Browser angezeigt werden, zeigen Sie ggf. zwischengespeicherten Inhalt an. Drücken Sie in Ihrem Browser STRG + F5, um das Laden der Antwort vom Server zu erzwingen.) Der Browser Titel wird mit dem `ViewBag.Title` erstellt, den wir in der Ansichts Vorlage *Index. cshtml* festgelegt haben, und der zusätzlichen &quot;-Movie-App&quot; in der Layoutdatei hinzugefügt.

Beachten Sie auch, wie der Inhalt in der Ansichts Vorlage *Index. cshtml* mit der Ansichts Vorlage *\_Layout. cshtml* zusammengeführt und eine einzelne HTML-Antwort an den Browser gesendet wurde. Layoutvorlagen erleichtern ungemein das Vornehmen von Änderungen an allen Seiten in Ihrer Anwendung.

![](adding-a-view/_static/image9.png)

Unser wenig &quot;Daten&quot; (in diesem Fall die &quot;Hello aus unserer Ansichts Vorlage!&quot; Nachricht) ist jedoch hart codiert. Die MVC-Anwendung verfügt über eine &quot;V-&quot; (Ansicht), und Sie haben eine &quot;C-&quot; (Controller), aber noch keine &quot;M&quot; (Model). In Kürze werden die Schritte zum Erstellen einer Datenbank und zum Abrufen von Modelldaten erläutert.

## <a name="passing-data-from-the-controller-to-the-view"></a>Übergeben von Daten vom Controller an die Ansicht

Bevor wir zu einer Datenbank gehen und über Modelle sprechen, sollten wir uns jedoch zunächst mit der Übergabe von Informationen vom Controller an eine Ansicht beschäftigen. Controller Klassen werden als Antwort auf eine eingehende URL-Anforderung aufgerufen. Eine Controller Klasse ist der Ort, an dem Sie den Code schreiben, der die eingehenden Browser Anforderungen verarbeitet, Daten aus einer Datenbank abruft und letztendlich entscheidet, welche Art von Antwort an den Browser zurückgesendet werden soll. Ansichts Vorlagen können dann von einem Controller verwendet werden, um eine HTML-Antwort an den Browser zu generieren und zu formatieren.

Controller sind dafür verantwortlich, welche Daten oder Objekte erforderlich sind, damit eine Ansichts Vorlage eine Antwort an den Browser Rendering. Eine bewährte Vorgehens **Weise: eine Ansichts Vorlage sollte niemals Geschäftslogik durchführen oder direkt mit einer Datenbank interagieren**. Stattdessen sollte eine Ansichts Vorlage nur mit den Daten arbeiten, die für den Controller bereitgestellt werden. Wenn Sie diese &quot;Trennung von Anliegen beibehalten&quot;, können Sie Ihren Code bereinigen, testen und besser verwalten.

Derzeit nimmt die `Welcome` Aktionsmethode in der `HelloWorldController`-Klasse eine `name` und einen `numTimes` Parameter an und gibt die Werte dann direkt an den Browser aus. Anstatt den Controller diese Antwort als Zeichenfolge zu erzeugen, ändern wir den Controller so, dass stattdessen eine Ansichts Vorlage verwendet wird. Die Ansichtsvorlage generiert eine dynamische Antwort. Das bedeutet, dass Sie die entsprechenden Datenelemente vom Controller an die Ansicht übergeben müssen, um die Antwort zu generieren. Hierzu können Sie den Controller die dynamischen Daten (Parameter), die die Ansichts Vorlage benötigt, in einem `ViewBag` Objekt ablegen, auf das die Ansichts Vorlage zugreifen kann.

Kehren Sie zur Datei *HelloWorldController.cs* zurück, und ändern Sie die `Welcome`-Methode, um dem `ViewBag` Objekt eine `Message` und `NumTimes` Wert hinzuzufügen. `ViewBag` ist ein dynamisches Objekt, d. h., Sie können den gewünschten Wert für die Anwendung einfügen. Das `ViewBag`-Objekt hat keine definierten Eigenschaften, bis Sie etwas darin ablegen. Das [ASP.NET MVC-Modell Bindungssystem](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) ordnet automatisch die benannten Parameter (`name` und `numTimes`) aus der Abfrage Zeichenfolge in der Adressleiste den Parametern in der-Methode zu. Die vollständige Datei *HelloWorldController.cs* sieht wie folgt aus:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Nun enthält das `ViewBag` Objektdaten, die automatisch an die Ansicht übermittelt werden.

Als nächstes benötigen Sie eine Willkommens Ansichts Vorlage. Wählen Sie im Menü **Erstellen** die Option **mvcmovie erstellen** aus, um sicherzustellen, dass das Projekt kompiliert wird.

Klicken Sie dann mit der rechten Maustaste in die `Welcome` Methode, und klicken Sie auf **Ansicht hinzufügen**

![](adding-a-view/_static/image10.png)

Das Dialogfeld **Ansicht hinzufügen** sieht wie folgt aus:

![](adding-a-view/_static/image11.png)

Klicken Sie auf **Hinzufügen**, und fügen Sie dann den folgenden Code unter dem `<h2>`-Element in der neuen Datei " *Welcome. cshtml* " hinzu. Sie erstellen eine Schleife, die &quot;Hello&quot; so oft wie der Benutzer sagt, dass dies der Fall sein sollte. Die vollständige Datei " *Welcome. cshtml* " ist unten dargestellt.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Führen Sie die Anwendung aus, und navigieren Sie zur folgenden URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Nun werden die Daten aus der URL entnommen und mithilfe des [Modell Binders](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)an den Controller übermittelt. Der Controller packt die Daten in ein `ViewBag` Objekt und übergibt dieses Objekt an die Ansicht. Die Ansicht zeigt dann die Daten für den Benutzer als HTML an.

![](adding-a-view/_static/image12.png)

Im obigen Beispiel haben wir ein `ViewBag` Objekt verwendet, um Daten vom Controller an eine Ansicht zu übergeben. In diesem Tutorial verwenden wir ein Ansichts Modell, um Daten von einem Controller an eine Ansicht zu übergeben. Der Ansatz zum Anzeigen von Modellen für das Übergeben von Daten wird im Allgemeinen vor dem Ansichts Behälter bevorzugt. Weitere Informationen finden Sie im Blogeintrag [Dynamic V-Ansichten mit starker Typisierung](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) .

Nun, das war eine Art &quot;M&quot; für das Modell, aber nicht die Daten Bank Art. Lassen Sie uns das Gelernte umsetzen und eine Filmdatenbank erstellen.

> [!div class="step-by-step"]
> [Zurück](adding-a-controller.md)
> [Weiter](adding-a-model.md)
