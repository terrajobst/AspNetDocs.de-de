---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Code Bearbeitung ASP.net Web Forms in Visual Studio 2013 | Microsoft-Dokumentation
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 3473ad476fbbebc58e12586334b4600f57cf17ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78513639"
---
# <a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Codebearbeitung in ASP.NET Web Forms in Visual Studio 2013

von [Erik Reitan](https://github.com/Erikre)

In vielen ASP.net Web Form-Seiten schreiben Sie Code in Visual Basic, C#oder in einer anderen Sprache. Der Code-Editor in Visual Studio kann Ihnen helfen, Code schnell zu schreiben, während Sie dabei helfen, Fehler zu vermeiden. Außerdem bietet der-Editor Möglichkeiten, wiederverwendbaren Code zu erstellen, um die benötigte Arbeit zu verringern.

Diese exemplarische Vorgehensweise veranschaulicht verschiedene Funktionen des Visual Studio Code-Editors.

Bei dieser exemplarischen Vorgehensweise lernen Sie Folgendes:

- Korrigieren Sie Inline Codierungsfehler.
- Umgestalten und Umbenennen von Code.
- Umbenennen von Variablen und Objekten.
- Fügen Sie Code Ausschnitte ein.

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

In diesem Teil der exemplarischen Vorgehensweise erstellen Sie ein Webanwendungs Projekt und fügen ihm eine neue Seite hinzu.

### <a name="to-create-a-web-application-project"></a>So erstellen Sie ein Webanwendungs Projekt

1. Öffnen Sie Microsoft Visual Studio.
2. Wählen Sie im Menü **Datei** die Option **Neues Projekt** aus.  
    Menü "Datei !["](code-editing-in-web-forms-pages/_static/image1.png)

    Das Dialogfeld **Neues Projekt** wird angezeigt.
