---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Neues bei der ASP.net-und Webentwicklung in Visual Studio 2012 | Microsoft-Dokumentation
author: rick-anderson
description: Mit der neuen Version von Visual Studio wird eine Reihe von Verbesserungen eingeführt, die sich auf die Verbesserung der Benutzererfahrung und Leistung bei der Arbeit mit Webtechnologien konzentrieren...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 80c77ec65ed86b06e417d3f6ba608e404c46768b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422097"
---
# <a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>Neue Funktionen für die Webentwicklung mit ASP.NET und Visual Studio 2012

vom [Web Camps-Team](https://twitter.com/webcamps)

> Die neue Version von Visual Studio bietet eine Reihe von Verbesserungen, die sich auf die Verbesserung der Benutzererfahrung und Leistung bei der Arbeit mit Webtechnologien konzentrieren. Die Visual Studio-Editoren für CSS, JavaScript und HTML wurden vollständig überarbeitet, um viele der am häufigsten ausgeführten Code Hilfen wie IntelliSense und automatischen Einzug einzubeziehen. Hinsichtlich der Leistung sind Bündelung und Minimierung nun als integrierte Features integriert, um die Seitenladezeit zu reduzieren.
> 
> Visual Studio ermöglicht es Ihnen, mit den neuesten Website Technologien zu arbeiten. Sie können Browser übergreifende CSS3-Ausschnitte verwenden, um sicherzustellen, dass Ihre Website unabhängig von der Client Plattform funktioniert, während gleichzeitig die neuen HTML5-Elemente und-Features genutzt werden.
> 
> Das Schreiben und die Profilerstellung für JavaScript-Code sollte mit dieser Visual Studio-Version einfacher sein. IntelliSense-Listen, integrierte XML-Dokumentation und Navigations Features sind jetzt für JavaScript-Code verfügbar. Sie haben jetzt den JavaScript-Katalog zur Hand. Außerdem können Sie die ECMAScript5-Konformität mit Ihren Skripts überprüfen und Syntax Fehler in einer frühen Phase erkennen.
> 
> Diese Visual Studio-Version implementiert schließlich die integrierte Bündelung und Minimierung. Ihre Skriptdateien und Stylesheets werden verpackt und komprimiert, sodass die Site schneller ausgeführt wird.
> 
> Diese Übungseinheit führt Sie durch die Verbesserungen und neuen Features, die Sie zuvor beschrieben haben, indem Sie kleinere Änderungen an einer im Quellordner bereitgestellten beispielweb Anwendung anwenden.
> 
> Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das unter [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)verfügbar ist.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In diesem praktischen Übungs Labor lernen Sie Folgendes:

- Verwenden Sie die neuen Features und Verbesserungen im CSS-Editor.
- Verwenden Sie die neuen Features und Verbesserungen im HTML-Editor.
- Verwenden der neuen Features und Verbesserungen im JavaScript-Editor
- Konfigurieren und Verwenden von Bündelung und Minimierung

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Voraussetzungen

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder Superior (Informationen zur Installation finden Sie in [Anhang A](#AppendixA) ).
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (für Setup Skripts, die bereits unter Windows 8 und Windows Server 2008 R2 installiert sind)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) oder ein mit HTML5 kompatibler Browser

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exerzitien

Diese praktische Übung umfasst die folgenden Übungen:

1. [Übung 1: Neuerungen im CSS-Editor](#Exercise1)
2. [Übung 2: Neuerungen im HTML-Editor](#Exercise2)
3. [Übung 3: Neuerungen im JavaScript-Editor](#Exercise3)
4. [Übung 4: bündeln und minimieren](#Exercise4)

Geschätzte Zeit bis zum Abschluss dieses Labs: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Übung 1: Neuerungen im CSS-Editor

Webentwickler sollten mit vielen der Probleme im Zusammenhang mit der CSS-Bearbeitung vertraut sein. Eines der größten Probleme bei CSS-Formaten ist die Browser übergreifende Kompatibilität. Es kommt häufig vor, dass Sie nach dem Anwenden von Stilen auf Ihre Website feststellen, dass es anders aussieht, wenn Sie es in einem anderen Browser oder Gerät öffnen. Daher können Sie viel Zeit für die Korrektur dieser visuellen Probleme aufwenden, um zu bemerken, dass Sie bei der letzten Arbeit in einem Browser nicht in den anderen Browsern funktionieren.

Visual Studio enthält jetzt Features, die Entwicklern helfen, effizient auf CSS-Stylesheets zuzugreifen, Sie zu bearbeiten und zu organisieren. In dieser Übung werden Sie die neuen Features für eine effektive Organisation und Edition sowie die CSS3-Code Ausschnitte für Browser übergreifende Kompatibilität erfüllen.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Aufgabe 1: neue Editor-Funktionen

In dieser Aufgabe werden Sie die neuen Funktionen des CSS-Editors ermitteln. Mithilfe dieses neuen Editors können Sie Ihre Produktivität steigern, indem Sie den neuen intelligenten Einzug, die verbesserten Code Kommentare und die erweiterte IntelliSense-Liste nutzen.

1. Starten Sie **Visual Studio** , und öffnen Sie die Projekt Mappe " **whatsnewaspnet. sln** ", die sich im Ordner " **source\whatsnewaspnet** " dieses Labs befindet.
2. Öffnen Sie in Projektmappen-Explorer die Datei " **Site. CSS** ", die sich unter dem Ordner " **Stile** " befindet. Stellen Sie sicher, dass die **Text-Editor** -Tools auf der Symbolleiste angezeigt werden. Wählen Sie hierzu die Menüoption | **Symbolleisten** **anzeigen** aus, und überprüfen Sie die Optionen des **Text-Editors** . Sie werden feststellen, dass seit dieser neuen Version die **Kommentar** Schaltfläche (![Kommentar Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png)) und die Schaltfläche **uncomment** (![uncomment-Button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) auch für den CSS-Editor aktiviert ist.

    ![Aktivieren von Editor-und CSS-Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Aktivieren von Editor-und CSS-Tools")

    *Aktivieren von Editor-und CSS-Tools*
3. Scrollen Sie den Code, und wählen Sie eine beliebige CSS-Klassendefinition. Klicken Sie auf die Schaltfläche **Kommentar** (![Kommentar Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png)), um die ausgewählten Zeilen zu kommentieren. Klicken Sie dann auf die Schaltfläche aus **Kommentar entfernen** (![Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png)), um die Änderungen rückgängig zu machen.
4. Klicken Sie auf die Schaltflächen **reduzieren (![** reduzieren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png)) und **erweitern** (![](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png)) am linken Rand des Texts. Beachten Sie, dass Sie jetzt die Stile ausblenden können, die Sie nicht für eine saubere Ansicht verwenden.

    ![Reduzieren von CSS-Klassen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Reduzieren von CSS-Klassen")

    *Reduzieren von CSS-Klassen*
5. Stellen Sie sicher, dass das Feature für intelligenter Einzug aktiviert ist. Wählen Sie die Menüoption Extras | **Optionen** aus, und wählen Sie dann im linken Bereich des Bild **Schirms den** **Text-Editor** | **CSS** - | **Formatierungs** Seite aus. Aktivieren Sie die **hierarchische** Einzugs Option.

    ![Aktivieren des hierarchischen Einzugs](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Aktivieren des hierarchischen Einzugs")

    *Aktivieren des hierarchischen Einzugs*
6. Suchen Sie die Hauptklassen Definition (. Main), und fügen Sie einen Stil an die div-Elemente an. Sie werden feststellen, dass sich der Code automatisch anpasst, sodass Benutzer die übergeordneten Klassen auf einen Blick finden können.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Hierarchische Ausrichtung in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Hierarchische Ausrichtung in CSS")

    *Hierarchische Ausrichtung in CSS*
7. Suchen Sie in der **. Main-div** -Klasse den Cursor am Ende des Rahmens **: 0px;** , und drücken **Sie die Eingabe** Taste, um die IntelliSense-Liste anzuzeigen. Beginnen Sie mit der Eingabe **oben** , und beachten Sie, wie die Liste während der Eingabe gefiltert wird. In der Liste werden die Elemente angezeigt, die in einem beliebigen Teil des Worts **Top** enthalten (in früheren Versionen von Visual Studio wird die Liste nach den Elementen gefiltert, die mit dem Begriff *beginnen* ).

    ![IntelliSense-Erweiterungen in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense-Erweiterungen in CSS")

    *IntelliSense-Erweiterungen in CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Aufgabe 2: Farbauswahl

In dieser Aufgabe ermitteln Sie die neue CSS-Farbauswahl, die in Visual Studio IntelliSense integriert ist.

1. Suchen Sie in **Site. CSS** die Header Klassendefinition (. Header), und platzieren Sie den Cursor neben dem **Background-Color-** Attribut zwischen den &quot;:&quot; und &quot;#&quot; Zeichen in dieser Codezeile **.**

    ![Suchen des Cursors](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Suchen des Cursors")

    *Suchen des Cursors*
2. Löschen Sie den **Doppel** Punkt (:) und schreiben Sie Sie erneut, um die Farbauswahl anzuzeigen. Beachten Sie, dass die ersten Farben, die Sie sehen werden, die am häufigsten vorkommenden Farben Ihrer Site sind. Wenn Sie auf die weiße Farbe klicken, ersetzt der zugehörige HTML-Farbcode (#FFF) den aktuellen Farbcode im Stylesheet.

    ![Farbauswahl](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Farbauswahl")

    *Farbauswahl*
3. Klicken Sie auf die Schaltfläche **erweitern** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png)) in der Farbauswahl, um den Farbverlauf anzuzeigen, und ziehen Sie dann den Farbverlaufs Cursor, um eine andere Farbe auszuwählen. Klicken Sie anschließend auf die **Schaltfläche** "Pipette", und wählen Sie eine beliebige Farbe auf dem Bildschirm aus. Beachten Sie, dass sich der Wert der Hintergrundfarbe dynamisch ändert, während Sie den Cursor bewegen.

    ![Farbauswahl Farbverlauf](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Farbauswahl Farbverlauf")

    *Farbauswahl Farbverlauf*
4. Bewegen Sie im Schieberegler für die **Deckkraft** den Selektor in den Mittelpunkt der Leiste, um die Deckkraft zu verringern. Beachten Sie, dass der Hintergrund Farbwert nun seine Skala in RGBA ändert.

    ![Deckkraft der Farbauswahl](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Deckkraft der Farbauswahl")

    *Deckkraft der Farbauswahl*

    > [!NOTE]
    > Die RGBA-Farbdefinition (rot, grün, blau, Alpha) in CSS3 ermöglicht es Ihnen, den Farb Deckkraft Wert für ein einzelnes Element zu definieren. Anders als bei **Deckkraft:** ein ähnliches CSS-Attribut **-** RGBA-Farben sind auch mit den neuesten Browsern kompatibel.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Aufgabe 3: CSS-kompatible Code Ausschnitte

In dieser Aufgabe erfahren Sie, wie Sie Browser übergreifende kompatible CSS3-Ausschnitte verwenden, um einige Features in Ihrer Website zu implementieren.

1. Suchen Sie in der Datei " **Site. CSS** " die **Header** -CSS-Klassendefinition (. Header), und platzieren Sie den Cursor unterhalb des **/\*Border RADIUS\*/** Platzhalter, um einen neuen Code Ausschnitt hinzuzufügen. Drücken **Sie die Eingabe** Taste, um die IntelliSense-Liste anzuzeigen und **RADIUS** zum Filtern der Liste einzugeben. Wählen Sie die Option Rahmen RADIUS aus der Liste mit einem Doppelklick aus, und drücken Sie dann die **Tab** **-** Taste, um den Code Ausschnitt einzufügen. Geben Sie dann eine RADIUS-Größe in Pixel ein, und drücken **Sie die Eingabe**Taste. Geben Sie beispielsweise **15px**ein.

    Die CSS3-Attribute, die durch den Ausschnitt hinzugefügt werden, werden in den meisten HTML5-Kompatibilitäts Browsern, einschließlich Mozilla-und WebKit-basierten Browsern, gerundet.

    ![Verwenden eines Border-RADIUS-Ausschnitts](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Verwenden eines Border-RADIUS-Ausschnitts")

    *Verwenden eines Border-RADIUS-Ausschnitts*
2. Anwenden **der gleichen Rahmen** Ausschnitte im seitenstil (. Page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Drücken Sie **F5** , um die Projekt Mappe auszuführen. Beachten Sie, dass jede Seite nun gerundete Rahmen hat.

    ![Abgerundete Ecken](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Abgerundete Ecken")

    *Abgerundete Ecken*
4. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.
5. Öffnen Sie die Datei " **Custom. CSS** ", die sich im Ordner " **Stile** " befindet, und platzieren Sie den Cursor in der " **div. Images UL LI IMG** Class
6. Drücken Sie die EINGABETASTE, um die IntelliSense-Liste anzuzeigen, geben Sie **Box-Shadow** ein, und drücken Sie zweimal die **Tab** -Taste, um den Standard Code Ausschnitt in der Klassendefinition einzufügen. Legen Sie die Schatten Werte auf **10px 10px 5 Pixel #888**fest. Geben Sie dann **Border-RADIUS** ein, und fügen Sie den Code Ausschnitt ein. Geben Sie **15px** ein, und drücken **Sie die Eingabe**Taste.

    ![Abgerundete Ecken mit Schatten](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Abgerundete Ecken mit Schatten")

    *Abgerundete Ecken mit Schatten*

    > [!NOTE]
    > Zu diesem Zeitpunkt wird das Schatten Attribut mit dem entsprechenden Präfix (moz, webkit, o) eingefügt, um Mozilla-und WebKit-Browser (Chrome, Safari, konkeror) zu unterstützen.
7. Erstellen Sie eine neue Klasse **div. Images UL LI IMG:** zeigen Sie auf die **div. Images UL LI IMG** -Klassendefinition, und platzieren Sie den Cursor innerhalb der Klammern **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Geben Sie **Transform** ein, und drücken Sie zweimal die **Tab** -Taste, um den Transformations Ausschnitt einzufügen. Geben Sie dann **drehen (-15deg)** ein, um den Drehungs Winkelwert zu ändern, wenn auf Bilder gezeigt wird.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Drücken Sie **F5** , um die Projekt Mappe auszuführen und zur CSS3-Seite zu navigieren. Beachten Sie, dass die Bilder über abgerundete Ecken und Feld Schatten verfügen. Bewegen Sie die Maus über die Bilder, und beobachten Sie, wie Sie sich drehen

    ![Transformieren eines Bilds für das Drehen eines Bilds](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transformieren eines Bilds für das Drehen eines Bilds")

    *Transformieren eines Bilds für das Drehen eines Bilds*

    > [!NOTE]
    > Wenn Sie Internet Explorer 10 verwenden und die Schatten nicht angezeigt werden, stellen Sie sicher, dass der Dokument Modus auf IE10-Standards festgelegt ist. Drücken Sie **F12** , um die Internet Explorer-Entwicklertools zu öffnen, und klicken Sie auf **Dokument Modus** , um zu IE10

    ![about-US](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Übung 2: Neuerungen im HTML-Editor

Visual Studio verfügt über einen verbesserten HTML-Editor. Einige der Verbesserungen in dieser Version sind intelligenter Einzug in HTML-Dokumenten, HTML5-Ausschnitten, HTML-Start-und endtagabgleich und HTML-Validierung. In dieser Übung werden Sie sehen, wie sich diese Änderungen bei der Arbeit im Website Markup verbessern lassen.

Der HTML-Editor wurde wie der CSS-Editor ebenfalls verbessert. Die meisten dieser Verbesserungen sind kleine, die das Leben des Webentwicklers vereinfachen. Einige dieser Verbesserungen sind z. b. mehr Code Ausschnitte für HTML5, intelligenter Einzug, übereinstimmende Start-und Endtags bei der Bearbeitung und Validierung für den HTML-dokumentdoctype.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Aufgabe 1: verbesserte DOCTYPE-Validierung

Der HTML-Editor ist nun in der Lage, den DOCTYPE der Seite zu überprüfen, auch wenn sich die Definition möglicherweise auf der Master Seite befindet. Abhängig vom DOCTYPE der Seite überprüft der HTML-Editor mit dem richtigen Regelsatz und filtert die IntelliSense-Liste nach Berücksichtigung der DOCTYPE-Elemente.

In dieser Aufgabe ändern Sie den DOCTYPE einer Seite, um zu sehen, wie sich das Verhalten des HTML-Editors entsprechend ändert.

1. Wenn Sie nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die Projekt Mappe " **whatsnewaspnet. sln** ", die sich im Ordner " **source\whatsnewaspnet** " dieses Labs befindet.
2. Öffnen Sie die Seite **Site. Master** .
3. Beachten Sie das Ziel Schema für die Validierungs Symbolleiste. Die Art und Weise, wie sich der HTML-Editor verhält (Validierung, IntelliSense usw.), wird ordnungsgemäß an den ausgewählten DOCTYPE angepasst.

    ![DOCTYPE in der Symbolleiste der HTML-Quell Bearbeitung verwenden](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "DOCTYPE in der Symbolleiste der HTML-Quell Bearbeitung verwenden")

    *DOCTYPE in der Symbolleiste der HTML-Quell Bearbeitung verwenden*
4. Ändern Sie das Ziel Schema in HTML 4,01.

    ![Ändern von DOCTYPE in der Symbolleiste zur Bearbeitung von HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Ändern von DOCTYPE in der Symbolleiste zur Bearbeitung von HTML")

    *Ändern von DOCTYPE in der Symbolleiste zur Bearbeitung von HTML*
5. Platzieren Sie den Cursor unter dem **Body** -Element, und beginnen Sie mit der Eingabe des Namens eines HTML5-Elements (z. b. **Video**). Beachten Sie, dass das-Element nicht in der IntelliSense-Liste verfügbar ist.

    ![Nicht aufgelistete HTML5-Elemente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "Nicht aufgelistete HTML5-Elemente")

    *Nicht aufgelistete HTML5-Elemente*
6. Machen Sie die Änderungen am Ziel Schema für die Validierungs Symbolleiste rückgängig, indem Sie DOCTYPE: XHTML5 aus der Dropdown Liste auswählen.

    ![DOCTYPE in der Symbolleiste der HTML-Quell Bearbeitung verwenden](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "DOCTYPE in der Symbolleiste der HTML-Quell Bearbeitung verwenden")

    *DOCTYPE auf der Symbolleiste der HTML-Quell Bearbeitung*
7. Platzieren Sie den Cursor unter dem **Body** -Element, und beginnen Sie erneut mit der Eingabe eines HTML5-Elements (z. b. **Video**). Beachten Sie, dass die HTML5-Elemente jetzt in der IntelliSense-Liste verfügbar sind.

    ![Aufgelisteten HTML5-Elemente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "Aufgelisteten HTML5-Elemente")

    *Aufgelisteten HTML5-Elemente*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Aufgabe 2: automatische Aktualisierung von Start-/Endtags

Visual Studio aktualisiert jetzt die HTML-öffnenden oder schließenden Tags des Elements, das Sie bearbeiten, damit Sie einander zuordnen. Mit dieser neuen Funktion können Sie Ihre Produktivität beim Bearbeiten von HTML-Tags verbessern.

1. Fügen Sie auf der Seite **default. aspx** ein **H3** -Element mit einem Titel hinzu (z. b. Visual Studio 2012 Rocks!).

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Ändern Sie das **H3** -Tag, und geben Sie **H2** oder **H1 ein.**

    Beachten Sie, dass das Endtag automatisch aktualisiert wird. Sie können auch das Endtag ändern, um festzustellen, dass das Starttag ebenfalls entsprechend aktualisiert wird.

    ![Automatisches Update des Endtags](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatisches Update des Endtags")

    *Automatisches Update des Endtags*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Aufgabe 3: neue HTML5-Code Ausschnitte

Visual Studio enthält nun verschiedene HTML5-Code Ausschnitte. In dieser Aufgabe verwenden Sie einige dieser Code Ausschnitte.

1. Fügen Sie dem Stammverzeichnis des Website Ordners einen neuen Ordner mit dem Namen " **Audiodatei** " hinzu. Öffnen Sie Windows-Explorer, und kopieren Sie eine beliebige Audiodatei in den Ordner " **Audiodateien** " der Projekt Mappe " **whatsnewaspnet. sln** ".
2. Suchen Sie auf der Seite **default. aspx** den Cursor unter der Web11 Rocks! Header. Geben Sie **audiotaste** ein, und drücken Sie die Tab

    Der neue HTML-Editor enthält Code Ausschnitte für HTML5-Inhalte. Verwenden Sie die richtige DOCTYPE-Definition, um die HTML5-Ausschnitte zu aktivieren.

    ![Einfügen von HTML5-Code Ausschnitten](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Einfügen von HTML5-Code Ausschnitten")

    *Einfügen von HTML5-Code Ausschnitten*
3. Aktualisieren Sie die Audioquelle so, dass Sie auf eine vorhandene Audiodatei verweist.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Sie müssen die Audiodatei der Projekt Mappe hinzufügen.
4. Drücken Sie **F5** , um die Website auszuführen und die Audiodatei wiederzugeben.

    ![Ausführen des audiosteuerelements](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Ausführen des audiosteuerelements")

    *Ausführen des audiosteuerelements*

    > [!NOTE]
    > Sie können auch weitere Code Ausschnitte in Visual Studio ausprobieren, wie z. b. Video, Figure usw.
5. Versuchen Sie jetzt, ein Steuerelement in einen Teil der Seite einzufügen. Versuchen Sie z. b., ein **GridView** -Steuerelement einzufügen, geben Sie jedoch anstelle von **&lt;GRI** **&lt;GV**ein. Beachten Sie, dass die IntelliSense-Liste das **ASP: GridView** -Steuerelement anzeigt.

    IntelliSense im HTML-Editor bietet jetzt die Suche nach Titel und Schreibweise sowie partielle Übereinstimmungen (Abrufen aller Elemente, die den Begriff enthalten).

    ![Einfügen von GridView mit IntelliSense-Listen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Einfügen von GridView mit IntelliSense-Listen")

    *Einfügen von GridView mit IntelliSense-Listen*

    Wenn Sie **&lt;Raster** eingeben, werden alle Elemente angezeigt, die dem Begriff entsprechen. Visual Studio schlägt jedoch das **GridView** -Steuerelement vor:

    ![Einfügen einer GridView mit IntelliSense-Listen und teilweiser Übereinstimmungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Einfügen einer GridView mit IntelliSense-Listen und teilweiser Übereinstimmungen")

    *Einfügen einer GridView mit IntelliSense-Listen und teilweiser Übereinstimmungen*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Aufgabe 4-HTML-Editor-Smarttags

Eine weitere Verbesserung im HTML-Editor ist das Feature Smarttags. Smarttags erleichtern das Ausführen allgemeiner oder wiederholter Entwicklungsaufgaben auf der Basis der Kontrolle. Diese Funktion war bereits im HTML-Designer verfügbar, aber nicht im HTML-Editor.

1. Öffnen Sie **Site. Master** , und suchen Sie das **ASP: Menu** -Element. Platzieren Sie den Cursor auf dem Starttag, und beachten Sie, dass das kleine Symbol am unteren Rand des Elements angezeigt wird. Klicken Sie darauf, um das Menü "intelligente Aufgaben" zu öffnen. Beachten Sie, dass Sie schnell auf einige Aufgaben im Zusammenhang mit dem Menü Steuerelement zugreifen können.

    ![Intelligente Aufgaben für das Menu-Steuerelement](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Intelligente Aufgaben für das Menu-Steuerelement")

    *Intelligente Aufgaben für das Menu-Steuerelement*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Aufgabe 5: intelligenter Einzug

Eine der bewährten Methoden in HTML ist die einzügung der in HTML eingefügten Elemente, um den Code lesbar zu machen. In Visual Studio 2012 werden Sie feststellen, dass der Editor die Elemente automatisch einzieht, während Sie den Code schreiben.

> [!NOTE]
> In früheren Versionen von Visual Studio war der intelligente Einzug im XML-Editor, aber nicht im HTML-Editor verfügbar.

1. Stellen Sie sicher, dass die Einzugs Konfiguration im HTML-Editor auf Intelligenter Einzug festgelegt ist. Wählen Sie hierzu die **Tools |** Menüoption Optionen, und wählen Sie dann den Text-Editor aus.  **HTML | Registerkarten** Seite im linken Bereich des Bildschirms. Wählen Sie die Option Intelligenter Einzug aus.

    ![HTML-Editor-Einstellungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML-Editor-Einstellungen")

    *HTML-Editor-Einstellungen*
2. Entfernen Sie auf der Seite **default. aspx** den gesamten Inhalt des Audioelements.
3. Platzieren Sie den Cursor am Ende des öffnenden **Audioelements** , und drücken **Sie die Eingabe**Taste.

    Beachten Sie, dass die neue Position des Cursors über eine zusätzliche Einzugs Ebene verfügt.

    ![Intelligenter Einzug im HTML-Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Intelligenter Einzug im HTML-Editor")

    *Intelligenter Einzug im HTML-Editor*
4. Stellen Sie das audiotag mit dem Inhalt wieder her, den Sie entfernt haben, oder schließen Sie **default. aspx** , ohne die Änderungen zu speichern.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Aufgabe 6: Extrahieren in das Benutzer Steuerelement

Die in Visual Studio enthaltenen refactoringtools, wie z. b. das Extrahieren eines Teils des Codes in eine Funktion, sind hervorragend Funktionen, die die Verbesserung und die Umgestaltung des vorhandenen Codes vereinfachen. Die Entsprechung für ASP.net Pages wäre die Extraktion von HTML-Code zu einem Benutzer Steuerelement. Ein manuelles ausführen würde mehrere Schritte umfassen, z. b. das Erstellen eines neuen Benutzer Steuer Elements, das Verschieben des Code Abschnitts in das Benutzer Steuerelement, das Registrieren eines Tagpräfixes für das Benutzer Steuerelement und schließlich das Instanziieren des Benutzer Steuer Elements auf den Seiten. Jetzt führt das neue Tool *Extract to User Control* automatisch alle Schritte für Sie aus.

In dieser Aufgabe verwenden Sie den neuen Kontext Vorgang extrahieren in Benutzer Steuerelement, um ein neues Benutzer Steuerelement aus dem ausgewählten Code zu generieren.

1. Wählen Sie auf der Seite **default. aspx** die Elemente **H2** und **Audioaus** .
2. Klicken Sie mit der rechten Maustaste, und wählen Sie **in Benutzer Steuer**Element

    ![Menüoption "in Benutzer Steuerelement extrahieren"](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Menüoption "in Benutzer Steuerelement extrahieren"")

    *Menüoption "in Benutzer Steuerelement extrahieren"*
3. Geben Sie einen Namen für das neue Benutzer Steuerelement ein. Beispiel: **Jukebox. ascx**, und klicken Sie dann auf **OK**.

    ![Speichern des extrahierten Benutzer Steuer Elements](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Speichern des extrahierten Benutzer Steuer Elements")

    *Speichern des extrahierten Benutzer Steuer Elements*
4. Beachten Sie, dass der ausgewählte Code in ein Benutzer Steuerelement extrahiert wurde und der ursprüngliche Speicherort des ausgewählten Codes durch eine Instanz des neuen Benutzer Steuer Elements ersetzt wurde.

    ![Seite automatisch aktualisiert, um das neue Benutzer Steuerelement zu verwenden](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Seite automatisch aktualisiert, um das neue Benutzer Steuerelement zu verwenden")

    *Seite automatisch aktualisiert, um das neue Benutzer Steuerelement zu verwenden*
5. Drücken Sie **F5** , um die Seite auszuführen und zu überprüfen, ob das Steuerelement funktioniert.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Übung 3: Neuerungen im JavaScript-Editor

Das Schreiben oder Bearbeiten von JavaScript-Code ist keine einfache Aufgabe, insbesondere dann, wenn sich die Größe Ihrer Anwendung vergrößert und Sie sich mit langen Dateien und Hunderten von Funktionen beschäftigen. Skriptentwickler müssen in der Regel einige zusätzliche Aufgaben durchführen, um die Code Lesbarkeit zu gewährleisten und zwischen Dateien zu navigieren. Durch die Einbindung von JavaScript-Bibliotheken wie jQuery wurde die Skript Navigation aufgrund der Codelänge zu einer Herausforderung.

Visual Studio hat den JavaScript-Editor durch die Zusage erneuert, dass der codemodus zugänglich und organisiert ist. Viele Visual Studio-Funktionen, die bereits C# in-oder VB-Editoren vorhanden waren, sind jetzt im JavaScript-Editor implementiert: Gehe zu Definition, automatischer Einzug, Dokumentation und Validierung beim Schreiben. Mit der erneuerten IntelliSense-Liste haben Sie den JavaScript-Funktionskatalog zur Hand.

In dieser Übung lernen Sie einige der neuen Features und Verbesserungen von JavaScript-Editor kennen. Sie werden Beispieldateien durchsuchen und die neuen Merkmale ermitteln, die Ihre JavaScript-Programmierung in Visual Studio 2012 effizienter gestalten.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Aufgabe 1: neue Features für den JavaScript-Editor

Diese Aufgabe führt Sie in einige der neuen Features von JavaScript-Editor ein, die sich auf die Organisation Ihres Codes und die Verbesserung der Benutzer Leistung konzentrieren.

1. Wenn Sie nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die Projekt Mappe " **whatsnewaspnet. sln** ", die sich im Ordner " **source\whatsnewaspnet** " dieses Labs befindet.
2. Drücken Sie **F5** , um die Anwendung auszuführen, und klicken Sie dann auf den JavaScript-Link in der Navigationsleiste. Aktualisieren Sie die Seite mehrmals, und überprüfen Sie, wie der-Wert erhöht wird.

    ![Seiten Zählers](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Seiten Zählers")

    *Seiten Zählers*
3. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.
4. Öffnen Sie die Seite " **JavaScript. aspx** ", und suchen Sie nach dem **&lt;Skript&gt;** Block (siehe unten).

    Im folgenden Code wird der lokale HTML5-Speicher verwendet, um eine *pageloadcount* -Variable zu speichern, die speichert, wie oft die Seite vom aktuellen Benutzer besucht wurde. Der lokale Speicher ist eine Client seitige Schlüssel-Wert-Datenbank, die mit dem HTML5-Standard eingeführt wurde. Die Daten werden auf dem lokalen Computer im Browser des Benutzers gespeichert.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Stellen Sie sicher, dass DOCTYPE auf XHTML5 festgelegt ist, bevor Sie mit den nächsten Schritten fortfahren.
5. Bearbeiten Sie den Code, und beachten Sie, dass IntelliSense für JavaScript HTML5-Features wie lokaler Speicher und ihre inneren Methoden enthält.

    ![HTML5-JavaScript-Funktionen in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5-JavaScript-Funktionen in JavaScript")

    *HTML5-JavaScript-Funktionen in JavaScript*
6. Klicken Sie im Skriptcode auf eine beliebige öffnende eckige Klammer ( **{** ), und beachten Sie, dass die Klammern hervorgehoben sind.

    ![Eckige Klammern sind hervorgehoben](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Eckige Klammern sind hervorgehoben")

    *Eckige Klammern sind hervorgehoben*
7. Heben Sie die Auskommentierung der Funktion **testautoalign ()** auf (Wählen Sie die drei Zeilen aus, und verwenden Sie **STRG** + **K**; **STRG** + **U**), und suchen Sie den Cursor innerhalb des Funktions Codes. Drücken Sie die EINGABETASTE, um eine zweite Zeile anzufügen. Beachten Sie, dass der Code jetzt **ausgerichtet** und **automatisch eingerückt**wird.

    ![JavaScript-Code wird automatisch ausgerichtet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript-Code wird automatisch ausgerichtet")

    *JavaScript-Code wird automatisch ausgerichtet*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Aufgabe 2: Validieren von JavaScript

In dieser Aufgabe werden Sie die neue JavaScript-Validierung für den ECMAScript5-Standard ermitteln. Diese Funktion hilft Ihnen, kompatiblen JavaScript-Code zu schreiben und gleichzeitig Skript Probleme vor der Standort Bereitstellung zu vermeiden.

> [!NOTE]
> Visual Studio 2010 hat ECMAStript3 Konformität implementiert, während Visual Studio 2012 die ECMAScript5-Konformität bereitstellt.

1. Öffnen Sie **ECMA5script5. js** , das sich unter dem **script\custom** -Projektordner befindet. Nun testen Sie die Überprüfung für den ECMAScript5-Standard.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Sie können die &quot; strikte &quot; Richtung in der ersten Zeile der Datei **verwenden** , die den ECMAScript5 **Strict-Modus**aktiviert. Dieser Modus besteht aus einer Teilmenge der Sprache, in der Mehrdeutigkeiten der früheren Edition erläutert werden. Außerdem werden einige neue Features, wie z. b. Getter und Setter, Bibliotheks Unterstützung für JSON und umfassende Reflektion von Objekteigenschaften, hinzugefügt.
2. Öffnen Sie die **Fehlerliste** , wenn Sie nicht bereits geöffnet ist (Menü**anzeigen** | **Fehlerliste**). Beachten Sie, dass die **Funktions** Deklaration unterstrichen ist. Dies liegt daran, dass in ECMA5-Standardfunktionen nicht innerhalb von Sprachstrukturen geschachtelt werden können. In der Fehlerliste unten finden Sie die Warnungs Details.

    ![JavaScript-Validierungs Fehlermeldung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript-Validierungs Fehlermeldung")

    *JavaScript-Validierungs Fehlermeldung*
3. Kommentieren Sie die **&quot;strikte&quot;Richtung verwenden** , und beachten Sie, dass Fehler ausgeblendet werden, die Warnungen jedoch weiterhin bestehen.
4. Schreiben Sie in der letzten Zeile der Datei eine beliebige Zeichenfolge wie **&quot;Test&quot;** (schließen Sie die Anführungszeichen ein, um anzugeben, dass es sich um eine Zeichenfolge handelt). Schreiben Sie einen Zeitraum neben der Zeichenfolge, um die IntelliSense-Liste anzuzeigen, und wählen Sie die Option **kürzen** aus.

    In ECMAScript5 Standard verfügen Zeichen folgen Werte und Variablen ebenfalls über Zeichen folgen Methoden, wie z. b. Trim, Großbuchstaben, suchen und ersetzen.

    ![IntelliSense-Liste in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "IntelliSense-Liste in JavaScript")

    *IntelliSense-Liste in JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Aufgabe 3: XML-Dokumentation für JavaScript

In dieser Aufgabe untersuchen Sie die Visual Studio-Funktionen für die XML-Dokumentation in JavaScript. Sie sehen, dass die JavaScript-IntelliSense-Liste jetzt die XML-Dokumentation jeder Funktion anzeigt. Außerdem werden Sie die Navigationsfunktion in JavaScript entdecken.

1. Öffnen Sie die Datei " **xmlDoc. js** " in " **Scripts/Custom** Project Folder". Diese Datei enthält die XML-Dokumentation zu den einzelnen JavaScript-Funktionen.

    ![JavaScript-XML-Dokumentation in IntelliSense integriert](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript-XML-Dokumentation in IntelliSense integriert")

    *JavaScript-XML-Dokumentation in IntelliSense integriert*
2. Erstellen **Sie** unten in der Datei " **xmlDoc. js** " eine neue Funktion mit dem Namen " **Test**".
3. In der **Test** Funktion wird die **Multiplikations** Funktion aufgerufen, die zwei Parameter empfängt. Beachten Sie, dass das QuickInfo-Feld die **Multiplikations** Funktions Dokumentation anzeigt

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![XML-Dokumentation für JavaScript-Funktionen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML-Dokumentation für JavaScript-Funktionen")

    *XML-Dokumentation für JavaScript-Funktionen*
4. Vervollständigen Sie die Function-Anweisung, und geben Sie einen *Punkt* ein, um die IntelliSense-Liste für den zurückgegebenen Wert zu öffnen. Beachten Sie, dass Visual Studio den **Rückgabe** Wert in der Dokumentation erkennt und den Wert als Zahl behandelt.

    ![XML-Dokumentation für Rückgabe Typen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML-Dokumentation für Rückgabe Typen")

    *XML-Dokumentation für Rückgabe Typen*
5. Fügen Sie nun einen-Befehl zum Hinzufügen der Funktion ein. Beachten Sie, dass der JavaScript-Editor jetzt Funktions Überladungen unterstützt. Wenn Sie einen Funktionsnamen schreiben, können Sie jede der verfügbaren über Ladungen auswählen, die in der Dokumentation angegeben sind.

    ![XML-Dokumentation für über Ladungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML-Dokumentation für über Ladungen")

    *XML-Dokumentation für über Ladungen*
6. Öffnen Sie die Datei " **gotodefinition. js** ", und suchen Sie den **$ (). html ()** -Funktions Aufruf. Suchen Sie den Cursor in **HTML**.
7. Drücken Sie **F12** , und navigieren Sie zur Definition. Beachten Sie, dass Sie jetzt **ohne das Tool suchen auf** Ihren JavaScript-Code zugreifen und diesen durchsuchen können.
8. Suchen Sie den Cursor in der jQuery-Anweisung vor dem Signatur Block am Ende der Codedatei. Drücken Sie **F12**. Navigieren Sie zu der jQuery-Bibliotheksdatei. Beachten Sie, dass Sie auch über die jQuery-Dateien mit **F12**navigieren können.

    ![Navigieren zu jQuery-Definitionen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigieren zu jQuery-Definitionen")

    *Navigieren zu jQuery-Definitionen*

> [!NOTE]
> Stellen Sie sicher, dass gotodefinition. js keine Syntax Fehler aufweist, bevor Sie die Datei speichern.

<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Übung 4: bündeln und minimieren

Wie oft umfassen Ihre Websites mehr als eine JavaScript-oder CSS-Datei? Dies ist ein sehr gängiges Szenario, bei dem Bündelung und Minimierung dabei helfen können, die Dateigröße zu reduzieren und die Site schneller auszuführen. Das neue Bundle-Feature in ASP.NET 4,5 packt einen Satz von JS-oder CSS-Dateien in ein einzelnes Element und verringert seine Größe durch das Minimieren des Inhalts (d. h. das Entfernen nicht erforderlicher Leerzeichen, das Entfernen von Kommentaren, das Reduzieren von bezeichlern).

Bündelung und Minimierung in ASP.NET 4,5 werden zur Laufzeit ausgeführt, sodass der Benutzer-Agent (z. b. Internet Explorer, Mozilla usw.) identifiziert und somit die Komprimierung verbessert werden kann, indem der Benutzer Browser als Ziel verwendet wird (z. b. das Entfernen von etwas, das Mozilla-spezifisch ist). Wenn die Anforderung von IE stammt).

In dieser Übung erfahren Sie, wie Sie die verschiedenen Arten der Bündelung und Minimierung in ASP.NET 4,5 aktivieren und verwenden.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Aufgabe 1: Installieren des Bundle-und Minification-Pakets von nuget

1. Wenn Sie nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die Projekt Mappe " **whatsnewaspnet. sln** ", die sich im Ordner " **source\whatsnewaspnet** " dieses Labs befindet.
2. Öffnen Sie die nuget-Paket-Manager-Konsole. Verwenden Sie hierzu die Menü **Ansicht** | **anderen Windows** | Paket- **Manager-Konsole**.

    ![Öffnen des Paket-Managers file:///C:/Users/User/AppData/Local/Temp/Marker3744//Media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-Application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Öffnen der Paket-Manager-Konsole")

    *Öffnen der Paket-Manager-Konsole*
3. Geben Sie in der Paket-Manager **-Konsole install-Package Microsoft. Web. Optimization** ein **,** und drücken **Sie die Eingabe**Taste.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Aufgabe 2: Standard Bündel

Die einfachste Möglichkeit zur Verwendung von Bündelung und Minimierung besteht darin, die Standard Bündel zu aktivieren. Diese Methode verwendet Konventionen, damit Sie auf die gebündelte und die minierte Version für die JS-und CSS-Dateien in einem Ordner verweisen können.

In dieser Aufgabe erfahren Sie, wie Sie die gebündelten und minisierten js-und CSS-Dateien aktivieren und referenzieren und die resultierende Ausgabe anzeigen.

1. Wenn Sie nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die Projekt Mappe " **whatsnewaspnet. sln** ", die sich im Ordner " **source\whatsnewaspnet** " dieses Labs befindet.
2. Erweitern Sie im **Projektmappen-Explorer**die Ordner **Stile**, **script\custom** und **script\bundle** .

    Beachten Sie, dass die Anwendung mehr als eine CSS-und JS-Datei verwendet.

    ![Mehrere Stylesheets und JavaScript-Dateien in der Anwendung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Mehrere Stylesheets und JavaScript-Dateien in der Anwendung")

    *Mehrere Stylesheets und JavaScript-Dateien in der Anwendung*
3. Öffnen Sie die Datei **Global.asax.cs** .

    Beachten Sie, dass der neue **Microsoft. Web. Optimization** -Namespace am Anfang der Datei auskommentiert wird. Heben Sie die Auskommentierung der using-Direktive auf, um die Funktionen für Bündelung und Minimierung

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Suchen Sie die **Anwendung\_Start** -Methode.

    Entfernen Sie in dieser Methode den Befehl enabledefaultbundles, wie im folgenden Code Ausschnitt gezeigt. Dies ermöglicht es uns, auf eine gebündelte Auflistung von CSS-Dateien in einem Ordner zu verweisen, indem der Pfad zu diesem Ordner sowie die &quot;CSS-&quot; oder das&quot; Suffix &quot;js verwendet werden.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Öffnen Sie die Datei " **Optimization. aspx** ", und suchen Sie das Inhalts Steuerelement für **headcontent**.

    Beachten Sie, dass die CSS-Dateien und die JS-Dateien ein einzelnes referenziertes Tag aufweisen.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Dieser Code dient zu Demo Zwecken. Im Idealfall verweisen Sie in der Datei "Site. Master" auf die Pakete. In diesem Beispielcode werden Sie feststellen, dass auf einige der gebündelten Dateien auch von der Datei "Site. Master" verwiesen wird, sodass der letzte Verweis redundant ist.
6. Beachten Sie, dass die Verknüpfungen die bündeln Konventionen im **href** -Attribut verwenden, um alle CSS-oder JS-Dateien aus den Stilen bzw. dem scripts\custom-Ordner zu erhalten.

    Sie können die Pfad **Skripts/Custom/js** verwenden, wie unten gezeigt, um alle JS-Dateien in einem **Scripts/Custom** -Ordner zu bündeln und zu minimieren. Dies ist das Standardverhalten mit den Standardpaketen.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Öffnen Sie die Datei **styles\site.CSS** .

    Beachten Sie, dass die ursprüngliche CSS-Datei eingerückt Code, Leerzeichen und Kommentare enthält, die die Datei vergrößern. (Außerdem enthält die JavaScript-Datei Leerzeichen und Kommentare).

    ![Eine der ursprünglichen CSS-Dateien im Ordner "Scripts"](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "Eine der ursprünglichen CSS-Dateien im Ordner "Scripts"")

    *Eine der ursprünglichen CSS-Dateien im Ordner "Scripts"*
8. Drücken Sie **F5** , um die Anwendung auszuführen und zur **Optimierungs** Seite zu navigieren.
9. Klicken Sie auf den Link **CSS-Bundle** , um die Datei herunterzuladen und zu öffnen.

    Sehen Sie sich die minierte gebündelte Datei an. Sie werden feststellen, dass alle leeren Leerzeichen, Kommentare und Einzugs Zeichen entfernt wurden, sodass eine kleinere Datei erzeugt wird.

    ![Gebündelte CSS-Dateien](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Gebündelte CSS-Dateien")

    *Gebündelte CSS-Dateien*
10. Klicken Sie nun auf den Link **js Bundle** , um die gebündelte JavaScript-Datei zu öffnen. Sie können die Explorer-Warnung gefahrlos ignorieren. Beachten Sie, dass die JavaScript-Dateien im **benutzerdefinierten** Ordner ebenfalls gebündelt und minimiert werden.

    ![Gebündelte JavaScript-Dateien](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Gebündelte JavaScript-Dateien")

    *Gebündelte JavaScript-Dateien*

    Die Aktivierung der Komprimierung für CSS-oder JS-Dateien war in der vorherigen ASP.NET-Version viel komplizierter. Wie Sie gesehen haben, müssen Sie nur eine Zeile in der Datei " *Global. asax* " hinzufügen, um die Bündelung zu aktivieren, und dann auf die gebündelten Dateien von Ihrer Website verweisen.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Aufgabe 3: statische Bündel

Der Ansatz mit statischen Paketen ermöglicht es Ihnen, den Satz der zu bündeln Dateien, den Verweis und die zu verwendende minisierungsmethode anzupassen.

In dieser Aufgabe konfigurieren Sie ein statisches Bündel, um einen bestimmten Satz von Dateien zu definieren, die gebündelt und minimal sind.

1. Schließen Sie den Browser.
2. Öffnen Sie die Datei **Global.asax.cs** , und suchen Sie die **Anwendung\_Start** -Methode.
3. Heben Sie die Auskommentierung des statischen Bündel Codes auf, wie im folgenden Code dargestellt.

    Sie definieren ein statisches Paket, auf das mit dem &quot; **~/StaticBundle**&quot; virtuellen Pfad verwiesen wird, und verwenden **jsminify** für die Minimierung aller angegebenen Dateien mit der **AddFile** -Methode. Schließlich fügen Sie der **bundletable** das statische Bundle hinzu und aktivieren es.

    Beachten Sie, dass sich die Dateien nicht am selben Ort befinden. Dies ist ein weiterer Vorteil gegenüber der Standard Bündelung.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Öffnen Sie die Datei " **Optimization. aspx** ".

    Beachten Sie, dass der Link zum **statischen js-Bundle** den Pfad verwendet, den Sie beim Konfigurieren des statischen Pakets in der Global.asax.cs-Datei deklariert haben: **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Drücken Sie **F5** , um die Anwendung auszuführen, und navigieren Sie dann zur **Optimierungs** Seite.
6. Klicken Sie auf den Link **statischer js-Bundle** , um die Datei zu öffnen.

    Beachten Sie, dass die minierte gebündelte JavaScript-Datei die Ausgabe für alle JavaScript-Dateien ist, die in der statischen Bundle-Datei unter dem Pfad &quot;/StaticBundle-&quot;konfiguriert sind.

    ![Paket für statische JavaScript-Dateien](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Paket für statische JavaScript-Dateien")

    *Paket für statische JavaScript-Dateien*
7. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Aufgabe 4: dynamische Ordner Bündel

In dieser Aufgabe erfahren Sie, wie Sie dynamische Ordner Pakete konfigurieren. Die Leistungsfähigkeit der dynamischen Bündelung besteht darin, dass Sie statische JavaScript-Dateien sowie andere Dateien in Sprachen einschließen können, die in JavaScript kompiliert werden, und daher einige Verarbeitungsschritte erfordern, bevor die Bündelung ausgeführt wird.

In diesem Beispiel erfahren Sie, wie Sie die **dynamicfolderbundle** -Klasse verwenden, um ein dynamisches Paket für in cofeescript geschriebene Dateien zu erstellen. Cofeescript ist eine Programmiersprache, die in JavaScript kompiliert wird und eine einfachere Syntax zum Schreiben von JavaScript-Code bereitstellt, um die über-und Lesbarkeit von JavaScript zu verbessern.

1. Öffnen Sie die Datei **Global.asax.cs** , und suchen Sie die **Anwendung\_Start** -Methode.
2. Heben Sie die Auskommentierung des dynamischen Bündel Codes auf, wie im folgenden Code dargestellt.

    Sie definieren ein dynamisches Ordner Paket, in dem der benutzerdefinierte **coffeeminify** -minierationsprozessor verwendet wird, der nur auf die Dateien mit der Erweiterung &quot; **. Coffee**&quot; (coffeescript-Dateien) angewendet wird. Beachten Sie, dass Sie ein Suchmuster verwenden können, um die Dateien auszuwählen, die in einem Ordner zusammengefasst werden sollen, z.b. "\*. Coffee".

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Öffnen Sie die nuget-Paket-Manager-Konsole. Verwenden Sie hierzu die Menü **Ansicht** | **anderen Windows** | Paket- **Manager-Konsole**.
4. Geben Sie in der Paket-Manager **-Konsole install-Package coffeesharp** ein **,** und drücken **Sie die Eingabe**Taste.
5. Klicken Sie im Fenster **Projektmappen-Explorer** auf die Schaltfläche **alle Dateien anzeigen** .

    ![Alle Dateien werden angezeigt.](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Alle Dateien werden angezeigt.")

    *Alle Dateien werden angezeigt.*
6. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Datei **CoffeeMinify.cs** , und wählen Sie **in Projekt einschließen**

    ![Fügen Sie die CoffeeMinify.cs-Datei in das Projekt ein.](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Fügen Sie die CoffeeMinify.cs-Datei in das Projekt ein.")

    *Fügen Sie die CoffeeMinify.cs-Datei in das Projekt ein.*
7. Öffnen Sie die Datei **CoffeeMinify.cs** .

    Diese Klasse erbt von jsminify, um die JavaScript-Ausgabe zu minimieren, die sich aus der coffeescript-Code Kompilierung ergibt. Er ruft den coffeescript-Compiler auf, um zunächst den JavaScript-Code zu generieren, und sendet ihn dann an die Methode "jsminify. Process", um den resultierenden Code zu minimieren.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Öffnen Sie die Dateien **Script1. Coffee** und **script2. Coffee** aus dem Ordner **Scripts/Bundle** .

    Diese Dateien enthalten den zu kompilierenden coffescript-Code, wenn die Bündelung mit der coffeeminify-Klasse durchgeführt wird.

    Aus Gründen der Einfachheit enthalten die bereitgestellten coffeescript-Dateien nur den coffeescript-Beispielcode. Die Kommentare werden vom jsminify-Prozess ausgeschlossen.

    ![Coffeescript-Dateien](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "Coffeescript-Dateien")

    *Coffeescript-Dateien*

    > [!NOTE]
    > [Cofeescript](https://github.com/jashkenas/coffeescript/) bietet eine einfachere Syntax zum Schreiben von JavaScript-Code, zur Verbesserung der Lesbarkeit und Lesbarkeit von JavaScript sowie zum Hinzufügen weiterer Funktionen wie Array Verständnis und Musterabgleich.
9. Öffnen Sie die Datei " **Optimization. aspx** ", und suchen Sie die Bündel Verknüpfungen.

    Beachten Sie, dass der Link zum **Dynamic js-Bundle** mit dem **/Coffee** -Suffix, das Sie für das dynamische Ordner Paket konfiguriert haben, auf den Ordner " **Scripts/Bundle** " verweist.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Drücken Sie **F5** , um die Anwendung auszuführen, und navigieren Sie dann zur **Optimierungs** Seite.
11. Klicken Sie auf den Link " **Dynamic js Bundle** ", um die generierte Datei zu öffnen.

    Beachten Sie, dass der Inhalt, der in diesem Bundle enthalten war, nur **. Coffee** -Dateien enthält. Sie können auch sehen, dass der coffeescript-Code in JavaScript kompiliert wurde und die auskommentierten Zeilen entfernt wurden.

    ![Paket für dynamische JS-Dateien](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Paket für dynamische JS-Dateien")

    *Paket für dynamische JS-Dateien*

> [!NOTE]
> Außerdem können Sie diese Anwendung in Windows Azure-Websites bereitstellen, [indem Sie Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy verwenden](#AppendixB).

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Mit dieser Übungseinheit können Sie verstehen, welche Neuerungen in ASP.net und der Webentwicklung in Visual Studio 2012 sind, und wie Sie die verschiedenen Erweiterungen in Visual Studio 2012 nutzen können.

Wenn Sie diese praktische Übungseinheit durcharbeiten, haben Sie gelernt, wie Sie die neuen Features und Verbesserungen in Visual Studio 2012-Editoren für CSS, JavaScript und HTML verwenden können. Außerdem haben Sie gelernt, wie Visual Studio 2012 integrierte Bündelung und Minimierung implementiert.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren. Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.

1. Wechseln Sie zu [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Windows Azure SDK</em>&quot;suchen.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.
3. Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.

    ![Installieren von Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.

    ![Akzeptieren der Lizenzbedingungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.

    ![Installationsstatus](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Installationsfortschritt*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.

    ![Installation abgeschlossen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Installation abgeschlossen*
7. Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .
8. Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .

    ![VS Express für Web-Kachel](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *VS Express für Web-Kachel*

---

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy

In diesem Anhang wird gezeigt, wie Sie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und die Anwendung veröffentlichen, die Sie durch die folgenden Schritte abgerufen haben. nutzen Sie dabei die von Windows Azure bereitgestellte Funktion zur Web deploy Veröffentlichung.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website über das Windows Azure-Portal

1. Wechseln Sie zum [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmelde Informationen an, die Ihrem Abonnement zugeordnet sind.

    > [!NOTE]
    > Mit Windows Azure können Sie 10 ASP.NET Websites kostenlos hosten und dann skalieren, wenn Ihr Datenverkehr zunimmt. Sie können sich [hier](https://aka.ms/aspnet-hol-azure)registrieren.

    ![Anmelden bei Windows Azure-Portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Anmelden bei Windows Azure-Portal")

    *Anmelden bei Windows Azure Verwaltungsportal*
2. Klicken Sie in der Befehlsleiste auf **neu** .

    ![Erstellen einer neuen Website](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **Compute** - | **Website**. Wählen Sie dann **schneller** Fassung aus. Geben Sie eine verfügbare URL für die neue Website an, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Eine Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud ausgeführt wird und die Sie steuern und verwalten können. Mit der Option schneller Fassung können Sie eine abgeschlossene Webanwendung von außerhalb des Portals auf der Windows Azure-Website bereitstellen. Sie enthält keine Schritte zum Einrichten einer Datenbank.

    ![Erstellen einer neuen Website mithilfe der schneller Fassung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Erstellen einer neuen Website mithilfe der schneller Fassung")

    *Erstellen einer neuen Website mithilfe der schneller Fassung*
4. Warten Sie, bis die neue **Website** erstellt wurde.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** -Spalte. Überprüfen Sie, ob die neue Website funktioniert.

    ![Navigieren zur neuen Website](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Navigieren zur neuen Website")

    *Navigieren zur neuen Website*

    ![Website wird ausgeführt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Website wird ausgeführt")

    *Website wird ausgeführt*
6. Wechseln Sie zurück zum Portal, und klicken Sie in der Spalte **Name** auf den Namen der Website, um die Verwaltungs Seiten anzuzeigen.

    ![Öffnen der Website-Verwaltungs Seiten](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Öffnen der Website-Verwaltungs Seiten")

    *Öffnen der Website-Verwaltungs Seiten*
7. Klicken Sie auf der Seite **Dashboard** im Abschnitt **kurzer Blick** auf den Link **Veröffentlichungs Profil herunterladen** .

    > [!NOTE]
    > Das *Veröffentlichungs Profil* enthält alle Informationen, die zum Veröffentlichen einer Webanwendung auf einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungs Methoden erforderlich sind. Das Veröffentlichungs Profil enthält die URLs, Benutzer Anmelde Informationen und Daten Bank Zeichenfolgen, die erforderlich sind, um eine Verbindung mit den einzelnen Endpunkten herzustellen und diese zu authentifizieren, für die eine Veröffentlichungs Methode aktiviert ist. **Microsoft webmatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** unterstützen das Lesen von Veröffentlichungs Profilen, um die Konfiguration dieser Programme für das Veröffentlichen von Webanwendungen auf Windows Azure-Websites zu automatisieren.

    ![Das Website-Veröffentlichungs Profil wird heruntergeladen.](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Das Website-Veröffentlichungs Profil wird heruntergeladen.")

    *Das Website-Veröffentlichungs Profil wird heruntergeladen.*
8. Laden Sie die Veröffentlichungs Profil Datei an einen bekannten Speicherort herunter. In dieser Übung erfahren Sie, wie Sie diese Datei zum Veröffentlichen einer Webanwendung auf Windows Azure-Websites in Visual Studio verwenden.

    ![Die Veröffentlichungs Profil Datei wird gespeichert.](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Das Veröffentlichungs Profil wird gespeichert.")

    *Die Veröffentlichungs Profil Datei wird gespeichert.*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: Konfigurieren des Datenbankservers

Wenn Ihre Anwendung SQL Server Datenbanken nutzt, müssen Sie einen SQL-Daten Bank Server erstellen. Wenn Sie eine einfache Anwendung bereitstellen möchten, die SQL Server nicht verwendet, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank. Sie können die SQL-Datenbankserver aus Ihrem Abonnement im Windows Azure-Verwaltungs Portal unter **SQL-Datenbanken** | **Server** | **Dashboard des Servers**anzeigen. Wenn Sie keinen Server erstellt haben, können Sie ihn mithilfe der Schaltfläche **Hinzufügen** auf der Befehlsleiste erstellen. Notieren Sie sich den **Servernamen und die URL, den Administrator Anmelde Namen und das Kennwort**, da Sie Sie in den nächsten Aufgaben verwenden werden. Erstellen Sie die Datenbank noch nicht, da Sie in einer späteren Phase erstellt wird.

    ![Dashboard des SQL-Datenbankservers](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "Dashboard des SQL-Datenbankservers")

    *Dashboard des SQL-Datenbankservers*
2. In der nächsten Aufgabe testen Sie die Datenbankverbindung aus Visual Studio. aus diesem Grund müssen Sie Ihre lokale IP-Adresse in die Liste der **zulässigen IP-Adressen**des Servers einschließen. Klicken Sie hierzu auf **Konfigurieren**, wählen Sie die IP-Adresse aus der **aktuellen Client-IP-Adresse** aus, und fügen Sie Sie in die Textfelder **Start-IP-Adresse** und **End IP Address** ein. Geben Sie einen Namen für die Regel ein, und klicken Sie auf die Schaltfläche ![Add-Client-IP-Address-OK-Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png).

    ![Client-IP-Adresse](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Client-IP-Adresse*
3. Sobald die **Client-IP-Adresse** der Liste zulässige IP-Adressen hinzugefügt wurde, klicken Sie auf **Speichern** , um die Änderungen zu bestätigen.

    ![Änderungen bestätigen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Änderungen bestätigen*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy

1. Kehren Sie zur ASP.NET MVC 4-Lösung zurück. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Website Projekt, und wählen Sie **veröffentlichen**aus.

    ![Veröffentlichen der Anwendung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungs Profil, das Sie in der ersten Aufgabe gespeichert haben.

    ![Importieren des Veröffentlichungs Profils](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importieren des Veröffentlichungs Profils")

    *Veröffentlichungs Profil wird importiert.*
3. Klicken Sie auf **Verbindung**überprüfen. Klicken Sie nach Abschluss der Überprüfung auf **weiter**.

    > [!NOTE]
    > Die Überprüfung ist fertiggestellt, sobald ein grünes Häkchen neben der Schaltfläche Verbindung überprüfen angezeigt wird.

    ![Die Verbindung wird überprüft.](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Die Verbindung wird überprüft.")

    *Die Verbindung wird überprüft.*
4. Klicken Sie auf der Seite **Einstellungen** unter dem Abschnitt **Datenbanken** auf die Schaltfläche neben dem Textfeld der Datenbankverbindung (z. b. **DefaultConnection**).

    ![Webbereitstellungs Konfiguration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Webbereitstellungs Konfiguration")

    *Webbereitstellungs Konfiguration*
5. Konfigurieren Sie die Datenbankverbindung wie folgt:

   - Geben Sie unter **Server Name** die URL des SQL-Datenbankservers unter Verwendung des *TCP:* -Präfix ein.
   - Geben Sie unter **Benutzername** den Anmelde Namen des Server Administrators ein.
   - Geben Sie unter **Kennwort** Ihren Server Administrator-Anmelde Kennwort ein.
   - Geben Sie einen neuen Datenbanknamen ein, z. b.: *MVC4SampleDB*.

     ![Konfigurieren der Ziel Verbindungs Zeichenfolge](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Konfigurieren der Ziel Verbindungs Zeichenfolge")

     *Konfigurieren der Ziel Verbindungs Zeichenfolge*
6. Klicken Sie dann auf **OK**. Wenn Sie zum Erstellen der Datenbank aufgefordert werden, klicken Sie auf **Ja**.

    ![Erstellen der Datenbank](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Erstellen der Daten Bank Zeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungs Zeichenfolge, die Sie zum Herstellen einer Verbindung mit der SQL-Datenbank in Windows Azure verwenden, wird im Textfeld Standardverbindung angezeigt. Klicken Sie dann auf **Weiter**.

    ![Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank")

    *Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank*
8. Klicken Sie auf der Seite **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Nachdem der Veröffentlichungs Vorgang abgeschlossen ist, wird die veröffentlichte Website in Ihrem Standardbrowser geöffnet.

    ![In Windows Azure veröffentlichte Anwendung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "In Windows Azure veröffentlichte Anwendung")

    *In Windows Azure veröffentlichte Anwendung*
