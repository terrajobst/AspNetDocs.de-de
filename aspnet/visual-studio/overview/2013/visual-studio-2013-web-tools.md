---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Praktische Übungseinheit: Visual Studio 2013 WebTools | Microsoft-Dokumentation'
author: rick-anderson
description: Visual Studio ist eine ausgezeichnete Entwicklungsumgebung für. NET-basierte Windows-und Webprojekte. Sie enthält einen leistungsfähigen Text-Editor, der problemlos verwendet werden kann...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 2fb987dd9b26ad9f0e8a88fd881bde4505ec4148
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505005"
---
# <a name="hands-on-lab-visual-studio-2013-web-tools"></a>Praktische Übungseinheit: Visual Studio 2013-WebTools

vom [Web Camps-Team](https://twitter.com/webcamps)

[Webcamps-Trainingskit herunterladen](https://aka.ms/webcamps-training-kit)

> Visual Studio ist eine ausgezeichnete Entwicklungsumgebung für. NET-basierte Windows-und Webprojekte. Sie enthält einen leistungsfähigen Text-Editor, der problemlos verwendet werden kann, um eigenständige Dateien ohne ein Projekt zu bearbeiten.
> 
> Visual Studio behält eine Analyse Struktur mit vollem Funktionsumfang bei, während Sie die einzelnen Dateien bearbeiten. Dies ermöglicht Visual Studio, eine unschöne automatische Vervollständigung und Dokument basierte Aktionen bereitzustellen und gleichzeitig die Entwicklung schneller und angenehmer zu gestalten. Diese Features sind besonders leistungsfähig in HTML-und CSS-Dokumenten.
> 
> Diese Leistungsfähigkeit steht auch für Erweiterungen zur Verfügung, sodass die Editoren einfach mit leistungsstarken neuen Features erweitert werden können, die Ihren Anforderungen entsprechen. Web Essentials ist eine Sammlung von (größtenteils) webbezogenen Erweiterungen für Visual Studio. Es enthält viele neue IntelliSense-Vervollständigungen (insbesondere für CSS), neue browserlinkfeatures, automatisches jshint für JavaScript-Dateien, neue Warnungen für HTML und CSS sowie viele weitere Features, die für die moderne Webentwicklung von entscheidender Bedeutung sind.
> 
> Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das unter [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit)verfügbar ist.

<a id="Overview"></a>
## <a name="overview"></a>Übersicht

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie Folgendes:

- Verwenden neuer HTML-Editor-Funktionen, die in Web Essentials enthalten sind, z. b. umfangreiche HTML5-Code Ausschnitte
- Verwenden neuer CSS-Editor-Features in WebEssentials, z. b. Farbauswahl und Browser Matrix-QuickInfo
- Verwenden neuer JavaScript-Editor-Funktionen in WebEssentials, z. b. Extrahieren in Dateien und IntelliSense für alle HTML-Elemente
- Austauschen von Daten zwischen Ihrem Browser und Visual Studio mithilfe von Browser Link

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser praktischen Übungseinheit ist Folgendes erforderlich:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) oder höher
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Einrichten

Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zuerst Ihre Umgebung einrichten.

1. Öffnen Sie ein Windows-Explorer-Fenster, und navigieren Sie zum **Quell** Ordner des Labs.
2. Klicken Sie mit der rechten Maustaste auf **Setup. cmd** , und wählen Sie **als Administrator ausführen** aus, um den Setup Vorgang zu starten, mit dem die Umgebung konfiguriert wird, und die Visual Studio-Code Ausschnitte für dieses Lab
3. Wenn das Dialogfeld Benutzerkontensteuerung angezeigt wird, bestätigen Sie die Aktion, um fortzufahren.

> [!NOTE]
> Vergewissern Sie sich, dass Sie vor dem Ausführen des Setups alle Abhängigkeiten für dieses Lab geprüft haben.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Verwenden der Code Ausschnitte

Im gesamten Lab-Dokument werden Sie angewiesen, Code Blöcke einzufügen. Der größte Teil dieses Codes wird zur einfacheren Verwendung als Visual Studio Code Ausschnitte bereitgestellt, auf die Sie in Visual Studio 2013 zugreifen können, um das manuelle Hinzufügen zu vermeiden.

> [!NOTE]
> Jede Übung wird von einer Start Lösung begleitet, die sich im Ordner " **Begin** " der Übung befindet und Ihnen ermöglicht, die einzelnen Übungen unabhängig von den anderen zu befolgen. Beachten Sie, dass die während einer Übung hinzugefügten Code Ausschnitte in diesen Projektmappen fehlen und möglicherweise erst nach Abschluss der Übung funktionieren. Innerhalb des Quellcodes für eine Übung finden Sie auch einen **endordner** mit einer Visual Studio-Projekt Mappe mit dem Code, der aus der Ausführung der Schritte in der entsprechenden Übung resultiert. Sie können diese Lösungen als Leitfaden verwenden, wenn Sie zusätzliche Hilfe benötigen, während Sie diese praktische Übungseinheit durcharbeiten.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Exerzitien

Diese praktische Übungseinheit umfasst die folgenden Übungen:

1. [Arbeiten mit Browser Link und Web Essentials](#Exercise1)
2. [Nutzen von Code Ausschnitten und IntelliSense](#Exercise2)

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungs Sammlungen auswählen. Jede vordefinierte Sammlung ist so konzipiert, dass Sie einem bestimmten Entwicklungsstil entspricht, und bestimmt Fensterlayouts, das Editor-Verhalten, IntelliSense-Code Ausschnitte und Dialogfeld Optionen. In den Prozeduren in dieser Übungseinheit werden die Aktionen beschrieben, die zum Ausführen einer bestimmten Aufgabe in Visual Studio bei Verwendung der **allgemeinen Entwicklungs Einstellungs** Auflistung erforderlich sind. Wenn Sie eine andere Einstellungs Sammlung für Ihre Entwicklungsumgebung auswählen, kann es Unterschiede in den Schritten geben, die Sie berücksichtigen sollten.

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Übung 1: Arbeiten mit Browser Link und Web Essentials

**WebEssentials** ist eine Visual Studio-Erweiterung, die eine Vielzahl nützlicher Features für die moderne Webentwicklung bietet. Sie konzentriert sich hauptsächlich darauf, die Webentwicklung viel schneller und angenehmer zu gestalten. Sie können Web Essentials aus dem Erweiterungs Katalog in Visual Studio installieren.

**Browser Link** ist ein neues Feature, das in Visual Studio 2013 enthalten ist, das einen Kanal zwischen der Visual Studio-IDE und einem beliebigen geöffneten Browser zum Austauschen von Daten zwischen Ihrer Webanwendung und Visual Studio bereitstellt. Web Essentials erweitert den Browser Link mit Tools, um das DOM-Objektmodell und die CSS-Stile Ihrer Webseiten direkt aus dem Browser zu bearbeiten.

In dieser Übung untersuchen Sie einige der Features, die von **Web Essentials** und **Browser Link** unterstützt werden, um eine einfache Quiz Seite zu verbessern.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Aufgabe 1: Ausführen des Projekts in mehreren Browsern

In dieser Aufgabe konfigurieren Sie die Webanwendung so, dass Sie in mehreren Browsern gleichzeitig ausgeführt wird, was für Browser übergreifende Tests nützlich ist.

1. Öffnen Sie **Microsoft Visual Studio**.
2. Wählen Sie im Menü **Datei** die Option Öffnen aus.  **Projekt/Projekt Mappe...** und navigieren Sie zu **EX1-WorkingwithBrowserLinkandWebEssentials\Begin** im **Quell** Ordner des Labs (c:\webcampstk\hol\vswebtooling\source). Wählen Sie **BEGIN. sln** aus, und klicken Sie auf **Öffnen**.
3. Erweitern Sie in der Visual Studio-Symbolleiste das Menü Browser, und wählen Sie **Durchsuchen mit...** aus.

    ![Menüoption "Durchsuchen"](visual-studio-2013-web-tools/_static/image1.png "Durchsuchen... im Menü Browser")

    *Menüoption "Durchsuchen"*
4. Wählen Sie im Dialogfeld **Durchsuchen mit** den Optionen **Google Chrome** und **Internet Explorer** aus, indem Sie die **STRG** -Taste gedrückt halten, und klicken Sie dann auf **als Standard festlegen**.

    ![Dialogfeld "Durchsuchen"](visual-studio-2013-web-tools/_static/image2.png "Dialogfeld "Durchsuchen"")

    *Auswählen mehrerer Standardbrowser*
5. Google Chrome und Internet Explorer sollten jetzt als Standardbrowser angezeigt werden. Klicken Sie auf **Abbrechen** , um das Dialogfeld zu schließen.

    ![Google Chrome und Internet Explorer als Standardbrowser](visual-studio-2013-web-tools/_static/image3.png "Google Chrome-und Internet Explorer-Standardbrowser")

    *Google Chrome und Internet Explorer als Standardbrowser*

    > [!NOTE]
    > Nach dem Konfigurieren der Standardbrowser wird die Option **mehrere Browser** im Menü Browser ausgewählt.
    > 
    > ![Mehrere Browser](visual-studio-2013-web-tools/_static/image4.png "Mehrere Browser")
6. Drücken Sie **STRG** + **F5** , um die Anwendung ohne Debuggen auszuführen.
7. Wenn beide Browserfenster geöffnet sind, platzieren Sie eine von Ihnen über der anderen, um die Aktualisierungen auf beiden Browsern gleichzeitig anzuzeigen. Die Browser sollten eine Webseite mit einem hellblauen Rechteck anzeigen.

    ![Platzieren eines Browsers über dem anderen](visual-studio-2013-web-tools/_static/image5.png "Platzieren eines Browsers über dem anderen")

    *Platzieren eines Browsers über dem anderen*
8. Schließen Sie die Browser nicht. Diese werden in der nächsten Aufgabe verwendet.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Aufgabe 2: Verwenden der Zen-Codierung zum Erstellen von HTML-Elementen

Bei der **Zen-Codierung** handelt es sich um ein Editor-Plug-in für schnelle HTML-, XML-, XSL-und andere strukturierte Code Formate, die Codieren und bearbeiten. Der Kern dieses Plug-ins ist eine leistungsstarke Abkürzung-Engine, mit der Sie Ausdrücke wie CSS-Selektoren in HTML-Code erweitern können. Die Zen-Codierung ist eine schnelle Möglichkeit zum Schreiben von HTML mithilfe einer Syntax der CSS-Stil Auswahl.

In dieser Übung verwenden Sie die von Web Essentials bereitgestellte Zen-Codierungsfunktion, um die HTML-Schaltflächen zu generieren, die die Optionen der Frage darstellen.

1. Wechseln Sie zurück zu Visual Studio.
2. Öffnen Sie die Datei " **Index. cshtml** ", die sich im Ordner **views** | **Home** befindet.
3. Ersetzen **Sie den&lt;!--TODO: hier hinzufügen-Optionen&gt;** Kommentar durch den folgenden Code, und drücken Sie die **Tab**-Taste.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. Der Code sollte auf HTML erweitert werden.

    ![Erweitertes HTML](visual-studio-2013-web-tools/_static/image6.png "Erweitertes HTML")

    *Erweitertes HTML*

    > [!NOTE]
    > Weitere Informationen zur Zen-Codierungs Syntax finden Sie im folgenden [Artikel](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Klicken Sie auf die Schaltfläche Verbindungs **Browser aktualisieren** , um beide Browser zu aktualisieren.

    ![Verknüpfte Browser aktualisieren](visual-studio-2013-web-tools/_static/image7.png "Verknüpfte Browser aktualisieren")

    *Verknüpfte Browser aktualisieren*

    ![Internet Explorer-Seite aktualisiert mit vier Schaltflächen](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer-Seite aktualisiert mit vier Schaltflächen")

    *Internet Explorer-Seite aktualisiert mit vier Schaltflächen*

    ![Google Chrome-Seite mit vier Schaltflächen aktualisiert](visual-studio-2013-web-tools/_static/image9.png "Google Chrome-Seite mit vier Schaltflächen aktualisiert")

    *Google Chrome-Seite mit vier Schaltflächen aktualisiert*
6. Wechseln Sie zurück zu Visual Studio.
7. Sie haben der Seite die Schaltflächen hinzugefügt, aber Sie müssen dennoch eine Vorlagen Frage hinzufügen. Zu diesem Zweck verwenden Sie ein neues Feature in Web Essentials namens " **Lorem Ipsum Generator**". Suchen Sie das **div** -Element mit dem **Class** -Attribut **Front**.
8. Fügen Sie den folgenden Code als erstes untergeordnetes Element des **div**-Elements hinzu, und drücken Sie die **Tab**-Taste.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. Der Code sollte auf HTML erweitert werden.

    !["Lorem ipsum" automatisch generiert](visual-studio-2013-web-tools/_static/image10.png ""Lorem ipsum" automatisch generiert")

    *"Lorem ipsum" automatisch generiert*

    > [!NOTE]
    > Im Rahmen der Zen-Codierung können Sie jetzt den Lorem ipsum-Code direkt im HTML-Editor generieren. Geben Sie einfach " **Lorem** " und "Treffer **Registerkarte** " ein, und ein 30-Wort-ipsum-Text wird eingefügt. Beispiel: *lorem10* fügt 10 Lorem ipsum-Wörter ein.
10. Sie fügen am Anfang der Frage ein Logo hinzu, indem Sie ein weiteres neues Feature in Web Essentials mit dem Namen " **Lorem Pixel Generator**" verwenden. Fügen Sie den folgenden Code als erstes untergeordnetes Element des **div** -Elements mit **Container** als **Klassen** Wert hinzu, und drücken Sie die **Tab**-Taste.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. Der Code sollte in HTML erweitert werden.

    ![Lorem-Pixel automatisch generiert](visual-studio-2013-web-tools/_static/image11.png "Lorem-Pixel automatisch generiert")

    *Lorem-Pixel automatisch generiert*

    > [!NOTE]
    > Im Rahmen der Zen-Codierung können Sie auch den Lorem-pixelcode direkt im HTML-Editor generieren. Geben Sie einfach **pix-200 x 200-animals** ein, und klicken Sie auf die **Registerkarte** , und ein **IMG** -Tag mit einem 200 x 200-Bild eines Tieres wird eingefügt. Weitere Informationen finden Sie unter [Lorem Pixel](http://www.lorempixel.com).
12. Klicken Sie auf die Schaltfläche Verbindungs **Browser aktualisieren** , um beide Browser zu aktualisieren.

    ![Internet Explorer: automatisch generiertes Bild und Text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer: automatisch generiertes Bild und Text")

    *Internet Explorer: automatisch generiertes Bild und Text*

    ![Google Chrome-automatisch generiertes Bild und Text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome-automatisch generiertes Bild und Text")

    *Google Chrome-automatisch generiertes Bild und Text*

    > [!NOTE]
    > Da das Bild beim Hinzufügen des Code Ausschnitts zufällig ausgewählt wird, kann sich das in den Browsern angezeigte Bild unterscheiden.
13. Schließen Sie die Browser nicht. Diese werden in der nächsten Aufgabe verwendet.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Aufgabe 3: Aktualisieren einer Style-Eigenschaft

In dieser Aufgabe verwenden Sie die Funktion zum über **prüfen** des Browsers, um den genauen Speicherort zu ermitteln, an dem das jeweilige DOM-Element generiert wird, und aktualisieren anschließend die Color-Eigenschaft dieses Elements mithilfe einer Farbauswahl, die von Web Essentials bereitgestellt wird.

1. Drücken Sie im Internet Explorer-Browser **STRG** + **alt** + **I** , um den Überprüfungs Modus zu aktivieren.
2. Bewegen Sie den Mauszeiger über den hellen blauen Rahmen, und klicken Sie auf.

    ![Bewegen des Zeigers über den hellen blauen Rahmen](visual-studio-2013-web-tools/_static/image14.png "Bewegen des Zeigers über den hellen blauen Rahmen")

    *Bewegen des Zeigers über den hellen blauen Rahmen*
3. Wechseln Sie zurück zu Visual Studio. Beachten Sie, dass das HTML-Element, das Sie im Browser ausgewählt haben, auch im HTML-Editor von Visual Studio ausgewählt ist.

    ![Im HTML-Editor von Visual Studio ausgewähltes HTML-Element](visual-studio-2013-web-tools/_static/image15.png "Im HTML-Editor von Visual Studio ausgewähltes HTML-Element")

    *Im HTML-Editor von Visual Studio ausgewähltes HTML-Element*
4. Sie aktualisieren nun die **Front** -CSS-Klasse, um die Formatierung des ausgewählten Elements zu ändern. Drücken Sie hierzu **STRG** +  **,** um das Suchfeld **zu** suchen. Geben Sie **Site. CSS** ein und drücken **Sie die Eingabe** Taste, um die Datei zu öffnen

    ![Datei Site. CSS wird geöffnet](visual-studio-2013-web-tools/_static/image16.png "Datei Site. CSS wird geöffnet")

    *Datei Site. CSS wird geöffnet*
5. Drücken Sie **STRG** + **F** , und geben Sie **. Flip-Container. Front** ein, um die CSS-Auswahl zu suchen.
6. Klicken Sie in der Border-Eigenschaft der-Klasse auf das helle blaue Quadrat, um die Farbauswahl zu öffnen.

    ![Öffnen der Farbauswahl](visual-studio-2013-web-tools/_static/image17.png "Öffnen der Farbauswahl")

    *Öffnen der Farbauswahl*
7. Erweitern Sie die Farbauswahl, indem Sie auf die Chevron-Schaltfläche klicken und eine neue Farbe auswählen.

    ![Erweitern der Farbauswahl](visual-studio-2013-web-tools/_static/image18.png "Erweitern der Farbauswahl")

    *Erweitern der Farbauswahl*
8. Drücken Sie **STRG** + **alt** + **Eingabe** Taste, um verknüpfte Browser zu aktualisieren.
9. Wechseln Sie zu Internet Explorer, und beachten Sie, dass sich die Farbe des Rahmens geändert hat.

    ![Internet Explorer-Rahmenfarbe aktualisiert](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer-Rahmenfarbe aktualisiert")

    *Internet Explorer-Rahmenfarbe aktualisiert*
10. Wechseln Sie zu Google Chrome, und beachten Sie, dass sich die Farbe des Rahmens geändert hat.

    ![Google Chrome-Rahmenfarbe aktualisiert](visual-studio-2013-web-tools/_static/image20.png "Google Chrome-Rahmenfarbe aktualisiert")

    *Google Chrome-Rahmenfarbe aktualisiert*
11. Wechseln Sie zurück zu Visual Studio.
12. Wechseln Sie zum Ende der Datei " **Site. CSS** ", und drücken Sie **STRG** + **F** , um die **. BTN** -Auswahl zu suchen.
13. Beachten Sie, dass die Eigenschaft **-webkit-border-radius** grün unterstrichen ist.

    ![-webkit-border-Radius-Eigenschaft des BTN-Selektor](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-Radius-Eigenschaft des BTN-Selektor")

    *-webkit-border-Radius-Eigenschaft des BTN-Selektor*
14. Platzieren Sie die Einfügemarke in der **-webkit-border-radius-** Eigenschaft. Unter dem ersten Buchstaben des ersten Worts der Eigenschaft sollte eine blaue Linie angezeigt werden. Dies ist das **Smarttag**.
15. Drücken Sie die **STRG** - +  **.** Öffnen Sie das Vorschlags Menü, und klicken Sie auf **Fehlende Standard Eigenschaft hinzufügen (border-radius)** .

    ![Fehlenden Standard Eigenschafts Vorschlag hinzufügen](visual-studio-2013-web-tools/_static/image22.png "Fehlenden Standard Eigenschafts Vorschlag hinzufügen")

    *Fehlenden Standard Eigenschafts Vorschlag hinzufügen*
16. Die **Border-RADIUS-** Eigenschaft wird der CSS-Regel automatisch hinzugefügt.

    ![Fehlende Standard Eigenschaft wurde hinzugefügt.](visual-studio-2013-web-tools/_static/image23.png "Fehlende Standard Eigenschaft wurde hinzugefügt.")

    *Fehlende Standard Eigenschaft wurde hinzugefügt.*
17. Bewegen Sie den Mauszeiger über die **Border-RADIUS-** Eigenschaft, um die **Browser Matrix**-QuickInfo anzuzeigen. Die **Browser Matrix** -QuickInfo zeigt die Verfügbarkeit der Eigenschaft in jedem Browser an.

    ![Browser Matrix-QuickInfo](visual-studio-2013-web-tools/_static/image24.png "Browser Matrix-QuickInfo")

    *Browser Matrix-QuickInfo*
18. Beachten Sie, dass der Wert der **Border-RADIUS-** Eigenschaft immer noch unterstrichen ist. Bewegen Sie den Zeiger auf den Wert, um die Warnmeldung anzuzeigen.

    ![Eigenschaft Wert für Border-Radius-Eigenschaft](visual-studio-2013-web-tools/_static/image25.png "Eigenschaft Wert für Border-Radius-Eigenschaft")

    *Eigenschaft Wert für Border-Radius-Eigenschaft*
19. Entfernen Sie die Einheit des **Border-RADIUS-** Eigenschafts Werts, wie von der QuickInfo vorgeschlagen.
20. Da **Border-RADIUS** die Standard Eigenschaft für die Definition von abgerundeten Rahmen Ecken ist, können Sie die **-webkit-border-radius** -Eigenschaft und den Wert aus der CSS-Regel entfernen.
21. Platzieren Sie die **Einfügemarke in der Word-Wrap-** Eigenschaft, und beachten Sie, dass das Smarttag auch unten angezeigt wird.
22. Öffnen Sie das Menü, und klicken Sie auf **fehlende Hersteller Spezifikationen hinzufügen**.

    ![Vorschlag für fehlende Hersteller Besonderheiten hinzufügen](visual-studio-2013-web-tools/_static/image26.png "Vorschlag für fehlende Hersteller Besonderheiten hinzufügen")

    *Vorschlag für fehlende Hersteller Besonderheiten hinzufügen*
23. Die **-MS-Word-Wrap** -Eigenschaft wird der CSS-Regel automatisch hinzugefügt.

    ![Anbieterspezifische Eigenschaft hinzugefügt](visual-studio-2013-web-tools/_static/image27.png "Anbieterspezifische Eigenschaft hinzugefügt")

    *Anbieterspezifische Eigenschaft hinzugefügt*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Aufgabe 4: Aktualisieren des HTML-Codes aus dem Browser

In dieser Aufgabe verwenden Sie die **Entwurfs Modus** -Funktion des Browser Links, um das DOM-Objekt aus dem Browser zu bearbeiten und die Änderungen an die HTML-Quelldatei in Visual Studio zu übertragen.

1. Drücken Sie in Google Chrome **STRG** + **alt** + **D** , um den Entwurfs Modus zu aktivieren.
2. Bewegen Sie den Mauszeiger über die Bezeichnung " **Lorem ipsum dolor sit amet** ", und klicken Sie auf.

    ![Bearbeiten der Frage](visual-studio-2013-web-tools/_static/image28.png "Bearbeiten der Frage")

    *Bearbeiten der Frage*
3. Ein Cursor sollte angezeigt werden. Ersetzen Sie den ursprünglichen Text durch das *aussehen, wenn ich eine längere Frage schreibe?* , und drücken Sie dann **ESC** , um den Entwurfs Modus zu beenden.

    ![Frage bearbeitet](visual-studio-2013-web-tools/_static/image29.png "Frage bearbeitet")

    *Frage bearbeitet*
4. Wechseln Sie zurück zu Visual Studio, und öffnen Sie die Datei **Index. cshtml**, sofern diese nicht bereits geöffnet ist. Beachten Sie, dass der innere Text des **&lt;p-&gt;** Elements aktualisiert wurde.

    ![Aktualisierte Frage auf der HTML-Seite](visual-studio-2013-web-tools/_static/image30.png "Aktualisierte Frage auf der HTML-Seite")

    *Aktualisierte Frage auf der HTML-Seite*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Aufgabe 5: Überprüfen von SEO-bezogenen Warnungen

Bei der **Suchmaschinenoptimierung** (SEO) handelt es sich um den Prozess, bei dem der Rang einer Website in der Liste der Ergebnisse einer Suchmaschine höher ist. Je höher die Website Rangfolge ist, desto genauer wird Sie aufgelistet. je mehr Besucher die Website von der Suchmaschine erhalten. Web Essentials umfasst ein analytisches Tool, das HTML untersucht, die gefundenen Probleme meldet und Hilfe zur Behebung von Problemen bietet.

1. Wechseln Sie zum Menü **Ansicht** , und klicken Sie auf **Fehlerliste** , um das Fenster **Fehlerliste** zu öffnen.

    ![Im Menü "Ansicht" Fehlerliste](visual-studio-2013-web-tools/_static/image31.png "Im Menü "Ansicht" Fehlerliste")

    *Im Menü "Ansicht" Fehlerliste*
2. Beachten Sie, dass eine SEO-Warnung mit dem Hinweis angezeigt wird, dass ein **&lt;Meta&gt;** -Tag für die Seiten Beschreibung fehlt. Doppelklicken Sie auf den Eintrag SEO Warning, um ihn zu beheben.

    ![Fehlerliste (Fenster)](visual-studio-2013-web-tools/_static/image32.png "Fehlerliste (Fenster)")

    *Fehlerliste (Fenster)*
3. Klicken Sie im Dialogfeld **Web Essentials** auf **Ja** , um eine Beschreibung &lt;Meta&gt;-Tags einzufügen.

    ![WebEssentials (Dialogfeld)](visual-studio-2013-web-tools/_static/image33.png "WebEssentials (Dialogfeld)")

    *WebEssentials (Dialogfeld)*
4. Der Editor für **\_Layout. cshtml** wird geöffnet, und das Tag **&lt;Meta&gt;** wird automatisch dem **Head** -Abschnitt der HTML-Datei hinzugefügt.

    ![Das Meta-Tag wurde automatisch auf _Layout Seite hinzugefügt.](visual-studio-2013-web-tools/_static/image34.png "Das Meta-Tag wurde automatisch auf _Layout Seite hinzugefügt.")

    *Das Meta-Tag wurde \_Layoutseite automatisch hinzugefügt*
5. Ändern Sie den Wert des **Content** -Attributs in *geekquiz* , und speichern Sie die Datei.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Übung 2: Nutzen von Code Ausschnitten und IntelliSense

Mit Web Essentials wurde der HTML-Editor mit zusätzlicher Funktionalität erweitert. In dieser Übung werden einige neue Features angezeigt, die beim Entwickeln von Webanwendungen hilfreich sind.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Aufgabe 1: Verwenden von IntelliSense in HTML-Dokumenten

Das erste neue Feature, das in dieser Aufgabe angezeigt wird, wird als **dynamischer IntelliSense**bezeichnet. Dynamische IntelliSense liest andere Tags und Attribute, um die möglichen IDs abzuleiten, die Sie verwenden werden.

In dieser Aufgabe erstellen Sie ein neues HTML-Formular Element, das eine Bezeichnung und ein Eingabefeld enthält. Dann fügen Sie der Bezeichnung ein **for** -Attribut hinzu, um es an die Eingabe zu binden, und es werden IntelliSense-Vorschläge auf der Grundlage der IDs der Eingaben im Bereich angezeigt.

1. Öffnen Sie **Visual Studio Express 2013 für das Web** und die Projekt Mappe " **BEGIN. sln** " im Ordner " **Source/EX2-takingbegünstigeof codesnippeer-andintellisense/BEGIN** ". Alternativ können Sie mit der Lösung fortfahren, die Sie in der vorherigen Übung abgerufen haben.
2. Öffnen Sie in **Projektmappen-Explorer**die Datei " **Index. cshtml** ", die sich im Ordner " **views** | **Home** " befindet.
3. Fügen Sie das folgende Formular innerhalb des **&lt;Abschnitts&gt;** -Elements hinzu.

    (Code Ausschnitt- *VisualStudio2013WebTooling* - *ex2* - *Formular*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. Dem Eingabetag sollte eine Bezeichnung mit einer Beschreibung des Felds vorangestellt sein. Fügen Sie die folgende Bezeichnung vor dem Eingabetag hinzu.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. Das **for** -Attribut einer **&lt;Bezeichnung&gt;** gibt an, an welches Formular Element eine Bezeichnung gebunden ist. Der Wert des Attributs sollte gleich der ID des zugehörigen Elements sein. Fügen Sie das **for** -Attribut der **&lt;Bezeichnung&gt;** -Elements hinzu. Wie in der folgenden Abbildung gezeigt, wird der &quot;Name&quot; Wert im Feld IntelliSense angezeigt, basierend auf der ID der Elemente innerhalb desselben Bereichs (das einschließende **&lt;Formular&gt;** ).

    ![Anzeige der ID in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Anzeige der ID in IntelliSense")

    *Anzeige der ID in IntelliSense*
6. Löschen Sie das zuletzt hinzugefügte **&lt;Formular&gt;** Element und dessen Inhalt.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Aufgabe 2: Verwenden von HTML-Code Ausschnitten

HTML5 hat mehr als 25 neue Semantik Tags eingeführt. Visual Studio verfügte bereits über IntelliSense-Unterstützung für diese Tags, Visual Studio 2013 jedoch das Schreiben von Markup durch Hinzufügen neuer Code Ausschnitte schneller und einfacher. Obwohl diese Tags nicht kompliziert sind, sind Sie mit einigen kleinen Feinheiten verknüpft, wie z. b. das Hinzufügen der richtigen Codec-Fallbacks für das *audiotag* . In dieser Aufgabe werden die HTML-Code Ausschnitte für das audiotag angezeigt.

1. Geben Sie in der Datei **Index. cshtml** **&lt;AUD** innerhalb des **&lt;Abschnitts&gt;** -Element ein, wie in der folgenden Abbildung dargestellt.

    ![Einfügen eines Audioelements](visual-studio-2013-web-tools/_static/image36.png "Einfügen eines Audioelements")

    *Einfügen eines Audioelements*
2. Drücken Sie zweimal die **Tab** -Taste, und beachten Sie, dass der folgende Code auf der Seite hinzugefügt wird und der Cursor auf dem **src** -Attribut der ersten Quelle platziert wird.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Wenn Sie die **Tab** -Taste zweimal drücken, wird der Code Ausschnitt eingefügt. Der Audioausschnitt zeigt die Standardverwendung des *Audiotags* mit zwei Quelldateien für eine verbesserte Unterstützung.
3. Löschen Sie die zweite Zeile, und aktualisieren Sie die Quelle der ersten Zeile mit dem folgenden Link zum webcampstv Katana Show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Der resultierende Code ist unten dargestellt.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Die Quelldatei " *katanaproject. MP3* " wird als Beispiel verwendet. Wenn Sie möchten, können Sie eine andere Quelle verwenden.
4. Drücken Sie **STRG** + **S** , um die Datei zu speichern.
5. Drücken Sie **STRG** + **F5** , um die Anwendung zu starten.
6. Beachten Sie, dass der Anwendung ein Audioplayer hinzugefügt wurde.

    ![Audioplayer in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audioplayer in Internet Explorer")

    *Audioplayer in Internet Explorer*

    ![Audioplayer in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audioplayer in Google Chrome")

    *Audioplayer in Google Chrome*
7. Schließen Sie die Browser nicht. Diese werden in der nächsten Aufgabe verwendet.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Aufgabe 3: Verwenden von IntelliSense in JavaScript-Dokumenten

Mit Web Essentials 2013 werden in Stylesheets und HTML-Seiten eine Liste von IDs und Klassennamen erzeugt. In dieser Aufgabe erfahren Sie, wie diese Listen die Unterstützung von JavaScript IntelliSense in Web Essentials 2013 verbessern.

1. Fügen Sie in der Datei " **Index. cshtml** " den folgenden Code hinzu, um ein **Skripttag** für JavaScript-Code zu definieren.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Fügen Sie den folgenden Code innerhalb des **Skripttags** hinzu, um die fertige Rückruffunktion zu definieren.

    (Code Ausschnitt- *VisualStudio2013WebTooling* - *ex2* - - *Funktion*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Platzieren Sie die Einfügemarke im **Skripttags** , und drücken Sie **STRG** +  **.** , um das Vorschlags Menü zu öffnen.
4. Klicken Sie **auf in Datei extrahieren**.

    ![Vorschlag zum Extrahieren von JavaScript in Dateien](visual-studio-2013-web-tools/_static/image39.png "Vorschlag zum Extrahieren von JavaScript in Dateien")

    *Vorschlag zum Extrahieren von JavaScript in Dateien*
5. Wählen Sie im Fenster **Speichern** unter den Ordner **Skripts** aus, benennen Sie die Datei **init. js** , und klicken Sie auf **Speichern**.

    ![Als Fenster speichern](visual-studio-2013-web-tools/_static/image40.png "Als Fenster speichern")

    *Als Fenster speichern*

    > [!NOTE]
    > Die Datei " **init. js** " wird erstellt, und der Inhalt des Skripts wird in die Datei verschoben.
    > 
    > ![Init. js-Datei, die mit dem enthaltenen Inhalt erstellt wurde](visual-studio-2013-web-tools/_static/image41.png "Init. js-Datei, die mit dem enthaltenen Inhalt erstellt wurde")
    > 
    > *Init. js-Datei, die mit dem enthaltenen Inhalt erstellt wurde*
6. Öffnen Sie die Datei **Index. cshtml** , und überprüfen Sie, ob das Skript-Tag durch einen Verweis auf die Datei **init. js** ersetzt wurde.

    ![Init. js-html-Referenz](visual-studio-2013-web-tools/_static/image42.png "Init. js-html-Referenz")

    *Init. js-html-Referenz*
7. Wechseln Sie zum **Projektmappen-Explorer** , und beachten Sie, dass die Datei " **init. js** " automatisch in der Lösung enthalten war.

    ![In der Lösung enthaltene Datei "init. js"](visual-studio-2013-web-tools/_static/image43.png "In der Lösung enthaltene Datei "init. js"")

    *In der Lösung enthaltene Datei "init. js"*
8. Wechseln Sie zurück zur Datei " **init. js** ", um den **Ready** -Funktions Rückruf zu aktualisieren.
9. Fügen Sie in der Funktions Rückruf Definition, die an *Ready*zurückgegeben wird, den folgenden Code hinzu, um alle Elemente nach einem bestimmten Klassen Attribut zu erhalten.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Drücken Sie **STRG** + **LEERTASTE** zwischen den Anführungszeichen innerhalb des **getElementsByClassName** -Funktions Aufrufes.

    ![IntelliSense für die getElementsByClassName-Funktion wird angezeigt.](visual-studio-2013-web-tools/_static/image44.png "IntelliSense für die getElementsByClassName-Funktion wird angezeigt.")

    *IntelliSense für die getElementsByClassName-Funktion wird angezeigt.*

    > [!NOTE]
    > Beachten Sie, dass IntelliSense die Klassen anzeigt, die in den Projekt Stylesheets definiert sind.
11. Ersetzen Sie die Zeile, die Sie erstellt haben, durch den folgenden Code.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Positionieren Sie den Cursor nach **au** innerhalb der Anführungszeichen in der **GetElementsByTagName** -Funktion, und drücken Sie **STRG** + **LEERTASTE**.

    ![IntelliSense für die getelementbytagname-Methode wird angezeigt.](visual-studio-2013-web-tools/_static/image45.png "IntelliSense für die getelementbytagname-Methode wird angezeigt.")

    *IntelliSense für die GetElementsByTagName-Methode wird angezeigt.*
13. Wählen Sie **&quot;Audio&quot;** aus der Liste, und drücken **Sie die Eingabe**Taste. Das Ergebnis ist in der folgenden Abbildung dargestellt.

    ![Abrufen von Audioelementen](visual-studio-2013-web-tools/_static/image46.png "Abrufen von Audioelementen")

    *Abrufen von Audioelementen*
14. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Datei " **init. js** " im Ordner " **Scripts** ", und wählen Sie im Menü **Web Essentials** die Option **Minify JavaScript File (s)** aus.

    ![JavaScript-Datei (en) minimieren](visual-studio-2013-web-tools/_static/image47.png "Minimieren von JavaScript-Dateien")

    *JavaScript-Datei (en) minimieren*
15. Wenn Sie zur Aktivierung der automatischen Minimierung aufgefordert werden, wenn sich die Quelldatei ändert, klicken Sie auf **Ja**.

    ![Aktivieren der automatischen minierungs Warnung](visual-studio-2013-web-tools/_static/image48.png "Aktivieren der automatischen minierungs Warnung")

    *Aktivieren der automatischen minierungs Warnung*

    > [!NOTE]
    > Die Datei " **init. min. js** " wird erstellt und als Abhängigkeit von der Datei " **init. js** " hinzugefügt.
    > 
    > ![Init. min. js-Datei erstellt](visual-studio-2013-web-tools/_static/image49.png "Init. min. js-Datei erstellt")
    > 
    > *Init. min. js-Datei erstellt*
16. Öffnen Sie die Datei " **init. min. js** ", und beachten Sie, dass die Datei minimiert ist.

    ![Inhalt der Datei "init. min. js"](visual-studio-2013-web-tools/_static/image50.png "Inhalt der Datei "init. min. js"")

    *Inhalt der Datei "init. min. js"*
17. Fügen Sie in der Datei **init. js** den folgenden Code unterhalb des **GetElementsByTagName** -Funktions Aufrufes ein, um alle Audioelemente wiederzugeben.

    (Code Ausschnitt- *VisualStudio2013WebTooling* - *ex2* - *playaudioelements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Klicken Sie auf **STRG** + **S** , um die Datei zu speichern. Da die minierte Datei bereits geöffnet ist, wird ein Dialogfeld angezeigt, das besagt, dass die Datei außerhalb des Quell-Editors geändert wurde. Klicken Sie auf **Ja**.

    ![Microsoft Visual Studio Warnung](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio Warnung")

    *Microsoft Visual Studio Warnung*
19. Wechseln Sie zurück zur Datei " **init. min. js** ", um zu überprüfen, ob die Datei mit dem neuen Code aktualisiert wurde.

    ![Die Datei "init. min. js" wurde aktualisiert.](visual-studio-2013-web-tools/_static/image52.png "Die Datei "init. min. js" wurde aktualisiert.")

    *Die Datei "init. min. js" wurde aktualisiert.*
20. Klicken Sie auf die Schaltfläche zum **Aktualisieren des Browser Links** .
21. Nachdem beide Browser aktualisiert wurden, werden die Audioplayer, die Sie in der vorherigen Aufgabe gesehen haben, automatisch wiedergegeben.

    ![In der Ansicht enthaltene Audioplayer](visual-studio-2013-web-tools/_static/image53.png "In der Ansicht enthaltene Audioplayer")

    *In der Ansicht enthaltene Audioplayer*

---

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Durch die Durchführung dieses praktischen Labs haben Sie Folgendes gelernt:

- Verwenden neuer HTML-Editor-Funktionen, die in Web Essentials enthalten sind, z. b. umfangreiche HTML5-Code Ausschnitte
- Verwenden neuer CSS-Editor-Features in WebEssentials, z. b. Farbauswahl und Browser Matrix-QuickInfo
- Verwenden neuer JavaScript-Editor-Funktionen in WebEssentials, z. b. Extrahieren in Dateien und IntelliSense für alle HTML-Elemente
- Austauschen von Daten zwischen Ihrem Browser und Visual Studio mithilfe von Browser Link
