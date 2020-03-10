---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4-Hilfsprogramme, Formulare und Validierung | Microsoft-Dokumentation
author: rick-anderson
description: In ASP.NET MVC 4-Modellen und in der praktischen Übungseinheit für den Datenzugriff haben Sie Daten aus der Datenbank geladen und angezeigt. In dieser praktischen Übungseinheit fügen Sie den...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 0e2605a4188eaf814f6ab0ebfeaabed4457bcfa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433791"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>ASP.NET MVC 4 – Hilfsprogramme, Formulare und Überprüfung

vom [Web Camps-Team](https://twitter.com/webcamps)

[Webcamps-Trainingskit herunterladen](https://aka.ms/webcamps-training-kit)

In **ASP.NET MVC 4-Modellen und** in der praktischen Übungseinheit für den Datenzugriff haben Sie Daten aus der Datenbank geladen und angezeigt. In dieser praktischen Übungseinheit fügen Sie der **Music Store** -Anwendung die Möglichkeit hinzu, diese Daten zu bearbeiten.

Mit diesem Ziel erstellen Sie zunächst den Controller, der die Aktionen zum Erstellen, lesen, aktualisieren und löschen (CRUD) von Alben unterstützt. Sie generieren eine Index Ansichts Vorlage, die die ASP.NET MVC-Gerüstbau Funktion nutzt, um die Eigenschaften der Alben in einer HTML-Tabelle anzuzeigen. Um diese Ansicht zu erweitern, fügen Sie ein benutzerdefiniertes HTML-Hilfsobjekt hinzu, mit dem lange Beschreibungen abgeschnitten werden.

Anschließend fügen Sie die Ansichten "Bearbeiten" und "erstellen" hinzu, mit denen Sie die Alben in der Datenbank mit Hilfe von Formular Elementen wie Dropdown Listen ändern können.

Schließlich können Sie Benutzer ein Album löschen und verhindern, dass Sie falsche Daten eingeben, indem Sie Ihre Eingaben validieren.

Diese praktische Übung geht davon aus, dass Sie über grundlegende Kenntnisse von **ASP.NET MVC**verfügen. Wenn Sie **ASP.NET MVC** noch nicht verwendet haben, empfehlen wir Ihnen, **ASP.NET MVC-Grundlagen** im praktischen Lab zu überspringen.

Diese Übungseinheit führt Sie durch die Verbesserungen und neuen Features, die Sie zuvor beschrieben haben, indem Sie kleinere Änderungen an einer im Quellordner bereitgestellten beispielweb Anwendung anwenden.

> [!NOTE]
> Der gesamte Beispielcode und die Code Ausschnitte sind im webcamps-Trainingskit enthalten, das unter [Microsoft-Web/webcamptrainingkit-Releases](https://aka.ms/webcamps-training-kit)verfügbar ist. Das für dieses Lab spezifische Projekt ist unter [ASP.NET MVC 4-Hilfsprogramme, Formularen und Validierung](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation)verfügbar.

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie Folgendes:

- Erstellen eines Controllers zur Unterstützung von CRUD-Vorgängen
- Generieren einer Index Sicht zum Anzeigen von Entitäts Eigenschaften in einer HTML-Tabelle
- Hinzufügen benutzerdefinierter HTML-Hilfsprogramme
- Erstellen und Anpassen einer Bearbeitungs Ansicht
- Unterscheiden zwischen Aktionsmethoden, die entweder auf HTTP-GET oder http-Post-Aufrufe reagieren
- Hinzufügen und Anpassen einer CREATE VIEW
- Löschen einer Entität
- Validieren von Benutzereingaben

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Labs müssen Sie über Folgendes verfügen:

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder Superior (Informationen zur Installation finden Sie in [Anhang A](#AppendixA) ).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Einrichten

**Installieren von Code Ausschnitten**

Der Vorteil ist, dass ein Großteil des Codes, den Sie in diesem Lab verwalten, als Visual Studio-Code Ausschnitte verfügbar ist. Um die Code Ausschnitte zu installieren, führen Sie **.\source\setup\codesnippeer-.vsi** -Datei aus.

Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, finden Sie den Anhang dieses Dokuments &quot;[Anhang B: Verwenden von Code Ausschnitten](#AppendixB)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exerzitien

Die folgenden Übungen bilden diese praktische Übungseinheit:

1. [Erstellen des Store Manager-Controllers und seiner Index Ansicht](#Exercise1)
2. [Hinzufügen eines HTML-Hilfsprogramms](#Exercise2)
3. [Erstellen der Bearbeitungs Ansicht](#Exercise3)
4. [Hinzufügen einer CREATE VIEW](#Exercise4)
5. [Behandeln der Löschung](#Exercise5)
6. [Hinzufügen der Validierung](#Exercise6)
7. [Verwenden von unaufdringlichen jQuery auf Client Seite](#Exercise7)

> [!NOTE]
> Jede Übung wird von einem **endordner** begleitet, der die sich ergebende Lösung enthält, die Sie nach Abschluss der Übungen erhalten. Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Durcharbeiten der Übungen benötigen.

Geschätzte Zeit bis zum Abschluss dieses Labs: **60 Minuten**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Übung 1: Erstellen des Store Manager-Controllers und seiner Index Ansicht

In dieser Übung erfahren Sie, wie Sie einen neuen Controller für die Unterstützung von CRUD-Vorgängen erstellen, seine Index Aktionsmethode anpassen, um eine Liste der Alben aus der Datenbank zurückzugeben, und schließlich eine Index Ansichts Vorlage erzeugen, die den ASP.NET MVC-Gerüstbau nutzt. Funktion, mit der die Eigenschaften der Alben in einer HTML-Tabelle angezeigt werden.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Aufgabe 1: Erstellen von storemanagercontroller

In dieser Aufgabe erstellen Sie einen neuen Controller mit dem Namen **storemanagercontroller** , um CRUD-Vorgänge zu unterstützen.

1. Öffnen Sie **die Projekt** Mappe "Start", die sich unter **Quelle/EX1-kreatingderstoremanagercontroller/BEGIN/** Folder befindet.

   1. Sie müssen einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Fügen Sie einen neuen Controller hinzu. Klicken Sie dazu mit der rechten Maustaste auf den Ordner **Controllers** innerhalb der Projektmappen-Explorer, und wählen Sie **Hinzufügen** und dann den **Controller** -Befehl aus. Ändern Sie den **Controller** **Namen** in **storemanagercontroller** , und stellen Sie sicher, dass die Option **MVC-Controller mit leeren Lese-/Schreibaktionen** ausgewählt ist. Klicken Sie auf **Hinzufügen**.

    ![Controller Dialog hinzufügen](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Controller Dialog hinzufügen")

    *Controller Dialog hinzufügen*

    Eine neue Controller Klasse wird generiert. Da Sie angegeben haben, dass Aktionen für Lese-/Schreibzugriff hinzugefügt werden, werden Stubmethoden für diese, häufige CRUD-Aktionen erstellt, bei denen TODO-Kommentare ausgefüllt sind, um die anwendungsspezifische Logik einzuschließen.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Aufgabe 2: Anpassen des StoreManager-Indexes

In dieser Aufgabe wird die Aktionsmethode des StoreManager-Indexes angepasst, um eine Ansicht mit der Liste der Alben aus der Datenbank zurückzugeben.

1. Fügen Sie in der storemanagercontroller-Klasse die folgenden *using* -Direktiven hinzu.

    (Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und Formulare und Validierung-EX1 mithilfe von mvcmusicstore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Fügen Sie dem **storemanagercontroller** ein Feld hinzu, um eine Instanz von " **musicstoreentities** " zu speichern.

    (Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und-Formulare und-Validierung-EX1 musicstoreentities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implementieren Sie die storemanagercontroller-Index Aktion, um eine Ansicht mit der Liste der Alben zurückzugeben.

    Die Aktions Logik des Controllers ähnelt der zuvor geschriebenen Index Aktion von StoreController. Verwenden Sie LINQ, um alle Alben abzurufen, einschließlich Genre-und Künstlerinformationen für die Anzeige.

    (Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und Formulare und Validierung-EX1 storemanagercontroller-Index*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Aufgabe 3: Erstellen der Index Ansicht

In dieser Aufgabe erstellen Sie die Vorlage Index Ansicht, um die Liste der vom **StoreManager** -Controller zurückgegebenen Alben anzuzeigen.

1. Vor dem Erstellen der neuen Ansichts Vorlage sollten Sie das Projekt so erstellen, dass das **Dialog Feld "Ansicht hinzufügen** " die zu verwendende **Album** Klasse kennt. **Build auswählen | Erstellen Sie mvcmusicstore** , um das Projekt zu erstellen.
2. Klicken Sie mit der rechten Maustaste in die **Index** Aktionsmethode, und wählen Sie **Ansicht hinzufügen**. Dadurch wird das Dialog **Feld Ansicht hinzufügen** angezeigt.

    ![Ansicht hinzufügen](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Ansicht hinzufügen")

    *Hinzufügen einer Sicht aus der Index-Methode*
3. Überprüfen Sie im Dialogfeld Ansicht hinzufügen, ob der Ansichts Name **Index**lautet. Wählen Sie die Option zum **Erstellen einer stark typisierten Ansicht** aus, und wählen Sie in der Dropdown-Datei der **Modell Klasse** die Option **Album (mvcmusicstore. Models)** aus. Wählen Sie in der Dropdown Liste **Gerüst Vorlage** die Option **Liste** aus. Belassen Sie die **Ansichts-Engine** **Razor** und die anderen Felder mit ihrem Standardwert, und klicken Sie dann auf **Hinzufügen**.

    ![Hinzufügen einer Index Ansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Hinzufügen einer Index Ansicht")

    *Hinzufügen einer Index Ansicht*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Aufgabe 4: Anpassen des Gerüsts der Index Ansicht

In dieser Aufgabe passen Sie die einfache Ansichts Vorlage an, die mit ASP.NET MVC-Gerüstbau Funktion erstellt wurde, damit die gewünschten Felder angezeigt werden.

> [!NOTE]
> Die **Gerüstbau** Unterstützung in ASP.NET MVC generiert eine einfache Ansichts Vorlage, die alle Felder im Album Modell auflistet. **Gerüstbau** bietet einen schnellen Einstieg in eine stark typisierte Ansicht: statt die Ansichts Vorlage manuell schreiben zu müssen, generiert Gerüstbau schnell eine Standardvorlage, und Sie können den generierten Code ändern.

1. Überprüfen Sie den erstellten Code. Die generierte Liste der Felder ist Teil der folgenden HTML-Tabelle, die von **Gerüstbau** zum Anzeigen von Tabellendaten verwendet wird.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Ersetzen Sie die **&lt;Tabelle&gt;** Code durch den folgenden Code, um nur die Felder **Genre**, **Maler**, **Album Titel**und **Preis** anzuzeigen. Hierdurch werden die Spalten " **albumId** " und " **Album** " angezeigt. Außerdem werden die Spalten GenreID und artistId geändert, um die Eigenschaften der verknüpften Klasse von **Artist.Name** und **Genre.Name**anzuzeigen und den Link **Details** zu entfernen.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Ändern Sie die folgenden Beschreibungen.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe überprüfen Sie, ob die Vorlage für die Vorlage " **StoreManager** **Index** View" nach dem Entwurf der vorherigen Schritte eine Liste von Alben anzeigt.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL in **/StoreManager** , um zu überprüfen, ob eine Liste mit Alben angezeigt wird, die Ihren **Titel**, Ihren **Künstler** und Ihr **Genre**anzeigt.

    ![Durchsuchen der Liste der Alben](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Durchsuchen der Liste der Alben")

    *Durchsuchen der Liste der Alben*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Übung 2: Hinzufügen eines HTML-Hilfsprogramms

Die Seite "StoreManager Index" hat ein mögliches Problem: die Eigenschaften "Titel" und "Name des Künstlers" können beide lang genug sein, um die Tabellen Formatierung auszulösen. In dieser Übung erfahren Sie, wie Sie ein benutzerdefiniertes HTML-Hilfsprogramm hinzufügen, um diesen Text abzuschneiden.

In der folgenden Abbildung können Sie sehen, wie das Format aufgrund der Länge des Texts geändert wird, wenn Sie eine kleine Browsergröße verwenden.

![Durchsuchen der Liste der Alben ohne Abgeschnittener Text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Durchsuchen der Liste der Alben ohne Abgeschnittener Text")

*Durchsuchen der Liste der Alben ohne Abgeschnittener Text*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Aufgabe 1: Erweitern des HTML-Hilfsprogramms

In dieser Aufgabe fügen Sie dem **HTML** -Objekt, das in ASP.NET MVC-Ansichten verfügbar gemacht wird, eine **neue Methode hinzu** . Zu diesem Zweck implementieren Sie eine **Erweiterungsmethode** für die integrierte **System. Web. MVC. htmlhelper** -Klasse, die von ASP.NET MVC bereitgestellt wird.

> [!NOTE]
> Weitere Informationen zu **Erweiterungs Methoden**finden Sie in diesem MSDN-Artikel. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).

1. Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Source/EX2-addinganhtmlhelper/BEGIN/** Folder". Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Öffnen Sie die Index Ansicht von StoreManager. Zu diesem Zweck erweitern Sie im Projektmappen-Explorer den Ordner " **views** " **, und öffnen Sie dann die Datei** " **Index. cshtml** ".
3. Fügen Sie den folgenden Code unterhalb der <strong>@model</strong> -Direktive hinzu, um die <strong>TRUNCATE</strong> Helper-Methode zu definieren.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Aufgabe 2: Abschneiden von Text auf der Seite

In dieser Aufgabe verwenden Sie die **TRUNCATE** -Methode, um den Text in der Ansichts Vorlage abzuschneiden.

1. Öffnen Sie die Index Ansicht von StoreManager. Zu diesem Zweck erweitern Sie im Projektmappen-Explorer den Ordner " **views** " **, und öffnen Sie dann die Datei** " **Index. cshtml** ".
2. Ersetzen Sie die Zeilen, die den **Namen des Künstlers** und den **Titel**des Albums anzeigen. Ersetzen Sie hierzu die folgenden Zeilen.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe testen Sie, ob die Vorlage " **StoreManager** **Index** View" den Titel und den Namen des Albums abschneidet.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL in **/StoreManager** , um zu überprüfen, ob lange Texte in der Spalte **Titel** und **Künstler** abgeschnitten sind.

    ![Abgeschnittene Titel und Künstlernamen](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Abgeschnittene Titel und Künstlernamen")

    *Abgeschnittene Titel und Künstlernamen*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Übung 3: Erstellen der Bearbeitungs Ansicht

In dieser Übung erfahren Sie, wie Sie ein Formular erstellen, mit dem Store-Manager ein Album bearbeiten können. Sie durchsucht die **/StoreManager/Edit/ID** -URL (**ID** ist die eindeutige ID des zu bearbeitenden Albums) und führt somit einen HTTP-GET-Befehl an den Server aus.

Die Controller Bearbeitungs Aktionsmethode Ruft das entsprechende Album aus der Datenbank ab, erstellt ein **storemanagerviewmodel** -Objekt, um es zu Kapseln (zusammen mit einer Liste von Künstlern und Genres), und übergibt es dann an eine Ansichts Vorlage, um die HTML-Seite wieder an den Benutzer zu rendern. Diese Seite enthält ein **&lt;Formular&gt;** Element mit Textfeldern und Dropdown Listen zum Bearbeiten der Album Eigenschaften.

Sobald der Benutzer die Album Formular Werte aktualisiert und auf die Schaltfläche **Speichern** klickt, werden die Änderungen über einen HTTP-Post-Rückruf an **/StoreManager/Edit/ID**übermittelt. Obwohl die URL mit der des letzten Aufrufes übereinstimmt, identifiziert ASP.NET MVC, dass es sich hierbei um eine HTTP-Post-Methode handelt, und führt daher eine andere Bearbeitungs Aktionsmethode aus (eine mit **[HttpPost]** versehen).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Aufgabe 1: Implementieren der HTTP-Get Edit-Aktionsmethode

In dieser Aufgabe implementieren Sie die HTTP-Get-Version der Edit-Aktionsmethode, um das entsprechende Album aus der Datenbank abzurufen, sowie eine Liste aller Genres und Künstler. Diese Daten werden in das im letzten Schritt definierte **storemanagerviewmodel** -Objekt Paketieren, das dann an eine Ansichts Vorlage übergeben wird, um die Antwort mit zu erzeugen.

1. Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Source/EX3-kreatingdie EditView/BEGIN/** Folder". Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Öffnen Sie die **storemanagercontroller** -Klasse. Erweitern Sie hierzu den Ordner **Controller** , und doppelklicken Sie auf **StoreManagerController.cs**.
3. Ersetzen Sie die **HTTP-Get Edit** Action-Methode durch den folgenden Code, um das passende **Album** sowie die Listen **Genres** und **Künstler** abzurufen.

    (Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und-Formulare und-Validierung-EX3 storemanagercontroller HTTP-Get Edit Action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Sie verwenden " **System. Web. MVC** **SelectList** " für Künstler und Genres anstelle der Liste " **System. Collections. Generic** ".
    > 
    > **SelectList** ist eine saubere Möglichkeit zum Auffüllen von HTML-Dropdown Listen und zum Verwalten von Dingen wie der aktuellen Auswahl. Durch das Instanziieren und spätere Einrichten dieser ViewModel-Objekte in der Controller Aktion wird das Bearbeitungs Formular Szenario bereinigt.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Aufgabe 2: Erstellen der Bearbeitungs Ansicht

In dieser Aufgabe erstellen Sie eine Vorlage zum Bearbeiten der Ansicht, in der die Album Eigenschaften später angezeigt werden.

1. Erstellen Sie die Bearbeitungs Ansicht. Klicken Sie dazu mit der rechten Maustaste in die **Edit** -Aktionsmethode, und wählen Sie **Ansicht hinzufügen**aus.
2. Überprüfen Sie im Dialogfeld Ansicht hinzufügen, ob der Name der Ansicht " **Bearbeiten**" lautet. Aktivieren Sie das Kontrollkästchen für **eine stark typisierte Ansicht erstellen** , und wählen Sie im Dropdown **Feld Datenklasse anzeigen** die Option **Album (mvcmusicstore. Models)** aus. Wählen Sie im Dropdown Feld **Gerüst Vorlage** die Option **Bearbeiten** aus. Belassen Sie die anderen Felder mit ihrem Standardwert, und klicken Sie dann auf **Hinzufügen**.

    ![Hinzufügen einer Bearbeitungs Ansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Hinzufügen einer Bearbeitungs Ansicht")

    *Hinzufügen einer Bearbeitungs Ansicht*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe testen Sie, ob auf der Seite " **StoreManager** - **Bearbeitungs** Ansicht" die Eigenschaften Werte für das als Parameter übergebenen Album angezeigt werden.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL in **/StoreManager/Edit/1** , um zu überprüfen, ob die Eigenschaften Werte für das übergebenen Album angezeigt werden.

    ![Anzeigen der Bearbeitungs Ansicht des Albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Anzeigen der Bearbeitungs Ansicht des Albums")

    *Anzeigen der Bearbeitungs Ansicht des Albums*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Aufgabe 4: Implementieren von Dropdown-Vorgängen für die Vorlage "Album-Editor"

In dieser Aufgabe fügen Sie der Ansichts Vorlage, die in der letzten Aufgabe erstellt wurde, Dropdown Listen hinzu, sodass der Benutzer aus einer Liste von Künstlern und Genres auswählen kann.

1. Ersetzen Sie den gesamten **Album** -FIELDSET-Code durch Folgendes:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Ein **HTML. DropDownList** -Hilfsprogramm wurde zum Rendering von Dropdown Listen für die Auswahl von Künstlern und Genres hinzugefügt. Die an **HTML. DropDownList** über gebenden Parameter lauten wie folgt:
    > 
    > 1. Der Name des Formular Felds ( **&quot;artistId&quot;** ).
    > 2. Die **SelectList** der Werte für die Dropdown Liste.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe testen Sie, ob auf der Seite " **StoreManager** - **Bearbeitungs** Ansicht" Dropdowns anstelle von Textfeldern für die Felder "Interpret" und "Genre" angezeigt werden.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL in **/StoreManager/Edit/1** , um zu überprüfen, ob Dropdown-Zeichen anstelle von Textfeldern für Text und Genre-ID angezeigt werden.

    ![Durchsuchen der Bearbeitungs Ansicht des Albums mit Dropdowns](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Durchsuchen der Bearbeitungs Ansicht des Albums mit Dropdowns")

    *Anzeigen der Bearbeitungs Ansicht des Albums, dieses Mal mit Dropdown Listen*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Aufgabe 6: Implementieren der HTTP-Post-Bearbeitungs Aktionsmethode

Nun, da die Bearbeitungs Ansicht erwartungsgemäß angezeigt wird, müssen Sie die Methode "http-Post Edit Action" implementieren, um die am Album vorgenommenen Änderungen zu speichern.

1. Schließen Sie den Browser bei Bedarf, um zum Visual Studio-Fenster zurückzukehren. Öffnen Sie **storemanagercontroller** aus dem Ordner **Controllers** .
2. Ersetzen Sie den **http-Post Edit** Action-Methoden Code durch Folgendes (Beachten Sie, dass die Methode, die ersetzt werden muss, eine überladene Version ist, die zwei Parameter empfängt):

    (Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und Formulare und Validierung-EX3 storemanagercontroller HTTP-Post Edit Action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Diese Methode wird ausgeführt, wenn der Benutzer auf die Schaltfläche **Save (speichern** ) der Ansicht klickt und eine HTTP-Post-Anweisung der Formular Werte an den Server ausführt, um Sie in der Datenbank beizubehalten. Der Decorator **[HttpPost]** gibt an, dass die-Methode für diese HTTP-Post-Szenarien verwendet werden soll. Die-Methode nimmt ein **Album** -Objekt an. ASP.NET MVC erstellt automatisch das Album-Objekt aus dem &lt;Formular&gt; Werte.
    > 
    > Die-Methode führt die folgenden Schritte aus:
    > 
    > 1. Wenn das Modell gültig ist:
    > 
    >     1. Aktualisieren Sie den Album Eintrag im Kontext, um ihn als geändertes Objekt zu markieren.
    >     2. Speichern Sie die Änderungen, und leiten Sie Sie an die Index Sicht um.
    > 2. Wenn das Modell nicht gültig ist, wird der viewbag mit dem **GenreID** und **artistId**aufgefüllt. Anschließend wird die Ansicht mit dem empfangenen Album-Objekt zurückgegeben, um dem Benutzer die Ausführung erforderlicher Updates zu ermöglichen.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Aufgabe 7: Ausführen der Anwendung

In dieser Aufgabe testen Sie, ob auf der Seite " **StoreManager-Bearbeitungs** Ansicht" tatsächlich die aktualisierten Album Daten in der Datenbank gespeichert werden.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL in **/StoreManager/Edit/1**. Ändern Sie den Titel des Albums in **Laden** , und klicken Sie auf **Speichern**. Überprüfen Sie, ob der Titel des Albums in der Liste der Alben tatsächlich geändert wurde.

    ![Aktualisieren eines Albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Aktualisieren eines Albums")

    *Aktualisieren eines Albums*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Übung 4: Hinzufügen einer CREATE VIEW

Nun, da der **storemanagercontroller** die **Bearbeitungs** Fähigkeit unterstützt, erfahren Sie in dieser Übung, wie Sie eine CREATE VIEW-Vorlage hinzufügen, um Store Manager neue Alben zur Anwendung hinzuzufügen.

Wie bei den Bearbeitungsfunktionen implementieren Sie das Erstellungs Szenario mit zwei separaten Methoden in der **storemanagercontroller** -Klasse:

1. Eine Aktionsmethode zeigt ein leeres Formular an, wenn die Speicher-Manager die **/StoreManager/Create** -URL zum ersten Mal besuchen.
2. Eine zweite Aktionsmethode behandelt das Szenario, in dem der Speicher-Manager im Formular auf die Schaltfläche **Speichern** klickt und die Werte als HTTP-Post an die **/StoreManager/Create** -URL übergibt.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Aufgabe 1: Implementieren der HTTP-Get Create Action-Methode

In dieser Aufgabe implementieren Sie die HTTP-Get-Version der Create Action-Methode, um eine Liste aller Genres und Künstler abzurufen, Verpacken diese Daten in ein **storemanagerviewmodel** -Objekt, das dann an eine Ansichts Vorlage übergeben wird.

1. Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Source/Ex4-addingacreateview/BEGIN/** Folder". Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Öffnen Sie die **storemanagercontroller** -Klasse. Erweitern Sie hierzu den Ordner **Controller** , und doppelklicken Sie auf **StoreManagerController.cs**.
3. Ersetzen Sie den Code der **Create** Action-Methode durch Folgendes:

    (Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und Formulare und Validierung-Ex4 storemanagercontroller HTTP-Get Create Action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Aufgabe 2: Hinzufügen der CREATE VIEW

In dieser Aufgabe fügen Sie die Vorlage CREATE VIEW hinzu, in der ein neues (leeres) Album Formular angezeigt wird.

1. Klicken Sie mit der rechten Maustaste in die Methode **Create** Action, und wählen Sie **Ansicht hinzufügen**. Dadurch wird das Dialogfeld Ansicht hinzufügen angezeigt.
2. Überprüfen Sie im Dialogfeld Ansicht hinzufügen, ob der Name der Sicht **Create**lautet. Wählen Sie die Option zum **Erstellen einer stark typisierten Ansicht** aus, und wählen Sie in der Dropdown-Dropdown- **Vorlage** für **Modell Klasse** die Option **Album (mvcmusicstore. Models)** **aus.** Belassen Sie die anderen Felder mit ihrem Standardwert, und klicken Sie dann auf **Hinzufügen**.

    ![Hinzufügen einer CREATE VIEW](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Adding-a-CREATE-VIEW. png")

    *Hinzufügen der CREATE VIEW*
3. Aktualisieren Sie die Felder **GenreID** und **artistId** so, dass Sie eine Dropdown Liste verwenden, wie unten dargestellt:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe testen Sie, ob auf der Seite " **StoreManager** **Create** View" ein leeres Album Formular angezeigt wird.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL in **/StoreManager/Create**. Überprüfen Sie, ob ein leeres Formular zum Ausfüllen der neuen Album Eigenschaften angezeigt wird.

    ![Erstellen einer Ansicht mit einem leeren Formular](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Erstellen einer Ansicht mit einem leeren Formular")

    *Erstellen einer Ansicht mit einem leeren Formular*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Aufgabe 4: Implementieren der HTTP-Post-Aktionsmethode "Create"

In dieser Aufgabe implementieren Sie die HTTP-Post-Version der Create Action-Methode, die aufgerufen wird, wenn ein Benutzer auf die Schaltfläche **Speichern** klickt. Die-Methode sollte das neue Album in der-Datenbank speichern.

1. Schließen Sie den Browser bei Bedarf, um zum Visual Studio-Fenster zurückzukehren. Öffnen Sie die **storemanagercontroller** -Klasse. Erweitern Sie hierzu den Ordner **Controller** , und doppelklicken Sie auf **StoreManagerController.cs**.
2. Ersetzen Sie den Code der Create Action-Methode von **http-Post** durch Folgendes:

    (Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und Formulare und Validierung-Ex4 storemanagercontroller HTTP-Post Create Action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > Die CREATE-Aktion ähnelt der vorherigen Edit-Aktionsmethode, aber anstatt das-Objekt als geändert festzulegen, wird es dem Kontext hinzugefügt.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe testen Sie, ob Sie auf der Seite " **StoreManager Create** View" ein neues Album erstellen und dann zur Ansicht "StoreManager-Index" umgeleitet werden können.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL in **/StoreManager/Create**. Füllen Sie alle Formularfelder mit Daten für ein neues Album aus, wie in der folgenden Abbildung dargestellt:

    ![Erstellen eines Albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Erstellen eines Albums")

    *Erstellen eines Albums*
3. Vergewissern Sie sich, dass Sie zur Ansicht "StoreManager Index" umgeleitet werden, die das soeben erstellte neue Album enthält.

    ![Neues Album erstellt](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Neues Album erstellt")

    *Neues Album erstellt*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Übung 5: Behandeln von Lösch Vorgängen

Die Möglichkeit zum Löschen von Alben ist noch nicht implementiert. Im folgenden wird diese Übung verwendet. Wie zuvor implementieren Sie das Lösch Szenario mit zwei separaten Methoden in der **storemanagercontroller** -Klasse:

1. Eine Aktionsmethode zeigt ein Bestätigungsformular an
2. Eine zweite Aktionsmethode behandelt die Formular Übermittlung.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Aufgabe 1: Implementieren der HTTP-Get DELETE-Aktionsmethode

In dieser Aufgabe implementieren Sie die HTTP-Get-Version der DELETE-Aktionsmethode, um die Informationen zum Album abzurufen.

1. Öffnen Sie die Projekt Mappe " **Anfang** " unter **Quelle/EX5-handlinglöschung/Anfang/** Ordner. Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Öffnen Sie die **storemanagercontroller** -Klasse. Erweitern Sie hierzu den Ordner **Controller** , und doppelklicken Sie auf **StoreManagerController.cs**.
3. Die Aktion zum Löschen des Controllers ist exakt identisch mit der vorherigen Aktion für den Speicher Details-Controller: Sie fragt das **Album** -Objekt mithilfe der in der URL angegebenen **ID** aus der Datenbank ab und gibt die entsprechende **Ansicht**zurück. Ersetzen Sie hierzu den Methoden Code "http-get **Delete** Action" durch Folgendes:

    (Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und Formulare und Validierung EX5 Behandlung von Lösch Vorgängen HTTP-Get delete action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Klicken Sie mit der rechten Maustaste in die Methode **Delete** Action, und wählen Sie **Ansicht hinzufügen**. Dadurch wird das Dialogfeld Ansicht hinzufügen angezeigt.
5. Überprüfen Sie im Dialogfeld Ansicht hinzufügen, ob der Name der Sicht **gelöscht**ist. Wählen Sie die Option zum **Erstellen einer stark typisierten Ansicht** aus, und wählen Sie in der Dropdown-Datei der **Modell Klasse** die Option **Album (mvcmusicstore. Models)** aus. Wählen Sie aus der Dropdown- **Vorlage für Gerüst Vorlagen** die Option **Löschen** aus. Belassen Sie die anderen Felder mit ihrem Standardwert, und klicken Sie dann auf **Hinzufügen**.

    ![Hinzufügen einer Lösch Ansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Hinzufügen einer Lösch Ansicht")

    *Hinzufügen einer Lösch Ansicht*
6. Die Vorlage löschen zeigt alle Felder aus dem Modell an. Es wird nur der Titel des Albums angezeigt. Ersetzen Sie dazu den Inhalt der Sicht durch den folgenden Code:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Aufgabe 2: Ausführen der Anwendung

In dieser Aufgabe testen Sie, ob auf der Seite " **StoreManager** **Delete** View" (Löschvorgang löschen) ein Bestätigungsformular angezeigt wird.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL in **/StoreManager**. Wählen Sie ein zu Lösch Endes Album aus, indem Sie auf **Löschen** klicken und überprüfen, ob die neue Ansicht hochgeladen

    ![Löschen eines Albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Löschen eines Albums")

    *Löschen eines Albums*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Aufgabe 3: Implementieren der HTTP-Post-Aktionsmethode "Delete"

In dieser Aufgabe implementieren Sie die HTTP-Post-Version der DELETE-Aktionsmethode, die aufgerufen wird, wenn ein Benutzer auf die Schaltfläche " **Löschen** " klickt. Die-Methode sollte das-Album in der-Datenbank löschen.

1. Schließen Sie den Browser bei Bedarf, um zum Visual Studio-Fenster zurückzukehren. Öffnen Sie die **storemanagercontroller** -Klasse. Erweitern Sie hierzu den Ordner **Controller** , und doppelklicken Sie auf **StoreManagerController.cs**.
2. Ersetzen Sie den Code der Methode " **http-Post DELETE** Action" durch Folgendes:

    (Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und Formulare und Validierung EX5 Behandlung von Lösch Vorgängen HTTP-Post DELETE-Aktion*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe testen Sie, ob Sie auf der Seite " **StoreManager DELETE** View" ein Album löschen und dann zur Ansicht "StoreManager-Index" umgeleitet werden können.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL in **/StoreManager**. Klicken Sie auf löschen, um ein zu Lösch Endes Album auszuwählen **.** Bestätigen Sie den Löschvorgang durch Klicken auf die Schaltfläche **Löschen**

    ![Löschen eines Albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Löschen eines Albums")

    *Löschen eines Albums*
3. Vergewissern Sie sich, dass das Album gelöscht wurde, da es nicht auf der **Index** Seite angezeigt wird.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Übung 6: Hinzufügen der Validierung

Derzeit führen die von Ihnen erstellten Formulare zum Erstellen und bearbeiten keine Überprüfung durch. Wenn der Benutzer ein Pflichtfeld leer lässt oder Buchstaben im Feld Price (Preis) eingeben, wird der erste Fehler, den Sie erhalten, von der Datenbank ausgegeben.

Sie können der Anwendung eine Validierung hinzufügen, indem Sie Ihrer Modell Klasse Daten Anmerkungen hinzufügen. Mit Daten Anmerkungen können Sie die Regeln beschreiben, die Sie auf die Modell Eigenschaften anwenden möchten, und ASP.NET MVC übernimmt die Erzwingung und Anzeige entsprechender Nachrichten für Benutzer.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Aufgabe 1: Hinzufügen von Daten Anmerkungen

In dieser Aufgabe fügen Sie dem Album Modelldaten Anmerkungen hinzu, mit denen die Seite erstellen und bearbeiten bei Bedarf Validierungs Meldungen anzeigen kann.

Bei einer einfachen Modell Klasse wird das Hinzufügen einer Daten Anmerkung nur durch Hinzufügen einer **using** -Anweisung für **System. ComponentModel. DataAnnotation**behandelt. Anschließend wird ein **[Required]** -Attribut in den entsprechenden Eigenschaften platziert. Im folgenden Beispiel wird die **Name** -Eigenschaft als Pflichtfeld in der Ansicht angezeigt.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Dies ist ein wenig komplexer in Fällen wie diese Anwendung, in der die Entity Data Model generiert wird. Wenn Sie Daten Anmerkungen direkt zu den Modellklassen hinzugefügt haben, werden Sie überschrieben, wenn Sie das Modell aus der Datenbank aktualisieren. Stattdessen können Sie metadatenpartielle Klassen verwenden, die zum Speichern der Anmerkungen vorhanden sind und den Modellklassen mit dem **[MetadataType]** -Attribut zugeordnet werden.

1. Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Quelle/Ex6-addingvalidation/BEGIN/** Folder". Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Öffnen Sie die **Album.cs** aus dem Ordner " **Models** ".
3. Ersetzen Sie **Album.cs** -Inhalt durch den hervorgehobenen Code, sodass er wie folgt aussieht:

    > [!NOTE]
    > Die Zeile **[Display Format (ConvertEmptyStringToNull = false)]** gibt an, dass leere Zeichen folgen aus dem Modell nicht in NULL konvertiert werden, wenn das Datenfeld in der Datenquelle aktualisiert wird. Mit dieser Einstellung wird eine Ausnahme vermieden, wenn die Entity Framework dem Modell NULL-Werte zuweist, bevor die Daten Anmerkung die Felder überprüft.

    (Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und-Formulare und-Validierung Ex6, partielle Klasse von Album-Metadaten*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Diese partielle **Album** Klasse verfügt über ein **metadataType** -Attribut, das auf die Klasse " **albummetadata** " für die Daten Anmerkungen zeigt. Dies sind einige der Attribute für die Daten Anmerkung, die Sie verwenden, um das Album Modell zu kommentieren:
    > 
    > - Required: gibt an, dass die Eigenschaft ein Pflichtfeld ist.
    > - Display Name: definiert den Text, der für Formularfelder und Validierungs Nachrichten verwendet werden soll.
    > - DisplayFormat: gibt an, wie Datenfelder angezeigt und formatiert werden.
    > - StringLength: definiert eine maximale Länge für ein Zeichen folgen Feld.
    > - Range: gibt einen maximalen und minimalen Wert für ein numerisches Feld an.
    > - Gerüst Column-ermöglicht das Ausblenden von Feldern aus Editor Formularen

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Aufgabe 2: Ausführen der Anwendung

In dieser Aufgabe testen Sie mithilfe der in der letzten Aufgabe ausgewählten anzeigen Amen, ob die Felder "erstellen" und "Bearbeiten"-Felder überprüft werden.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL in **/StoreManager/Create**. Stellen Sie sicher, dass die anzeigen Amen denen in der partiellen Klasse entsprechen (z. b. **Album Art-URL** anstelle von **albumarturl**).
3. Klicken Sie auf **Erstellen**, ohne das Formular zu füllen. Vergewissern Sie sich, dass Sie die entsprechenden Validierungs Meldungen erhalten.

    ![Überprüfte Felder auf der Seite "erstellen"](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Überprüfte Felder auf der Seite "erstellen"")

    *Überprüfte Felder auf der Seite "erstellen"*
4. Sie können überprüfen, ob das gleiche auf der Seite **Bearbeiten** auftritt. Ändern Sie die URL in **/StoreManager/Edit/1** , und überprüfen Sie, ob die anzeigen Amen denen in der partiellen Klasse entsprechen (z. b. **Album Art-URL** anstelle von **albumarturl**). Leeren Sie die Felder **Titel** und **Preis** , und klicken Sie auf **Speichern**. Vergewissern Sie sich, dass Sie die entsprechenden Validierungs Meldungen erhalten.

    ![Überprüfte Felder auf der Seite "Bearbeiten"](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Überprüfte Felder auf der Seite "Bearbeiten"*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Übung 7: Verwenden von unaufdringlichen jQuery auf Client Seite

In dieser Übung erfahren Sie, wie Sie auf Clientseite die unaufdringliche jquery-Validierung von MVC 4 aktivieren.

> [!NOTE]
> Die unaufdringliche jQuery verwendet das Data-AJAX-Präfix JavaScript, um Aktionsmethoden auf dem Server aufzurufen, anstatt Inline Client Skripts auszulösen.

<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Aufgabe 1: Ausführen der Anwendung vor dem Aktivieren von unaufdringlichem jQuery

In dieser Aufgabe führen Sie die Anwendung aus, bevor Sie jQuery einschließen, damit beide Validierungs Modelle verglichen werden können.

1. Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Source/Ex7-unauffällig sivejqueryvalidation/BEGIN/** Folder". Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Drücken Sie **F5**, um die Anwendung auszuführen.
3. Das Projekt wird auf der Startseite gestartet. Durchsuchen Sie **/StoreManager/Create** , und klicken Sie auf **Erstellen** , ohne das Formular auszufüllen.

    ![Client Validierung deaktiviert](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client Validierung deaktiviert")

    *Client Validierung deaktiviert*
4. Öffnen Sie im Browser den HTML-Quellcode:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Aufgabe 2: Aktivieren der unaufdringlichen Client Validierung

In dieser Aufgabe aktivieren Sie die jQuery- **unaufdringliche Client Validierung** aus der **Web. config** -Datei, die in allen neuen ASP.NET MVC 4-Projekten standardmäßig auf false festgelegt ist. Außerdem fügen Sie die erforderlichen Skripts Verweise hinzu, um die unaufdringliche Client Validierung in jQuery zu vereinfachen.

1. Öffnen Sie die Datei **Web. config** im Projektstamm Verzeichnis, und stellen Sie sicher, dass die Schlüsselwerte **clientvalidationaktivierter** und **unauffällig sivejavascriptenabled** auf **true**festgelegt sind.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > Sie können die Client Validierung auch durch Code auf Global.asax.cs aktivieren, um dieselben Ergebnisse zu erhalten:
    > 
    > **Htmlhelper. clientvalidationaktivierte = true;**
    > 
    > Außerdem können Sie einem beliebigen Controller das clientvalidationaktivierte-Attribut zuweisen, um ein benutzerdefiniertes Verhalten zu erhalten.
2. Öffnen Sie **Create. cshtml** unter **views\storemanager**.
3. Stellen Sie sicher, dass die folgenden Skriptdateien, **jQuery. Validate** und **jQuery. Validate. unaufdringlich**, über das &quot; **~/Bundles/jqueryval**&quot; Bundle in der Ansicht referenziert werden.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Alle diese jQuery-Bibliotheken sind in den neuen Projekten von MVC 4 enthalten. Weitere Bibliotheken finden Sie im Ordner **"/Scripts"** Ihres Projekts.
    > 
    > Damit diese Validierungs Bibliotheken funktionieren, müssen Sie einen Verweis auf die jQuery-frameworkbibliothek hinzufügen. Da dieser Verweis bereits in der **\_Layout. cshtml** -Datei hinzugefügt wurde, müssen Sie ihn nicht in dieser speziellen Ansicht hinzufügen.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Aufgabe 3: Ausführen der Anwendung mithilfe der unaufdringlichen jquery-Validierung

In dieser Aufgabe testen Sie, ob die Vorlage "Create View" von " **StoreManager** " die Client seitige Validierung mithilfe von jQuery-Bibliotheken durchführt, wenn der Benutzer ein neues Album erstellt.

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Durchsuchen Sie **/StoreManager/Create** , und klicken Sie auf **Erstellen** , ohne das Formular auszufüllen.

    ![Client Validierung mit aktiviertem jQuery](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client Validierung mit aktiviertem jQuery")

    *Client Validierung mit aktiviertem jQuery*
3. Öffnen Sie im Browser den Quellcode für CREATE VIEW:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Für jede Client Validierungs Regel fügt unaufdringliche jQuery ein Attribut mit Data-Val-*RuleName*=&quot;*Message*&quot;hinzu. Im folgenden finden Sie eine Liste von Tags, die unaufdringliche jQuery in das HTML-Eingabefeld eingefügt werden, um die Client Validierung auszuführen:
   > 
   > - Data-Val
   > - Data-Val-Number
   > - Data-Val-Range
   > - Data-Val-Range-min/Data-Val-Range-Max
   > - Data-Val-required
   > - Daten-Val-Länge
   > - Data-Val-length-Max/Data-Val-length-min
   > 
   > Alle Datenwerte werden mit der Modell **Daten**Anmerkung gefüllt. Anschließend kann die gesamte Logik, die auf der Serverseite funktioniert, auf der Clientseite ausgeführt werden. Beispielsweise hat Price Attribute die folgende Daten Anmerkung im Modell:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Nachdem Sie unaufdringliche jQuery verwendet haben, lautet der generierte Code wie folgt:
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Wenn Sie diese praktische Übungseinheit durcharbeiten, haben Sie gelernt, wie Sie es Benutzern ermöglichen können, die in der Datenbank gespeicherten Daten mit folgendem zu ändern:

- Controller Aktionen wie Index, CREATE, Edit, DELETE
- ASP.NET MVC-Gerüstbau Funktion zum Anzeigen von Eigenschaften in einer HTML-Tabelle
- Benutzerdefinierte HTML-Hilfsprogramme zur Verbesserung der Benutzer Leistung
- Aktionsmethoden, die entweder auf HTTP-GET oder http-Post-Aufrufe reagieren
- Eine freigegebene Editor Vorlage für ähnliche Ansichts Vorlagen wie CREATE und Edit
- Formularelemente wie Dropdown-Elemente
- Daten Anmerkungen für die Modell Validierung
- Client seitige Validierung mit der jQuery-unaufdringlichen Bibliothek

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren. Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.

1. Wechseln Sie zu [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Windows Azure SDK</em>&quot;suchen.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.
3. Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.

    ![Installieren von Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Installationsfortschritt*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.

    ![Installation abgeschlossen](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Installation abgeschlossen*
7. Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .
8. Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .

    ![VS Express für Web-Kachel](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Anhang B: Verwenden von Code Ausschnitten

Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen. Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")

*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*

***So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)***

1. Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.
2. Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).
3. Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.
4. Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).
5. Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.

![Beginnen Sie mit der Eingabe des Ausschnitt namens.](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")

*Beginnen Sie mit der Eingabe des Ausschnitt namens.*

![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")

*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*

![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")

*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*

So ***fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)*** 1. Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.

1. Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.
2. Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.

![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")

*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*

![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")

*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*
