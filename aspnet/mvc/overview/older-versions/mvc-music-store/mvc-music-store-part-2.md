---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Teil 2: Controller | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. In Teil 2 werden Controller behandelt.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9dc2226f4951d4bed122df37d35bbb94730a00ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451191"
---
# <a name="part-2-controllers"></a>Teil 2: Controller

von [Jon Galloway](https://github.com/jongalloway)

> Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.  
>   
> Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.  
>   
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. In Teil 2 werden Controller behandelt.

Bei herkömmlichen Webframe Works werden eingehende URLs normalerweise Dateien auf dem Datenträger zugeordnet. Beispiel: eine Anforderung für eine URL wie "/Products.aspx" oder "/Products.php" wird möglicherweise von der Datei "Products. aspx" oder "Products. php" verarbeitet.

Webbasierte MVC-Frameworks ordnen URLs auf etwas andere Weise dem Servercode zu. Anstatt eingehende URLs zu Dateien zuzuordnen, ordnen Sie stattdessen URLs Methoden für Klassen zu. Diese Klassen werden als "controller" bezeichnet und sind dafür verantwortlich, eingehende HTTP-Anforderungen zu verarbeiten, Benutzereingaben zu verarbeiten, Daten abzurufen und zu speichern und die Antwort zu bestimmen, die an den Client zurückgesendet werden soll (HTML anzeigen, Datei herunterladen, zu einer anderen umleiten). URL usw.).

## <a name="adding-a-homecontroller"></a>Hinzufügen eines HomeController

Wir beginnen mit unserer MVC Music Store-Anwendung, indem wir eine Controller Klasse hinzufügen, die URLs zur Startseite unserer Website verarbeitet. Befolgen Sie die Standard Benennungs Konventionen von ASP.NET MVC, und nennen Sie es HomeController.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner "Controllers", und wählen Sie "hinzufügen" und dann "Controller..." aus. s

![](mvc-music-store-part-2/_static/image1.jpg)

Dadurch wird das Dialogfeld "Controller hinzufügen" angezeigt. Nennen Sie den Controller "HomeController", und klicken Sie auf die Schaltfläche hinzufügen.

![](mvc-music-store-part-2/_static/image1.png)

Dadurch wird eine neue Datei erstellt, HomeController.cs mit folgendem Code:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Um so einfach wie möglich zu beginnen, ersetzen wir die Index-Methode durch eine einfache Methode, die nur eine Zeichenfolge zurückgibt. Wir nehmen zwei Änderungen vor:

- Ändern Sie die Methode so, dass anstelle eines Aktions Ergebnisses eine Zeichenfolge zurückgegeben wird.
- Ändern der Return-Anweisung, sodass "Hello from Home" zurückgegeben wird

Die-Methode sollte nun wie folgt aussehen:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Ausführen der Anwendung

Nun führen wir die Website aus. Wir können unseren Web-Server starten und die Website mit einem der folgenden Schritte ausprobieren:

- Auswählen des Menü Elements Debuggen ⇨ Debuggen starten
- Klicken Sie in der Symbolleiste auf die grüne Pfeil Schaltfläche ![](mvc-music-store-part-2/_static/image2.jpg)
- Verwenden Sie die Tastenkombination F5.

Mit einem der oben aufgeführten Schritte wird das Projekt kompiliert und dann die ASP.NET Development Server, die in Visual Web Developer integriert ist, gestartet. In der unteren Ecke des Bildschirms wird eine Benachrichtigung angezeigt, die anzeigt, dass die ASP.NET Development Server gestartet wurde, und zeigt die Portnummer an, unter der Sie ausgeführt wird.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer öffnet dann automatisch ein Browserfenster, dessen URL auf den Webserver zeigt. Auf diese Weise können wir die Webanwendung schnell ausprobieren:

![](mvc-music-store-part-2/_static/image3.png)

Das war ziemlich schnell – wir haben eine neue Website erstellt, eine Funktion mit drei Zeilen hinzugefügt, und wir haben Text in einem Browser. Nicht in der Abwehr von Raketen Wissenschaften, aber es ist ein Start.

*Hinweis: Visual Web Developer enthält die ASP.NET Development Server, die Ihre Website auf einer zufälligen kostenlosen "Port"-Nummer ausführen wird. Im obigen Screenshot wird die Website unter `http://localhost:26641/`ausgeführt. Daher wird Port 26641 verwendet. Die Portnummer wird unterschiedlich angezeigt. Wenn wir über URLs wie/Store/Browse in diesem Tutorial sprechen, erfolgt die Portnummer. Wenn Sie die Portnummer 26641 aufrufen, bedeutet das Navigieren zu/Store/Browse, zu `http://localhost:26641/Store/Browse`zu navigieren.*

## <a name="adding-a-storecontroller"></a>Hinzufügen eines StoreController

Wir haben einen einfachen HomeController hinzugefügt, der die Startseite der Website implementiert. Nun fügen wir einen weiteren Controller hinzu, mit dem wir die Browser Funktionalität unseres Musik Stores implementieren. Der Store-Controller unterstützt drei Szenarien:

