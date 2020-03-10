---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Verwenden von Seitenprüfung in ASP.NET MVC | Microsoft-Dokumentation
author: rick-anderson
description: Seitenprüfung in Visual Studio 2012 ist ein Webentwicklungs Tool mit einem integrierten Browser. Wählen Sie ein beliebiges Element im integrierten Browser aus, und Seitenprüfung...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5da3e142c52a770f59222c21d9f9a53cbbdbf498
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78432453"
---
# <a name="using-page-inspector-in-aspnet-mvc"></a>Verwenden der Seitenprüfung in ASP.NET MVC

von Tim Ammann

> Seitenprüfung in Visual Studio 2012 ist ein Webentwicklungs Tool mit einem integrierten Browser. Wählen Sie ein beliebiges Element im integrierten Browser aus, und Seitenprüfung die Quelle und das CSS des Elements sofort hervorheben. Sie können eine beliebige MVC-Ansicht durchsuchen, schnell nach den Quellen des gerenderten Markups suchen und die Browser Tools direkt in der Visual Studio-Umgebung verwenden.
> 
> [Video ansehen](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> In diesem Tutorial wird gezeigt, wie Sie Überprüfungsmodus aktivieren und dann schnell Markup und CSS innerhalb des Webprojekts suchen und bearbeiten können. Im Tutorial wird ein MVC-Projekt verwendet, aber Sie können auch Seitenprüfung für [Web Forms](https://go.microsoft.com/?linkid=9802001) und andere ASP.NET-Anwendungen verwenden.
> 
> Das Tutorial enthält die folgenden Abschnitte:
> 
> - [Erforderliche Komponenten](#_1_prerequisites)
> - [Erstellen einer Webanwendung](#_2_creating_a)
> - [Verwenden Sie Seitenprüfung, um zu einer Ansicht zu navigieren.](#_3_using_page)
> - [Aktivieren von Überprüfungsmodus](#_4_inspection_mode)
> - [Verwenden von Seitenprüfung zum vornehmen von Änderungen an Markup](#_5_using_page)
> - [Überprüfungsmodus und das HTML-Fenster](#_6_inspection_mode)
> - [Anzeigen einer Vorschau von CSS-Änderungen im Fenster "Stile"](#_7_previewing_css)
> - [Automatische CSS-Synchronisierung](#css_auto_sync)
> - [Verwenden der CSS-Farbauswahl](#css_color_picker)
> - [Zuordnung dynamischer Seitenelemente zu JavaScript](#map_dynamic_elements)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Voraussetzungen

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) oder [Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Um die neueste Version von Seitenprüfung zu erhalten, verwenden Sie den [Webplattform-Installer](https://go.microsoft.com/fwlink/?LinkId=255386) , um das Windows Azure SDK für .NET 2,0 zu installieren.

Seitenprüfung ist mit Microsoft Web Developer Tools gebündelt. Die neueste Version ist 1,3. Um zu überprüfen, welche Version Sie besitzen, führen Sie Visual Studio aus, und wählen **Microsoft Visual Studio** Sie im Menü **Hilfe** die Option Info aus.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Erstellen einer Webanwendung

Erstellen Sie zunächst eine Webanwendung, die Sie mit Seitenprüfung verwenden werden. Wählen Sie in Visual Studio **Datei** &gt; **Neues Projekt**aus. Erweitern Sie auf der linken **Seite C#Visualisierung** , wählen Sie **Web**aus, und wählen Sie dann **ASP.net MVC4 Webanwendung**aus.

![Neue ASP.NET-MVC-Anwendung](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Klicken Sie auf **OK**.

Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Option **Internet Anwendung**aus. Belassen Sie **Razor** als Standard Ansichts-Engine.

![Neues ASP.NET MVC-Projekt-Internet Anwendung](using-page-inspector-in-aspnet-mvc/_static/image4.png)

Die Anwendung wird in der **Quell** Ansicht geöffnet.

![Neue ASP.NET MVC-Anwendung in der Quell Ansicht](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Nachdem Sie nun über eine Anwendung verfügen, mit der Sie arbeiten können, können Sie Seitenprüfung verwenden, um Sie zu überprüfen und zu ändern.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Verwenden Sie Seitenprüfung, um zu einer Ansicht zu navigieren.

In Visual Studio 2012 können Sie mit der rechten Maustaste auf eine beliebige Ansicht in Ihrem Projekt klicken, **in Seitenprüfung anzeigen**auswählen und Seitenprüfung die Route herausfinden und die Seite anzeigen.

Erweitern Sie in **Projektmappen-Explorer**den Ordner " **views** " und dann den Ordner " **Home** ". Klicken Sie mit der rechten Maustaste auf die Datei index. cshtml, und wählen Sie **in Seitenprüfung anzeigen**.

![Index. cshtml in Seitenprüfung anzeigen](using-page-inspector-in-aspnet-mvc/_static/image8.png)

Standardmäßig ist Seitenprüfung als Fenster auf der linken Seite der Visual Studio-Umgebung angedockt. Wenn Sie es vorziehen, können Sie es an anderer Stelle andocken oder das Fenster Abdocken. Siehe Gewusst [wie: anordnen und Andocken von Fenstern](https://msdn.microsoft.com/library/z4y0hsax.aspx).

Im oberen Bereich des Fensters Seitenprüfung wird die aktuelle Seite in einem Browserfenster angezeigt. Im unteren Bereich wird die Seite im HTML-Markup angezeigt, zusammen mit einigen Registerkarten, mit denen Sie verschiedene Aspekte der Seite untersuchen können. Der untere Bereich ähnelt dem [Entwicklertools F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.

![ASP.NET MVC-Anwendung in Seitenprüfung](using-page-inspector-in-aspnet-mvc/_static/image10.png)

In diesem Tutorial verwenden Sie die Registerkarten **HTML** und **Stile** , um schnell zu navigieren und Änderungen an der Anwendung vorzunehmen.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>Enableinspection-Modus

Um Seitenprüfung in Überprüfungsmodus einzufügen, klicken Sie auf die Schaltfläche über **prüfen** . Wenn Sie in Überprüfungsmodus den Mauszeiger über einen Teil der gerenderten Seite halten, wird das entsprechende Quell Markup oder der Code hervorgehoben.

![Überprüfungsmodus umschalten](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Bewegen Sie nun den Mauszeiger über verschiedene Teile der Seite in Seitenprüfung. Wie Sie dies tun, ändert sich der Mauszeiger in ein großes Pluszeichen, und das untergeordnete Element wird hervorgehoben:

![Zeigen Sie mit dem Mauszeiger auf div. Content-Wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

Wenn Sie den Mauszeiger bewegen, werden die entsprechenden Razor-Syntax in der Quelldatei von Visual Studio hervorgehoben. Wenn das HTML-Element aus einer anderen Quelldatei stammt, wird die Datei automatisch von Visual Studio geöffnet.

In Seitenprüfung zeigt die Registerkarte **HTML** den HTML-Code an, der aus dem Razor-Syntax generiert wurde. Wenn Sie den Mauszeiger bewegen, werden die HTML-Elemente hervorgehoben. Auf der Registerkarte **Stile** werden die CSS-Regeln für das-Element angezeigt.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Verwenden von Seitenprüfung zum vornehmen von Änderungen an Markup

Mit Seitenprüfung können Sie Markup suchen, dessen Speicherort möglicherweise nicht offensichtlich ist. Anschließend können Sie das Markup ändern und die resultierenden Änderungen anzeigen.

Um dies zu sehen, klicken Sie auf über **prüfen** , und Scrollen Sie dann im Seitenprüfung Fenster zum unteren Rand der Seite.

Wenn Sie den Mauszeiger in den Footerbereich bewegen, öffnet Seitenprüfung die Datei \_Layout. cshtml und hebt den Abschnitt der Layoutseite hervor, die Sie ausgewählt haben. Wie Sie sehen, wird der Fußzeile in der Layoutdatei und nicht in der Ansicht selbst definiert.

![Fußzeile](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Bewegen Sie nun den Mauszeiger über die Zeile mit dem <a id="a"> </a>Urheberrechts Hinweis. Auf der Seite \_Layout. cshtml wird die entsprechende Zeile hervorgehoben.

![Vorder-und-Fußzeile hervorgehoben](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Fügen Sie am Ende der Zeile in der Datei \_Layout. cshtml Text hinzu.

&lt;p&gt;&amp;kopieren; @DateTime.Now.Year-My ASP.NET MVC-Anwendungs Felsen!&lt;/p&gt;

Drücken Sie nun Strg + Alt + Eingabe, oder klicken Sie auf die Update Leiste, um die Ergebnisse im Seitenprüfung Browserfenster anzuzeigen.

![My ASP.NET Application Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Sie haben vielleicht schon einmal gedacht, dass der Fußzeile in der Datei "index. cshtml" definiert ist, sich jedoch im \_"Layout. cshtml" befunden hat und Seitenprüfung ihn für Sie gefunden hat.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Überprüfungsmodus und das HTML-Fenster

Im nächsten Schritt sehen Sie sich das HTML-Fenster und die Art der Zuordnung von Elementen an.

Klicken Sie auf über **prüfen** , um Seitenprüfung in Überprüfungsmodus einzufügen.

Klicken Sie auf den oberen Teil der Seite, der "Ihr Logo hier" anzeigt. Sie untersuchen ein bestimmtes Element ausführlicher, sodass sich die Anzeige im Browserfenster nicht mehr ändert, wenn Sie den Mauszeiger bewegen.

Bewegen Sie nun den Mauszeiger auf das **HTML** -Fenster. Wenn Sie den Mauszeiger bewegen, wird Seitenprüfung das Element innerhalb des **HTML** -Fensters umrissen und das entsprechende Element im Browserfenster hervorgehoben.

![HTML-Fenster](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Wie zuvor öffnet Seitenprüfung die Datei "\_Layout. cshtml" für Sie auf einer temporären Registerkarte. Klicken Sie auf die Registerkarte "\_Layout. cshtml", und das entsprechende Markup wird im Abschnitt &lt;Header&gt; für Sie hervorgehoben:

![Hervorgehobene Markup](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Anzeigen einer Vorschau von CSS-Änderungen im Fenster "Stile"

Im nächsten Schritt verwenden Sie das Fenster Seitenprüfung **Stile** , um eine Vorschau der Änderungen an CSS anzuzeigen.

Klicken Sie auf über **prüfen** , um Seitenprüfung in Überprüfungsmodus einzufügen.

Bewegen Sie im Seitenprüfung Browserfenster den Mauszeiger über den Abschnitt "Home Page", bis die **div. Content-Wrapper-** Bezeichnung angezeigt wird.

![Zeigen Sie mit dem Mauszeiger auf div. Content-Wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Klicken Sie einmal in den Abschnitt div. Content-Wrapper, und bewegen Sie den Mauszeiger auf das Fenster **Stile** . Im Fenster **Stile** werden alle CSS-Regeln für dieses Element angezeigt. Scrollen Sie nach unten, um die. Featured. Content-Wrapper-Klassenauswahl zu suchen. Deaktivieren Sie nun das Kontrollkästchen für die Eigenschaft Hintergrundfarbe.

![Hintergrundfarbe löschen](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Beachten Sie, dass die Änderungs Vorschau sofort im Seitenprüfung Browserfenster angezeigt wird.

Aktivieren Sie das Kontrollkästchen erneut, doppelklicken Sie auf den Eigenschafts Wert, und ändern Sie ihn in rot. Die Änderung wird sofort angezeigt:

![Rote Hintergrundfarbe](using-page-inspector-in-aspnet-mvc/_static/image30.png)

Das Fenster **Stile** vereinfacht das Testen und die Vorschau von CSS-Änderungen, bevor Sie die Änderungen im Stylesheet selbst übertragen.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Automatische CSS-Synchronisierung

> [!NOTE]
> Diese Funktion erfordert die Version 1,3 von Seitenprüfung.

Mit der Funktion "Automatische CSS-Synchronisierung" können Sie eine CSS-Datei direkt bearbeiten und die Änderungen sofort im Seitenprüfung Browser anzeigen.

Klicken Sie auf über **prüfen** , um Seitenprüfung in Überprüfungsmodus einzufügen.

Bewegen Sie im Seitenprüfung Browser den Mauszeiger über den Abschnitt "Home Page", bis die **div. Content-Wrapper-** Bezeichnung angezeigt wird. Klicken Sie einmal, um dieses Element auszuwählen.

Im Fenster **Stile** werden alle CSS-Regeln für dieses Element angezeigt. Scrollen Sie nach unten, um die. Featured. Content-Wrapper-Klassenauswahl zu suchen. Klicken Sie auf ". Featured. Content-Wrapper". Seitenprüfung öffnet die CSS-Datei, mit der dieser Stil (Site. CSS) definiert wird, und hebt den entsprechenden CSS-Stil hervor.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Ändern Sie nun den Wert für `background-color` in "rot". Die Änderung wird sofort im Seitenprüfung Browser angezeigt.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>Verwenden der CSS-Farbauswahl

Der CSS-Editor in Visual Studio 2012 verfügt über eine Farbauswahl, mit der Farben leicht ausgewählt und eingefügt werden können. Die Farbauswahl enthält eine Standardpalette von Farben, unterstützt Standardfarben, Hashcodes, RGB-, RGBA-, HSL-und HSLA-Farben und verwaltet eine Liste der Farben, die Sie zuletzt im Dokument verwendet haben.

Im vorherigen Abschnitt haben Sie den Wert der `background-color`-Eigenschaft geändert. Um die Farbauswahl aufzurufen, platzieren Sie die Einfügemarke nach dem Eigenschaftsnamen, und geben Sie **#** oder **RGB (** ein.

![Die CSS-Farbauswahl Leiste](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Klicken Sie auf eine Farbe, um Sie auszuwählen, oder drücken Sie die nach-unten-Taste, und verwenden Sie dann die Pfeiltasten nach links und nach rechts, um die Farben zu durch Wenn Sie eine Farbe aufrufen, wird der entsprechende Hexadezimalwert in der Vorschau angezeigt:

![Vorschau der Eigenschaft "Background-Color" in der Vorschau](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Wenn die Farbleiste nicht die gewünschte Farbe hat, können Sie das Popup Fenster Farbauswahl verwenden. Um es zu öffnen, klicken Sie auf das doppelte Chevron am rechten Ende der Farbleiste, oder drücken Sie auf der Tastatur einmal oder zweimal den Pfeil nach unten.

![CSS-Farbauswahl-Popup](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Klicken Sie auf eine Farbe aus der vertikalen Leiste auf der rechten Seite. Dies zeigt einen Farbverlauf für diese Farbe im Hauptfenster. Wählen Sie eine Farbe direkt aus der vertikalen Leiste aus, indem Sie die EINGABETASTE drücken, oder klicken Sie auf einen beliebigen Punkt im Hauptfenster, um eine höhere Genauigkeit auszuwählen.

Wenn auf dem Computerbildschirm eine Farbe vorhanden ist, die Sie verwenden möchten (Sie muss sich nicht in der Visual Studio-Benutzeroberfläche befinden), können Sie Ihren Wert mit dem Tool "Pipette" unten rechts erfassen.

Sie können auch die Deckkraft einer Farbe ändern, indem Sie den Schieberegler am unteren Rand der Farbauswahl verschieben. Dadurch werden Farbwerte in RGBA-Werte geändert, da das RGBA-Format eine Deckkraft darstellen kann.

Nachdem Sie eine Farbe ausgewählt haben, drücken Sie die EINGABETASTE, und geben Sie dann ein Semikolon ein, um den Hintergrund Farb Eintrag in der Datei " *Site. CSS* " abzuschließen.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Die Seitenprüfung Update Leiste

Seitenprüfung erkennt die Änderung an der Datei " *Site. CSS* " sofort und zeigt eine Warnung in einer Update Leiste an.

![Update Leiste](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Um alle Dateien zu speichern und den Seitenprüfung Browser zu aktualisieren, drücken Sie Strg + Alt + Eingabe, oder klicken Sie auf die Update Leiste. Die Änderung in der Hervorhebungs Farbe wird im Browser angezeigt.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Zuordnung dynamischer Seitenelemente zu JavaScript

In modernen Webanwendungen werden Elemente auf der Seite häufig dynamisch mit JavaScript generiert. Dies bedeutet, dass kein statisches Markup (HTML oder Razor) vorhanden ist, das diesen Seitenelementen entspricht.

Ab Version 1,3 können Seitenprüfung Elemente, die der Seite dynamisch hinzugefügt wurden, jetzt dem entsprechenden JavaScript-Code zuordnen. Um dieses Feature zu veranschaulichen, verwenden wir die [Vorlage für die Einzelseiten Anwendung (Single Page Application, Spa)](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> Die Spa-Vorlage erfordert das Update [ASP.net and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650) .

Wählen Sie in Visual Studio **Datei** &gt; **Neues Projekt**aus. Erweitern Sie auf der linken **Seite C#Visualisierung** , wählen Sie **Web**aus, und wählen Sie dann **ASP.net MVC4 Webanwendung**aus. Klicken Sie auf **OK**.

Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Option **Single-Page-Anwendung**aus.

Erweitern Sie in Projektmappen-Explorer den Ordner " **views** " und dann den Ordner " **Home** ". Klicken Sie mit der rechten Maustaste auf die Datei index. cshtml, und wählen Sie **in Seitenprüfung anzeigen**.

Das erste, was im Seitenprüfung Browser angezeigt wird, ist eine Anmeldeseite. Klicken Sie auf "registrieren", und erstellen Sie einen Benutzernamen und ein Kennwort. Nachdem Sie sich angemeldet haben, werden Sie von der Anwendung protokolliert, und es wird eine Aufgabenliste mit einigen Beispiel Elementen erstellt.

Klicken Sie auf über **prüfen** , um Seitenprüfung in Überprüfungsmodus einzufügen. Klicken Sie im Seitenprüfung Browser auf eines der to-do-Elemente. Beachten Sie, dass das-Element nicht blau hervorgehoben ist, sondern mit "js" neben dem Elementnamen hervorgehoben ist. Dies gibt an, dass das Element dynamisch über das Skript erstellt wurde.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Außerdem wird ein orangefarbener **Unterstrich auf der Register** Karte "Registerkarte" angezeigt. Dies gibt an, dass der Bereich " **aufrufsstapel** " Weitere Informationen über das Element enthält.

Klicken Sie auf **die Register** Karte "Registerkarte". Der Bereich " **Aufrufe** " zeigt die aufrufsstapel für den JavaScript-Befehl an, der das Element erstellt hat. Aufrufe externer Bibliotheken, z. b. jQuery, werden reduziert, sodass Sie die Aufrufe Ihres Anwendungs Skripts leicht erkennen können.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Um den vollständigen Stapel, einschließlich der Aufrufe externer Bibliotheken, anzuzeigen, können Sie die Knoten mit der Bezeichnung "externe Bibliotheken" erweitern:

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Wenn Sie auf ein Element in der aufrufsstapel klicken, öffnet Visual Studio die Codedatei und hebt das entsprechende Skript hervor.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Weitere Informationen

Einführung [in ASP.NET MVC 4 mit Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.NET-Website)

[Einführung in Seitenprüfung](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9-Video)

[Seitenprüfung-Fehlermeldungen](https://go.microsoft.com/?linkid=9813062) (MSDN)