3. Wählen Sie auf der linken Seite die **Vorlagen** -&gt;  **C# Visual** -&gt; **Webvorlagen** Gruppe aus.
4. Wählen Sie die Vorlage **ASP.NET Web Application** in der mittleren Spalte aus.
5. Nennen Sie das Projekt ***basicwebapp*** , und klicken Sie auf die Schaltfläche **OK** .   
![Dialogfeld „Neues Projekt“](code-editing-in-web-forms-pages/_static/image2.png)
6. Wählen Sie dann die Vorlage **Web Forms** aus, und klicken Sie auf die Schaltfläche **OK** , um das Projekt zu erstellen.  
![Dialogfeld "Neues ASP.NET-Projekt"](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio erstellt ein neues Projekt, das eine vorgefertigte Funktionalität enthält, die auf der Web Forms Vorlage basiert.

## <a name="creating-a-new-aspnet-web-forms-page"></a>Erstellen einer neuen ASP.net-Web Forms Seite

Wenn Sie eine neue Web Forms Anwendung mithilfe der Projektvorlage " **ASP.NET-Webanwendung** " erstellen, fügt Visual Studio eine ASP.NET-Seite (Web Forms Seite) namens " *default. aspx*" sowie mehrere andere Dateien und Ordner hinzu. Sie können die Seite *default. aspx* als Startseite für Ihre Webanwendung verwenden. In dieser exemplarischen Vorgehensweise erstellen Sie jedoch eine neue Seite und arbeiten damit.

### <a name="to-add-a-page-to-the-web-application"></a>So fügen Sie der Webanwendung eine Seite hinzu

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Namen der Webanwendung (in diesem Tutorial lautet der Name der Anwendung **basicwebsite**), und klicken Sie dann auf -&gt; **Neues Element** **Hinzufügen** .   
Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie auf der linken Seite die Gruppe **Visual C#**  -&gt; **Web** Templates aus. Wählen Sie dann in der mittleren Liste **Web Form** aus, und nennen Sie es " *firstwebseite. aspx*".   
    ![Dialogfeld „Neues Element hinzufügen“](code-editing-in-web-forms-pages/_static/image4.png)
3. Klicken Sie auf **Hinzufügen** , um dem Projekt die Web Forms Seite hinzuzufügen.  
 Visual Studio erstellt die neue Seite und öffnet sie.
4. Legen Sie als nächstes diese neue Seite als Standard Startseite fest. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die neue Seite mit dem Namen *firstwebseite. aspx* , und wählen Sie **als Start Seite festlegen**aus. Wenn Sie diese Anwendung das nächste Mal ausführen, um den Fortschritt zu testen, wird diese neue Seite automatisch im Browser angezeigt.

## <a name="correcting-inline-coding-errors"></a>Korrigieren von Inline Codierungs Fehlern

Der Code-Editor in Visual Studio unterstützt Sie bei der Vermeidung von Fehlern beim Schreiben von Code. Wenn Sie einen Fehler gemacht haben, hilft Ihnen der Code-Editor dabei, den Fehler zu beheben. In diesem Teil der exemplarischen Vorgehensweise schreiben Sie eine Codezeile, die die Fehlerkorrektur Features im Editor veranschaulicht.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>So korrigieren Sie einfache Codierungsfehler in Visual Studio

1. Doppelklicken Sie in der **Entwurfs** Ansicht auf die leere Seite, um einen Handler für das **Lade** Ereignis für die Seite zu erstellen.   
   Sie verwenden den Ereignishandler nur als Speicherort, um Code zu schreiben.
2. Geben Sie im Handler die folgende Zeile ein, die einen Fehler enthält, und drücken **Sie die Eingabe**Taste:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Wenn Sie die **Eingabe**Taste drücken, platziert der Code-Editor die grünen und roten Unterstriche (häufig &quot;Wellenlinien&quot; Zeilen) unter den Bereichen des Codes, die Probleme haben. Eine grüne Unterstreichung weist auf eine Warnung hin. Eine rote Unterstreichung weist auf einen Fehler hin, der behoben werden muss. 

    Halten Sie den Mauszeiger über `myStr`, um eine QuickInfo anzuzeigen, in der Sie über die Warnung informiert werden. Halten Sie auch den Mauszeiger auf die rote Unterstreichung, um die Fehlermeldung anzuzeigen.

    Die folgende Abbildung zeigt den Code mit den unterstrichen.

    ![Begrüßungstext in Designansicht](code-editing-in-web-forms-pages/_static/image5.png "Begrüßungstext in Designansicht")  
   Der Fehler muss korrigiert werden, indem am Ende der Zeile ein Semikolon `;` hinzugefügt wird. Die Warnung benachrichtigt Sie lediglich, dass Sie die `myStr` Variable noch nicht verwendet haben.  

    > [!NOTE] 
    > 
    > Sie zeigen die aktuellen Code Formatierungs Einstellungen in Visual Studio an, indem Sie **Tools** -&gt; **Optionen** -&gt; **Schriftarten und Farben**auswählen.

## <a name="refactoring-and-renaming"></a>Umgestaltung und Umbenennen

Refactoring ist eine Software Methodik, bei der der Code umstrukturiert werden muss, damit er leichter zu verstehen und zu warten ist, während seine Funktionalität beibehalten wird. Ein einfaches Beispiel könnte darin bestehen, dass Sie Code in einem Ereignishandler schreiben, um Daten aus einer Datenbank zu erhalten. Wenn Sie Ihre Seite entwickeln, stellen Sie fest, dass Sie auf die Daten von mehreren verschiedenen Handlern zugreifen müssen. Daher müssen Sie den Code der Seite umgestalten, indem Sie eine Datenzugriffs Methode auf der Seite erstellen und in den Handlern Aufrufe an die-Methode einfügen.

Der Code-Editor enthält Tools, die Sie beim Ausführen verschiedener Umgestaltungs Aufgaben unterstützen. In dieser exemplarischen Vorgehensweise arbeiten Sie mit zwei Refactoring-Techniken: Umbenennen von Variablen und Extrahieren von Methoden. Weitere Optionen für Umgestaltung sind das Kapseln von Feldern, das herauf Stufen von lokalen Variablen zu Methoden Parametern und das Verwalten von Methoden Parametern. Die Verfügbarkeit dieser Refactoring-Optionen hängt von der Position im Code ab.

### <a name="refactoring-code"></a>Umgestalten von Code

Ein gängiges Umgestaltungs Szenario besteht darin, eine Methode aus Code zu erstellen (extrahieren), der sich in einem anderen Member befindet, z. b. eine-Methode. Dadurch wird die Größe des ursprünglichen Members reduziert und der extrahierte Code wiederverwendbar.

In diesem Teil der exemplarischen Vorgehensweise schreiben Sie einfachen Code und extrahieren anschließend eine Methode. Refactoring wird für C#unterstützt, sodass Sie eine Seite erstellen, die C# als Programmiersprache verwendet.

### <a name="to-extract-a-method-in-a-c-page"></a>So extrahieren Sie eine Methode in C# einer Seite

1. Wechseln Sie zur **Entwurfs** Ansicht.
2. Ziehen Sie in der **Toolbox**auf der Registerkarte **Standard** ein [Schalt](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Flächen-Steuerelement auf die Seite.
3. Doppelklicken Sie auf das **Schalt** Flächen-Steuerelement, um einen Handler für das [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) -Ereignis zu erstellen, und fügen Sie dann den folgenden markierten Code hinzu:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   Der Code erstellt ein **ArrayList** -Objekt, verwendet eine-Schleife, um es mit Werten zu laden, und verwendet dann eine andere Schleife, um den Inhalt des **ArrayList** -Objekts anzuzeigen.
4. Drücken Sie **STRG + F5** , um die Seite auszuführen, und klicken Sie dann auf die **Schaltfläche** , um sicherzustellen, dass die folgende Ausgabe angezeigt wird:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Kehren Sie zum Code-Editor zurück, und wählen Sie dann im-Ereignishandler die folgenden Zeilen aus.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Klicken Sie mit der rechten Maustaste auf die Auswahl, klicken Sie auf **umgestalten**, und wählen Sie dann **Methode extrahieren**. 

    Das Dialogfeld **Methode extrahieren** wird angezeigt.
7. Geben Sie im Feld **neuer Methoden Name den Namen** **DisplayArray**ein, und klicken Sie dann auf **OK**. 

    Der Code-Editor erstellt eine neue Methode mit dem Namen `DisplayArray`und fügt einen aufzurufenden Befehl in den **Click** -Handler ein, bei dem die Schleife ursprünglich war.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Drücken Sie **STRG + F5** , um die Seite erneut auszuführen, und klicken Sie auf die **Schaltfläche**.

    Die Seite funktioniert genauso wie zuvor. Die `DisplayArray`-Methode kann nun von überall in der Page-Klasse aufgerufen werden.

## <a name="renaming-variables"></a>Umbenennen von Variablen

Wenn Sie mit Variablen und Objekten arbeiten, möchten Sie Sie möglicherweise umbenennen, nachdem Sie bereits im Code referenziert wurden. Das Umbenennen von Variablen und Objekten kann jedoch dazu führen, dass der Code unterbricht, wenn Sie die Umbenennung eines der Verweise verpasst haben. Daher können Sie Refactoring verwenden, um die Umbenennung durchzuführen.

### <a name="to-use-refactoring-to-rename-a-variable"></a>So verwenden Sie Refactoring zum Umbenennen einer Variablen

1. Suchen **Sie im Click** -Ereignishandler die folgende Zeile:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Klicken Sie mit der rechten Maustaste auf den Variablennamen `alist`, wählen Sie **umgestalten**und dann **Umbenennen**aus.

    Das Dialogfeld **Umbenennen** wird angezeigt.
3. Geben Sie im Feld **neuer Name den Namen** **ArrayList1** ein, und stellen Sie sicher, dass das Kontrollkästchen **Vorschau der Verweis Änderungen** ausgewählt wurde. Klicken Sie dann auf **OK**.

    Das Dialogfeld **Vorschau der Änderungen** wird angezeigt, und es wird eine Struktur angezeigt, die alle Verweise auf die Variable enthält, die Sie umbenennen.
4. Klicken Sie auf **anwenden** , um das Dialogfeld **Vorschau der Änderungen** zu schließen.

    Die Variablen, die speziell auf die-Instanz verweisen, die Sie ausgewählt haben, werden umbenannt. Beachten Sie jedoch, dass die Variable `alist` in der folgenden Zeile nicht umbenannt wird.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    Die in dieser Zeile `alist` Variable wird nicht umbenannt, da Sie nicht denselben Wert wie die Variable `alist` darstellt, die Sie umbenannt haben. Die Variable `alist` in der `DisplayArray` Deklaration ist eine lokale Variable für diese Methode. Dies veranschaulicht, dass die Verwendung von Refactoring zum Umbenennen von Variablen anders ist als das einfache Ausführen einer Aktion zum Suchen und ersetzen im Editor. Refactoring benennt Variablen um, die die Semantik der Variablen kennen, mit der Sie arbeiten.

## <a name="inserting-snippets"></a>Einfügen von Ausschnitten

Da es viele Codierungs Aufgaben gibt, die Web Forms-Entwicklern häufig ausführen müssen, stellt der Code-Editor eine Bibliothek mit Code Ausschnitten oder Blöcke mit vorgeschriebenem Code bereit. Sie können diese Ausschnitte in die Seite einfügen.

Jede Sprache, die Sie in Visual Studio verwenden, unterscheidet sich geringfügig von der Art und Weise, wie Code Ausschnitte eingefügt werden. Weitere Informationen zum Einfügen von Code Ausschnitten finden Sie unter [Visual Basic IntelliSense-Code Ausschnitte](https://msdn.microsoft.com/library/18yz4be4.aspx). Weitere Informationen zum Einfügen von Code Ausschnitten in Visual C#finden Sie unter [visuelle C# Code Ausschnitte](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Nächste Schritte

In dieser exemplarischen Vorgehensweise wurden die grundlegenden Funktionen des Code-Editors von Visual Studio 2010 veranschaulicht, um Fehler in Ihrem Code zu korrigieren, Code umzugestalten, Variablen umzubenennen und Code Ausschnitte in Ihren Code einzufügen. Mit zusätzlichen Features im Editor können Sie die Anwendungsentwicklung schnell und einfach gestalten. Auf diese Weise können Sie beispielsweise folgende Vorgänge durchführen:

- Erfahren Sie mehr über die Features von IntelliSense, z. b. das Ändern von IntelliSense-Optionen, das Verwalten von Code Ausschnitten und das Online suchen von Code Ausschnitten Weitere Informationen finden Sie unter [Verwenden von IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Erfahren Sie, wie Sie eigene Code Ausschnitte erstellen. Weitere Informationen finden Sie unter [Erstellen und Verwenden von IntelliSense-Code Ausschnitten](https://msdn.microsoft.com/library/ms165392.aspx) .
- Erfahren Sie mehr über die Visual Basic spezifischen Features von IntelliSense-Code Ausschnitten, wie z. b. das Anpassen der Code Ausschnitte und die Problembehandlung. Weitere Informationen finden Sie unter [Visual Basic IntelliSense-Code Ausschnitte](https://msdn.microsoft.com/library/18yz4be4.aspx) .
- Erfahren Sie mehr über C#die-spezifischen Features von IntelliSense, wie z. b. Umgestaltung und Code Ausschnitte. Weitere Informationen finden Sie unter [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
