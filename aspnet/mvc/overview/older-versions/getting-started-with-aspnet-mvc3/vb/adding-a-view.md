---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Hinzufügen einer Ansicht (VB) | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: fa200935d83bb26c07b302449a6eba6fd67b5322
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457325"
---
# <a name="adding-a-view-vb"></a>Hinzufügen einer Ansicht (VB)

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mit Microsoft Visual Web Developer 2010 Express Service Pack 1, einer kostenlosen Version von Microsoft Visual Studio. Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen. Sie können alle Komponenten installieren, indem Sie auf den folgenden Link klicken: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten einzeln mithilfe der folgenden Links installieren:
> 
> - [Erforderliche Komponenten für Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3-Tools aktualisieren](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Laufzeit + Tool Unterstützung)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, installieren Sie die erforderlichen Komponenten, indem Sie auf den folgenden Link klicken: [Visual Studio 2010 Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit VB.NET-Quellcode ist für dieses Thema verfügbar. [Laden Sie die VB.NET-Version herunter](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wechseln Sie C#ggf. zur [ C# Version](../cs/adding-a-view.md) dieses Tutorials.

In diesem Abschnitt wird die `HelloWorldController`-Klasse so geändert, dass Sie eine Ansichts Vorlagen Datei verwendet, um den Prozess der Erstellung von HTML-Antworten auf einen Client sauber zu kapseln.

Beginnen wir mit der Verwendung einer Ansichts Vorlage mit der `Index`-Methode in der `HelloWorldController`-Klasse. Die `Index`-Methode gibt derzeit eine Zeichenfolge mit einer Nachricht zurück, die in der Controller Klasse hart codiert ist. Ändern Sie die `Index`-Methode, um ein `View` Objekt zurückzugeben, wie im folgenden dargestellt:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Nun fügen wir dem Projekt eine Ansichts Vorlage hinzu, die Sie mit der `Index`-Methode aufrufen können. Klicken Sie dazu mit der rechten Maustaste in die `Index`-Methode, und klicken Sie auf **Ansicht hinzufügen**.

[![Indexaddview](adding-a-view/_static/image2.png "Indexaddview")](adding-a-view/_static/image1.png)

Das Dialogfeld **Ansicht hinzufügen** wird angezeigt. Belassen Sie die Standardeinträge, und klicken Sie auf die Schaltfläche **Hinzufügen**

[![3addview](adding-a-view/_static/image4.png "3addview")](adding-a-view/_static/image3.png)

Der Ordner " *mvcmuvie\views\helloworld* " und die Datei " *mvcmuvie\views\helloworld\index.vbhtml* " werden erstellt. Sie können Sie in **Projektmappen-Explorer**anzeigen:

[![Solnexphelloworldindx](adding-a-view/_static/image6.png "Solnexphelloworldindx")](adding-a-view/_static/image5.png)

Fügen Sie unter dem `<h2>`-Tag HTML hinzu. Die geänderte *mvcmuvie\views\helloworld\index.vbhtml* -Datei ist unten dargestellt.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Führen Sie die Anwendung aus, und navigieren Sie zum &quot;Hello World&quot; Controller (`http://localhost:xxxx/HelloWorld`). Die `Index`-Methode in Ihrem Controller hat nicht viel funktioniert. Es wurde einfach die-Anweisung `return View()`ausgeführt, die anzeigt, dass eine Ansichts Vorlagen Datei verwendet werden soll, um eine Antwort an den Client zu Renren. Da wir den Namen der zu verwendenden Ansichts Vorlagen Datei nicht explizit angegeben haben, verwendet ASP.NET MVC standardmäßig die Ansichts Datei " *Index. vbhtml* " im Ordner " *\views\helloworld* ". Die folgende Abbildung zeigt die Zeichenfolge, die in der Ansicht hart codiert ist.

[![3helloworld](adding-a-view/_static/image8.png "3helloworld")](adding-a-view/_static/image7.png)

Sieht ziemlich gut aus. Beachten Sie jedoch, dass in der Titelleiste des Browsers &quot;Index&quot; und der große Titel auf der Seite &quot;meine MVC-Anwendung lautet.&quot; wir diese ändern.

## <a name="changing-views-and-layout-pages"></a>Ändern von Ansichten und Layoutseiten

Zunächst ändern wir den Text &quot;meine MVC-Anwendung.&quot;, dass der Text freigegeben ist und auf jeder Seite angezeigt wird. Es wird nur an einer Stelle in unserem Projekt angezeigt, obwohl es sich auf jeder Seite in der Anwendung befindet. Wechseln Sie in **Projektmappen-Explorer** zum Ordner */views/Shared* , und öffnen Sie die Datei *\_Layout. vbhtml* . Diese Datei wird als Layoutseite bezeichnet und ist die freigegebene &quot;Shell&quot;, die von allen anderen Seiten verwendet wird.

Beachten Sie die `@RenderBody()` Codezeile am Ende der Datei. `RenderBody` ist ein Platzhalter, bei dem alle von Ihnen erstellten Seiten auf der Layoutseite &quot;umschließenen&quot; angezeigt werden. Ändern Sie die `<h1>` Überschrift von **&quot;** MVC-Anwendungs&quot; in &quot;MVC-Movie-App-&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Führen Sie die Anwendung aus, und beachten Sie, dass Sie jetzt &quot;MVC Movie App&quot;. Klicken Sie auf **den Link info** , und diese Seite zeigt auch &quot;MVC Movie App&quot;.

Die Datei Complete *\_Layout. vbhtml* ist unten dargestellt:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Nun ändern wir den Titel der Index Seite (Sicht).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Öffnen Sie *mvcmuvie\views\helloworld\index.vbhtml*. Es gibt zwei Möglichkeiten, eine Änderung vorzunehmen: zuerst den Text, der im Titel des Browsers angezeigt wird, und dann im sekundären Header (das `<h2>`-Element). Wir legen Sie geringfügig anders fest, damit Sie sehen können, welcher Code in welchem Teil der APP geändert wird.

Führen Sie die Anwendung aus, und navigieren Sie zu`http://localhost:xx/HelloWorld`. Sie sehen, dass sich der Browsertitel, die primäre Überschrift und die sekundären Überschriften geändert haben. Es ist einfach, große Änderungen an Ihrer Anwendung mit kleinen Änderungen an einer Ansicht vorzunehmen. (Wenn die Änderungen nicht im Browser angezeigt werden, zeigen Sie ggf. zwischengespeicherten Inhalt an. Drücken Sie in Ihrem Browser STRG+F5, um das Laden der Antwort vom Server zu erzwingen.)

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Unser wenig &quot;Daten&quot; (in diesem Fall die &quot;Hallo Welt!&quot; Meldung) ist jedoch hart codiert. Unsere MVC-Anwendung verfügt über V (views), und wir haben C (Controller), aber noch kein M (Model). In Kürze werden die Schritte zum Erstellen einer Datenbank und zum Abrufen von Modelldaten erläutert.

## <a name="passing-data-from-the-controller-to-the-view"></a>Übergeben von Daten vom Controller an die Ansicht

Bevor wir zu einer Datenbank gehen und über Modelle sprechen, sollten wir uns jedoch zunächst mit der Übergabe von Informationen vom Controller an eine Ansicht beschäftigen. Wir möchten eine Ansichts Vorlage übergeben, um eine HTML-Antwort an einen Client zu rendereinigen. Diese Objekte werden in der Regel erstellt und von einer Controller Klasse an eine Ansichts Vorlage weitergegeben. Sie sollten nur die Daten enthalten, die die Ansichts Vorlage benötigt – und nicht mehr.

Zuvor mit der `HelloWorldController`-Klasse hat die `Welcome` Aktionsmethode eine `name` und einen `numTimes` Parameter angenommen und die Parameterwerte dann an den Browser ausgegeben. Anstatt den Controller weiterhin direkt zu Rendering, legen wir diese Daten in eine Behälter für die Ansicht. Controller und Sichten können ein `ViewBag` Objekt verwenden, um diese Daten zu speichern. Diese werden automatisch an eine Ansichts Vorlage übergeben und zum Rendering der HTML-Antwort mit dem Inhalt des Behälters als Daten verwendet. Auf diese Weise wird für den Controller eine Sache und die Ansichts Vorlage mit einer anderen –, sodass wir saubere &quot;Trennung von Anliegen&quot; in der Anwendung behalten können.

Alternativ könnten wir eine benutzerdefinierte Klasse definieren, dann eine Instanz dieses Objekts selbst erstellen, Sie mit Daten füllen und Sie an die Ansicht übergeben. Dies wird häufig als "ViewModel" bezeichnet, da es sich um ein benutzerdefiniertes Modell für die Ansicht handelt. Bei kleinen Datenmengen funktioniert der viewbag jedoch hervorragend.

Wechseln Sie zur Datei " *helloworldcontroller. vb* ", und ändern Sie die `Welcome` Methode innerhalb des Controllers, um die Meldung und die numtimes in den viewbag zu versetzen. Der viewbag ist ein dynamisches Objekt. Dies bedeutet, dass Sie den gewünschten Wert für den gewünschten Wert einfügen können. Der viewbag hat keine definierten Eigenschaften, bis Sie etwas darin ablegen.

Die gesamte `HelloWorldController.vb` mit der neuen-Klasse in derselben Datei.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Nun enthält der viewbag Daten, die automatisch an die Ansicht übergeben werden. Auch hier könnten wir auch das eigene Objekt wie folgt weitergegeben haben:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Nun benötigen wir eine `WelcomeView` Vorlage! Führen Sie die Anwendung aus, damit der neue Code kompiliert wird. Schließen Sie den Browser, klicken Sie mit der rechten Maustaste in die `Welcome` Methode, und klicken Sie dann auf **Ansicht hinzufügen**.

So sieht das Dialogfeld **Ansicht hinzufügen** aus.

[![3addwelcomeview](adding-a-view/_static/image12.png "3addwelcomeview")](adding-a-view/_static/image11.png)

Fügen Sie den folgenden Code unter dem `<h2>`-Element in der neuen Willkommens Datei hinzu <em>.</em> vbhtml-Datei. Wir erstellen eine Schleife und sagen &quot;Hello&quot; so oft wie der Benutzer sagt:

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Führen Sie die Anwendung aus, und navigieren Sie zu `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Nun werden die Daten aus der URL entnommen und automatisch an den Controller übermittelt. Der Controller packt die Daten in ein `Model` Objekt und übergibt dieses Objekt an die Ansicht. In der Ansicht werden die Daten für den Benutzer als HTML angezeigt.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Nun, das war eine Art &quot;M&quot; für das Modell, aber nicht die Daten Bank Art. Lassen Sie uns das Gelernte umsetzen und eine Filmdatenbank erstellen.

> [!div class="step-by-step"]
> [Zurück](adding-a-controller.md)
> [Weiter](adding-a-model.md)
