---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Verwenden von Visual Studio 2013 zum Erstellen einer ASP.net-Basis Seite Web Forms 4,5
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 5d13a51128eecd92a82cfd06054448582a348e11
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78511077"
---
# <a name="using-visual-studio-2013-to-create-a-basic-aspnet-45-web-forms-page"></a>Verwenden von Visual Studio 2013 zum Erstellen einer ASP.net-Basis Seite Web Forms 4,5

von [Erik Reitan](https://github.com/Erikre)

[!INCLUDE[](~/includes/rp.md)]

Diese exemplarische Vorgehensweise bietet eine Einführung in die Webentwicklungs Umgebung in [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) und in [Microsoft Visual Studio Express 2013 für das Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Diese exemplarische Vorgehensweise führt Sie durch die Erstellung einer einfachen ASP.net-Web Forms Seite und veranschaulicht die grundlegenden Techniken zum Erstellen einer neuen Seite, Hinzufügen von Steuerelementen und Schreiben von Code.

In dieser exemplarischen Vorgehensweise werden u. a. folgende Aufgaben veranschaulicht:

- Erstellen eines Dateisystems Web Forms Anwendungsprojekt.
- Machen Sie sich mit Visual Studio vertraut.
- Erstellen einer ASP.NET-Seite.
- Hinzufügen von Steuerelementen
- Hinzufügen von Ereignis Handlern.
- Ausführen und Testen einer Seite aus Visual Studio.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung dieser exemplarischen Vorgehensweise benötigen Sie Folgendes:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) oder [Microsoft Visual Studio Express 2013 für das Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Der .NET Framework wird automatisch installiert. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 und Microsoft Visual Studio Express 2013 für das Web werden in dieser tutorialreihe häufig als Visual Studio bezeichnet.  
    >   
    > Wenn Sie Visual Studio verwenden, wird in dieser exemplarischen Vorgehensweise davon ausgegangen, dass Sie die **Webentwicklungs** Sammlung von Einstellungen beim ersten Start von Visual Studio ausgewählt haben. Weitere Informationen finden Sie unter Gewusst [wie: Auswählen von Einstellungen für die Webentwicklungs Umgebung](https://msdn.microsoft.com/library/ff521558.aspx).

## <a name="creating-a-web-application-project-and-a-page"></a>Erstellen eines Webanwendungs Projekts und einer Seite

<a id="sectionToggle0"></a>

In diesem Teil der exemplarischen Vorgehensweise erstellen Sie ein Webanwendungs Projekt und fügen ihm eine neue Seite hinzu. Außerdem fügen Sie HTML-Text hinzu und führen die Seite in Ihrem Browser aus.

### <a name="to-create-a-web-application-project"></a>So erstellen Sie ein Webanwendungs Projekt

1. Öffnen Sie Microsoft Visual Studio.
2. Wählen Sie im Menü **Datei** die Option **Neues Projekt** aus.  
    Menü "Datei !["](creating-a-basic-web-forms-page/_static/image1.png)

    Das Dialogfeld **Neues Projekt** wird angezeigt.
3. Wählen Sie auf der linken Seite die **Vorlagen** -&gt;  **C# Visual** -&gt; **Webvorlagen** Gruppe aus.
4. Wählen Sie die Vorlage **ASP.NET Web Application** in der mittleren Spalte aus.
5. Nennen Sie das Projekt ***basicwebapp*** , und klicken Sie auf die Schaltfläche **OK** .   
![Dialogfeld „Neues Projekt“](creating-a-basic-web-forms-page/_static/image2.png)
6. Wählen Sie dann die Vorlage **Web Forms** aus, und klicken Sie auf die Schaltfläche **OK** , um das Projekt zu erstellen.  
![Dialogfeld "Neues ASP.NET-Projekt"](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio erstellt ein neues Projekt, das eine vorgefertigte Funktionalität enthält, die auf der Web Forms Vorlage basiert. Es bietet Ihnen nicht nur die Seite " *Home. aspx* ", eine Seite " *about. aspx* ", eine " *Contact. aspx* "-Seite, sondern auch Mitgliedschafts Funktionen, die Benutzer registrieren und ihre Anmelde Informationen speichern, damit Sie sich bei Ihrer Website anmelden können. Wenn eine neue Seite erstellt wird, zeigt Visual Studio die Seite standardmäßig in der **Quell** Ansicht an, wo Sie die HTML-Elemente der Seite sehen können. In der folgenden Abbildung wird gezeigt, was in der **Quell** Ansicht angezeigt wird, wenn Sie eine neue Webseite mit dem Namen " *basicwebapp. aspx*" erstellt haben.  
    ![Quellansicht](creating-a-basic-web-forms-page/_static/image4.png)

### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Eine Tour durch die Visual Studio-Webentwicklungs Umgebung

Bevor Sie fortfahren, indem Sie die Seite ändern, ist es sinnvoll, sich mit der Visual Studio-Entwicklungsumgebung vertraut zu machen. Die folgende Abbildung zeigt die Fenster und Tools, die in Visual Studio verfügbar sind, und Visual Studio Express für das Web.

> [!NOTE] 
> 
> Dieses Diagramm zeigt die Standardfenster und Fenster Fenster. Im Menü **Ansicht** können Sie weitere Fenster anzeigen und die Fenster Fenster neu anordnen und die Größe ändern, um Ihre Einstellungen anzupassen. Wenn an der Fensteranordnung bereits Änderungen vorgenommen wurden, werden die Ergebnisse nicht mit der Abbildung angezeigt.

 Die Visual Studio-Umgebung

![Visual Studio-Umgebung](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Machen Sie sich mit dem Webdesigner vertraut.

Sehen Sie sich die obige Abbildung an, und vergleichen Sie den Text mit der folgenden Liste, in der die am häufigsten verwendeten Fenster und Tools beschrieben werden. (Nicht alle Fenster und Tools, die Sie sehen, sind hier aufgelistet, nur die in der obigen Abbildung markierten.)

- Symbolleisten. Stellen Sie Befehle zum Formatieren von Text, zum Suchen nach Text usw. bereit. Einige Symbolleisten sind nur verfügbar, wenn Sie in der **Entwurfs** Ansicht arbeiten.
- **Projektmappen-Explorer** Fenster. Zeigt die Dateien und Ordner in der Webanwendung an.
- Dokument Fenster. Zeigt die Dokumente an, an denen Sie in Fenstern im Registerkarten Format arbeiten. Sie können zwischen den Dokumenten wechseln, indem Sie auf Registerkarten klicken.
- **Eigenschaften** Fenster. Ermöglicht das Ändern von Einstellungen für die Seite, HTML-Elemente, Steuerelemente und andere Objekte.
- Registerkarten anzeigen. Präsentieren Sie verschiedene Ansichten desselben Dokuments. Die **Entwurfs** Ansicht ist eine fast-WYSIWYG-Bearbeitungs Oberfläche. Die **Quell** Ansicht ist der HTML-Editor für die Seite. In der **geteilten** Ansicht werden die **Entwurfs** Ansicht und die **Quell** Ansicht für das Dokument angezeigt. Sie arbeiten später in dieser exemplarischen Vorgehensweise mit den **Entwurfs** -und **Quell** Ansichten. Wenn Sie Webseiten **in der** **Entwurfs** Ansicht öffnen möchten, klicken Sie im Menü Extras auf **Optionen**, wählen Sie den Knoten **HTML-Designer** aus, und ändern Sie die Option **Start Seiten in** .
- **Toolbox**. Stellt Steuerelemente und HTML-Elemente bereit, die Sie auf die Seite ziehen können. **Toolbox** Elemente werden nach allgemeiner Funktion gruppiert.
- S **erver-Explorer**. Zeigt Datenbankverbindungen an. Wenn Server-Explorer nicht sichtbar ist, klicken Sie im Menü Ansicht auf Server-Explorer.

### <a name="creating-a-new-aspnet-web-forms-page"></a>Erstellen einer neuen ASP.net-Web Forms Seite

Wenn Sie eine neue Web Forms Anwendung mithilfe der Projektvorlage " **ASP.NET-Webanwendung** " erstellen, fügt Visual Studio eine ASP.NET-Seite (Web Forms Seite) namens " *default. aspx*" sowie mehrere andere Dateien und Ordner hinzu. Sie können die Seite *default. aspx* als Startseite für Ihre Webanwendung verwenden. In dieser exemplarischen Vorgehensweise erstellen Sie jedoch eine neue Seite und arbeiten damit.

### <a name="to-add-a-page-to-the-web-application"></a>So fügen Sie der Webanwendung eine Seite hinzu

1. Schließen Sie die Seite *default. aspx* . Klicken Sie hierzu auf die Registerkarte, auf der der Dateiname angezeigt wird, und klicken Sie dann auf die Option schließen.
2. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Namen der Webanwendung (in diesem Tutorial lautet der Name der Anwendung **basicwebsite**), und klicken Sie dann auf -&gt; **Neues Element** **Hinzufügen** .   
Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
3. Wählen Sie auf der linken Seite die Gruppe **Visual C#**  -&gt; **Web** Templates aus. Wählen Sie dann in der mittleren Liste **Web Form** aus, und nennen Sie es " *firstwebseite. aspx*".   
    ![Dialogfeld „Neues Element hinzufügen“](creating-a-basic-web-forms-page/_static/image6.png)
4. Klicken Sie auf **Hinzufügen** , um die Webseite dem Projekt hinzuzufügen.  
Visual Studio erstellt die neue Seite und öffnet sie.

### <a name="adding-html-to-the-page"></a>Hinzufügen von HTML zur Seite

In diesem Teil der exemplarischen Vorgehensweise fügen Sie der Seite einen statischen Text hinzu.

### <a name="to-add-text-to-the-page"></a>So fügen Sie der Seite Text hinzu

1. Klicken Sie am unteren Rand des Dokument Fensters auf die Registerkarte **Entwurf** , um zur **Entwurfs** Ansicht zu wechseln.

    Designansicht zeigt die aktuelle Seite auf WYSIWYG-like-Weise an. An diesem Punkt verfügen Sie über keinen Text oder Steuerelemente auf der Seite. die Seite ist also leer, mit Ausnahme einer gestrichelten Linie, die ein Rechteck umreißt. Dieses Rechteck stellt ein **div** -Element auf der Seite dar.
2. Klicken Sie innerhalb des Rechtecks, das durch eine gestrichelte Linie dargestellt wird.
3. Geben Sie **Willkommen bei Visual Web Developer ein,** und drücken **Sie die Eingabe** Taste.

    Die folgende Abbildung zeigt den Text, den Sie in der **Entwurfs** Ansicht eingegeben haben.

    ![Begrüßungstext in Designansicht](creating-a-basic-web-forms-page/_static/image7.png "Begrüßungstext in Designansicht")
4. Wechseln Sie zur **Quell** Ansicht.

    Sie können den HTML-Code in der **Quell** Ansicht sehen, den Sie bei der Typisierung in der **Entwurfs** Ansicht erstellt haben.  
    ![Webseite mit statischem Text](creating-a-basic-web-forms-page/_static/image8.png)

### <a name="running-the-page"></a>Ausführen der Seite

Bevor Sie fortfahren, indem Sie der Seite Steuerelemente hinzufügen, können Sie Sie zuerst ausführen.

### <a name="to-run-the-page"></a>So führen Sie die Seite aus

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf *firstwebseite. aspx* , und wählen Sie **als Start Seite festlegen**aus.
2. Drücken Sie **STRG + F5** , um die Seite auszuführen.

    Die Seite wird im Browser angezeigt. Obwohl die Seite, die Sie erstellt haben, die Dateinamenerweiterung " *. aspx*" aufweist, wird Sie zurzeit wie jede beliebige HTML-Seite ausgeführt.

    Zum Anzeigen einer Seite im Browser können Sie auch in **Projektmappen-Explorer** mit der rechten Maustaste auf die Seite klicken und **in Browser anzeigen**auswählen.
3. Schließen Sie den Browser, um die Webanwendung zu beenden.

## <a name="adding-and-programming-controls"></a>Hinzufügen und Programmier Steuerelemente

<a id="sectionToggle1"></a>

Nun fügen Sie der Seite Server Steuerelemente hinzu. Server Steuerelemente, z. b. Schaltflächen, Bezeichnungen, Textfelder und andere vertraute Steuerelemente, stellen typische Funktionen zur Formular Verarbeitung für Ihre Web Forms Seiten bereit. Allerdings können Sie die Steuerelemente mit Code programmieren, der auf dem Server ausgeführt wird, und nicht auf dem Client.

Sie fügen der Seite ein [Schalt](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Flächen-Steuerelement, ein [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) -Steuerelement und ein [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) -Steuerelement hinzu und schreiben Code, um das [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) -Ereignis für das [Schalt](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Flächen-Steuerelement zu verarbeiten.

### <a name="to-add-controls-to-the-page"></a>So fügen Sie der Seite Steuerelemente hinzu

1. Klicken Sie auf die Registerkarte **Entwurf** , um zur **Entwurfs** Ansicht zu wechseln.
2. Platzieren Sie die Einfügemarke am Ende der Willkommensseite in **Visual Web Developer** Text, und drücken **Sie die Eingabe** fünfmal oder mehrmals, um im Feld **div** -Element einen Platz zu schaffen.
3. Erweitern Sie in der **Toolbox**die **Standard** Gruppe, wenn Sie nicht bereits erweitert ist.  
Beachten Sie, dass Sie möglicherweise das Fenster " **Toolbox** " auf der linken Seite erweitern müssen, um es anzuzeigen.
4. Ziehen Sie ein [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) -Steuerelement auf die Seite, und legen Sie es in der Mitte des **div** -Element Felds ab, das in der ersten Zeile **Willkommen bei Visual Web Developer** angezeigt wird.
5. Ziehen Sie ein [Schalt](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Flächen-Steuerelement auf die Seite, und legen Sie es rechts neben dem [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) -Steuerelement ab.
6. Ziehen Sie ein [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) -Steuerelement auf die Seite, und legen Sie es in einer separaten Zeile unterhalb des [Schalt](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Flächen Steuer Elements ab.
7. Legen Sie die Einfügemarke oberhalb des [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) -Steuer Elements ab, und **Geben Sie dann Ihren Namen ein:** .

    Dieser statische HTML-Text ist die Beschriftung für das [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) -Steuerelement. Statische HTML-und Server Steuerelemente können auf derselben Seite gemischt werden. In der folgenden Abbildung wird gezeigt, wie die drei Steuerelemente in der **Entwurfs** Ansicht angezeigt werden.

    ![Drei Steuerelemente in Designansicht](creating-a-basic-web-forms-page/_static/image9.png "Drei Steuerelemente in Designansicht")

### <a name="setting-control-properties"></a>Festlegen von Steuerelement Eigenschaften

Visual Studio bietet Ihnen verschiedene Möglichkeiten, die Eigenschaften von Steuerelementen auf der Seite festzulegen. In diesem Teil der exemplarischen Vorgehensweise legen Sie Eigenschaften sowohl in der **Entwurfs** Ansicht als auch in der **Quell** Ansicht fest.

### <a name="to-set-control-properties"></a>So legen Sie Steuerelement Eigenschaften fest

1. Zeigen Sie zunächst die **Eigenschaften** Fenster an, indem Sie im Menü **Ansicht**&gt; **andere Fenster** -&gt; **Eigenschaften Fenster**auswählen. Alternativ können Sie **F4** auswählen, um das Fenster **Eigenschaften** anzuzeigen.
2. Wählen Sie das [Schalt](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Flächen-Steuerelement aus, und legen Sie dann im Fenster **Eigenschaften** den Wert von **Text** auf **Anzeige Name**fest. Der eingegebene Text wird auf der Schaltfläche im Designer angezeigt, wie in der folgenden Abbildung dargestellt.

    ![Schaltflächen Text festlegen](creating-a-basic-web-forms-page/_static/image10.png "Schaltflächen Text festlegen")
3. Wechseln Sie zur **Quell** Ansicht.

    In der **Quell** Ansicht wird der HTML-Code für die Seite angezeigt, einschließlich der Elemente, die von Visual Studio für die Server Steuerelemente erstellt wurden. Steuerelemente werden mit HTML-ähnlicher Syntax deklariert, mit dem Unterschied, dass die Tags das Präfix **ASP:** verwenden und das Attribut **runat =&quot;Server&quot;** einschließen.

    Steuerelement Eigenschaften werden als Attribute deklariert. Wenn Sie z. b. die [Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) -Eigenschaft für das [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) -Steuerelement festgelegt haben, haben Sie in Schritt 1 das **Text** -Attribut im Markup des-Steuer Elements festgelegt.

    > [!NOTE] 
    > 
    > Alle-Steuerelemente befinden sich in einem **Form** -Element, das auch das Attribut **runat =&quot;Server&quot;** hat. Das **runat =&quot;Server&quot;** -Attribut und das **ASP:** -Präfix für Steuerungs Tags markieren die Steuerelemente, sodass Sie von ASP.net auf dem Server verarbeitet werden, wenn die Seite ausgeführt wird. Code außerhalb **&lt;Formulars runat =&quot;Server&quot;&gt;** und **&lt;Skript runat =&quot;Server&quot;&gt;** Elemente werden unverändert an den Browser gesendet. aus diesem Grund muss sich der ASP.NET-Code innerhalb eines Elements befinden, dessen Starttag das **runat =&quot;Server&quot;** -Attribut enthält.
4. Als Nächstes fügen Sie dem [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) -Steuerelement eine zusätzliche Eigenschaft hinzu. Platzieren Sie die Einfügemarke direkt hinter **ASP: Label** im **&lt;ASP: Label&gt;** -Tag, und drücken Sie dann die **LEERTASTE**.

    Eine Dropdown Liste wird angezeigt, in der die Liste der verfügbaren Eigenschaften angezeigt wird, die Sie für ein [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) -Steuerelement festlegen können. Diese Funktion, die als **IntelliSense**bezeichnet wird, unterstützt Sie in der **Quell** Ansicht mit der Syntax von Server Steuerelementen, HTML-Elementen und anderen Elementen auf der Seite. Die folgende Abbildung zeigt die **IntelliSense** -Dropdown Liste für das [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) -Steuerelement.

    ![IntelliSense-Attribute](creating-a-basic-web-forms-page/_static/image11.png "IntelliSense-Attribute")
5. Wählen Sie **ForeColor** aus, und geben Sie ein Gleichheitszeichen ein.

    IntelliSense zeigt eine Liste von Farben an.

    > [!NOTE] 
    > 
    > Sie können jederzeit eine **IntelliSense** -Dropdown Liste anzeigen, indem Sie beim Anzeigen von Code **STRG + J** drücken.
6. Wählen Sie eine Farbe für den Text des Bezeichnungs **[Steuer Elements](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** aus. Stellen Sie sicher, dass Sie eine Farbe auswählen, die dunkel genug ist, um mit einem weißen Hintergrund zu lesen.

    Das **ForeColor** -Attribut wird mit der ausgewählten Farbe, einschließlich des schließenden Anführungs Zeichens, abgeschlossen.

### <a name="programming-the-button-control"></a>Programmieren des Button-Steuer Elements

In dieser exemplarischen Vorgehensweise schreiben Sie Code, der den Namen liest, den der Benutzer in das Textfeld eingibt, und zeigt dann den Namen im [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) -Steuerelement an.

### <a name="add-a-default-button-event-handler"></a>Standard Ereignishandler für Schaltflächen hinzufügen

1. Wechseln Sie zur **Entwurfs** Ansicht.
2. Doppelklicken Sie auf das [Schalt](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Flächen-Steuerelement.

    Standardmäßig wechselt Visual Studio zu einer Code Behind-Datei und erstellt einen Skeleton-Ereignishandler für das Standard Ereignis des [Schalt](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Flächen-Steuer Elements, das [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) -Ereignis. Die Code-Behind-Datei trennt Ihr Benutzeroberflächen Markup (z. b. html) von Ihrem Server C#Code (z. b.).   
   Der Cursor befindet sich im hinzugefügten Code für diesen Ereignishandler.

    > [!NOTE] 
    > 
    > Das Doppelklicken auf ein Steuerelement in der **Entwurfs** Ansicht ist nur eine von mehreren Möglichkeiten, Ereignishandler zu erstellen.
3. Geben **Sie im Button1-\_Click** -Ereignishandler **Label1** gefolgt von einem Zeitraum ( **.** ) ein.

    Wenn Sie den Zeitraum nach der **ID** der Bezeichnung (**Label1**) eingeben, zeigt Visual Studio eine Liste der verfügbaren Member für das [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) -Steuerelement an, wie in der folgenden Abbildung dargestellt. Ein Member, der häufig eine Eigenschaft, eine Methode oder ein Ereignis ist.

    ![IntelliSense in der Code Ansicht](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense in der Code Ansicht")
4. Beenden Sie den **Click** -Ereignishandler für die Schaltfläche, sodass er wie im folgenden Codebeispiel dargestellt liest.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Wechseln Sie zurück zum Anzeigen der **Quell** Ansicht Ihres HTML-Markups, indem Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf *firstwebseite. aspx* klicken und **Markup anzeigen**auswählen.
6. Führen Sie einen Bildlauf zum **&lt;ASP: Button&gt;** Element aus. Beachten Sie, dass das **&lt;ASP: Button&gt;** -Element jetzt das Attribut " **OnClick =&quot;Button1" aufweist\_klicken Sie auf&quot;** .

    Dieses Attribut bindet das [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) -Ereignis der Schaltfläche an die Handlermethode, die Sie im vorherigen Schritt programmiert haben.

    Ereignishandlermethoden können einen beliebigen Namen haben. der Name, den Sie sehen, ist der Standardname, der von Visual Studio erstellt wurde. Wichtig ist, dass der Name, der für das **OnClick** -Attribut in der HTML-Datei verwendet wird, mit dem Namen einer Methode, die im Code Behind definiert ist, identisch sein muss.

### <a name="running-the-page"></a>Ausführen der Seite

Sie können nun die Server Steuerelemente auf der Seite testen.

### <a name="to-run-the-page"></a>So führen Sie die Seite aus

1. Drücken Sie **STRG + F5** , um die Seite im Browser auszuführen. Wenn ein Fehler auftritt, überprüfen Sie die obigen Schritte.
2. Geben Sie einen Namen in das Textfeld ein, und klicken Sie auf die Schaltfläche **Anzeige Name** .

    Der eingegebene Name wird im [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) -Steuerelement angezeigt. Beachten Sie, dass die Seite beim Klicken auf die Schaltfläche an den Webserver gesendet wird. ASP.NET erstellt dann die Seite neu, führt Ihren Code aus (in diesem Fall wird der [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) -Ereignishandler des [Schalt](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Flächen-Steuer Elements ausgeführt) und sendet die neue Seite dann an den Browser. Wenn Sie sich die Statusleiste im Browser ansehen, sehen Sie, dass die Seite jedes Mal, wenn Sie auf die Schaltfläche klicken, einen Roundtrip zum Webserver durchführen kann.
3. Zeigen Sie im Browser die Quelle der Seite an, auf der Sie ausgeführt werden, indem Sie mit der rechten Maustaste auf die Seite klicken und **Quelltext anzeigen**auswählen.

    Im Quellcode der Seite wird HTML ohne Servercode angezeigt. Insbesondere wird der **&lt;ASP:&gt;** Elemente, mit denen Sie in der **Quell** Ansicht gearbeitet haben, nicht angezeigt. Wenn die Seite ausgeführt wird, verarbeitet ASP.net die Server Steuerelemente und rendert HTML-Elemente auf der Seite, auf der die Funktionen ausgeführt werden, die das Steuerelement darstellen. Beispielsweise wird das **&lt;ASP: Button&gt;** -Steuerelement als HTML- **&lt;Input Type =&quot;Submit&quot;&gt;** -Element gerendert.
4. Schließen Sie den Browser.

## <a name="working-with-additional-controls"></a>Arbeiten mit zusätzlichen Steuerelementen

<a id="sectionToggle2"></a>

In diesem Teil der exemplarischen Vorgehensweise arbeiten Sie mit dem [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement, mit dem Datumsangaben im Monat angezeigt werden. Das [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement ist ein komplexeres Steuerelement als die Schaltfläche, das Textfeld und die Bezeichnung, mit denen Sie gearbeitet haben, und es werden einige weitere Funktionen von Server Steuerelementen veranschaulicht.

In diesem Abschnitt fügen Sie der Seite ein [System. Web. UI. WebControls. Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) -Steuerelement hinzu und formatieren es.

### <a name="to-add-a-calendar-control"></a>So fügen Sie ein Kalender Steuerelement hinzu

1. Wechseln Sie in Visual Studio zur **Entwurfs** Ansicht.
2. Ziehen Sie im Abschnitt **Standard** der **Toolbox**ein [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) -Steuerelement auf die Seite, und legen Sie es unter dem **div** -Element ab, das die anderen Steuerelemente enthält.

    Der Smarttag-Bereich des Kalenders wird angezeigt. Der Bereich zeigt Befehle an, die es Ihnen erleichtern, die gängigsten Aufgaben für das ausgewählte Steuerelement auszuführen. Die folgende Abbildung zeigt das in der **Entwurfs** Ansicht gerenderte [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement.

    ![Calendar-Steuerelement in Designansicht](creating-a-basic-web-forms-page/_static/image13.png "Calendar-Steuerelement in Designansicht")
3. Wählen Sie im smarttagpanel die Option **Auto Format**aus.

    Das Dialogfeld **automatisch formatieren** wird angezeigt, in dem Sie ein Formatierungsschema für den Kalender auswählen können. Die folgende Abbildung zeigt das Dialogfeld **automatisch formatieren** für das [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement.

    ![Dialogfeld ' Auto Formatierung ' (Calendar-Steuerelement)](creating-a-basic-web-forms-page/_static/image14.png "Dialogfeld ' Auto Formatierung ' (Calendar-Steuerelement)")
4. Wählen Sie in der Liste **Schema auswählen** die Option **einfach** aus, und klicken Sie dann auf **OK**.
5. Wechseln Sie zur **Quell** Ansicht.

    Sie können das **&lt;ASP: Calendar&gt;** -Element sehen. Dieses Element ist wesentlich länger als die Elemente für die einfachen Steuerelemente, die Sie zuvor erstellt haben. Sie enthält auch unter Elemente, wie z. b. **&lt;WeekendDayStyle-&gt;** , die verschiedene Formatierungs Einstellungen darstellen. Die folgende Abbildung zeigt das [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement in der **Quell** Ansicht. (Das genaue Markup, das Sie in der **Quell** Ansicht sehen, kann sich geringfügig von der Abbildung unterscheiden.)

    ![Calendar-Steuerelement in der Quell Ansicht](creating-a-basic-web-forms-page/_static/image15.png "Calendar-Steuerelement in der Quell Ansicht")

### <a name="programming-the-calendar-control"></a>Programmieren des Kalender Steuer Elements

In diesem Abschnitt programmieren Sie das [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement, um das aktuell ausgewählte Datum anzuzeigen.

### <a name="to-program-the-calendar-control"></a>So programmieren Sie das Kalender Steuerelement

1. Doppelklicken Sie in der **Entwurfs** Ansicht auf das [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement.

    Ein neuer Ereignishandler wird erstellt und in der Code Behind-Datei mit dem Namen *FirstWebPage.aspx.cs*angezeigt.
2. Beenden Sie den [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) -Ereignishandler mit dem folgenden Code.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    Der obige Code legt den Text des Label-Steuer Elements auf das ausgewählte Datum des Kalender Steuer Elements fest.

### <a name="running-the-page"></a>Ausführen der Seite

Sie können den Kalender jetzt testen.

### <a name="to-run-the-page"></a>So führen Sie die Seite aus

1. Drücken Sie **STRG + F5** , um die Seite im Browser auszuführen.
2. Klicken Sie auf ein Datum im Kalender.

    Das Datum, auf das geklickt wurde, wird im [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) -Steuerelement angezeigt.
3. Zeigen Sie im Browser den Quellcode für die Seite an.

    Beachten Sie, dass das [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) -Steuerelement auf der Seite als **Tabelle**gerendert wurde, wobei jeden Tag als **TD** -Element angezeigt wird.
4. Schließen Sie den Browser.

## <a name="next-steps"></a>Nächste Schritte

<a id="nextStepsToggle"></a>

In dieser exemplarischen Vorgehensweise wurden die grundlegenden Funktionen des Visual Studio-Seiten Designers veranschaulicht. Nachdem Sie nun wissen, wie Sie eine Web Forms Seite in Visual Studio erstellen und bearbeiten, sollten Sie weitere Features erkunden. Beispielsweise können Sie folgende Schritte ausführen:

- Weitere Informationen zu ASP.net Web Forms finden Sie in den Schritt-für-Schritt-Tutorials für die ersten Schritte [mit ASP.NET 4,5 Web Forms und Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Erfahren Sie mehr über Cascading Stylesheets (CSS). Weitere Informationen finden Sie unter [Working with CSS Overview](https://msdn.microsoft.com/library/bb398931.aspx).
