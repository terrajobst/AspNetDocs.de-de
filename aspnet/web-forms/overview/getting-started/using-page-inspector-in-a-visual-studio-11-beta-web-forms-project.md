---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Verwenden von Seitenprüfung für Visual Studio 2012 in ASP.net Web Forms | Microsoft-Dokumentation
author: rick-anderson
description: Seitenprüfung für Visual Studio 2012 ist ein Webentwicklungs Tool mit einem integrierten Browser. Wählen Sie ein beliebiges Element im integrierten Browser aus, und Seitenprüfung...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: c165bbea505b4cb8eae1312cdd587f4ed36541a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78464943"
---
# <a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Verwenden der Seitenprüfung für Visual Studio 2012 in ASP.NET Web Forms

von Tim Ammann

> Seitenprüfung für Visual Studio 2012 ist ein Webentwicklungs Tool mit einem integrierten Browser. Wählen Sie ein beliebiges Element im integrierten Browser aus, und Seitenprüfung die Quelle und das CSS des Elements sofort hervorheben. Sie können eine beliebige Seite in der Anwendung durchsuchen, schnell nach den Quellen des gerenderten Markups suchen und die Browser Tools direkt in der Visual Studio-Umgebung verwenden.
> 
> In diesem Tutorial wird gezeigt, wie Sie Überprüfungsmodus aktivieren und dann schnell CSS-Regeln und Text in Ihrem Webprojekt suchen und bearbeiten können. In diesem Tutorial wird ein Web Forms Anwendungsprojekt verwendet, Sie können jedoch auch Seitenprüfung für Website Projekte und [MVC](https://go.microsoft.com/?linkid=9802002) -Anwendungen verwenden.
> 
> Das Tutorial enthält die folgenden Abschnitte:
> 
> [Erforderliche Komponenten](#_1_prerequisites)
> 
> [Erstellen einer Webanwendung](#_2_creating_a)
> 
> [Verwenden Sie Seitenprüfung, um die Anwendung anzuzeigen.](#_3_using_page)
> 
> [Aktivieren von Überprüfungsmodus](#_4_inspection_mode)
> 
> [Verwenden von Seitenprüfung zum vornehmen von Änderungen an Markup](#_5_using_page)
> 
> [Überprüfungsmodus und das HTML-Fenster](#_6_inspection_mode)
> 
> [Anzeigen einer Vorschau von CSS-Änderungen im Fenster "Stile"](#_7_previewing_css)
> 
> [Automatische CSS-Synchronisierung](#css_auto_sync)
> 
> [Verwenden der CSS-Farbauswahl](#css_color_picker)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Voraussetzungen

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) oder [Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Um die neueste Version von Seitenprüfung zu erhalten, verwenden Sie den [Webplattform-Installer](https://go.microsoft.com/fwlink/?LinkId=255386) , um das Azure SDK für .NET 2,0 zu installieren.

Seitenprüfung ist mit Microsoft Web Developer Tools gebündelt. Die neueste Version ist 1,3. Um zu überprüfen, welche Version Sie besitzen, führen Sie Visual Studio aus, und wählen **Microsoft Visual Studio** Sie im Menü **Hilfe** die Option Info aus.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Erstellen einer Webanwendung

Zuerst erstellen Sie eine Webanwendung, die Sie Seitenprüfung mit verwenden. Wählen Sie in Visual Studio **Datei** &gt; **Neues Projekt**aus. Erweitern Sie auf der linken **Seite C#Visualisierung** , wählen Sie **Web**aus, und wählen Sie dann **ASP.net Web Forms Anwendung**aus.

![Neue Web Forms Anwendung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Klicken Sie auf **OK**.

Die Anwendung wird in der **Quell** Ansicht geöffnet.

![Neue Web Forms Anwendung in der Quell Ansicht](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Nachdem Sie nun über eine Anwendung verfügen, mit der Sie arbeiten können, können Sie Seitenprüfung verwenden, um Sie zu überprüfen und zu ändern.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Verwenden Sie Seitenprüfung, um die Anwendung anzuzeigen.

Im nächsten Schritt wird die Anwendung mit Seitenprüfung angezeigt. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie dann **in Seitenprüfung anzeigen aus**.

![In Seitenprüfung anzeigen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Wenn Seitenprüfung zum ersten Mal gestartet wird, wird es standardmäßig auf der linken Seite der Visual Studio-Umgebung als schmale Fenster angedockt. Lassen Sie Sie auf der linken Seite angedockt, legen Sie Sie auf eine für Sie bequeme Breite fest, oder docken Sie Sie in einem der Tool Bereiche oben, unten oder rechts an:

![Andock Positionen Seitenprüfung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Wenn Sie das Seitenprüfung Fenster abdocken, können Sie es außerhalb von Visual Studio oder sogar auf einem zweiten Monitor platzieren. Wenn das Seitenprüfung Fenster nicht angedockt ist, **wechseln Sie zu Extras &gt;** **Optionen** &gt; **Umgebung** &gt; **Registerkarten und Fenster**, um zwischen Seitenprüfung und Visual Studio die Tab-Taste + Tab-Taste zu wechseln, und deaktivieren Sie unter " **Tab**" das Kontrollkästchen " **Floating Tool Windows immer" im oberen Bereich des Hauptfensters**:

![Deaktivieren Sie das Kontrollkästchen "Floating Tool Windows" in Alt + Tab zwischen Visual Studio und dem nicht angedockten Seitenprüfung Fenster](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Im oberen Bereich des Fensters Seitenprüfung wird die aktuelle Seite in einem Browserfenster angezeigt. Der untere Bereich zeigt die Seite im HTML-Markup auf der linken Seite und einige Registerkarten auf der rechten Seite an, mit denen Sie verschiedene Aspekte der Seite überprüfen können. Der untere Bereich ähnelt dem [Entwicklertools F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer. (Im Gegensatz zu den Entwicklertools können Sie jedoch Seitenprüfung direkt in Visual Studio verwenden.)

![Seitenprüfung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

In diesem Tutorial verwenden Sie den Seitenprüfung Browserbereich und die Registerkarten **HTML** und **Stile** , um Sie bei der schnellen Navigation und Änderung der Anwendung zu unterstützen.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Aktivieren von Überprüfungsmodus

Als nächstes sehen Sie, wie Seitenprüfung Überprüfungsmodus funktioniert. Klicken Sie im Seitenprüfung Fenster auf die Schaltfläche über **prüfen** .

![Element überprüfen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Um den Inspektions Modus in Aktion zu sehen, bewegen Sie den Mauszeiger über verschiedene Teile der Seite im Seitenprüfung Browserfenster. Wie Sie dies tun, ändert sich der Mauszeiger in ein großes Pluszeichen, und das untergeordnete Element wird hervorgehoben:

![Zeigen Sie mit dem Mauszeiger auf div. Content-Wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Wenn Sie den Mauszeiger bewegen, beachten Sie, dass

- Der Inhalt in der **Quell** Ansicht wird geändert, um das Markup anzuzeigen, das dem ausgewählten Element auf der Seite entspricht. Das relevante Markup wird hervorgehoben. Wenn sich die Quelle in einer anderen Datei befindet, wird diese Datei in der Quell Ansicht geöffnet, wobei das relevante Markup hervorgehoben ist.

- Das Markup, das auf der Registerkarte **HTML** in Seitenprüfung angezeigt wird, ändert sich auch entsprechend dem ausgewählten Element auf der Seite. Auf der Registerkarte **HTML** wird das relevante Markup aufgeführt.

- Die Registerkarte **Stile** zeigt die CSS-Regeln an, die für die aktuelle Auswahl relevant sind.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Verwenden von Seitenprüfung zum vornehmen von Änderungen an Markup

Nun sehen Sie, wie Sie mithilfe von Seitenprüfung Markup oder Text suchen und Änderungen vornehmen können, deren Speicherort möglicherweise nicht sofort ersichtlich ist.

Legen Sie Seitenprüfung in Überprüfungsmodus, und Scrollen Sie zum unteren Rand der Startseite.

Sobald Sie den Footerbereich eingeben, öffnet Seitenprüfung die Layoutdatei " *Site. Master* " in der **Quell** Ansicht auf einer temporären Registerkarte rechts von den anderen Registerkarten und hebt den Abschnitt der von Ihnen ausgewählten Master Seite hervor. Dadurch wird gezeigt, wie Seitenprüfung Inhalt auf einer Seite suchen und anzeigen können, die tatsächlich aus einer anderen als der ursprünglich geöffneten Datei stammt.

![Footer-Highlights in Überprüfungsmodus](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Bewegen Sie den Mauszeiger im Seitenprüfung Browserfenster über die Zeile mit dem Urheberrechts <a id="a"> </a>Hinweis.

Auf der Seite *Site. Master* wird die entsprechende Zeile hervorgehoben.

![Vorder-und-Fußzeile hervorgehoben](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Fügen Sie am Ende der Zeile in der Datei " *Site. Master* " Text hinzu.

&lt;p&gt;&amp;kopieren; &lt;%: DateTime. now. Year%&gt;-My ASP.NET Application Rocks!&lt;/p&gt;

Drücken Sie nun Strg + Alt + Eingabe, oder klicken Sie auf die Update Leiste, um die Ergebnisse im Seitenprüfung Browserfenster anzuzeigen.

![My ASP.NET Application Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Möglicherweise haben Sie sich der Fußzeile auf der Seite " *default. aspx* " befunden, sich jedoch auf der Seite "Master Layout" befunden und Seitenprüfung für Sie gefunden.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Überprüfungsmodus und das HTML-Fenster

Im nächsten Schritt sehen Sie sich das HTML-Fenster und die Art der Zuordnung von Elementen an.

Legen Sie Seitenprüfung in Überprüfungsmodus.

![Element überprüfen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Klicken Sie auf den oberen Teil der Seite, der "Ihr Logo hier" anzeigt. Sie untersuchen ein bestimmtes Element ausführlicher, sodass sich die Anzeige im Browserfenster nicht mehr ändert, wenn Sie den Mauszeiger bewegen.

Bewegen Sie nun den Mauszeiger auf das **HTML** -Fenster. Wenn Sie den Mauszeiger bewegen, wird Seitenprüfung das Element innerhalb des **HTML** -Fensters umrissen und das entsprechende Element im Browserfenster hervorgehoben.

![HTML-Fenster](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Wie zuvor öffnet Seitenprüfung die Datei " *Site. Master* " für Sie auf einer temporären Registerkarte. Klicken Sie auf die Registerkarte "Site. Master", und das entsprechende Markup wird im&gt; Abschnitt &lt;Header hervorgehoben:

![Hervorgehobene Markup](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Anzeigen einer Vorschau von CSS-Änderungen im Fenster "Stile"

Als Nächstes erfahren Sie, wie Sie das Fenster Seitenprüfung **Stile** zum Anzeigen einer Vorschau von Änderungen an CSS verwenden können.

Klicken Sie auf die Schaltfläche über **prüfen** , um Seitenprüfung in Überprüfungsmodus einzufügen.

Bewegen Sie im Seitenprüfung Browserfenster den Mauszeiger über den Abschnitt "Home Page", bis die **div. Content-Wrapper-** Bezeichnung angezeigt wird.

![Zeigen auf Elemente](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Klicken Sie einmal in den Abschnitt div. Content-Wrapper, und bewegen Sie den Mauszeiger auf das Fenster **Stile** . Deaktivieren Sie das Kontrollkästchen für die Background-Color-Eigenschaft unter der. Featured. Content-Wrapper-Klasse, und aktivieren Sie das Kontrollkästchen.

![Hintergrundfarbe löschen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Beachten Sie, dass die Änderungs Vorschau sofort im Seitenprüfung Browserfenster angezeigt wird.

Aktivieren Sie das Kontrollkästchen erneut, doppelklicken Sie auf den Eigenschafts Wert, und ändern Sie ihn in `red`. Die Änderung wird sofort angezeigt:

![Rote Hintergrundfarbe](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

Das Fenster **Stile** vereinfacht das Testen und die Vorschau von CSS-Änderungen, bevor Sie die Änderungen im Stylesheet selbst übertragen.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Automatische CSS-Synchronisierung

> [!NOTE]
> Diese Funktion erfordert die Version 1,3 von Seitenprüfung.

Mit der Funktion "Automatische CSS-Synchronisierung" können Sie eine CSS-Datei direkt bearbeiten und die Änderungen sofort im Seitenprüfung Browser anzeigen.

Klicken Sie auf über **prüfen** , um Seitenprüfung in Überprüfungsmodus einzufügen.

Bewegen Sie im Seitenprüfung Browser den Mauszeiger über den Abschnitt "Home Page", bis die **div. Content-Wrapper-** Bezeichnung angezeigt wird. Klicken Sie einmal, um dieses Element auszuwählen.

Im Fenster **Stile** werden alle CSS-Regeln für dieses Element angezeigt. Scrollen Sie nach unten, um die. Featured. Content-Wrapper-Klassenauswahl zu suchen. Klicken Sie auf ". Featured. Content-Wrapper". Seitenprüfung öffnet die CSS-Datei, mit der dieser Stil (Site. CSS) definiert wird, und hebt den entsprechenden CSS-Stil hervor.

![CSS-Datei](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Ändern Sie nun den Wert für `background-color` in "rot". Die Änderung wird sofort im Seitenprüfung Browser angezeigt.

![Seitenprüfung Browser](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Verwenden der CSS-Farbauswahl

Als Nächstes erfahren Sie, wie Sie mit Seitenprüfung das CSS für markierten Text in der Standardanwendung schnell finden und ändern können. In diesem Beispiel haben Sie festgelegt, dass Sie die blaue Hervorhebung nicht wünschen und Sie in eine andere Farbe ändern möchten.

Klicken Sie auf die Schaltfläche **Suchen** .

![Element überprüfen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Bewegen Sie im Seitenprüfung Browserfenster den Mauszeiger über den markierten Text "Videos, Tutorials und Beispiele", sodass die CSS-Bezeichnung "Mark" angezeigt wird.

![Zeigen auf das Markierungs Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Klicken Sie auf den Text, um ihn auszuwählen. Die entsprechende CSS-Markierung wird unten im Fenster " **Stile** " angezeigt.

![Markierungs Auswahl im Fenster "Stile"](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Klicken Sie auf die Markierungs Auswahl. Dadurch wird die Datei " *Site. CSS* " für die Webanwendung geöffnet. Klicken Sie auf die Registerkarte Site. CSS, und das entsprechende CSS für den Selektor wird hervorgehoben:

![Markierungs Auswahl im Stylesheet](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Wählen Sie die Zeile mit der Eigenschaft background-color aus, und entfernen Sie Sie.

Nun verwenden Sie die neue Visual Studio 2012-CSS-Farbauswahl, um eine neue Farbe für die Eigenschaft "Hintergrundfarbe **Markieren** " auszuwählen.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Verwenden der CSS-Farbauswahl in Visual Studio 2012

Der CSS-Editor in Visual Studio 2012 verfügt über eine Farbauswahl, mit der Farben leicht ausgewählt und eingefügt werden können. Es verfügt über eine einfache Farbleiste und eine "Popdown"-Auswahl, die eine präzisere Steuerung bietet.

Die Farbauswahl enthält eine Standardpalette von Farben, unterstützt Standardfarben, Hashcodes, RGB-, RGBA-, HSL-und HSLA-Farben und verwaltet eine Liste der Farben, die Sie zuletzt im Dokument verwendet haben.

Geben Sie in der Zeile, in der die Background-Color-Eigenschaft war, "BC" ein, und drücken Sie den Pfeil nach unten.

Wenn Sie das erste Zeichen jedes Worts in eine durch Bindestriche getrennte Eigenschaft wie "Background-Color" eingeben, filtert IntelliSense die Liste so, dass nur die Eigenschaften angezeigt werden, die mit den folgenden Eigenschaften identisch sind:

![Gefilterte IntelliSense-Werte](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Geben Sie nun einen Doppelpunkt ein. Wenn Sie dies tun, wird der vollständige Eigenschaften Name für die Hintergrundfarbe eingefügt. Geben Sie **#** oder **RGB (** ) ein, und die Farbauswahl Leiste wird angezeigt:

![Die CSS-Farbauswahl Leiste](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Um zu sehen, wie die Farbauswahl Leiste funktioniert, klicken Sie mit dem Mauszeiger auf die Farben, oder drücken Sie die nach-unten-Taste, und verwenden Sie dann die Pfeiltasten nach links und nach rechts, um die Farben zu durch Wenn Sie eine Farbe aufrufen, wird der entsprechende Wert für die Eigenschaft background-color in der Vorschau angezeigt:

![Vorschau der Eigenschaft "Background-Color" in der Vorschau](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

An diesem Punkt können Sie die EINGABETASTE drücken, um den Wert auszuwählen, und dann ein Semikolon (;) zum Vervollständigen des CSS-Eintrags. Fahren Sie mit dem nächsten Abschnitt fort, damit Sie sehen können, wie das Popup Fenster für die Farbauswahl funktioniert.

#### <a name="using-the-color-picker-pop-down"></a>Verwenden des Popups für die Farbauswahl

Wenn die Farbleiste nicht die genaue Farbe hat, nach der Sie suchen, können Sie das Popup Fenster für die Farbauswahl verwenden.

Um es zu öffnen, klicken Sie auf das doppelte Chevron am rechten Ende der Farbleiste, oder drücken Sie auf der Tastatur einmal oder zweimal den Pfeil nach unten.

![CSS-Farbauswahl-Popup](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Klicken Sie auf eine Farbe aus der vertikalen Leiste auf der rechten Seite. Dies zeigt einen Farbverlauf für diese Farbe im Hauptfenster. Wählen Sie eine Farbe direkt aus der vertikalen Leiste aus, indem Sie die EINGABETASTE drücken, oder klicken Sie auf einen beliebigen Punkt im Hauptfenster, um eine höhere Genauigkeit auszuwählen.

Wenn auf dem Computerbildschirm eine Farbe vorhanden ist, die Sie verwenden möchten (Sie muss sich nicht in der Visual Studio-Benutzeroberfläche befinden), können Sie Ihren Wert mit dem Tool "Pipette" unten rechts erfassen.

Sie können auch die Deckkraft einer Farbe ändern, indem Sie den Schieberegler am unteren Rand der Farbauswahl verschieben. Dadurch werden Farbwerte in RGBA-Werte geändert, da das RGBA-Format eine Deckkraft darstellen kann.

Nachdem Sie eine Farbe ausgewählt haben, drücken Sie die EINGABETASTE, und geben Sie dann ein Semikolon ein, um den Hintergrund Farb Eintrag in der Datei " *Site. CSS* " abzuschließen.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Die Seitenprüfung Update Leiste

Seitenprüfung erkennt sofort die Änderung an der Datei " *Site. CSS* " (oder in einer beliebigen Datei in der Anwendung) und zeigt eine Warnung in einer Update Leiste an.

![Update Leiste](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Um alle Dateien zu speichern und den Seitenprüfung Browser zu aktualisieren, drücken Sie Strg + Alt + Eingabe, oder klicken Sie auf die Update Leiste. Die Änderung in der Hervorhebungs Farbe wird im Browser angezeigt:

![Hervorhebungs Farbe geändert](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Beachten Sie, dass Sie den Seitenprüfung Browser direkt in der Visual Studio-Umgebung aktualisiert haben. Wenn Sie Seitenprüfung anstelle eines externen Browsers verwenden, können Sie im Editor bleiben, wenn Sie Ihre Webanwendungen entwickeln.

## <a name="see-also"></a>Weitere Informationen

[Einführung in Seitenprüfung](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9-Video)

[Seitenprüfung-Fehlermeldungen](https://go.microsoft.com/?linkid=9813062) (MSDN)
