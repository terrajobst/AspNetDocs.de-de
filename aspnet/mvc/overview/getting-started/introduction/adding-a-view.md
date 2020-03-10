---
title: Hinzufügen einer Ansicht zu einer MVC-App
author: Rick-Anderson
description: Hinzufügen einer Ansicht zu einer MVC-App
ms.author: riande
ms.date: 01/23/2019
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 0bc6ac06d12aaee4b2a11c1bf246f9f20f0be017
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78470769"
---
# <a name="adding-a-view"></a>Hinzufügen einer Ansicht

von [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

In diesem Abschnitt ändern Sie die `HelloWorldController`-Klasse so, dass Sie Vorlagen Dateien anzeigen verwendet, um den Prozess der Erstellung von HTML-Antworten für einen Client zu bereinigen. 

Sie erstellen mithilfe der [Razor-Ansichts-Engine](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md)eine Ansichts Vorlagen Datei. Razor-basierte Ansichts Vorlagen verfügen über die Dateierweiterung " *. cshtml* " und bieten eine elegante Möglichkeit zum Erstellen C#einer HTML-Ausgabe mithilfe von. Razor minimiert die Anzahl der zum Schreiben einer Ansichts Vorlage erforderlichen Zeichen und Tastatureingaben und ermöglicht einen schnellen, fließenden Codierungs Workflow.

Derzeit gibt die `Index`-Methode eine Zeichenfolge mit der Meldung zurück, die in der Controllerklasse hartcodiert ist. Ändern Sie die `Index`-Methode so, dass Sie die Controller [Ansichts](/dotnet/api/microsoft.aspnetcore.mvc.controller.view#Microsoft_AspNetCore_Mvc_Controller_View) Methode aufruft, wie im folgenden Code gezeigt:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

Die oben beschriebene `Index` Methode verwendet eine Ansichts Vorlage, um eine HTML-Antwort an den Browser zu generieren. Controller Methoden (auch [Aktionsmethoden](http://rachelappel.com/asp.net-mvc-actionresults-explained)genannt), wie z. b. die oben genannte `Index` Methode, geben im Allgemeinen ein [Action result](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (oder eine von " [Action result](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)" abgeleitete Klasse) und keine primitiven Typen wie String zurück.

Klicken Sie mit der rechten Maustaste auf den Ordner *views\helloworld* , klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **MVC 5-Ansichts Seite mit Layout**
  
![](adding-a-view/_static/image1.png)   
  
Geben Sie im Dialogfeld **Name für Element angeben** den Wert *Index*ein, und klicken Sie dann auf **OK**.  
  
![](adding-a-view/_static/image2.png)  
  
Akzeptieren Sie im Dialogfeld **Layoutseite auswählen** die Standard **\_Layout. cshtml** , und klicken Sie auf **OK**.  
  
![](adding-a-view/_static/image3.png)  
  
Im obigen Dialogfeld wird im linken Bereich der Ordner " *views\shared* " ausgewählt. Wenn Sie über eine benutzerdefinierte Layoutdatei in einem anderen Ordner verfügen, können Sie Sie auswählen. Die Layoutdatei wird später in diesem Tutorial erläutert.

Die *mvcmuvie\views\helloworld\index.cshtml* -Datei wird erstellt.

![](adding-a-view/_static/image4.png)

Fügen Sie das folgende hervorgehobene Markup hinzu.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Klicken Sie mit der rechten Maustaste auf die Datei *Index. cshtml* , und wählen Sie **in Browser anzeigen**.

![PI](adding-a-view/_static/image5.png)

Sie können auch mit der rechten Maustaste auf die Datei *Index. cshtml* klicken und **in Seitenprüfung** die Option Ansicht auswählen. Weitere Informationen finden Sie im [Seitenprüfung-Tutorial](../../views/using-page-inspector-in-aspnet-mvc.md) .

Alternativ können Sie die Anwendung ausführen und zum `HelloWorld` Controller (`http://localhost:xxxx/HelloWorld`) navigieren. Die `Index`-Methode in Ihrem Controller hat nicht viel funktioniert. Es wurde einfach die-Anweisung `return View()`ausgeführt, mit der angegeben wurde, dass die Methode eine Ansichts Vorlagen Datei verwenden soll, um eine Antwort an den Browser auszugeben. Da Sie den Namen der zu verwendenden Ansichts Vorlagen Datei nicht explizit angegeben haben, verwendet ASP.NET MVC standardmäßig die Ansichts Datei " *Index. cshtml* " im Ordner " *\views\helloworld* ". Die folgende Abbildung zeigt die Zeichenfolge &quot;Hello aus unserer Ansichts Vorlage.&quot; in der-Sicht hart codiert.

![](adding-a-view/_static/image6.png)

Sieht ziemlich gut aus. Beachten Sie jedoch, dass in der Titelleiste des Browsers "Index-My ASP.NET Application" und der große Link oben auf der Seite "Anwendungsname" angezeigt wird. Je nachdem, wie klein Sie das Browserfenster machen, müssen Sie möglicherweise auf die drei Balken in der oberen rechten Ecke klicken, um die Links zu den Links **Start** **, Info,** **Contact**, **Register** und **Log in** anzuzeigen.

## <a name="changing-views-and-layout-pages"></a>Ändern von Ansichten und Layoutseiten

Zuerst möchten Sie den &quot;Anwendungsnamen&quot; Links oben auf der Seite ändern. Dieser Text ist für jede Seite üblich. Es wird tatsächlich nur an einer Stelle im Projekt implementiert, auch wenn es auf jeder Seite in der Anwendung angezeigt wird. Wechseln Sie in **Projektmappen-Explorer** zum Ordner */views/Shared* , und öffnen Sie die Datei *\_Layout. cshtml* . Diese Datei wird als *Layoutseite* bezeichnet und befindet sich im freigegebenen Ordner, der von allen anderen Seiten verwendet wird.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Mit Layoutvorlagen können Sie das HTML-Container Layout Ihrer Website an einem Ort angeben und dann auf mehrere Seiten auf Ihrer Website anwenden. Suchen Sie die Zeile `@RenderBody()`. `RenderBody` ist ein Platzhalter, bei dem alle ansichtsspezifischen Seiten, die Sie erstellen, von der Layoutseite &quot;umschlossen&quot; angezeigt werden. Wenn Sie z. b. den **Link info** auswählen, wird die Ansicht *views\home\about.cshtml* innerhalb der `RenderBody`-Methode gerendert.

Ändern Sie den Inhalt des Elements „title“. Ändern Sie den [Aktionslink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) in der Layoutvorlage von &quot;Anwendungsname&quot; in &quot;MVC Movie&quot; und den Controller von `Home` zu `Movies`. Die komplette Layoutdatei ist unten dargestellt:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Führen Sie die Anwendung aus, und beachten Sie, dass Sie jetzt &quot;MVC Movie &quot;heißt. Klicken Sie auf **den Link info** , und Sie sehen, dass auf dieser Seite auch &quot;MVC Movie&quot;angezeigt wird. Wir konnten die Änderung einmal in der Layoutvorlage vornehmen, sodass alle Seiten auf der Website den neuen Titel widerspiegeln.

![](adding-a-view/_static/image8.png)

Beim ersten Erstellen der Datei " *views\helloworld\index.cshtml* " enthielt Sie den folgenden Code:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Der obige Razor-Code legt die Layoutseite explizit fest. Überprüfen Sie die *Ansichten\\_ViewStart cshtml* -Datei, die genau dasselbe Razor-Markup enthält. Die *[Ansichten\\_ViewStart. cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* -Datei definiert das gemeinsame Layout, das von allen Ansichten verwendet wird. Daher können Sie diesen Code aus der Datei " *views\helloworld\index.cshtml* " auskommentieren oder entfernen.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

Sie können mithilfe der `Layout`-Eigenschaft eine andere Layoutansicht festlegen oder diese auf `null` festlegen, damit keine Layoutdatei verwendet wird.

Nun ändern wir den Titel der Index Sicht.

Öffnen Sie *mvcmuvie\views\helloworld\index.cshtml*. Es gibt zwei Möglichkeiten, eine Änderung vorzunehmen: zuerst den Text, der im Titel des Browsers angezeigt wird, und dann im sekundären Header (das `<h2>`-Element). Sie nehmen diese geringfügig anders vor, damit Sie erkennen können, welcher Teil der App mithilfe einer Codebearbeitung geändert wird.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

Um den HTML-Titel anzugeben, der angezeigt werden soll, legt der obige Code eine `Title` Eigenschaft des `ViewBag` Objekts fest (in der Ansichts Vorlage *Index. cshtml* ). Beachten Sie, dass die Layoutvorlage ( *views\shared\\_Layout. cshtml* ) diesen Wert im `<title>`-Element als Teil des `<head>` Abschnitts von HTML verwendet, den wir zuvor geändert haben.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Mithilfe dieses `ViewBag` Ansatzes können Sie problemlos andere Parameter zwischen der Ansichts Vorlage und der Layoutdatei übergeben.

Führen Sie die Anwendung aus. Sie sehen, dass sich der Browsertitel, die primäre Überschrift und die sekundären Überschriften geändert haben. (Wenn die Änderungen nicht im Browser angezeigt werden, zeigen Sie ggf. zwischengespeicherten Inhalt an. Drücken Sie in Ihrem Browser STRG + F5, um das Laden der Antwort vom Server zu erzwingen.) Der Browser Titel wird mit dem `ViewBag.Title` erstellt, den wir in der Ansichts Vorlage *Index. cshtml* festgelegt haben, und der zusätzlichen &quot;-Movie-App&quot; in der Layoutdatei hinzugefügt.

Beachten Sie auch, wie der Inhalt in der Ansichts Vorlage *Index. cshtml* mit der Ansichts Vorlage *\_Layout. cshtml* zusammengeführt und eine einzelne HTML-Antwort an den Browser gesendet wurde. Layoutvorlagen erleichtern ungemein das Vornehmen von Änderungen an allen Seiten in Ihrer Anwendung.

![](adding-a-view/_static/image9.png)

Unser wenig &quot;Daten&quot; (in diesem Fall die &quot;Hello aus unserer Ansichts Vorlage!&quot; Nachricht) ist jedoch hart codiert. Die MVC-Anwendung verfügt über eine &quot;V-&quot; (Ansicht), und Sie haben eine &quot;C-&quot; (Controller), aber noch keine &quot;M&quot; (Model). In Kürze wird erläutert, wie Sie eine Datenbank erstellen und Modelldaten daraus abrufen.

## <a name="passing-data-from-the-controller-to-the-view"></a>Übergeben von Daten vom Controller an die Ansicht

Bevor wir zu einer Datenbank gehen und über Modelle sprechen, sollten wir uns jedoch zunächst mit der Übergabe von Informationen vom Controller an eine Ansicht beschäftigen. Controller Klassen werden als Antwort auf eine eingehende URL-Anforderung aufgerufen. Eine Controller Klasse ist der Ort, an dem Sie den Code schreiben, der die eingehenden Browser Anforderungen verarbeitet, Daten aus einer Datenbank abruft und letztendlich entscheidet, welche Art von Antwort an den Browser zurückgesendet werden soll. Ansichts Vorlagen können dann von einem Controller verwendet werden, um eine HTML-Antwort an den Browser zu generieren und zu formatieren.

Controller sind dafür verantwortlich, welche Daten oder Objekte erforderlich sind, damit eine Ansichts Vorlage eine Antwort an den Browser Rendering. Eine bewährte Vorgehens **Weise: eine Ansichts Vorlage sollte niemals Geschäftslogik durchführen oder direkt mit einer Datenbank interagieren**. Stattdessen sollte eine Ansichts Vorlage nur mit den Daten arbeiten, die für den Controller bereitgestellt werden. Wenn Sie diese &quot;Trennung von Anliegen beibehalten&quot;, können Sie Ihren Code bereinigen, testen und besser verwalten.

Derzeit nimmt die `Welcome` Aktionsmethode in der `HelloWorldController`-Klasse eine `name` und einen `numTimes` Parameter an und gibt die Werte dann direkt an den Browser aus. Anstatt den Controller diese Antwort als Zeichenfolge zu erzeugen, ändern wir den Controller so, dass stattdessen eine Ansichts Vorlage verwendet wird. Die Ansichtsvorlage generiert eine dynamische Antwort. Das bedeutet, dass Sie die entsprechenden Datenelemente vom Controller an die Ansicht übergeben müssen, um die Antwort zu generieren. Hierzu können Sie den Controller die dynamischen Daten (Parameter), die die Ansichts Vorlage benötigt, in einem `ViewBag` Objekt ablegen, auf das die Ansichts Vorlage zugreifen kann.

Kehren Sie zur Datei *HelloWorldController.cs* zurück, und ändern Sie die `Welcome`-Methode, um dem `ViewBag` Objekt eine `Message` und `NumTimes` Wert hinzuzufügen. `ViewBag` ist ein dynamisches Objekt, d. h., Sie können den gewünschten Wert für die Anwendung einfügen. Das `ViewBag`-Objekt hat keine definierten Eigenschaften, bis Sie etwas darin ablegen. Das [ASP.NET MVC-Modell Bindungssystem](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) ordnet automatisch die benannten Parameter (`name` und `numTimes`) aus der Abfrage Zeichenfolge in der Adressleiste den Parametern in der-Methode zu. Die vollständige Datei *HelloWorldController.cs* sieht wie folgt aus:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

Nun enthält das `ViewBag` Objektdaten, die automatisch an die Ansicht übermittelt werden. Als nächstes benötigen Sie eine Willkommens Ansichts Vorlage. Wählen Sie im Menü **Erstellen** die Option Projekt Mappe **Erstellen** aus (oder drücken Sie STRG + UMSCHALT + B), um sicherzustellen, dass das Projekt kompiliert wird. Klicken Sie mit der rechten Maustaste auf den Ordner *views\helloworld* , klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **MVC 5-Ansichts Seite mit Layout**
  
![](adding-a-view/_static/image10.png)   
  
Geben Sie im Dialogfeld **Name für Element angeben die Bezeichnung** *Willkommen*ein, und klicken Sie dann auf **OK**.   
  
Akzeptieren Sie im Dialogfeld **Layoutseite auswählen** die Standard **\_Layout. cshtml** , und klicken Sie auf **OK**.  
  
![](adding-a-view/_static/image11.png)   

Die *mvcmuvie\views\helloworld\welcome.cshtml* -Datei wird erstellt.

Ersetzen Sie das Markup in der Datei " *Welcome. cshtml* ". Sie erstellen eine Schleife, die &quot;Hello&quot; so oft wie der Benutzer sagt, dass dies der Fall sein sollte. Die vollständige Datei " *Welcome. cshtml* " ist unten dargestellt.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Führen Sie die Anwendung aus, und navigieren Sie zur folgenden URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Nun werden die Daten aus der URL entnommen und mithilfe des [Modell Binders](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)an den Controller übermittelt. Der Controller packt die Daten in ein `ViewBag` Objekt und übergibt dieses Objekt an die Ansicht. Die Ansicht zeigt dann die Daten für den Benutzer als HTML an.

![](adding-a-view/_static/image12.png)

Im obigen Beispiel haben wir ein `ViewBag` Objekt verwendet, um Daten vom Controller an eine Ansicht zu übergeben. Später in diesem Tutorial verwenden wir eine Ansichtsmodell, um Daten von einem Controller an eine Ansicht zu übergeben. Der Ansatz zum Anzeigen von Modellen für das Übergeben von Daten wird im Allgemeinen vor dem Ansichts Behälter bevorzugt. Weitere Informationen finden Sie im Blogeintrag [Dynamic V-Ansichten mit starker Typisierung](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) . 

Nun, das war eine Art &quot;M&quot; für das Modell, aber nicht die Daten Bank Art. Lassen Sie uns das Gelernte umsetzen und eine Filmdatenbank erstellen.

> [!div class="step-by-step"]
> [Zurück](adding-a-controller.md)
> [Weiter](adding-a-model.md)
