---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4-Grundlagen | Microsoft-Dokumentation
author: rick-anderson
description: Diese praktische Übungseinheit basiert auf MVC (Model View Controller) Music Store, einer Lernprogramm Anwendung, die Schritt für Schritt die Verwendung von ASP.net MV erläutert und erläutert.
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 95e9b9f55b2080c0ed01dc34e3a32f9f1c905644
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484569"
---
# <a name="aspnet-mvc-4-fundamentals"></a>ASP.NET MVC 4 Fundamentals

vom [Web Camps-Team](https://twitter.com/webcamps)

[Webcamps-Trainingskit herunterladen](https://aka.ms/webcamps-training-kit)

Diese praktische Übungseinheit basiert auf MVC (Model View Controller) Music Store, einer Lernprogramm Anwendung, die Schritt für Schritt zur Verwendung von ASP.NET MVC und Visual Studio führt und erläutert. Im gesamten Lab erlernen Sie die Einfachheit, aber die Leistungsfähigkeit, diese Technologien zu nutzen. Sie beginnen mit einer einfachen Anwendung und erstellen Sie, bis Sie über eine voll funktionsfähige ASP.NET MVC 4-Webanwendung verfügen.

Diese Übungseinheit funktioniert mit ASP.NET MVC 4.

Wenn Sie die ASP.NET MVC 3-Version der Tutorial-Anwendung untersuchen möchten, finden Sie diese in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

Diese praktische Übungseinheit geht davon aus, dass der Entwickler Erfahrung in Webentwicklungs Technologien wie HTML und JavaScript hat.

> [!NOTE]
> Der gesamte Beispielcode und die Code Ausschnitte sind im webcamps-Trainingskit enthalten, das unter [Microsoft-Web/webcamptrainingkit-Releases](https://aka.ms/webcamps-training-kit)verfügbar ist. Das für dieses Lab spezifische Projekt ist unter [ASP.NET MVC 4-Grundlagen](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals)verfügbar.

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>Die Music Store-Anwendung

Die Music Store-Webanwendung, die in diesem Lab erstellt wird, umfasst drei Hauptkomponenten: Einkaufen, Auschecken und Verwaltung. Besucher können Alben nach Genre durchsuchen, dem Warenkorb Alben hinzufügen, Ihre Auswahl überprüfen und schließlich mit dem Auschecken fortfahren und die Bestellung vervollständigen. Außerdem können Geschäfts Administratoren die verfügbaren Alben und deren Haupteigenschaften verwalten.

![Bildschirme des Musik Stores](aspnet-mvc-4-fundamentals/_static/image1.png "Bildschirme des Musik Stores")

*Bildschirme des Musik Stores*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

Die Music Store-Anwendung wird mit dem **Model View Controller (MVC)** erstellt, einem Architekturmuster, das eine Anwendung in drei Hauptkomponenten trennt:

- **Modelle**: Modell Objekte sind die Teile der Anwendung, die die Domänen Logik implementieren. Häufig rufen Modell Objekte auch den Modell Zustand in einer Datenbank ab und speichern Sie.
- **Sichten:** Ansichten sind die Komponenten, die die Benutzeroberfläche der Anwendung anzeigen. In der Regel wird diese Benutzeroberfläche aus den Modelldaten erstellt. Ein Beispiel wäre die Bearbeitungs Ansicht von Alben, in der Textfelder und eine Dropdown Liste basierend auf dem aktuellen Zustand eines Album Objekts angezeigt werden.
- **Controller:** Controller sind die Komponenten, die die Benutzerinteraktion verarbeiten, das Modell bearbeiten und letztendlich eine Ansicht auswählen, um die Benutzeroberfläche zu Rendering. In einer MVC-Anwendung zeigt die Ansicht nur Informationen an. Benutzereingaben und -interaktionen werden vom Controller verarbeitet und beantwortet.

Das MVC-Muster hilft Ihnen beim Erstellen von Anwendungen, die die verschiedenen Aspekte der Anwendung (Eingabe Logik, Geschäftslogik und UI-Logik) trennen, während gleichzeitig eine lose Kopplung zwischen diesen Elementen ermöglicht wird. Diese Trennung hilft Ihnen bei der Verwaltung der Komplexität, wenn Sie eine Anwendung erstellen, da Sie sich auf einen Aspekt der Implementierung gleichzeitig konzentrieren kann. Darüber hinaus vereinfacht das MVC-Muster das Testen von Anwendungen und fördert außerdem die Verwendung der Test gesteuerten Entwicklung (Test-gesteuerte Entwicklung, TDD) zum Erstellen von Anwendungen.

Das **ASP.NET-MVC** -Framework bietet eine Alternative zum ASP.net-Web Forms Muster zum Erstellen von ASP.NET MVC-basierten Webanwendungen. Das **ASP.NET-MVC** -Framework ist ein schlankes, hochgradig testbares Präsentations Framework, das (wie bei Webformular basierten Anwendungen) in vorhandene ASP.NET-Features wie Masterseiten und Mitgliedschafts basierte Authentifizierung integriert ist, sodass Sie die gesamte Leistungsfähigkeit des .NET Framework-Kern Servers erhalten. Dies ist nützlich, wenn Sie bereits mit ASP.net Web Forms vertraut sind, da alle bereits verwendeten Bibliotheken auch in ASP.NET MVC 4 verfügbar sind.

Außerdem fördert die lose Kopplung zwischen den drei Hauptkomponenten einer MVC-Anwendung auch die parallele Entwicklung. Beispielsweise kann ein Entwickler an der Ansicht arbeiten, ein zweiter Entwickler kann an der Controller Logik arbeiten, und ein Dritter Entwickler kann sich auf die Geschäftslogik im Modell konzentrieren.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie Folgendes:

- Erstellen Sie eine ASP.NET MVC-Anwendung basierend auf dem Lernprogramm für Musikspeicher Anwendungen von Grund auf neu.
- Hinzufügen von Controllern zum Verarbeiten von URLs zur Startseite der Website und zum Durchsuchen der Hauptfunktionen
- Fügen Sie eine Ansicht hinzu, um den angezeigten Inhalt zusammen mit dem Stil anzupassen.
- Hinzufügen von Modellklassen, die Daten und Domänen Logik enthalten und verwalten
- Verwenden Sie das Modell Muster anzeigen, um Informationen von Controller Aktionen an die Ansichts Vorlagen zu übergeben.
- Erkunden Sie die neue Vorlage ASP.NET MVC 4 für Internetanwendungen.

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Labs müssen Sie über Folgendes verfügen:

- [Visual Studio 2012 Express für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (Weitere Informationen zur Installation finden Sie in [Anhang A](#AppendixA) )

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Einrichten

**Installieren von Code Ausschnitten**

Der Vorteil ist, dass ein Großteil des Codes, den Sie in diesem Lab verwalten, als Visual Studio-Code Ausschnitte verfügbar ist. Um die Code Ausschnitte zu installieren, führen Sie **.\source\setup\codesnippeer-.vsi** -Datei aus.

Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, können Sie den Anhang dieses Dokuments &quot;[Anhang C: Verwenden von Code Ausschnitten](#AppendixC)&quot;entnehmen.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exerzitien

Diese praktische Übungseinheit besteht aus den folgenden Übungen:

1. [Übung 1: Erstellen eines Musik Stores ASP.NET MVC-Webanwendungs Projekts](#Exercise1)
2. [Übung 2: Erstellen eines Controllers](#Exercise2)
3. [Übung 3: übergeben von Parametern an einen Controller](#Exercise3)
4. [Übung 4: Erstellen einer Ansicht](#Exercise4)
5. [Übung 5: Erstellen eines Ansichts Modells](#Exercise5)
6. [Übung 6: Verwenden von Parametern in der Ansicht](#Exercise6)
7. [Übung 7: A Lap around ASP.NET MVC 4 New Template](#Exercise7)

> [!NOTE]
> Jede Übung wird von einem **endordner** begleitet, der die sich ergebende Lösung enthält, die Sie nach Abschluss der Übungen erhalten. Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Durcharbeiten der Übungen benötigen.

Geschätzte Zeit bis zum Abschluss dieses Labs: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Übung 1: Erstellen eines Musik Stores ASP.NET MVC-Webanwendungs Projekts

In dieser Übung erfahren Sie, wie Sie eine ASP.NET MVC-Anwendung in Visual Studio 2012 Express für das Web und in ihrer Hauptordner Organisation erstellen. Darüber hinaus erfahren Sie, wie ein neuer Controller hinzugefügt und eine einfache Zeichenfolge auf der Startseite der Anwendung angezeigt wird.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Aufgabe 1: Erstellen des ASP.NET MVC-Webanwendungs Projekts

1. In dieser Aufgabe erstellen Sie ein leeres ASP.NET MVC-Anwendungsprojekt mithilfe der MVC-Vorlage für Visual Studio. Starten Sie **vs Express für Web**.
2. Klicken Sie im Menü **Datei** auf **Neues Projekt**.
3. Wählen Sie im Dialogfeld **Neues Projekt** den Projekttyp **ASP.NET MVC 4-Webanwendung** aus, der sich unter **Visual C#** **Web** Template List befindet.
4. Ändern Sie den **Namen** in *mvcmusicstore*.
5. Legen Sie den Speicherort der Projekt Mappe in einem neuen Ordner " **Begin** " im Quellordner dieser Übung fest, z. b. **[your-Hol-path] \source\ex01-deatingmusicstoreproject\begin**. Klicken Sie auf **OK**.

    ![Dialog Feld "Neues Projekt erstellen"](aspnet-mvc-4-fundamentals/_static/image2.png "Dialog Feld "Neues Projekt erstellen"")

    *Dialog Feld "Neues Projekt erstellen"*
6. Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die **Basis** Vorlage aus, und vergewissern Sie sich, dass die ausgewählte **Ansichts-Engine** **Razor**ist. Klicken Sie auf **OK**.

    ![Neues ASP.NET MVC 4-Projekt (Dialog Feld)](aspnet-mvc-4-fundamentals/_static/image3.png "Neues ASP.NET MVC 4-Projekt (Dialog Feld)")

    *Neues ASP.NET MVC 4-Projekt (Dialog Feld)*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Aufgabe 2: Untersuchen der Lösungs Struktur

Das ASP.NET MVC-Framework enthält eine Visual Studio-Projektvorlage, mit der Sie Webanwendungen erstellen können, die das MVC-Muster unterstützen. Diese Vorlage erstellt eine neue ASP.NET MVC-Webanwendung mit den erforderlichen Ordnern, Element Vorlagen und Konfigurationsdatei Einträgen.

In dieser Aufgabe überprüfen Sie die Lösungs Struktur, um die beteiligten Elemente und ihre Beziehungen zu verstehen. Die folgenden Ordner sind in der ASP.NET MVC-Anwendung enthalten, da das ASP.NET MVC-Framework standardmäßig eine &quot;Konvention über die Konfiguration&quot; Ansatzes verwendet und einige Standard Annahmen basierend auf den Benennungs Konventionen für Ordner vornimmt.

1. Nachdem das Projekt erstellt wurde, überprüfen Sie die Ordnerstruktur, die in der Projektmappen-Explorer auf der rechten Seite erstellt wurde:

    ![ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer")

    *ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer*

   1. **Controller**. Dieser Ordner enthält die Controller Klassen. In einer MVC-basierten Anwendung sind Controller für die Handhabung von Endbenutzer Interaktionen, das Bearbeiten des Modells und schließlich das Auswählen einer Ansicht zum Rendering der Benutzeroberfläche verantwortlich.

       > [!NOTE]
       > Das MVC-Framework erfordert, dass die Namen aller Controller mit &quot;Controller&quot;enden, z. b. HomeController, logincontroller oder ProductController.
   2. **Modelle**. Dieser Ordner wird für Klassen bereitgestellt, die das Anwendungsmodell für die MVC-Webanwendung darstellen. Dies schließt in der Regel den Code ein, der Objekte und die Logik für die Interaktion mit dem Datenspeicher definiert. In der Regel befinden sich die tatsächlichen Modell Objekte in separaten Klassenbibliotheken. Wenn Sie jedoch eine neue Anwendung erstellen, können Sie Klassen einschließen und Sie zu einem späteren Zeitpunkt im Entwicklungszyklen in separate Klassenbibliotheken verschieben.
   3. **Sichten**. Dieser Ordner ist der empfohlene Speicherort für Ansichten, die Komponenten, die für die Anzeige der Benutzeroberfläche der Anwendung verantwortlich sind. In Sichten werden neben anderen Dateien, die sich auf das Rendern von Sichten beziehen, auch die Dateien aspx,. ascx,. cshtml und. Master verwendet. Der Ordner views enthält einen Ordner für jeden Controller. der Ordner wird mit dem Controller Namen Präfix benannt. Wenn Sie z. b. einen Controller mit dem Namen " **HomeController**" haben, enthält der Ordner "Views" einen Ordner namens "Home". Wenn das ASP.NET-MVC-Framework eine Ansicht lädt, sucht es standardmäßig nach einer ASPX-Datei mit dem angeforderten Ansichts Namen im Ordner "views\controllername" (**views [ControllerName] [Action]. aspx**) oder (**views [Controller Name] [Action]. cshtml**) für Razor-Ansichten.

      > [!NOTE]
      > Zusätzlich zu den zuvor aufgeführten Ordnern verwendet eine MVC-Webanwendung die Datei " **Global. asax** ", um globale URL-Routing Standardwerte festzulegen, und die Datei " **Web. config** " wird verwendet, um die Anwendung zu konfigurieren.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Aufgabe 3: Hinzufügen eines HomeController

In ASP.NET-Anwendungen, in denen das MVC-Framework nicht verwendet wird, wird die Benutzerinteraktion um Seiten organisiert, um Ereignisse von diesen Seiten zu erhöhen und zu behandeln. Im Gegensatz dazu ist die Benutzerinteraktion mit ASP.NET MVC-Anwendungen auf Controllern und ihren Aktionsmethoden organisiert.

Das ASP.NET-MVC-Framework ordnet URLs jedoch Klassen zu, die als Controller bezeichnet werden. Controller verarbeiten eingehende Anforderungen, behandeln Benutzereingaben und Interaktionen, führen die entsprechende Anwendungslogik aus und bestimmen die Antwort, die an den Client zurückgesendet werden soll (HTML anzeigen, Datei herunterladen, zu einer anderen URL umleiten usw.). Wenn HTML angezeigt wird, ruft eine Controller Klasse in der Regel eine separate Ansichts Komponente auf, um das HTML-Markup für die Anforderung zu generieren. In einer MVC-Anwendung zeigt die Ansicht nur Informationen an. Benutzereingaben und -interaktionen werden vom Controller verarbeitet und beantwortet.

In dieser Aufgabe fügen Sie eine Controller Klasse hinzu, die URLs auf der Startseite der Music Store-Website behandelt.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Controller** Ordner, wählen Sie **Hinzufügen** und dann **Controller** Befehl aus:

    ![Controller Befehl hinzufügen](aspnet-mvc-4-fundamentals/_static/image5.png "Controller Befehl hinzufügen")

    *Controller Befehl hinzufügen*
2. Das Dialogfeld **Controller hinzufügen** wird angezeigt. Nennen Sie den Controller *HomeController* , und klicken **Sie auf Hinzufügen**.

    ![Controller Dialog hinzufügen](aspnet-mvc-4-fundamentals/_static/image6.png "Controller Dialog hinzufügen")

    *Controller Dialog hinzufügen*
3. Die Datei **HomeController.cs** wird im Ordner **Controller** erstellt. Damit der **HomeController** eine Zeichenfolge für die Index Aktion zurückgibt, ersetzen Sie die **Index** -Methode durch den folgenden Code:

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-EX1 HomeController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe testen Sie die Anwendung in einem Webbrowser.

1. Drücken Sie **F5** , um die Anwendung auszuführen. Das Projekt wird kompiliert, und der lokale IIS-Webserver wird gestartet. Der lokale IIS-Webserver öffnet automatisch einen Webbrowser, der auf die URL des Webservers zeigt.

    ![Anwendung, die in einem Webbrowser ausgeführt wird](aspnet-mvc-4-fundamentals/_static/image7.png "Anwendung, die in einem Webbrowser ausgeführt wird")

    *Anwendung, die in einem Webbrowser ausgeführt wird*

    > [!NOTE]
    > Der lokale IIS-Webserver führt die Website auf einer zufälligen, kostenlosen Portnummer aus. In der obigen Abbildung wird die Website unter `http://localhost:50103/`ausgeführt. Daher wird Port 50103 verwendet. Die Portnummer kann variieren.
2. Schließen Sie den Browser.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Übung 2: Erstellen eines Controllers

In dieser Übung erfahren Sie, wie Sie den Controller aktualisieren, um einfache Funktionen der Music Store-Anwendung zu implementieren. Dieser Controller definiert Aktionsmethoden, um die folgenden spezifischen Anforderungen zu behandeln:

- Eine Listenseite der Musik-Genres im Music Store
- Eine Seite zum Durchsuchen, die alle Musikalben für ein bestimmtes Genre auflistet
- Eine Detailseite, auf der Informationen zu einem bestimmten Musikalbum angezeigt werden.

Im Rahmen dieser Übung geben diese Aktionen jetzt einfach eine Zeichenfolge zurück.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Aufgabe 1: Hinzufügen einer neuen StoreController-Klasse

In dieser Aufgabe fügen Sie einen neuen Controller hinzu.

1. Wenn Sie nicht bereits geöffnet ist, starten Sie **vs Express für Web 2012**.
2. Klicken Sie im Menü **Datei** auf **Projekt öffnen**. Navigieren Sie im Dialogfeld Projekt öffnen zu **source\ex02-kreatingacontroller\begin**, wählen Sie **BEGIN. sln** aus, und klicken Sie auf **Öffnen**. Alternativ können Sie mit der Lösung fortfahren, die Sie nach Abschluss der vorherigen Übung abgerufen haben.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
3. Fügen Sie den neuen Controller hinzu. Klicken Sie dazu mit der rechten Maustaste auf den Ordner **Controllers** innerhalb der Projektmappen-Explorer, und wählen Sie **Hinzufügen** und dann den **Controller** -Befehl aus. Ändern Sie den **Controller Namen** in *StoreController*, und klicken Sie auf **Hinzufügen**.

    ![Controller Dialog hinzufügen](aspnet-mvc-4-fundamentals/_static/image8.png "Controller Dialog hinzufügen")

    *Controller Dialog hinzufügen*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Aufgabe 2: Ändern der StoreController-Aktionen

In dieser Aufgabe ändern Sie die Controller Methoden, die als **Aktionen**bezeichnet werden. Aktionen sind für die Verarbeitung von URL-Anforderungen und das Ermitteln des Inhalts zuständig, der an den Browser oder den Benutzer gesendet werden soll, der die URL aufgerufen hat.

1. Die **StoreController** -Klasse verfügt bereits über eine **Index** -Methode. Sie werden Sie später in dieser Übungseinheit verwenden, um die Seite zu implementieren, die alle Genres des Musik Stores auflistet. Ersetzen Sie vorerst einfach die **Index** -Methode durch den folgenden Code, der eine Zeichenfolge &quot;Hello from Store. Index ()&quot;zurückgibt:

    (Code Ausschnitt- *ASP.NET MVC 4-Grundlagen-EX2 StoreController-Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Fügen Sie **Browse** -und **Details** -Methoden hinzu. Fügen Sie zu diesem Zweck den folgenden Code in den **StoreController**ein:

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-EX2 StoreController browseanddetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe testen Sie die Anwendung in einem Webbrowser.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der **Start** Seite gestartet. Ändern Sie die URL, um die Implementierung der einzelnen Aktionen zu überprüfen.

    1. **/Store**. **&quot;Hello from Store. Index ()-&quot;** wird angezeigt.
    2. **/Store/Browse**. **&quot;Hello from Store. Browse ()-&quot;** wird angezeigt.
    3. **/Store/Details**. **&quot;Hello from Store. Details ()-&quot;** wird angezeigt.

        ![Durchsuchen von storebrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Durchsuchen von storebrowse")

        *Durchsuchen/Store/Browse*
3. Schließen Sie den Browser.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Übung 3: übergeben von Parametern an einen Controller

Bis jetzt haben Sie Konstante Zeichen folgen von den Controllern zurückgegeben. In dieser Übung erfahren Sie, wie Sie Parameter mithilfe der URL und der QueryString an einen Controller übergeben und dann die Methoden Aktionen mit Text auf den Browser reagieren.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Aufgabe 1: Hinzufügen von Genre Parametern zu StoreController

In dieser Aufgabe verwenden Sie die **Abfrage Zeichenfolge** , um Parameter an die Methode zum **Durchsuchen** von Aktionen in **StoreController**zu senden.

1. Wenn Sie nicht bereits geöffnet ist, starten Sie **vs Express für Web**.
2. Klicken Sie im Menü **Datei** auf **Projekt öffnen**. Navigieren Sie im Dialogfeld Projekt öffnen zu **source\ex03-passingparameterstoacontroller\begin**, wählen Sie **BEGIN. sln** aus, und klicken Sie auf **Öffnen**. Alternativ können Sie mit der Lösung fortfahren, die Sie nach Abschluss der vorherigen Übung abgerufen haben.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
3. Öffnen Sie die **StoreController** -Klasse. Erweitern Sie dazu im **Projektmappen-Explorer**den Ordner **Controller** , und doppelklicken Sie auf **StoreController.cs**.
4. Ändern Sie die **Browse** -Methode, und fügen Sie einen Zeichen folgen Parameter hinzu, um einen bestimmten Genre anzufordern. ASP.NET MVC übergibt automatisch alle QueryString-oder Formular Bereitstellungs Parameter mit dem Namen **Genre** an diese Aktionsmethode, wenn Sie aufgerufen werden. Ersetzen Sie hierzu die **Browse** -Methode durch den folgenden Code:

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-EX3 StoreController browsemethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> Sie verwenden die hilfsprogrammmethode **HttpUtility. HtmlEncode** , um zu verhindern, dass Benutzer JavaScript in der Ansicht mit einem Link wie **/Store/Browse einfügen? Genre =&lt;Skript&gt;Window. Location = '[http://hackersite.com](http://hackersite.com)'&lt;/Script&gt;** .
> 
> Weitere Erläuterungen finden Sie in [diesem MSDN-Artikel](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Aufgabe 2: Ausführen der Anwendung

In dieser Aufgabe testen Sie die Anwendung in einem Webbrowser und verwenden den **Genre** -Parameter.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der **Start** Seite gestartet. Ändern Sie die URL in */Store/Browse? Genre = Disco* , um zu überprüfen, ob die Aktion den Genre-Parameter empfängt.

    ![Durchsuchen von storebrowsergenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Durchsuchen von storebrowsergenre = Disco")

    */Store/Browse durchsuchen? Genre = Disco*
3. Schließen Sie den Browser.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Aufgabe 3: Hinzufügen eines in die URL eingebetteten ID-Parameters

In dieser Aufgabe verwenden Sie die **URL** , um einen **ID** -Parameter an die **Details** -Aktionsmethode von **StoreController**zu übergeben. Die Standard Routing Konvention von ASP.NET MVC besteht darin, das Segment einer URL nach dem Namen der Aktionsmethode als Parameter mit dem Namen **ID**zu behandeln. Wenn die Aktionsmethode einen Parameter mit dem Namen ID hat, übergibt ASP.NET MVC automatisch das URL-Segment als Parameter an Sie. In URL **Store/Details/5**wird die **ID** als **5**interpretiert.

1. Ändern Sie die **Details** -Methode von **StoreController**, und fügen Sie einen **int** -Parameter mit dem Namen **ID**hinzu. Ersetzen Sie **hierzu die Details** -Methode durch den folgenden Code:

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-EX3 StoreController detailsmethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe testen Sie die Anwendung in einem Webbrowser und verwenden den **ID** -Parameter.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der **Start** Seite gestartet. Ändern Sie die URL in */Store/Details/5* , um sicherzustellen, dass die Aktion den ID-Parameter erhält.

    ![Durchsuchen StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Durchsuchen StoreDetails5")

    *Durchsuchen/Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Übung 4: Erstellen einer Ansicht

Bisher haben Sie Zeichen folgen aus Controller Aktionen zurückgegeben. Obwohl es sich um ein nützliches Verfahren zum Verständnis der Funktionsweise von Controllern handelt, ist es nicht, wie ihre realen Webanwendungen erstellt werden. Sichten sind Komponenten, die einen besseren Ansatz zum Erstellen von HTML-Code zum Browser mit Vorlagen Dateien bieten.

In dieser Übung erfahren Sie, wie Sie eine layoutmasterseite hinzufügen, um eine Vorlage für gemeinsamen HTML-Inhalt einzurichten, ein Stylesheet, um das Aussehen und Verhalten der Site zu verbessern, und schließlich eine Ansichts Vorlage, mit der HomeController HTML zurückgeben kann.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-_layoutcshtml"></a>Aufgabe 1: Ändern der Datei \_"Layout. cshtml"

Mit der Datei **~/views/Shared/\_Layout. cshtml** können Sie eine Vorlage für gemeinsamen HTML-Code für die gesamte Website einrichten. In dieser Aufgabe fügen Sie eine layoutmasterseite mit einem gemeinsamen Header mit Links zur Startseite und zum Speicherbereich hinzu.

1. Wenn Sie nicht bereits geöffnet ist, starten Sie **vs Express für Web**.
2. Klicken Sie im Menü **Datei** auf **Projekt öffnen**. Navigieren Sie im Dialogfeld Projekt öffnen zu **source\ex04-anatingaview\begin**, wählen Sie **BEGIN. sln** aus, und klicken Sie auf **Öffnen**. Alternativ können Sie mit der Lösung fortfahren, die Sie nach Abschluss der vorherigen Übung abgerufen haben.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
3. Die Datei <strong>\_"Layout. cshtml</strong> " enthält das HTML-Container Layout für alle Seiten auf der Website. Sie enthält das <strong>&lt;HTML-&gt;</strong> -Element für die HTML-Antwort sowie die <strong>&lt;Head-&gt;</strong> und <strong>&lt;Body&gt;</strong> -Elemente. mit <strong>@RenderBody()</strong> im HTML-Text werden Bereiche identifiziert, die Ansichts Vorlagen mit dynamischem Inhalt ausfüllen können.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Fügen Sie eine gemeinsame Kopfzeile mit Links zur Startseite und zum Speicherbereich auf allen Seiten der Website hinzu. Fügen Sie dazu den folgenden Code unterhalb &lt;Body&gt;-Anweisung hinzu.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Fügen Sie ein div-ein, um den Textabschnitt jeder Seite zu Rendering. Ersetzen Sie <strong>@RenderBody()</strong> durch den folgenden hervorgehobenen CodeC#: ()

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Wussten Sie schon? Visual Studio 2012 verfügt über Ausschnitte, mit denen Sie häufig verwendeten Code in HTML, Code Dateien und mehr hinzufügen können. Probieren Sie es aus, indem Sie **&lt;div&gt;** eingeben und zweimal die **Tab** -Taste drücken, um ein umfassendes **div** -Tag einzufügen.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Aufgabe 2: Hinzufügen eines CSS-Stylesheets

Die leere Projektvorlage enthält eine sehr optimierte CSS-Datei, die nur Stile enthält, die zum Anzeigen von grundlegenden Formularen und Validierungs Nachrichten verwendet werden. Sie verwenden zusätzliche CSS und Images (die möglicherweise von einem Designer bereitgestellt werden), um das Erscheinungsbild der Website zu verbessern.

In dieser Aufgabe fügen Sie ein CSS-Stylesheet hinzu, um die Stile der Site zu definieren.

1. Die CSS-Datei und die Images sind im Ordner **source\assets\content** dieses Labs enthalten. Um Sie der Anwendung hinzuzufügen, ziehen Sie Ihren Inhalt aus einem **Windows-Explorer** -Fenster in Visual Studio Express für **Projektmappen-Explorer** das Web, wie unten dargestellt:

    ![Ziehen von Stil Inhalt](aspnet-mvc-4-fundamentals/_static/image12.png "Ziehen von Stil Inhalt")

    *Ziehen von Stil Inhalt*
2. Es wird ein Warn Dialogfeld angezeigt, in dem Sie aufgefordert werden, die Datei " **Site. CSS** " und einige vorhandene Images zu ersetzen Aktivieren Sie **für alle Elemente** übernehmen, und klicken Sie auf **Ja**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Aufgabe 3: Hinzufügen einer Ansichts Vorlage

In dieser Aufgabe fügen Sie eine Ansichts Vorlage hinzu, um die HTML-Antwort zu generieren, die die layoutmasterseite und das in dieser Übung hinzugefügte CSS verwendet.

1. Wenn Sie beim Durchsuchen der Homepage der Website eine Ansichts Vorlage verwenden möchten, müssen Sie zunächst angeben, dass anstelle einer Zeichenfolge die Methode **HomeController Index** eine **Ansicht**zurückgibt. Öffnen Sie die **HomeController** -Klasse, und ändern Sie Ihre **Index** -Methode, um ein **Aktions Ergebnis**zurückzugeben, und lassen Sie **View ()** zurückgeben.

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex4 HomeController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Nun müssen Sie eine geeignete Ansichts Vorlage hinzufügen. **Klicken Sie** dazu mit der rechten Maustaste in die **Index** Aktionsmethode, und wählen Sie **Ansicht hinzufügen**aus. Dadurch wird das Dialog **Feld Ansicht hinzufügen** angezeigt.

    ![Hinzufügen einer Sicht aus der Index-Methode](aspnet-mvc-4-fundamentals/_static/image13.png "Hinzufügen einer Sicht aus der Index-Methode")

    *Hinzufügen einer Sicht aus der Index-Methode*
3. Das Dialog **Feld Ansicht hinzufügen** wird angezeigt, um eine Ansichts Vorlagen Datei zu generieren. In diesem Dialogfeld wird standardmäßig der Name der Ansichts Vorlage aufgefüllt, sodass er mit der Aktionsmethode übereinstimmt, von der er verwendet wird. Da Sie im HomeController das Kontextmenü **Ansicht hinzufügen** innerhalb der **Index** Aktionsmethode verwendet haben, enthält das Dialog **Feld Ansicht hinzufügen** einen Index als Standard Ansichts Namen. Klicken Sie auf **Hinzufügen**.

    ![Sicht Ansicht hinzufügen](aspnet-mvc-4-fundamentals/_static/image14.png "Sicht Ansicht hinzufügen")

    *Sicht Ansicht hinzufügen*
4. Visual Studio generiert eine **Index. cshtml** -Ansichts Vorlage im Ordner " **views\home** " und öffnet diese.

    ![Start Index Ansicht erstellt](aspnet-mvc-4-fundamentals/_static/image15.png "Start Index Ansicht erstellt")

    *Start Index Ansicht erstellt*

    > [!NOTE]
    > Name und Speicherort der Datei " **Index. cshtml** " sind relevant und folgen den standardmäßigen ASP.NET-MVC-Benennungs Konventionen.
    > 
    > Der Ordner \views\**Home** entspricht dem Controller Namen (**Home** Controller). Der Ansichts Vorlagen Name (**Index**) entspricht der Controller Aktionsmethode, die die Ansicht anzeigt.
    > 
    > Auf diese Weise kann ASP.NET MVC den Namen oder den Speicherort einer Ansichts Vorlage nicht explizit angeben, wenn diese Benennungs Konvention verwendet wird, um eine Ansicht zurückzugeben.
5. Die generierte Ansichts Vorlage basiert auf der zuvor definierten **\_Layout. cshtml** -Vorlage. Aktualisieren Sie die viewbag. Title-Eigenschaft auf **Home**, und ändern Sie den Hauptinhalt in **die Startseite**, wie im folgenden Code gezeigt:

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Wählen Sie im Projektmappen-Explorer **mvcmusicstore** -Projekt aus, und drücken Sie **F5** , um die Anwendung auszuführen.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Aufgabe 4: Überprüfung

Um sicherzustellen, dass Sie alle Schritte in der vorherigen Übung ordnungsgemäß ausgeführt haben, gehen Sie wie folgt vor:

Wenn die Anwendung in einem Browser geöffnet ist, sollten Sie Folgendes beachten:

1. Die Index Aktionsmethode von HomeController wurde gefunden und zeigt die Ansichts Vorlage **\views\home\index.cshtml** an, obwohl der Code **Return View ()** aufgerufen hat, da die Ansichts Vorlage der Standard Benennungs Konvention folgte.
2. Auf der Startseite wird die Begrüßungsnachricht angezeigt, die in der Ansichts Vorlage " **\views\home\index.cshtml** " definiert ist.
3. Auf der Startseite wird die Vorlage " **\_Layout. cshtml** " verwendet, sodass die Willkommensnachricht im HTML-Layout der Standard Website enthalten ist.

    ![Start Index Ansicht mit dem definierten layoutpage-und Style-Stil](aspnet-mvc-4-fundamentals/_static/image16.png "Start Index Ansicht mit dem definierten layoutpage-und Style-Stil")

    *Start Index Ansicht mit dem definierten layoutpage-und Style-Stil*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Übung 5: Erstellen eines Ansichts Modells

Bisher haben Sie in ihren Ansichten hart codiertes HTML angezeigt, aber um dynamische Webanwendungen zu erstellen, sollte die Ansichts Vorlage Informationen vom Controller erhalten. Ein gängiges Verfahren, das für diesen Zweck verwendet wird, ist das **ViewModel** -Muster, das es einem Controller ermöglicht, alle Informationen zu verpacken, die zum Generieren der entsprechenden HTML-Antwort erforderlich sind.

In dieser Übung erfahren Sie, wie Sie eine ViewModel-Klasse erstellen und die erforderlichen Eigenschaften hinzufügen: die Anzahl der Genres im Speicher und eine Liste dieser Genres. Außerdem wird StoreController so aktualisiert, dass das erstellte ViewModel verwendet wird, und schließlich wird eine neue Ansichts Vorlage erstellt, in der die erwähnten Eigenschaften auf der Seite angezeigt werden.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Aufgabe 1: Erstellen einer ViewModel-Klasse

In dieser Aufgabe erstellen Sie eine ViewModel-Klasse, mit der das Szenario für die Speicher Genre Auflistung implementiert wird.

1. Wenn Sie nicht bereits geöffnet ist, starten Sie **vs Express für Web**.
2. Klicken Sie im Menü **Datei** auf **Projekt öffnen**. Navigieren Sie im Dialogfeld Projekt öffnen zu **source\ex05-kreatingaviewmodel\begin**, wählen Sie **BEGIN. sln** aus, und klicken Sie auf **Öffnen**. Alternativ können Sie mit der Lösung fortfahren, die Sie nach Abschluss der vorherigen Übung abgerufen haben.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
3. Erstellen Sie einen **ViewModels** -Ordner für das ViewModel. Klicken Sie hierzu mit der rechten Maustaste auf das **mvcmusicstore** -Projekt der obersten Ebene, und wählen Sie **Hinzufügen** und dann **neuer Ordner**aus.

    ![Hinzufügen eines neuen Ordners](aspnet-mvc-4-fundamentals/_static/image17.png "Hinzufügen eines neuen Ordners")

    *Hinzufügen eines neuen Ordners*
4. Nennen Sie den Ordner *ViewModels*.

    ![Ordner "ViewModels" in Projektmappen-Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "Ordner "ViewModels" in Projektmappen-Explorer")

    *Ordner "ViewModels" in Projektmappen-Explorer*
5. Erstellen Sie eine **ViewModel** -Klasse. Klicken Sie hierzu mit der rechten Maustaste auf den zuletzt erstellten Ordner " **ViewModels** ", und wählen Sie **Hinzufügen** und dann **Neues Element**aus. Wählen Sie unter **Code**das **Klassen** Element aus, benennen Sie die Datei *StoreIndexViewModel.cs*, und klicken Sie dann auf **Hinzufügen**.

    ![Hinzufügen einer neuen Klasse](aspnet-mvc-4-fundamentals/_static/image19.png "Hinzufügen einer neuen Klasse")

    *Hinzufügen einer neuen Klasse*

    ![Erstellen der storeindexviewmodel-Klasse](aspnet-mvc-4-fundamentals/_static/image20.png "Erstellen der storeindexviewmodel-Klasse")

    *Erstellen der storeindexviewmodel-Klasse*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Aufgabe 2: Hinzufügen von Eigenschaften zur ViewModel-Klasse

Es gibt zwei Parameter, die von StoreController an die Ansichts Vorlage übermittelt werden müssen, um die erwartete HTML-Antwort zu generieren: die Anzahl der Genres im Speicher und eine Liste dieser Genres.

In dieser Aufgabe fügen Sie diese beiden Eigenschaften der **storeindexviewmodel** -Klasse hinzu: " **numofgenres** (a Integer)" und " **Genres** " (eine Liste von Zeichen folgen).

1. Fügen Sie die Eigenschaften " **numofgenres** " und " **Genres** " der **storeindexviewmodel** -Klasse hinzu. Fügen Sie zu diesem Zweck der Klassendefinition die folgenden zwei Zeilen hinzu:

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-EX5 storeindexviewmodel-Eigenschaften*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> Die **{Get; Set;}** -Notation verwendet die C#Funktion für automatisch implementierte Eigenschaften von. Er bietet die Vorteile einer Eigenschaft, ohne dass wir ein dahinter liegendes Feld deklarieren müssen.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Aufgabe 3: Aktualisieren von StoreController zur Verwendung von storeindexviewmodel

Die **storeindexviewmodel** -Klasse kapselt die Informationen, die erforderlich sind, um die **Index** Methode von **StoreController**an eine Ansichts Vorlage zu übergeben, um eine Antwort zu generieren.

In dieser Aufgabe aktualisieren Sie **StoreController** , damit das **storeindexviewmodel**verwendet werden kann.

1. Öffnen Sie die **StoreController** -Klasse.

    ![StoreController-Klasse wird geöffnet](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController-Klasse wird geöffnet")

    *StoreController-Klasse wird geöffnet*
2. Fügen Sie am Anfang des **StoreController** -Codes den folgenden Namespace hinzu, um die **storeindexviewmodel** -Klasse aus dem **StoreController**zu verwenden:

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-EX5 storeindexviewmodel mithilfe von ViewModels*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Ändern Sie die **Index** Aktionsmethode von **StoreController**so, dass Sie ein **storeindexviewmodel** -Objekt erstellt und auffüllt und es dann an eine Ansichts Vorlage übergibt, um eine HTML-Antwort damit zu generieren.

    > [!NOTE]
    > In Lab &quot;ASP.NET MVC-Modellen und Datenzugriff&quot; Sie Code schreiben, der die Liste der Speicher-Genres aus einer Datenbank abruft. Im folgenden Code erstellen Sie eine **Liste** von dummydatengenres, die das **storeindexviewmodel**auffüllen.
    > 
    > Nachdem das **storeindexviewmodel** -Objekt erstellt und eingerichtet wurde, wird es als Argument an die **View** -Methode übermittelt. Dies gibt an, dass die Ansichts Vorlage dieses Objekt verwendet, um eine HTML-Antwort mit ihr zu generieren.
4. Ersetzen Sie die **Index** -Methode durch den folgenden Code:

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-EX5 StoreController Index-Methode*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> Wenn Sie mit nicht vertraut C#sind, können Sie davon ausgehen, dass die Verwendung von **var** bedeutet, dass die **ViewModel** -Variable spät gebunden ist. Das ist nicht korrekt: der C# Compiler verwendet den Typrückschluss basierend auf dem, was Sie der Variablen zuweisen, um zu bestimmen, dass **ViewModel** den Typ " **storeindexviewmodel**" hat. Außerdem erhalten Sie durch Kompilieren der lokalen **ViewModel** -Variablen als **storeindexviewmodel** -Typ eine Kompilierzeit Überprüfung und Visual Studio Code-Editor-Unterstützung.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Aufgabe 4: Erstellen einer Ansichts Vorlage, die storeindexviewmodel verwendet

In dieser Aufgabe erstellen Sie eine Ansichts Vorlage, die ein storeindexviewmodel-Objekt verwendet, das vom Controller zum Anzeigen einer Liste von Genres übermittelt wird.

1. Vor dem Erstellen der neuen Ansichts Vorlage erstellen wir das Projekt, damit das **Dialog Feld "Ansicht hinzufügen** " die Klasse " **storeindexviewmodel** " kennt. Erstellen Sie das Projekt, indem Sie das Menü Element **Build** auswählen und dann **mvcmusicstore erstellen**.

    ![Projekt wird aufgebaut](aspnet-mvc-4-fundamentals/_static/image22.png "Projekt wird aufgebaut")

    *Projekt wird aufgebaut*
2. Erstellen Sie eine neue Ansichts Vorlage. Klicken Sie dazu mit der rechten Maustaste in die **Index** -Methode, und wählen Sie **Ansicht hinzufügen**aus.

    ![Hinzufügen einer Ansicht](aspnet-mvc-4-fundamentals/_static/image23.png "Hinzufügen einer Ansicht")

    *Hinzufügen einer Ansicht*
3. Da das **Dialog Feld Ansicht hinzufügen** von **StoreController**aufgerufen wurde, wird die Ansichts Vorlage standardmäßig in der Datei " **\views\store\index.cshtml** " hinzugefügt. Aktivieren Sie das Kontrollkästchen **eine stark typisierte Ansicht erstellen** , und wählen Sie dann **storeindexviewmodel** als **Modell Klasse**aus. Stellen Sie außerdem sicher, dass die ausgewählte Ansichts-Engine **Razor**ist. Klicken Sie auf **Hinzufügen**.

    ![Sicht Ansicht hinzufügen](aspnet-mvc-4-fundamentals/_static/image24.png "Sicht Ansicht hinzufügen")

    *Sicht Ansicht hinzufügen*

    Die Ansichts Vorlagen Datei " **\views\store\index.cshtml** " wird erstellt und geöffnet. Basierend auf den Informationen, die im Dialog **Feld Ansicht hinzufügen** im letzten Schritt bereitgestellt wurden, erwartet die Ansichts Vorlage eine **storeindexviewmodel** -Instanz als Daten, die zum Generieren einer HTML-Antwort verwendet werden. Sie werden feststellen, dass die Vorlage eine `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#erbt.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Aufgabe 5: Aktualisieren der Ansichts Vorlage

In dieser Aufgabe aktualisieren Sie die Ansichts Vorlage, die in der letzten Aufgabe erstellt wurde, um die Anzahl der Genres und deren Namen innerhalb der Seite abzurufen.

> [!NOTE]
> Zum Ausführen von Code in der Ansichts Vorlage verwenden Sie @ Syntax (häufig als &quot;Code-Nuggets&quot;bezeichnet).

1. Ersetzen Sie in der Datei " **Index. cshtml** " im Ordner " **Store** " den Code durch Folgendes:

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. Führen Sie eine Schleife für die Genre Liste in **storeindexviewmodel** aus, und erstellen Sie mithilfe einer **foreach** -Schleife eine HTML- **&lt;UL&gt;** Liste.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Drücken Sie **F5** , um die Anwendung auszuführen und **/Store**zu durchsuchen. Die Liste der Genres, die im **storeindexviewmodel** -Objekt von **StoreController** an die Ansichts Vorlage übermittelt werden, wird angezeigt.

    ![Anzeigen einer Liste von Genres](aspnet-mvc-4-fundamentals/_static/image26.png "Anzeigen einer Liste von Genres")

    *Anzeigen einer Liste von Genres*
4. Schließen Sie den Browser.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Übung 6: Verwenden von Parametern in der Ansicht

In Übung 3 haben Sie gelernt, wie Sie Parameter an den Controller übergeben. In dieser Übung erfahren Sie, wie diese Parameter in der Ansichts Vorlage verwendet werden. Zu diesem Zweck werden Sie zunächst in Modellklassen eingeführt, die Ihnen helfen, Ihre Daten und Domänen Logik zu verwalten. Darüber hinaus erfahren Sie, wie Sie Links zu Seiten innerhalb der ASP.NET MVC-Anwendung erstellen, ohne sich Gedanken über die Codierung von URL-Pfaden machen zu müssen.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Aufgabe 1: Hinzufügen von Modellklassen

Anders als bei ViewModels, die nur zum Übergeben von Informationen vom Controller an die Ansicht erstellt werden, werden Modellklassen erstellt, um Daten und Domänen Logik zu speichern und zu verwalten. In dieser Aufgabe fügen Sie zwei Modellklassen hinzu, die diese Konzepte darstellen: **Genre** und **Album**.

1. Wenn Sie nicht bereits geöffnet ist, starten Sie **vs Express für Web**
2. Klicken Sie im Menü **Datei** auf **Projekt öffnen**. Navigieren Sie im Dialogfeld Projekt öffnen zu **source\ex06-usingparametersinview\begin**, wählen Sie **BEGIN. sln** aus, und klicken Sie auf **Öffnen**. Alternativ können Sie mit der Lösung fortfahren, die Sie nach Abschluss der vorherigen Übung abgerufen haben.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
3. Fügen Sie eine **Genre** Modell Klasse hinzu. Klicken Sie dazu mit der rechten Maustaste auf den Ordner **Models** im **Projektmappen-Explorer**, und wählen Sie **Hinzufügen** und dann die Option **Neues Element** aus. Wählen Sie unter **Code**das **Klassen** Element aus, benennen Sie die Datei *Genre.cs*, und klicken Sie dann auf **Hinzufügen**.

    ![Hinzufügen einer Klasse](aspnet-mvc-4-fundamentals/_static/image27.png "Hinzufügen einer Klasse")

    *Hinzufügen eines neuen Elements*

    ![Genre Modell Klasse hinzufügen](aspnet-mvc-4-fundamentals/_static/image28.png "Genre Modell Klasse hinzufügen")

    *Genre Modell Klasse hinzufügen*
4. Fügen Sie der Genre Klasse eine **Name** -Eigenschaft hinzu. Fügen Sie zu diesem Zweck den folgenden Code hinzu:

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex6 Genre*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Fügen Sie der gleichen Prozedur wie zuvor eine **Album** -Klasse hinzu. Klicken Sie dazu mit der rechten Maustaste auf den Ordner **Models** im **Projektmappen-Explorer**, und wählen Sie **Hinzufügen** und dann die Option **Neues Element** aus. Wählen Sie unter **Code**das **Klassen** Element aus, benennen Sie die Datei *Album.cs*, und klicken Sie dann auf **Hinzufügen**.
6. Fügen Sie der Album-Klasse zwei Eigenschaften hinzu: **Genre** und **Title**. Fügen Sie zu diesem Zweck den folgenden Code hinzu:

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex6 Album*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Aufgabe 2: Hinzufügen eines storebrow\viewmodel

In dieser Aufgabe wird ein **storebrowsviewmodel** verwendet, um die mit einem ausgewählten Genre abgleichen Alben anzuzeigen. In dieser Aufgabe erstellen Sie diese Klasse und fügen dann zwei Eigenschaften hinzu, um das **Genre** und seine **Album**Liste zu verarbeiten.

1. Fügen Sie eine **storebrowsviewmodel** -Klasse hinzu. Klicken Sie dazu mit der rechten Maustaste auf den Ordner **ViewModels** im **Projektmappen-Explorer**, und wählen Sie **Hinzufügen** und dann die Option **Neues Element** aus. Wählen Sie unter **Code**das **Klassen** Element aus, benennen Sie die Datei *StoreBrowseViewModel.cs*, und klicken Sie dann auf **Hinzufügen**.
2. Fügen Sie einen Verweis auf die Modelle in der **storebrowsviewmodel** -Klasse hinzu. Fügen Sie zu diesem Zweck den folgenden using-Namespace hinzu:

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex6 usingmodel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Fügen Sie zwei Eigenschaften zur **storebrowsviewmodel** -Klasse hinzu: **Genre** und **Alben**. Fügen Sie zu diesem Zweck den folgenden Code hinzu:

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex6 modelproperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> Was ist **List&lt;Album&gt;** ?: in dieser Definition wird die **List&lt;t&gt;** Type verwendet, wobei **t** den Typ einschränkt, zu dem Elemente dieser **Liste** gehören, in diesem Fall " **Album** " (oder einem seiner Nachfolger).
> 
> Diese Fähigkeit zum Entwerfen von Klassen und Methoden, die die Angabe eines oder mehrerer Typen verzögern, bis die Klasse oder Methode von Client Code deklariert und instanziiert wird, ist eine C# Funktion der Sprache **Generika**.
> 
> **List&lt;t&gt;** ist die generische Entsprechung des **ArrayList** -Typs und steht im **System. Collections. Generic** -Namespace zur Verfügung. Einer der Vorteile bei der Verwendung von **Generika** besteht darin, dass Sie bei der Angabe des Typs keine typüberprüfungs Vorgänge durchführen müssen, wie z. b. das Umwandeln der Elemente in ein **Album** wie bei einer **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Aufgabe 3: Verwenden des neuen ViewModel in StoreController

In dieser Aufgabe ändern Sie die Aktionsmethoden " **Browse** " und " **Details** " von **StoreController**, um das neue " **storebrowsseviewmodel**" zu verwenden.

1. Fügen Sie einen Verweis auf den Ordner Models in der **StoreController** -Klasse hinzu. Erweitern Sie hierzu den Ordner **Controllers** in der **Projektmappen-Explorer** , und öffnen Sie die **StoreController** -Klasse. Fügen Sie dann den folgenden Code hinzu:

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex6 usingmodelincontroller*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Ersetzen Sie die **Browse** -Aktionsmethode, um die **storeviewbrowsecontroller** -Klasse zu verwenden. Sie erstellen ein Genre und zwei neue Alben-Objekte mit Dummydaten (in der nächsten praktischen Übungseinheit verwenden Sie echte Daten aus einer Datenbank). Ersetzen Sie hierzu die **Browse** -Methode durch den folgenden Code:

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex6 browsemethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Ersetzen Sie die " **Details** "-Aktionsmethode zur Verwendung der **storeviewbrowsecontroller** -Klasse. Sie erstellen ein neues **Album** -Objekt, das an die **Ansicht**zurückgegeben wird. Ersetzen Sie **hierzu die Details** -Methode durch den folgenden Code:

    (Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex6 detailsmethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Aufgabe 4: Hinzufügen einer Vorlage zum Durchsuchen von Ansichten

In dieser Aufgabe fügen Sie eine **browseinsicht** hinzu, um die für ein bestimmtes Genre gefundenen Alben anzuzeigen.

1. Vor dem Erstellen der neuen Ansichts Vorlage sollten Sie das Projekt so erstellen, dass das Dialog **Feld "Ansicht hinzufügen** " die zu verwendende **ViewModel** -Klasse kennt. Erstellen Sie das Projekt, indem Sie das Menü Element **Build** auswählen und dann **mvcmusicstore erstellen**.
2. Fügen Sie eine **browseinsicht** hinzu. Klicken Sie hierzu mit der rechten Maustaste auf die Aktion zum **Durchsuchen** von **StoreController** , und klicken Sie auf **Ansicht hinzufügen**.
3. Überprüfen Sie im Dialogfeld **Ansicht hinzufügen** , ob der Ansichts Name **Durchsuchen**angezeigt wird. Aktivieren Sie das Kontrollkästchen für **eine stark typisierte Ansicht erstellen** , und wählen Sie in der Dropdown Liste **Modell Klasse** die Option **storebrowsviewmodel** aus. Belassen Sie die anderen Felder mit ihrem Standardwert. Klicken Sie anschließend auf **Hinzufügen**.

    ![Hinzufügen einer browseinsicht](aspnet-mvc-4-fundamentals/_static/image29.png "Hinzufügen einer browseinsicht")

    *Hinzufügen einer browseinsicht*
4. Ändern Sie die Datei **Browse. cshtml** , um die Informationen des Genres anzuzeigen, indem Sie auf das **storebrowseviewmodel** -Objekt zugreifen, das an die Ansichts Vorlage übergeben wird. Ersetzen Sie dazu den Inhalt durch Folgendes: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe testen Sie, ob die **Browse** -Methode Alben aus der Aktion "Methode **Durchsuchen** " abruft.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL in **/Store/Browse? Genre = Disco** , um zu überprüfen, ob die Aktion zwei Alben zurückgibt.

    ![Store-Disco-Alben durchsuchen](aspnet-mvc-4-fundamentals/_static/image30.png "Store-Disco-Alben durchsuchen")

    *Store-Disco-Alben durchsuchen*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Aufgabe 6: Anzeigen von Informationen zu einem bestimmten Album

In dieser Aufgabe implementieren Sie die Ansicht " **Store/Details** ", um Informationen zu einem bestimmten Album anzuzeigen. In dieser praktischen Übungseinheit ist alles, was Sie über das Album anzeigen werden, bereits in der **Ansichts** Vorlage enthalten. Anstatt also eine **storedetailsviewmodel** -Klasse zu erstellen, verwenden Sie die aktuelle **storebrowsviewmodel** -Vorlage, die das Album übergibt.

1. Schließen Sie den Browser bei Bedarf, um zum Visual Studio-Fenster zurückzukehren. Fügen Sie eine neue **Detail** Ansicht für die **Details** Aktionsmethode von **StoreController**hinzu. Klicken Sie dazu mit der rechten Maustaste auf die **Detail** Methode in der **StoreController** -Klasse, und klicken Sie auf **Ansicht hinzufügen**.
2. Überprüfen **Sie im Dialogfeld Ansicht hinzufügen** , ob der **Name der Sicht** **Details**lautet. Aktivieren Sie das Kontrollkästchen für **eine stark typisierte Ansicht erstellen** , und wählen Sie im Dropdown Feld **Modell Klasse** die Option **Album** aus. Belassen Sie die anderen Felder mit ihrem Standardwert. Klicken Sie anschließend auf **Hinzufügen**. Dadurch wird die Datei " **\views\store\details.cshtml** " erstellt und geöffnet.

    ![Hinzufügen einer Detailansicht](aspnet-mvc-4-fundamentals/_static/image31.png "Hinzufügen einer Detailansicht")

    *Hinzufügen einer Detailansicht*
3. Ändern Sie die Datei " **Details. cshtml** ", um die Informationen des Albums anzuzeigen, indem Sie auf das an die Ansichts Vorlage über gegebene **Album** Objekt zugreifen. Ersetzen Sie dazu den Inhalt durch Folgendes: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Aufgabe 7: Ausführen der Anwendung

In dieser Aufgabe testen Sie, ob in der **Detail** Ansicht die Informationen des Albums von der **Details-Aktions** Methode abgerufen werden.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der **Start** Seite gestartet. Ändern Sie die URL in **/Store/Details/5** , um die Informationen des Albums zu überprüfen.

    ![Alben-Detail durchsuchen](aspnet-mvc-4-fundamentals/_static/image32.png "Alben-Detail durchsuchen")

    *Details zum Browser Album*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Aufgabe 8: Hinzufügen von Links zwischen Seiten

In dieser Aufgabe fügen Sie einen Link in der Speicher Ansicht hinzu, um einen Link in jedem Genre Namen mit der entsprechenden **/Store/Browse** -URL zu erhalten. Auf diese Weise wird bei einem Klick auf ein Genre (z. a. **Disco**) zu **/Store/Browse? Genre = Disco** -URL navigiert.

1. Schließen Sie den Browser bei Bedarf, um zum Visual Studio-Fenster zurückzukehren. Aktualisieren Sie die **Index** Seite, um der Seite " **Durchsuchen** " einen Link hinzuzufügen. Zu diesem Zweck erweitern Sie im **Projektmappen-Explorer** den Ordner **Sichten** , und doppelklicken Sie auf die Seite **Index. cshtml** .
2. Fügen Sie der Such Ansicht einen Link hinzu, der angibt, dass das Genre ausgewählt ist. Ersetzen Sie hierzu den folgenden hervorgehobenen Code in den **&lt;Li&gt;** Tags: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > ein anderer Ansatz wäre eine direkte Verknüpfung mit der Seite mit einem Code wie dem folgenden:
   > 
   > &lt;a href =&quot;/Store/Browse? Genre =@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > Obwohl dieser Ansatz funktioniert, hängt er von einer hart codierten Zeichenfolge ab. Wenn Sie den Controller später umbenennen, müssen Sie diese Anweisung manuell ändern. Eine bessere Alternative ist die Verwendung einer **HTML** -Hilfsmethode. ASP.NET MVC enthält eine HTML-Hilfsmethode, die für solche Aufgaben verfügbar ist. Die Hilfsmethode **HTML. Action Link ()** vereinfacht die Erstellung von HTML- **&lt;einer&gt;** Links und stellt sicher, dass URL-Pfade ordnungsgemäß URL-codiert sind.
   > 
   > HTML. Action Link verfügt über mehrere über Ladungen. In dieser Übung verwenden Sie eine, die drei Parameter annimmt:
   > 
   > 1. Linktext, in dem der Genre Name angezeigt wird
   > 2. Controller Aktionsname (**Durchsuchen**)
   > 3. Routen Parameterwerte und angeben des Namens (**Genre**) und des Werts (**Genre Name**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Aufgabe 9: Ausführen der Anwendung

In dieser Aufgabe testen Sie, ob die einzelnen Genres mit einem Link zur entsprechenden **/Store/Browse** -URL angezeigt werden.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL in **/Store** , um sicherzustellen, dass jedes Genre mit der entsprechenden **/Store/Browse** -URL verknüpft ist.

    ![Durchsuchen von Genres mit Links zur Seite "Durchsuchen"](aspnet-mvc-4-fundamentals/_static/image33.png "Durchsuchen von Genres mit Links zur Seite "Durchsuchen"")

    *Durchsuchen von Genres mit Links zur Seite "Durchsuchen"*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Aufgabe 10: Verwenden der dynamischen ViewModel-Auflistung zum Übergeben von Werten

In dieser Aufgabe lernen Sie eine einfache und leistungsfähige Methode kennen, um Werte zwischen dem Controller und der Ansicht zu übergeben, ohne Änderungen am Modell vorzunehmen. ASP.NET MVC 4 stellt die Auflistung &quot;ViewModel-&quot;bereit, die einem beliebigen dynamischen Wert zugewiesen werden kann und auf die auch innerhalb von Controllern und Ansichten zugegriffen werden kann.

Sie verwenden nun die dynamische viewbag-Auflistung, um eine Liste mit &quot;**Sterne-Genres**&quot; vom Controller an die Ansicht zu übergeben. Die Speicher Index Sicht greift auf **ViewModel** zu und zeigt die Informationen an.

1. Schließen Sie den Browser bei Bedarf, um zum Visual Studio-Fenster zurückzukehren. Öffnen Sie **StoreController.cs** , und ändern Sie die **Index** -Methode, um eine Liste mit den Sternen Genres in der ViewModel-Auflistung

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > Sie können auch die Syntax **viewbag [&quot;Sterne&quot;]** verwenden, um auf die Eigenschaften zuzugreifen.
2. Das Stern Symbol **&quot;Sterne. png&quot;** ist im Ordner " **source\asset\images** " dieses Labs enthalten. Um Sie der Anwendung hinzuzufügen, ziehen Sie Ihren Inhalt aus einem **Windows-Explorer** -Fenster in das **Projektmappen-Explorer** in Visual Web Developer Express, wie unten dargestellt:

    ![Der Projekt Mappe wird ein Stern Bild hinzugefügt.](aspnet-mvc-4-fundamentals/_static/image34.png "Der Projekt Mappe wird ein Stern Bild hinzugefügt.")

    *Der Projekt Mappe wird ein Stern Bild hinzugefügt.*
3. Öffnen Sie die Ansicht " **Store/Index. cshtml** ", und ändern Sie den Inhalt. Sie lesen die &quot;&quot;-Eigenschaft in der **viewbag** -Auflistung und Fragen, ob der aktuelle Genre Name in der Liste enthalten ist. In diesem Fall wird ein Stern Symbol direkt zum Genre Link angezeigt.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Aufgabe 11: Ausführen der Anwendung

In dieser Aufgabe testen Sie, ob die gesternten Genres ein Stern Symbol anzeigen.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der **Start** Seite gestartet. Ändern Sie die URL in **/Store** , um sicherzustellen, dass jedes der vorgestellten Genres die Bezeichnung "Achtung" hat:

    ![Durchsuchen von Genres mit gestarteten Elementen](aspnet-mvc-4-fundamentals/_static/image35.png "Durchsuchen von Genres mit gestarteten Elementen")

    *Durchsuchen von Genres mit gestarteten Elementen*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Übung 7: A Lap around ASP.NET MVC 4 New Template

In dieser Übung werden die Verbesserungen in den ASP.NET MVC 4-Projektvorlagen erläutert, wobei die relevantesten Features der neuen Vorlage erläutert werden.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Aufgabe 1: Untersuchen der ASP.NET MVC 4-Internet Anwendungs Vorlage

1. Wenn Sie nicht bereits geöffnet ist, starten Sie **vs Express für Web**
2. Wählen Sie die Datei aus. **Neu |** Menübefehl "Projekt". Wählen Sie im Dialogfeld **Neues Projekt** die **Option C#Visual | Webvorlage** im linken Fensterbereich, und wählen Sie die **ASP.NET MVC 4-Webanwendung**aus. **Nennen** Sie das *Projekt Musicstore* *, und*aktualisieren Sie den Projektmappennamen, und wählen Sie dann einen Speicherort aus (oder über **lassen Sie den**Standardwert)

    ![Erstellen eines neuen ASP.NET MVC 4-Projekts](aspnet-mvc-4-fundamentals/_static/image36.png "Erstellen eines neuen ASP.NET MVC 4-Projekts")

    *Erstellen eines neuen ASP.NET MVC 4-Projekts*
3. Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Projektvorlage **Internet Anwendung** aus, und klicken Sie auf **OK**. Beachten Sie, dass Sie entweder Razor oder aspx als Ansichts-Engine auswählen können.

    ![Erstellen einer neuen ASP.NET MVC 4-Internet Anwendung](aspnet-mvc-4-fundamentals/_static/image37.png "Erstellen einer neuen ASP.NET MVC 4-Internet Anwendung")

    *Erstellen einer neuen ASP.NET MVC 4-Internet Anwendung*

    > [!NOTE]
    > Razor-Syntax wurde in ASP.NET MVC 3 eingeführt. Das Ziel besteht darin, die Anzahl der in einer Datei benötigten Zeichen und Tastatureingaben zu minimieren und so einen schnellen und flüssigen Codierungs Workflow zu ermöglichen. Razor nutzt vorhandene C#/VB (oder andere) Sprachkenntnisse und bietet eine Vorlagen Markup Syntax, die einen tollen HTML-Konstruktions Workflow ermöglicht.
4. Drücken Sie **F5** , um die Projekt Mappe auszuführen und die erneuerte Vorlage anzuzeigen. Sehen Sie sich die folgenden Features an:

    1. **Moderne Vorlagen**

        Die Vorlagen wurden erneuert und bieten modernere Stile.

        ![ASP.NET MVC 4-Vorlagen mit Neuformatierung](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4-Vorlagen mit Neuformatierung")

        *ASP.NET MVC 4-Vorlagen mit Neuformatierung*
    2. **Adaptive Rendering**

        Sehen Sie sich die Größe des Browserfensters an, und beachten Sie, wie das Seitenlayout dynamisch an die neue Fenstergröße angepasst wird. Diese Vorlagen verwenden die Adaptive renderingtechnik, um auf Desktop-und mobilen Plattformen ohne Anpassung ordnungsgemäß zu rendern.

        ![ASP.NET MVC 4-Projektvorlage in unterschiedlichen Browser Größen](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4-Projektvorlage in unterschiedlichen Browser Größen")

        *ASP.NET MVC 4-Projektvorlage in unterschiedlichen Browser Größen*
5. Schließen Sie den Browser, um den Debugger zu beenden und zu Visual Studio zurückzukehren.
6. Jetzt können Sie die Lösung untersuchen und einige der neuen Features kennenlernen, die von ASP.NET MVC 4 in der Projektvorlage eingeführt wurden.

    ![ASP.net MVC4-Internet-Application-Project-Template](aspnet-mvc-4-fundamentals/_static/image40.png "Die Projektvorlage "ASP.NET MVC 4 Internet Application"")

    *Die Projektvorlage "ASP.NET MVC 4 Internet Application"*

   1. **HTML5-Markup**

       Durchsuchen Sie Vorlagen Sichten, um das neue Design Markup zu ermitteln, z. b. Open **about. cshtml** - **Ansicht im Basis** Ordner.

       ![Neue Vorlage mit Razor-und HTML5-Markup](aspnet-mvc-4-fundamentals/_static/image41.png "Neue Vorlage mit Razor-und HTML5-Markup")

       *Neue Vorlage mit Razor-und HTML5-Markup*
   2. **Enthaltene JavaScript-Bibliotheken**

      1. **jQuery**: jQuery vereinfacht das Durchlaufen von HTML-Dokumenten, Ereignis Behandlung, Animationen und AJAX-Interaktionen.
      2. **jQuery-Benutzeroberfläche**: Diese Bibliothek bietet Abstraktionen für Interaktion und Animation auf niedriger Ebene, erweiterte Effekte und Aufzähl Bare Widgets, die auf der jQuery JavaScript-Bibliothek aufbaut.

         > [!NOTE]
         > Weitere Informationen zu jQuery und jQuery UI finden Sie in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).
      3. **Knockoutjs**: die ASP.NET MVC 4-Standardvorlage enthält jetzt " **knockoutjs**", ein JavaScript-MVVM-Framework, mit dem Sie umfangreiche und sehr reaktionsfähige Webanwendungen mit JavaScript und HTML erstellen können. Wie in ASP.NET MVC 3 sind auch jQuery-und jQuery-UI-Bibliotheken in ASP.NET MVC 4 enthalten.

          > [!NOTE]
          > Weitere Informationen zur Datei "knockoutjs" finden Sie unter diesem Link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).
      4. **Modernizr**: Diese Bibliothek wird automatisch ausgeführt, sodass Ihre Site bei Verwendung von HTML5-und CSS3-Technologien mit älteren Browsern kompatibel ist.

          > [!NOTE]
          > Weitere Informationen zur modernizr-Bibliothek finden Sie unter diesem Link: [http://www.modernizr.com/](http://www.modernizr.com/).
   3. **Simplemembership in der Lösung enthalten**

       Simplemembership wurde als Ersatz für das vorherige ASP.NET-Rollen-und Mitgliedschafts Anbieter System entwickelt. Es verfügt über viele neue Features, die es dem Entwickler erleichtern, Webseiten auf flexiblere Weise zu schützen.

       In der Internet Vorlage sind bereits einige Dinge zum Integrieren von simplemembership eingerichtet. der AccountController ist beispielsweise für die Verwendung von oauthwebsecurity (für die OAuth-Kontoregistrierung, Anmeldung, Verwaltung usw.) und die Websicherheit vorbereitet.

       ![Simplemembership in der Lösung enthalten](aspnet-mvc-4-fundamentals/_static/image42.png "Simplemembership in der Lösung enthalten")

       *Simplemembership in der Lösung enthalten*

       > [!NOTE]
       > Weitere Informationen zu [oauthwebsecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) finden Sie in MSDN.

> [!NOTE]
> Außerdem können Sie diese Anwendung in Windows Azure-Websites bereitstellen, [indem Sie Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy verwenden](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Durch die Durchführung dieses praktischen Labs haben Sie die Grundlagen von ASP.NET MVC kennengelernt:

- Die Kernelemente einer MVC-Anwendung und ihre Interaktion
- Erstellen einer ASP.NET-MVC-Anwendung
- Hinzufügen und Konfigurieren von Controllern zum Verarbeiten von Parametern, die über die URL und QueryString geleitet werden
- Vorgehensweise beim Hinzufügen einer layoutmasterseite zum Einrichten einer Vorlage für gemeinsamen HTML-Inhalt, eines Stylesheets zum Verbessern von Aussehen und Verhalten und einer Ansichts Vorlage zum Anzeigen von HTML-Inhalten
- Verwenden des ViewModel-Musters zum Übergeben von Eigenschaften an die Ansichts Vorlage zum Anzeigen dynamischer Informationen
- Verwenden von Parametern, die an Controller in der Ansichts Vorlage übermittelt werden
- Hinzufügen von Links zu Seiten in der ASP.NET MVC-Anwendung
- Vorgehensweise beim Hinzufügen und verwenden dynamischer Eigenschaften in einer Ansicht
- Verbesserungen in den ASP.NET MVC 4-Projektvorlagen

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren. Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.

1. Wechseln Sie zu [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Windows Azure SDK</em>&quot;suchen.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.
3. Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.

    ![Installieren von Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Installationsfortschritt*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.

    ![Installation abgeschlossen](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Installation abgeschlossen*
7. Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .
8. Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .

    ![VS Express für Web-Kachel](aspnet-mvc-4-fundamentals/_static/image47.png)

    *VS Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy

In diesem Anhang wird gezeigt, wie Sie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und die Anwendung veröffentlichen, die Sie durch die folgenden Schritte abgerufen haben. nutzen Sie dabei die von Windows Azure bereitgestellte Funktion zur Web deploy Veröffentlichung.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website über das Windows Azure-Portal

1. Wechseln Sie zum [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmelde Informationen an, die Ihrem Abonnement zugeordnet sind.

    > [!NOTE]
    > Mit Windows Azure können Sie 10 ASP.NET Websites kostenlos hosten und dann skalieren, wenn Ihr Datenverkehr zunimmt. Sie können sich [hier](https://aka.ms/aspnet-hol-azure)registrieren.

    ![Anmelden bei Windows Azure-Portal](aspnet-mvc-4-fundamentals/_static/image48.png "Anmelden bei Windows Azure-Portal")

    *Anmelden bei Windows Azure Verwaltungsportal*
2. Klicken Sie in der Befehlsleiste auf **neu** .

    ![Erstellen einer neuen Website](aspnet-mvc-4-fundamentals/_static/image49.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **Compute** - | **Website**. Wählen Sie dann **schneller** Fassung aus. Geben Sie eine verfügbare URL für die neue Website an, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Eine Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud ausgeführt wird und die Sie steuern und verwalten können. Mit der Option schneller Fassung können Sie eine abgeschlossene Webanwendung von außerhalb des Portals auf der Windows Azure-Website bereitstellen. Sie enthält keine Schritte zum Einrichten einer Datenbank.

    ![Erstellen einer neuen Website mithilfe der schneller Fassung](aspnet-mvc-4-fundamentals/_static/image50.png "Erstellen einer neuen Website mithilfe der schneller Fassung")

    *Erstellen einer neuen Website mithilfe der schneller Fassung*
4. Warten Sie, bis die neue **Website** erstellt wurde.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** -Spalte. Überprüfen Sie, ob die neue Website funktioniert.

    ![Navigieren zur neuen Website](aspnet-mvc-4-fundamentals/_static/image51.png "Navigieren zur neuen Website")

    *Navigieren zur neuen Website*

    ![Website wird ausgeführt](aspnet-mvc-4-fundamentals/_static/image52.png "Website wird ausgeführt")

    *Website wird ausgeführt*
6. Wechseln Sie zurück zum Portal, und klicken Sie in der Spalte **Name** auf den Namen der Website, um die Verwaltungs Seiten anzuzeigen.

    ![Öffnen der Website-Verwaltungs Seiten](aspnet-mvc-4-fundamentals/_static/image53.png "Öffnen der Website-Verwaltungs Seiten")

    *Öffnen der Website-Verwaltungs Seiten*
7. Klicken Sie auf der Seite **Dashboard** im Abschnitt **kurzer Blick** auf den Link **Veröffentlichungs Profil herunterladen** .

    > [!NOTE]
    > Das *Veröffentlichungs Profil* enthält alle Informationen, die zum Veröffentlichen einer Webanwendung auf einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungs Methoden erforderlich sind. Das Veröffentlichungs Profil enthält die URLs, Benutzer Anmelde Informationen und Daten Bank Zeichenfolgen, die erforderlich sind, um eine Verbindung mit den einzelnen Endpunkten herzustellen und diese zu authentifizieren, für die eine Veröffentlichungs Methode aktiviert ist. **Microsoft webmatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** unterstützen das Lesen von Veröffentlichungs Profilen, um die Konfiguration dieser Programme für das Veröffentlichen von Webanwendungen auf Windows Azure-Websites zu automatisieren.

    ![Das Website-Veröffentlichungs Profil wird heruntergeladen.](aspnet-mvc-4-fundamentals/_static/image54.png "Das Website-Veröffentlichungs Profil wird heruntergeladen.")

    *Das Website-Veröffentlichungs Profil wird heruntergeladen.*
8. Laden Sie die Veröffentlichungs Profil Datei an einen bekannten Speicherort herunter. In dieser Übung erfahren Sie, wie Sie diese Datei zum Veröffentlichen einer Webanwendung auf Windows Azure-Websites in Visual Studio verwenden.

    ![Die Veröffentlichungs Profil Datei wird gespeichert.](aspnet-mvc-4-fundamentals/_static/image55.png "Das Veröffentlichungs Profil wird gespeichert.")

    *Die Veröffentlichungs Profil Datei wird gespeichert.*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: Konfigurieren des Datenbankservers

Wenn Ihre Anwendung SQL Server Datenbanken nutzt, müssen Sie einen SQL-Daten Bank Server erstellen. Wenn Sie eine einfache Anwendung bereitstellen möchten, die SQL Server nicht verwendet, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank. Sie können die SQL-Datenbankserver aus Ihrem Abonnement im Windows Azure-Verwaltungs Portal unter **SQL-Datenbanken** | **Server** | **Dashboard des Servers**anzeigen. Wenn Sie keinen Server erstellt haben, können Sie ihn mithilfe der Schaltfläche **Hinzufügen** auf der Befehlsleiste erstellen. Notieren Sie sich den **Servernamen und die URL, den Administrator Anmelde Namen und das Kennwort**, da Sie Sie in den nächsten Aufgaben verwenden werden. Erstellen Sie die Datenbank noch nicht, da Sie in einer späteren Phase erstellt wird.

    ![Dashboard des SQL-Datenbankservers](aspnet-mvc-4-fundamentals/_static/image56.png "Dashboard des SQL-Datenbankservers")

    *Dashboard des SQL-Datenbankservers*
2. In der nächsten Aufgabe testen Sie die Datenbankverbindung aus Visual Studio. aus diesem Grund müssen Sie Ihre lokale IP-Adresse in die Liste der **zulässigen IP-Adressen**des Servers einschließen. Klicken Sie hierzu auf **Konfigurieren**, wählen Sie die IP-Adresse der **aktuellen Client-IP-Adresse** aus, und fügen Sie Sie in die Textfelder Start-IP- **Adresse** und End- **IP-Adresse** ein, und klicken Sie auf die Schaltfläche ![Add-Client-IP-Address-OK-Button](aspnet-mvc-4-fundamentals/_static/image57.png)

    ![Client-IP-Adresse](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Client-IP-Adresse*
3. Sobald die **Client-IP-Adresse** der Liste zulässige IP-Adressen hinzugefügt wurde, klicken Sie auf **Speichern** , um die Änderungen zu bestätigen.

    ![Änderungen bestätigen](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Änderungen bestätigen*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy

1. Kehren Sie zur ASP.NET MVC 4-Lösung zurück. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Website Projekt, und wählen Sie **veröffentlichen**aus.

    ![Veröffentlichen der Anwendung](aspnet-mvc-4-fundamentals/_static/image60.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungs Profil, das Sie in der ersten Aufgabe gespeichert haben.

    ![Importieren des Veröffentlichungs Profils](aspnet-mvc-4-fundamentals/_static/image61.png "Importieren des Veröffentlichungs Profils")

    *Veröffentlichungs Profil wird importiert.*
3. Klicken Sie auf **Verbindung**überprüfen. Klicken Sie nach Abschluss der Überprüfung auf **weiter**.

    > [!NOTE]
    > Die Überprüfung ist fertiggestellt, sobald ein grünes Häkchen neben der Schaltfläche Verbindung überprüfen angezeigt wird.

    ![Die Verbindung wird überprüft.](aspnet-mvc-4-fundamentals/_static/image62.png "Die Verbindung wird überprüft.")

    *Die Verbindung wird überprüft.*
4. Klicken Sie auf der Seite **Einstellungen** unter dem Abschnitt **Datenbanken** auf die Schaltfläche neben dem Textfeld der Datenbankverbindung (z. b. **DefaultConnection**).

    ![Webbereitstellungs Konfiguration](aspnet-mvc-4-fundamentals/_static/image63.png "Webbereitstellungs Konfiguration")

    *Webbereitstellungs Konfiguration*
5. Konfigurieren Sie die Datenbankverbindung wie folgt:

   - Geben Sie unter **Server Name** die URL des SQL-Datenbankservers unter Verwendung des *TCP:* -Präfix ein.
   - Geben Sie unter **Benutzername** den Anmelde Namen des Server Administrators ein.
   - Geben Sie unter **Kennwort** Ihren Server Administrator-Anmelde Kennwort ein.
   - Geben Sie einen neuen Datenbanknamen ein, z. b.: *MVC4SampleDB*.

     ![Konfigurieren der Ziel Verbindungs Zeichenfolge](aspnet-mvc-4-fundamentals/_static/image64.png "Konfigurieren der Ziel Verbindungs Zeichenfolge")

     *Konfigurieren der Ziel Verbindungs Zeichenfolge*
6. Klicken Sie dann auf **OK**. Wenn Sie zum Erstellen der Datenbank aufgefordert werden, klicken Sie auf **Ja**.

    ![Erstellen der Datenbank](aspnet-mvc-4-fundamentals/_static/image65.png "Erstellen der Daten Bank Zeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungs Zeichenfolge, die Sie zum Herstellen einer Verbindung mit der SQL-Datenbank in Windows Azure verwenden, wird im Textfeld Standardverbindung angezeigt. Klicken Sie dann auf **Weiter**.

    ![Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank](aspnet-mvc-4-fundamentals/_static/image66.png "Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank")

    *Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank*
8. Klicken Sie auf der Seite **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](aspnet-mvc-4-fundamentals/_static/image67.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Nachdem der Veröffentlichungs Vorgang abgeschlossen ist, wird die veröffentlichte Website in Ihrem Standardbrowser geöffnet.

    ![In Windows Azure veröffentlichte Anwendung](aspnet-mvc-4-fundamentals/_static/image68.png "In Windows Azure veröffentlichte Anwendung")

    *In Windows Azure veröffentlichte Anwendung*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Anhang C: Verwenden von Code Ausschnitten

Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen. Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-fundamentals/_static/image69.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")

*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*

***So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)***

1. Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.
2. Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).
3. Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.
4. Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).
5. Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.

![Beginnen Sie mit der Eingabe des Ausschnitt namens.](aspnet-mvc-4-fundamentals/_static/image70.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")

*Beginnen Sie mit der Eingabe des Ausschnitt namens.*

![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](aspnet-mvc-4-fundamentals/_static/image71.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")

*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*

![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](aspnet-mvc-4-fundamentals/_static/image72.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")

*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*

So ***fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)*** 1. Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.

1. Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.
2. Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.

![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](aspnet-mvc-4-fundamentals/_static/image73.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")

*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*

![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](aspnet-mvc-4-fundamentals/_static/image74.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")

*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*