- Eine Listenseite der Musikgenres in unserem Music Store
- Eine Seite zum Durchsuchen, auf der alle Musikalben eines bestimmten Genres aufgelistet sind.
- Eine Detailseite, auf der Informationen zu einem bestimmten Musikalbum angezeigt werden.

Wir beginnen mit dem Hinzufügen einer neuen StoreController-Klasse. Wenn Sie dies noch nicht getan haben, beenden Sie die Ausführung der Anwendung, indem Sie entweder den Browser schließen oder das Menü Element Debuggen ⇨ beenden auswählen.

Fügen Sie nun einen neuen StoreController hinzu. Wie bei HomeController wird dies durchgeführt, indem Sie in der Projektmappen-Explorer mit der rechten Maustaste auf den Ordner "Controllers" klicken und das Menü Element Add-&gt;Controller auswählen.

![](mvc-music-store-part-2/_static/image4.png)

Unser neuer StoreController verfügt bereits über eine "index"-Methode. Wir verwenden diese "index"-Methode, um unsere Auflistungs Seite zu implementieren, die alle Genres in unserem Musikspeicher auflistet. Wir fügen außerdem zwei zusätzliche Methoden hinzu, um die beiden anderen Szenarien zu implementieren, die von StoreController behandelt werden sollen: Durchsuchen und Details.

Diese Methoden (Index, Browse und Details) im Controller werden als "Controller Aktionen" bezeichnet, und wie Sie bereits mit der HomeController. Index ()-Aktionsmethode gesehen haben, besteht ihre Aufgabe darin, auf URL-Anforderungen zu reagieren und (im allgemeinen) den Inhalt zu ermitteln. sollte an den Browser oder den Benutzer zurückgesendet werden, der die URL aufgerufen hat.

Wir starten unsere StoreController-Implementierung, indem wir die theindex ()-Methode ändern, um die Zeichenfolge "Hello from Store. Index ()" zurückzugeben, und wir fügen ähnliche Methoden für "Browse ()" und "Details ()" hinzu:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Führen Sie das Projekt erneut aus, und Durchsuchen Sie die folgenden URLs:

- /Store
- /Store/Browse
- /Store/Details

Durch den Zugriff auf diese URLs werden die Aktionsmethoden in unserem Controller aufgerufen und Zeichen folgen Antworten zurückgegeben:

![](mvc-music-store-part-2/_static/image5.png)

Das ist großartig, aber es handelt sich hierbei nur um konstante Zeichen folgen. Wir machen Sie dynamisch, sodass Sie Informationen aus der URL übernehmen und in der Seiten Ausgabe anzeigen.

Zuerst ändern wir die Methode zum Durchsuchen, um einen QueryString-Wert aus der URL abzurufen. Zu diesem Zweck können Sie der Aktionsmethode einen "Genre"-Parameter hinzufügen. Wenn wir dies tun, übergibt MVC automatisch alle QueryString-oder Form Post-Parameter mit dem Namen "Genre" an unsere Aktionsmethode, wenn Sie aufgerufen wird.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Hinweis: die Benutzereingaben werden mithilfe der hilfsprogrammmethode HttpUtility. HtmlEncode bereinigt. Dadurch wird verhindert, dass Benutzer JavaScript in unserer Ansicht mit einem Link wie/Store/Browse einfügen? Genre =&lt;Skript&gt;Window. Location = 'http://hackersite.com'&lt;/Script&gt;.*

Navigieren wir nun zu/Store/Browse? Genre = Disco

![](mvc-music-store-part-2/_static/image6.png)

Als nächstes ändern wir die Aktion "Details", um einen Eingabeparameter mit dem Namen "ID" anzuzeigen und anzuzeigen. Anders als bei der vorherigen Methode wird der ID-Wert nicht als QueryString-Parameter einbettet. Stattdessen Betten wir ihn direkt in die URL ein. Beispiel:/Store/Details/5.

Mit ASP.NET MVC können wir dies problemlos tun, ohne dass Sie etwas konfigurieren müssen. Die Standard Routing Konvention von ASP.NET MVC besteht darin, das Segment einer URL nach dem Namen der Aktionsmethode als Parameter mit dem Namen "ID" zu behandeln. Wenn die Aktionsmethode einen Parameter mit dem Namen ID hat, übergibt ASP.NET MVC das URL-Segment automatisch als Parameter an Sie.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Führen Sie die Anwendung aus, und navigieren Sie zu/Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Wir wollen uns nun weitermachen, was wir bisher getan haben:

- Wir haben in Visual Web Developer ein neues ASP.NET MVC-Projekt erstellt.
- Wir haben die grundlegende Ordnerstruktur einer ASP.NET MVC-Anwendung erläutert.
- Wir haben gelernt, wie unsere Website mithilfe der ASP.NET Development Server
- Wir haben zwei Controller Klassen erstellt: einen HomeController und einen StoreController.
- Wir haben unseren Controllern Aktionsmethoden hinzugefügt, die auf URL-Anforderungen reagieren und Text an den Browser zurückgeben.

> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-1.md)
> [Weiter](mvc-music-store-part-3.md)
