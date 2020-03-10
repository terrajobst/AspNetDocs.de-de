---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Hinzufügen einer Ansicht | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Erstellen Sie eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 462b1210c45da67058899193afcea973f3daf122
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469773"
---
# <a name="adding-a-view"></a>Hinzufügen einer Ansicht

von [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Sie erstellen eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt. Besuchen Sie das [ASP.NET MVC Learning Center](../../../index.md) , um weitere ASP.NET MVC-Tutorials und-Beispiele zu finden.

In diesem Abschnitt wird erläutert, wie wir mit der helloworldcontroller-Klasse eine Ansichts Vorlagen Datei verwenden können, um das Erstellen von HTML-Antworten auf einen Client zu bereinigen.

Beginnen wir mit der Verwendung einer Ansichts Vorlage mit der Index-Methode. Unsere Methode wird als Index bezeichnet und befindet sich im helloworldcontroller. Zurzeit gibt unsere Index ()-Methode eine Zeichenfolge mit einer Nachricht zurück, die innerhalb der Controller Klasse hart codiert ist.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Nun ändern wir die Index Methode dahin, dass Sie wie folgt aussieht:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Nun fügen wir dem Projekt eine Ansichts Vorlage hinzu, die wir für unsere Index ()-Methode verwenden können. Klicken Sie hierzu mit der rechten Maustaste auf eine beliebige Stelle in der Mitte der Index Methode, und klicken Sie auf Ansicht hinzufügen...

![Bild](getting-started-with-mvc-part3/_static/image1.png)

Dadurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt, in dem einige Optionen für die Erstellung einer Ansichts Vorlage angezeigt werden, die von der Index Methode verwendet werden kann. Ändern Sie vorerst nichts, und klicken Sie einfach auf die Schaltfläche hinzufügen.

[Dialog !["Ansicht hinzufügen"](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Nachdem Sie auf Hinzufügen klicken, werden ein neuer Ordner und eine neue Datei im Projektmappenordner angezeigt, wie hier zu sehen. Jetzt habe ich einen Ordner "HelloWorld" unter "Views" und eine Datei "index. aspx" in diesem Ordner.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Die neue Index Datei ist auch bereits geöffnet und bereit zum Bearbeiten. Fügen Sie unter dem ersten &lt;H2&gt;Index&lt;/H2&gt; wie "Hallo Welt" Text hinzu.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Führen Sie die Anwendung aus, und besuchen Sie [`http://localhost:xx/HelloWorld`](http://localhostxx) erneut in Ihrem Browser. Die Index-Methode in unserem Controller in diesem Beispiel hat keine Arbeit ausgeführt, aber "Return View ()" aufgerufen, die anzeigt, dass eine Ansichts Vorlagen Datei verwendet werden soll, um eine Antwort an den Client zurückzugeben. Da wir den Namen der zu verwendenden Ansichts Vorlagen Datei nicht explizit angegeben haben, verwendet ASP.NET MVC standardmäßig die Ansichts Datei "index. aspx" im Ordner "\views\helloworld". Nun sehen wir die Zeichenfolge, die wir in unserer Ansicht hart codiert haben.

[![Index-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Sieht ziemlich gut aus. Beachten Sie jedoch, dass der Titel des Browsers "index" und der große Titel auf der Seite "meine MVC-Anwendung" lautet. Ändern wir diese Änderungen.

### <a name="changing-views-and-master-pages"></a>Ändern von Ansichten und Master Seiten

Zunächst ändern wir den Text "meine MVC-Anwendung". Dieser Text ist freigegeben und wird auf jeder Seite angezeigt. Es wird nur an einer Stelle in unserem Code angezeigt, obwohl es sich auf jeder Seite in der APP befindet. Wechseln Sie in der Projektmappen-Explorer zum Ordner/views/Shared, und öffnen Sie die Datei Site. Master. Diese Datei wird als Master Seite bezeichnet und ist die freigegebene "Shell", die von allen anderen Seiten verwendet wird.

Beachten Sie einen Text, der contentplachalter "mainContent" in dieser Datei besagt.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Dieser Platzhalter ist der Ort, an dem alle von Ihnen erstellten Seiten auf der Master Seite angezeigt werden. Ändern Sie den Titel, führen Sie die APP aus, und besuchen Sie mehrere Seiten. Sie werden feststellen, dass eine Änderung auf mehreren Seiten angezeigt wird.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Nun erhält jede Seite die primäre Überschrift, das ist H1 von "My MVC Movie Application". , Der den weißen Text am oberen Rand der Seite verarbeitet, der von allen Seiten gemeinsam genutzt wird.

Hier ist die Website. Master vollständig mit dem geänderten Titel:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Nun ändern wir den Titel der Index Seite.

/HelloWorld/Index.aspx. Öffnen Es gibt zwei zu ändernde Orte. Zuerst der Titel, der im Titel des Browsers angezeigt wird, und dann der sekundäre Header, auch das H2. Ich mache Sie jeweils etwas anders, damit Sie sehen können, welcher Code in welchem Teil der APP geändert wird.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Führen Sie Ihre APP aus, und besuchen Sie/Movies. Beachten Sie, dass sich der Browser Titel, die primäre Überschrift und die sekundären Überschriften geändert haben. Es ist einfach, große Änderungen an Ihrer APP mit kleinen Änderungen an ihrer Ansicht vorzunehmen.

[![Movie List-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Unser bisschen "Data" (in diesem Fall "Hallo Welt!" Message) wurde jedoch hart codiert. Wir haben V (views), und wir haben C (Controller), aber noch kein M (Model). Wir werden kurz darauf eingehen, wie eine Datenbank erstellt und Modelldaten aus ihr abgerufen werden.

## <a name="passing-a-viewmodel"></a>Übergeben eines ViewModel

Bevor wir zu einer Datenbank gehen und über Modelle sprechen, sprechen wir jedoch zuerst über "ViewModels". Dies sind Objekte, die darstellen, was eine Ansichts Vorlage zum Rendering einer HTML-Antwort an einen Client erfordert. Sie werden in der Regel erstellt und von einer Controller Klasse an eine Ansichts Vorlage weitergegeben. Sie sollten nur die Daten enthalten, die die Ansichts Vorlage benötigt, und nicht mehr.

Zuvor hat unsere Willkommens ()-Aktionsmethode einen Namen und einen numtimes-Parameter angenommen und im Browser ausgegeben. Anstatt dass der Controller diese Antwort nicht mehr direkt Renderern soll, erstellen wir stattdessen eine kleine Klasse, um diese Daten zu speichern und Sie dann an eine Ansichts Vorlage zu übergeben, um die HTML-Antwort mit der Antwort zurückzugeben. Auf diese Weise ist der Controller mit einer Aktion und der Ansichts Vorlage eine andere –, sodass wir eine saubere Trennung der Belange in unserer Anwendung gewährleisten können.

Kehren Sie zur Datei HelloWorldController.cs zurück, und fügen Sie eine neue "welcomeviewmodel"-Klasse hinzu, und ändern Sie die Willkommens Methode in Ihrem Controller. Im folgenden finden Sie die komplette HelloWorldController.cs mit der neuen-Klasse in derselben Datei.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Obwohl es sich in mehreren Zeilen befindet, ist unsere Willkommens Methode eigentlich nur zwei Code Anweisungen. Die erste Anweisung packt die beiden Parameter in ein ViewModel-Objekt, und das zweite übergibt das resultierende Objekt an die Ansicht.

Nun benötigen wir eine Willkommens Ansichts Vorlage! Klicken Sie mit der rechten Maustaste auf die Willkommens Methode, und wählen Sie hinzufügen Diesmal aktivieren wir "Erstellen einer stark typisierten Ansicht" und wählen die Klasse "welcomeviewmodel" aus der Dropdown Liste aus. Diese neue Ansicht kennt nur "welcomeviewmodels" und keine anderen Objekttypen.

> *Hinweis: Sie müssen nach dem Hinzufügen von "welcomeviewmodel" ein einmal kompiliert haben, damit es in der Dropdown Liste angezeigt wird.*

Das Dialogfeld "Ansicht hinzufügen" sollte wie folgt aussehen: Klicken Sie auf die Schaltfläche Add. ![Ansicht hinzufügen](getting-started-with-mvc-part3/_static/image10.png)

Fügen Sie diesen Code unter der &lt;H2-&gt; in der neuen Datei "Welcome. aspx" hinzu. Wir werden eine Schleife erstellen und Hello so oft wie der Benutzer sagen.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Beachten Sie auch, dass Sie diese Ansicht über das "welcomeviewmodel" erhalten haben (Sie sind verheiratet, denken Sie daran?), dass wir jedes Mal, wenn wir auf unser Modell Objekt verweisen, hilfreiche IntelliSense erhalten, wie im folgenden Screenshot zu sehen:

[![numtime-Quellcode](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Führen Sie die Anwendung aus, und besuchen Sie `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` erneut. Wir nehmen nun Daten aus der URL auf, die automatisch an den Controller weitergeleitet werden. der Controller packt die Daten in ein ViewModel und übergibt das Objekt an unsere Ansicht. In der Ansicht werden die Daten für den Benutzer als HTML angezeigt.

[![Willkommen: Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Nun, das war eine Art "M" für das Modell, aber nicht die Daten Bank Art. Wir haben das gelernt und eine Datenbank mit Filmen erstellt.

> [!div class="step-by-step"]
> [Zurück](getting-started-with-mvc-part2.md)
> [Weiter](getting-started-with-mvc-part4.md)
