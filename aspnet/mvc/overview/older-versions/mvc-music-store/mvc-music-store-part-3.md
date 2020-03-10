---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Teil 3: Ansichten und ViewModels | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. In Teil 3 werden Sichten und ViewModels behandelt.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3fcfc816cde22c697a78bab2c9ea7ace1bf68501
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451131"
---
# <a name="part-3-views-and-viewmodels"></a>Teil 3: Ansichten und ViewModels

von [Jon Galloway](https://github.com/jongalloway)

> Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.  
>   
> Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.  
>   
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. In Teil 3 werden Sichten und ViewModels behandelt.

Bisher haben wir soeben Zeichen folgen aus Controller Aktionen zurückgegeben. Das ist eine gute Möglichkeit, um eine Vorstellung davon zu erhalten, wie Controller funktionieren, aber es ist nicht so, dass Sie eine echte Webanwendung erstellen möchten. Wir wünschen eine bessere Möglichkeit, HTML-Code zu Browsern zu generieren, die unsere Website besuchen – eine, bei der wir Vorlagen Dateien verwenden können, um den HTML-Inhalt des sendebacks leichter anzupassen. Das ist genau das, was die Ansichten tun.

## <a name="adding-a-view-template"></a>Hinzufügen einer Ansichts Vorlage

Um eine Ansichts Vorlage zu verwenden, ändern wir die Methode HomeController Index so, dass ein Aktions Ergebnis zurückgegeben wird, und lassen Sie View () wie folgt zurückgeben:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

Die obige Änderung gibt an, dass anstelle einer Zeichenfolge eine "Ansicht" verwendet werden soll, um ein Ergebnis zurückzugeben.

Nun fügen wir dem Projekt eine passende Ansichts Vorlage hinzu. Zu diesem Zweck positionieren wir den Textcursor innerhalb der Index Aktionsmethode, klicken mit der rechten Maustaste und wählen "Ansicht hinzufügen" aus. Dadurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt:

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

Mit dem Dialogfeld "Ansicht hinzufügen" können Sie schnell und einfach Ansichts Vorlagen Dateien generieren. Standardmäßig wird im Dialogfeld "Ansicht hinzufügen" der Name der zu erstellenden Ansichts Vorlage vorab aufgefüllt, sodass Sie mit der Aktionsmethode übereinstimmt, von der Sie verwendet wird. Da wir das Kontextmenü "Ansicht hinzufügen" innerhalb der Index ()-Aktionsmethode des HomeController verwendet haben, enthält das obige Dialogfeld "Add View" (Ansicht hinzufügen) "index", da der Ansichts Name standardmäßig bereits aufgefüllt ist. Wir müssen die Optionen in diesem Dialogfeld nicht ändern, und klicken Sie auf die Schaltfläche hinzufügen.

Wenn Sie auf die Schaltfläche Hinzufügen klicken, erstellt Visual Web Developer eine neue Index. cshtml-Ansichts Vorlage für uns im Verzeichnis "\views\home" und erstellt den Ordner, wenn er nicht bereits vorhanden ist.

![](mvc-music-store-part-3/_static/image2.png)

Der Name und der Ordner Speicherort der Datei "index. cshtml" sind wichtig und folgen den standardmäßigen ASP.NET-MVC-Benennungs Konventionen. Der Verzeichnisname \views\home entspricht dem Controller mit dem Namen HomeController. Der Ansichts Vorlagen Name, Index, entspricht der Controller Aktionsmethode, die die Ansicht anzeigt.

ASP.NET MVC ermöglicht uns, den Namen oder den Speicherort einer Ansichts Vorlage nicht explizit anzugeben, wenn diese Benennungs Konvention verwendet wird, um eine Ansicht zurückzugeben. Standardmäßig wird die Ansichts Vorlage \views\home\index.cshtml gerenden, wenn wir Code wie unten im HomeController schreiben:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer hat die Ansichts Vorlage "index. cshtml" erstellt und geöffnet, nachdem wir im Dialogfeld "Ansicht hinzufügen" auf die Schaltfläche "hinzufügen" geklickt haben. Der Inhalt der Datei "index. cshtml" ist unten dargestellt.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

In dieser Ansicht wird der Razor-Syntax verwendet, der präziser ist als die Web Forms Ansichts-Engine, die in ASP.net Web Forms und früheren Versionen von ASP.NET MVC verwendet wurde. Die Web Forms Ansichts-Engine ist weiterhin in ASP.NET MVC 3 verfügbar, aber viele Entwickler haben festgestellt, dass die Razor-Ansichts-Engine ASP.net die MVC-Entwicklung wirklich gut passt.

In den ersten drei Zeilen wird der Seitentitel mithilfe von "viewbag. Title" festgelegt. Wir sehen uns an, wie dies in Kürze ausführlicher funktioniert, aber zuerst aktualisieren wir den Text Überschriften Text und zeigen die Seite an. Aktualisieren Sie das Tag &lt;H2&gt; auf "Dies ist die Startseite", wie unten gezeigt.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Das Ausführen der Anwendung zeigt, dass der neue Text auf der Startseite angezeigt wird.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Verwenden eines Layouts für allgemeine Standort Elemente

Die meisten Websites verfügen über Inhalte, die von vielen Seiten gemeinsam genutzt werden: Navigation, Fußzeilen, Logo Bilder, Stylesheet-Verweise usw. Die Razor-Ansichts-Engine vereinfacht diese Verwaltung mithilfe einer Seite namens \_Layout. cshtml, die automatisch für uns im Ordner/views/Shared erstellt wurde.

![](mvc-music-store-part-3/_static/image4.png)

Doppelklicken Sie auf diesen Ordner, um die unten gezeigten Inhalte anzuzeigen.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Der Inhalt unserer einzelnen Ansichten wird mit dem Befehl "@RenderBody()" angezeigt, und alle allgemeinen Inhalte, die außerhalb von angezeigt werden sollen, können dem \_Layout. cshtml-Markup hinzugefügt werden. Wir möchten, dass unser MVC Music Store über einen allgemeinen Header mit Links zu unserer Startseite und dem Store-Bereich auf allen Seiten der Website verfügt. daher fügen wir ihn der Vorlage direkt oberhalb dieser @RenderBody()-Anweisung hinzu.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Aktualisieren des Stylesheets

Die leere Projektvorlage enthält eine sehr optimierte CSS-Datei, die nur Stile zum Anzeigen von Validierungs Meldungen enthält. Unser Designer hat einige zusätzliche CSS-und Bilddateien bereitgestellt, um das Erscheinungsbild für unsere Website zu definieren, sodass wir diese jetzt hinzufügen.

Die aktualisierten CSS-Dateien und-Images sind im Inhaltsverzeichnis von MvcMusicStore-Assets. zip enthalten, das unter [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store)verfügbar ist. Wir wählen beide in Windows-Explorer aus und legen Sie in Visual Web Developer im Inhalts Ordner unserer Projekt Mappe ab, wie unten dargestellt:

![](mvc-music-store-part-3/_static/image5.png)

Sie werden aufgefordert zu bestätigen, dass Sie die vorhandene Datei "Site. CSS" überschreiben möchten. Klicken Sie auf Ja.

![](mvc-music-store-part-3/_static/image6.png)

Der Inhalts Ordner der Anwendung wird nun wie folgt angezeigt:

![](mvc-music-store-part-3/_static/image7.png)

Führen Sie nun die Anwendung aus, und sehen Sie sich an, wie sich unsere Änderungen auf der Startseite ansehen.

![](mvc-music-store-part-3/_static/image8.png)

- Sehen wir uns an, was sich geändert hat: die Index Aktionsmethode von HomeController hat gefunden und die Vorlage \views\home\index.cshtmlview angezeigt, auch wenn der Code "Return View ()" genannt wird, weil unsere Ansichts Vorlage der Standard Benennungs Konvention folgte.
- Auf der Startseite wird eine einfache Willkommensnachricht angezeigt, die in der Ansichts Vorlage "\views\home\index.cshtml" definiert ist.
- Auf der Startseite wird unsere Vorlage "\_Layout. cshtml" verwendet, sodass die Willkommensnachricht im HTML-Layout der Standard Website enthalten ist.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Verwenden eines Modells zum Übergeben von Informationen an unsere Ansicht

Eine Ansichts Vorlage, in der nur hart codiertes HTML angezeigt wird, macht keine sehr interessante Website. Zum Erstellen einer dynamischen Website möchten wir stattdessen Informationen von unseren Controller Aktionen an unsere Ansichts Vorlagen übergeben.

Im Model-View-Controller-Muster bezieht sich der Begriff Model auf Objekte, die die Daten in der Anwendung darstellen. Modell Objekte entsprechen häufig Tabellen in der Datenbank, jedoch nicht.

Controller Aktionsmethoden, die ein "action result" zurückgeben, können ein Modell Objekt an die Ansicht übergeben. Dies ermöglicht es einem Controller, alle Informationen, die erforderlich sind, um eine Antwort zu generieren, ordnungsgemäß zu verpacken und diese Informationen dann an eine Ansichts Vorlage zu übergeben, die zum Generieren der entsprechenden HTML-Antwort verwendet werden soll. Dies ist am einfachsten zu verstehen, wenn Sie in Aktion angezeigt wird. beginnen wir also.

Zunächst erstellen wir einige Modellklassen, die Genres und Alben in unserem Geschäft darstellen. Beginnen wir mit der Erstellung einer Genre Klasse. Klicken Sie mit der rechten Maustaste auf den Ordner "Models" in Ihrem Projekt, wählen Sie die Option "Klasse hinzufügen" aus, und geben Sie der Datei den Namen "Genre.cs".

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Fügen Sie dann der erstellten Klasse eine Eigenschaft für den öffentlichen Zeichen folgen Namen hinzu:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Hinweis: für den Fall, dass Sie sich Fragen, verwendet die {Get; Set;} C#-Notation die automatisch implementierte Eigenschaft "Properties". Dies bietet uns die Vorteile einer Eigenschaft, ohne dass wir ein dahinter liegendes Feld deklarieren müssen.*

Führen Sie als nächstes die gleichen Schritte aus, um eine Album-Klasse (mit dem Namen Album.cs) zu erstellen, die einen Titel und eine Genre-Eigenschaft aufweist:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Nun können wir StoreController so ändern, dass Ansichten verwendet werden, die dynamische Informationen aus unserem Modell anzeigen. Wenn zurzeit zu Demonstrationszwecken unsere Alben basierend auf der Anforderungs-ID benannt wurden, könnten diese Informationen wie in der folgenden Ansicht angezeigt werden.

![](mvc-music-store-part-3/_static/image10.png)

Zunächst ändern wir die Aktion "Store-Details", sodass die Informationen für ein einzelnes Album angezeigt werden. Fügen Sie am Anfang der **storecontrollers** -Klasse eine using-Anweisung hinzu, um den mvcmusicstore. Models-Namespace einzuschließen. Daher müssen Sie nicht jedes Mal, wenn Sie die Album-Klasse verwenden möchten, mvcmusicstore. Models. Album eingeben. Der Abschnitt "using-Direktiven" dieser Klasse sollte nun wie folgt aussehen.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Als nächstes aktualisieren wir die Aktion "Details Controller", sodass anstelle einer Zeichenfolge ein "action result" zurückgegeben wird, wie dies bei der Index Methode von HomeController der Fall war.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Nun können wir die Logik ändern, um ein Album Objekt an die Ansicht zurückzugeben. Später in diesem Tutorial werden die Daten aus einer Datenbank abgerufen – aber zurzeit werden wir "Dummydaten" verwenden, um zu beginnen.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Hinweis: Wenn Sie mit nicht vertraut C#sind, können Sie davon ausgehen, dass die Verwendung von "var" bedeutet, dass die Album-Variable spät gebunden ist. Das ist nicht korrekt – der C# Compiler verwendet den Typrückschluss basierend auf dem, was wir der Variablen zuweisen, um zu bestimmen, ob das Album vom Typ "Album" ist, und die lokale Album Variable als einen albummtyp zu kompilieren, sodass wir die Kompilierzeit Überprüfung und die Visual Studio Code-Editor-Unterstützung erhalten.*

Nun erstellen wir eine Ansichts Vorlage, in der das Album zum Generieren einer HTML-Antwort verwendet wird. Bevor wir dies tun, müssen wir das Projekt so erstellen, dass das Dialogfeld "Ansicht hinzufügen" die neu erstellte Album-Klasse kennt. Sie können das Projekt erstellen, indem Sie das Menü Element Debuggen ⇨ Build mvcmusicstore auswählen (um das Projekt mit der Tastenkombination STRG + UMSCHALT + B zu erstellen).

![](mvc-music-store-part-3/_static/image11.png)

Nachdem wir nun unsere unterstützenden Klassen eingerichtet haben, sind wir bereit, unsere Ansichts Vorlage zu erstellen. Klicken Sie mit der rechten Maustaste in die Detail Methode, und wählen Sie "Ansicht hinzufügen" aus. über das Kontextmenü.

![](mvc-music-store-part-3/_static/image12.png)

Wir erstellen eine neue Ansichts Vorlage wie zuvor mit dem HomeController. Da wir Sie aus dem StoreController erstellen, wird Sie standardmäßig in der Datei "\views\store\index.cshtml" generiert.

Anders als zuvor aktivieren wir das Kontrollkästchen "eine stark typisierte Ansicht erstellen". Dann wählen wir unsere "Album"-Klasse in der Dropdown Liste "Datenklasse anzeigen" aus. Dadurch wird das Dialogfeld "Ansicht hinzufügen" dazu geführt, eine Ansichts Vorlage zu erstellen, die erwartet, dass ein Album-Objekt zur Verwendung an Sie übermittelt wird.

![](mvc-music-store-part-3/_static/image13.png)

Wenn wir auf die Schaltfläche "Add" (hinzufügen) klicken, wird die Ansichts Vorlage "\views\store\details.cshtml" erstellt, die den folgenden Code enthält.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Beachten Sie die erste Zeile, die angibt, dass diese Ansicht stark typisiert ist. Die Razor-Ansichts-Engine erkennt, dass ihr ein Album-Objekt weitergereicht wurde, sodass wir leicht auf Modell Eigenschaften zugreifen können und sogar den Vorteil von IntelliSense im Visual Web Developer-Editor haben.

Aktualisieren Sie das Tag &lt;H2&gt;, sodass die Title-Eigenschaft des Albums angezeigt wird, indem Sie diese Zeile so ändern, dass Sie wie folgt aussieht.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Beachten Sie, dass IntelliSense ausgelöst wird, wenn Sie den Zeitraum nach dem @Model-Schlüsselwort eingeben, das die Eigenschaften und Methoden anzeigt, die die Album-Klasse unterstützt.

Nun führen wir das Projekt erneut aus und besuchen die URL/Store/Details/5. Wir sehen uns die Details eines Albums wie unten dargestellt an.

![](mvc-music-store-part-3/_static/image14.png)

Nun erstellen wir ein ähnliches Update für die Aktionsmethode "Store durchsuchen". Aktualisieren Sie die Methode so, dass Sie ein Aktions Ergebnis zurückgibt, und ändern Sie die Methoden Logik, damit ein neues Genre Objekt erstellt und an die Ansicht zurückgegeben wird.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Klicken Sie mit der rechten Maustaste in die Browse-Methode, und wählen Sie "Ansicht hinzufügen..." Fügen Sie im Kontextmenü eine Ansicht hinzu, die stark typisiert ist, fügen Sie der Genre Klasse einen stark typisierten hinzu.

![](mvc-music-store-part-3/_static/image15.png)

Aktualisieren Sie das Element &lt;H2&gt; im Ansichts Code (in/views/Store/Browse.cshtml), um die Genre Informationen anzuzeigen.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Führen Sie nun das Projekt erneut aus, und navigieren Sie zu/Store/Browse? Genre = Disco-URL. Wir sehen, dass die Seite zum Durchsuchen wie unten dargestellt angezeigt wird.

![](mvc-music-store-part-3/_static/image16.png)

Zum Schluss erstellen wir ein etwas komplexeres Update der Aktionsmethode und der Ansicht des **Speicher Indexes** , um eine Liste aller Genres in unserem Geschäft anzuzeigen. Dazu verwenden wir eine Liste der Genres als Modell Objekt und nicht nur ein einzelnes Genre.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Klicken Sie mit der rechten Maustaste in die Aktionsmethode Store-Index, und wählen Sie Ansicht hinzufügen wie zuvor aus, wählen Sie Genre als Modell Klasse aus, und klicken Sie auf die Schaltfläche

![](mvc-music-store-part-3/_static/image17.png)

Zunächst ändern wir die @model Deklaration, um anzugeben, dass die Sicht mehrere Genre Objekte erwartet, anstatt nur eine zu erhalten. Ändern Sie die erste/Store/Index.cshtml Zeile wie folgt:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Dadurch wird der Razor-Ansichts-Engine mitgeteilt, dass Sie mit einem Modell Objekt arbeitet, das mehrere Genre Objekte enthalten kann. Wir verwenden ein IEnumerable-&lt;Genre&gt; anstatt eine Liste&lt;Genre&gt; da es allgemeinerer ist. Dadurch können wir unseren Modelltyp später in einen beliebigen Objekttyp ändern, der die IEnumerable-Schnittstelle unterstützt.

Als nächstes durchlaufen wir die Genre Objekte im Modell, wie im nachfolgenden Code der abgeschlossenen Ansicht gezeigt.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Beachten Sie, dass bei der Eingabe dieses Codes vollständige IntelliSense-Unterstützung vorhanden ist, wenn wir "@Model" eingeben. Wir sehen alle Methoden und Eigenschaften, die von einem IEnumerable-Element vom Typ Genre unterstützt werden.

![](mvc-music-store-part-3/_static/image18.png)

In unserer "foreach"-Schleife weiß Visual Web Developer, dass jedes Element vom Typ Genre ist, sodass für jeden Genre-Typ IntelliSense angezeigt wird.

![](mvc-music-store-part-3/_static/image19.png)

Im nächsten Schritt untersuchte die Gerüstbau Funktion das Genre Objekt und stellte fest, dass jede eine Name-Eigenschaft hat. Sie führt Sie durch und schreibt Sie. Außerdem werden die Links "Bearbeiten", "Details" und "Löschen" für die einzelnen Elemente generiert. Wir nutzen dies später in unserem Store Manager, aber vorerst möchten wir stattdessen eine einfache Liste verwenden.

Wenn wir die Anwendung ausführen und zu/Store navigieren, sehen wir, dass sowohl die Anzahl als auch die Liste der Genres angezeigt werden.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Hinzufügen von Links zwischen Seiten

Unsere/Store-URL, die Genres auflistet, listet derzeit die Genre Namen einfach als nur-Text auf. Wir ändern dies, sodass anstelle von nur-Text die Genre Namen mit der entsprechenden/Store/Browse-URL verknüpft werden, damit das Klicken auf ein Musikgenre wie "Disco" zur/Store/Browse? Genre = Disco-URL navigiert. Wir könnten die Ansichts Vorlage "\views\store\index.cshtml" aktualisieren, um diese Links mithilfe von Code wie unten auszugeben **(geben Sie diesen Link nicht ein, da wir ihn verbessern werden)** :

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Dies funktioniert, kann aber später zu Problemen führen, da es auf einer hart codierten Zeichenfolge basiert. Wenn wir beispielsweise den Controller umbenennen wollten, müssten wir den Code suchen, der nach links sucht, die aktualisiert werden müssen.

Ein alternativer Ansatz, den wir verwenden können, ist die Verwendung einer HTML-Hilfsmethode. ASP.NET MVC enthält HTML-Hilfsmethoden, die über den Code der Ansichts Vorlage verfügbar sind, um eine Vielzahl allgemeiner Aufgaben wie diese auszuführen. Die "HTML. Action Link ()"-Hilfsmethode ist eine besonders nützliche Methode und vereinfacht das Erstellen von HTML-&lt;einer&gt; Links und kümmert sich um lästige Details wie das sicherstellen, dass URL-Pfade ordnungsgemäß URL-codiert sind.

HTML. Action Link () verfügt über mehrere verschiedene über Ladungen, die es ermöglichen, so viele Informationen anzugeben, wie Sie für Ihre Verknüpfungen benötigen. Im einfachsten Fall geben Sie nur den Linktext und die Aktionsmethode an, zu der Sie wechseln, wenn auf dem Client auf den Link geklickt wird. Beispielsweise können wir eine Verknüpfung mit der "/Store/" Index ()-Methode auf der Seite "Store-Details" mit dem Linktext "Gehe zum Speicher Index" mithilfe des folgenden Aufrufes aufrufen:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Hinweis: in diesem Fall müssen wir den Controller Namen nicht angeben, da wir einfach eine Verbindung mit einer anderen Aktion innerhalb desselben Controllers herstellen, der die aktuelle Ansicht rendert.*

Unsere Links zur Seite "Durchsuchen" müssen jedoch einen Parameter übergeben. Daher verwenden wir eine andere Überladung der HTML. Action Link-Methode, die drei Parameter annimmt:

- 1. Linktext, in dem der Genre Name angezeigt wird
- 2. Controller Aktionsname (Durchsuchen)
- 3. Routen Parameterwerte und angeben des Namens (Genre) und des Werts (Genre Name)

Im folgenden wird erläutert, wie wir diese Links in die Speicher Index Ansicht schreiben:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Wenn wir nun das Projekt erneut ausführen und auf die/Store/-URL zugreifen, wird eine Liste mit Genres angezeigt. Jedes Genre ist ein Hyperlink – Wenn Sie darauf klicken, gelangen wir zur URL "/Store/Browse? Genre = *[Genre]* ".

![](mvc-music-store-part-3/_static/image3.jpg)

Der HTML-Code für die Genre Liste sieht wie folgt aus:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]

> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-2.md)
> [Weiter](mvc-music-store-part-4.md)
