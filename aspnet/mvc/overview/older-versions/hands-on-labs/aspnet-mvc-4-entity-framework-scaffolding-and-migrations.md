---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework Gerüstbau und Migrationen | Microsoft-Dokumentation
author: rick-anderson
description: Wenn Sie mit ASP.NET MVC 4-Controller Methoden vertraut sind oder die &quot;Hilfsprogramme, Formulare und Validierung&quot; praktische Übungseinheit abgeschlossen haben, sollten Sie sich bewusst sein...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 2b26224390af70e19ca0593abe93a6867140f8ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484641"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>ASP.NET MVC 4 – Gerüstbau und Migrationen mit dem Entity Framework

vom [Web Camps-Team](https://twitter.com/webcamps)

[Webcamps-Trainingskit herunterladen](https://aka.ms/webcamps-training-kit)

Wenn Sie mit ASP.NET MVC 4-Controller Methoden vertraut sind oder die &quot;Hilfsprogramme, Formulare und Validierungs&quot; praktische Übungseinheit abgeschlossen haben, sollten Sie beachten, dass viele der Logik zum Erstellen, aktualisieren, auflisten und entfernen beliebiger Daten Entitäten, die Sie in der Anwendung wiederholt. Wenn Ihr Modell über mehrere Klassen zum Bearbeiten verfügt, werden Sie wahrscheinlich viel Zeit aufwenden, um die Post-und Get Action-Methoden für jeden Entitäts Vorgang und jede der Ansichten zu schreiben.

In dieser Übungseinheit erfahren Sie, wie Sie den ASP.NET MVC 4-Gerüstbau zum automatischen Generieren der Baseline der CRUD (Create, Read, Update und DELETE) Ihrer Anwendung verwenden. Beginnend mit einer einfachen Modell Klasse und, ohne eine einzige Codezeile zu schreiben, erstellen Sie einen Controller, der alle CRUD-Vorgänge sowie alle notwendigen Sichten enthält. Nachdem Sie die einfache Lösung erstellt und ausgeführt haben, wird die Anwendungsdatenbank sowie die MVC-Logik und-Sichten für die Datenbearbeitung generiert.

Darüber hinaus erfahren Sie, wie einfach es ist, Entity Framework Migrationen zu verwenden, um Modell Updates in der gesamten Anwendung auszuführen. Mit Entity Framework Migrationen können Sie Ihre Datenbank ändern, nachdem das Modell mit einfachen Schritten geändert wurde. Mit all diesen Bedenken können Sie Webanwendungen effizienter erstellen und pflegen und dabei die neuesten Features von ASP.NET MVC 4 nutzen.

> [!NOTE]
> Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das in den [Releases Microsoft-Web/webcamptrainingkit](https://aka.ms/webcamps-training-kit)verfügbar ist. Das für dieses Lab spezifische Projekt ist unter [ASP.NET MVC 4 Entity Framework Gerüstbau und Migrationen](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations)verfügbar.

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie Folgendes:

- Verwenden Sie ASP.net Gerüstbau für CRUD-Vorgänge in Controllern.
- Ändern Sie das Datenbankmodell mithilfe Entity Framework Migrationen.

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

In der folgenden Übung wird diese praktische Übungseinheit eingerichtet:

1. [Verwenden von ASP.NET MVC 4-Gerüstbau mit Entity Framework Migrationen](#Exercise1)

> [!NOTE]
> Diese Übung wird von einem **End** -Ordner mit der resultierenden Lösung begleitet, die Sie nach Abschluss der Übung erhalten sollten. Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Arbeiten mit der Übung benötigen.

Geschätzte Zeit bis zum Abschluss dieses Labs: **30 Minuten**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Übung 1: Verwenden von ASP.NET MVC 4-Gerüstbau mit Entity Framework Migrationen

ASP.NET MVC-Gerüstbau bietet eine schnelle Möglichkeit, die CRUD-Vorgänge auf standardisierte Weise zu generieren und so die erforderliche Logik zu erstellen, mit der Ihre Anwendung mit der Datenbankschicht interagieren kann.

In dieser Übung erfahren Sie, wie Sie ASP.NET MVC 4 Gerüstbau mit Code First verwenden, um die CRUD-Methoden zu erstellen. Anschließend erfahren Sie, wie Sie das Modell aktualisieren können, indem Sie die Änderungen in der Datenbank mithilfe Entity Framework Migrationen anwenden.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Aufgabe 1: Erstellen eines neuen ASP.NET MVC 4-Projekts mithilfe eines Gerüsts

1. Wenn Sie nicht bereits geöffnet ist, starten Sie **Visual Studio 2012**.
2. **Datei auswählen | Neues Projekt**. Klicken Sie im Dialogfeld Neues Projekt unter **dem C# Visual | Webabschnitt** wählen Sie **ASP.NET MVC 4-Webanwendung**aus. Benennen Sie das Projekt mit **MVC4andEFMigrations** , und legen Sie den Speicherort auf den Ordner **source\ex1-usingmvc4gerüschesfmigrationen** dieses Labs fest. Legen Sie den Projektmappennamen auf **beginnen** fest, und stellen Sie sicher, dass Projektmappenverzeichnis **Erstellen** Klicken Sie auf **OK**.

    ![Neues ASP.NET MVC 4-Projekt (Dialog Feld)](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "Neues ASP.NET MVC 4-Projekt (Dialog Feld)")

    *Neues ASP.NET MVC 4-Projekt (Dialog Feld)*
3. Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Vorlage **Internet Anwendung** aus, und vergewissern Sie sich, dass **Razor** die ausgewählte **Ansichts-Engine**ist. Klicken Sie auf **OK**, um das Projekt zu erstellen.

    ![Neu ASP.NET MVC 4-Internet Anwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "Neu ASP.NET MVC 4-Internet Anwendung")

    *Neu ASP.NET MVC 4-Internet Anwendung*
4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Modelle** , und wählen Sie **hinzufügen aus. Klasse** zum Erstellen einer einfachen Klasse Person (poco). Nennen Sie es **Person** , und klicken Sie auf **OK**.
5. Öffnen Sie die Klasse Person, und fügen Sie die folgenden Eigenschaften ein.

    (Code Ausschnitt- *ASP.NET MVC 4 und Entity Framework Migrationen-EX1 Person-Eigenschaften*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Klicken Sie auf **Erstellen | Erstellen** Sie die Lösung, um die Änderungen zu speichern und das Projekt zu erstellen.

    ![Erstellen der Anwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Erstellen der Anwendung")

    *Erstellen der Anwendung*
7. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controller, und wählen Sie **hinzufügen aus. Controller**.
8. Benennen Sie den Controller *personcontroller* , und vervollständigen Sie die **Gerüstbau Optionen** mit den folgenden Werten.

   1. Wählen Sie in der Dropdown Liste **Vorlage** den **MVC-Controller mit Lese-/Schreibaktionen und-Sichten mit Entity Framework** Option aus.
   2. Wählen Sie in der Dropdown Liste **Modell Klasse** die **Person** -Klasse aus.
   3. Wählen Sie in der Liste **Datenkontext Klasse** die Option **&lt;neuer Datenkontext...&gt;** aus. Wählen Sie einen beliebigen Namen, und klicken Sie auf **OK**.
   4. Vergewissern Sie sich, dass in der Dropdown Liste **Sichten** die Option **Razor** ausgewählt ist.

      ![Hinzufügen des Person-Controllers mit Gerüstbau](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Hinzufügen des Person-Controllers mit Gerüstbau")

      *Hinzufügen des Person-Controllers mit Gerüstbau*
9. Klicken Sie auf **Hinzufügen** , um den neuen Controller für Person mit Gerüst zu erstellen. Sie haben nun die Controller Aktionen und die Ansichten generiert.

    ![Nach dem Erstellen des Person-Controllers mit Gerüstbau](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "Nach dem Erstellen des Person-Controllers mit Gerüstbau")

    *Nach dem Erstellen des Person-Controllers mit Gerüstbau*
10. Öffnen Sie die **personcontroller** -Klasse. Beachten Sie, dass die vollständigen CRUD-Aktionsmethoden automatisch generiert wurden.

   ![Innerhalb des Person-Controllers](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Innerhalb des Person-Controllers")

   *Innerhalb des Person-Controllers*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Aufgabe 2: Ausführen der Anwendung

An diesem Punkt wurde die Datenbank noch nicht erstellt. In dieser Aufgabe führen Sie die Anwendung zum ersten Mal aus und testen die CRUD-Vorgänge. Die Datenbank wird im Handumdrehen mit Code First erstellt.

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Fügen Sie im Browser **/Person** zur URL hinzu, um die Person-Seite zu öffnen.

    ![Anwendung zuerst ausführen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Anwendung zuerst ausführen")

    *Anwendung: erste Testlauf*
3. Sie werden nun die Personen Seiten untersuchen und die CRUD-Vorgänge testen.

    1. Klicken Sie auf **neu erstellen** , um eine neue Person hinzuzufügen. Geben Sie einen Vornamen und einen Nachnamen ein, und klicken Sie auf **Erstellen**.

        ![Hinzufügen einer neuen Person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Hinzufügen einer neuen Person")

        *Hinzufügen einer neuen Person*
    2. In der Liste der Personen können Sie Elemente löschen, bearbeiten oder hinzufügen.

        ![Personen Liste](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "Personen Liste")

        *Personen Liste*
    3. Klicken Sie auf **Details** , um die Details der Person zu öffnen.

        ![Personen Details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Personen Details")

        *Personen Details*
4. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück. Beachten Sie, dass Sie in der gesamten Anwendung den gesamten CRUD für die Person-Entität erstellt haben, ohne eine einzige Codezeile schreiben zu müssen.

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Aufgabe 3: Aktualisieren der Datenbank mithilfe Entity Framework Migrationen

In dieser Aufgabe aktualisieren Sie die Datenbank, indem Sie Entity Framework Migrationen verwenden. Sie werden feststellen, wie einfach es ist, das Modell zu ändern und die Änderungen in ihren Datenbanken mithilfe der Entity Framework Migrations Funktion widerzuspiegeln.

1. Öffnen Sie die Paket-Manager-Konsole. Klicken Sie auf **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.
2. Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Aktivieren von Migrationen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Aktivieren von Migrationen")

    *Aktivieren von Migrationen*

    Mit dem Befehl enable-Migration wird der Ordner **Migrationen** erstellt, in dem ein Skript zum Initialisieren der Datenbank enthalten ist.

    ![Migrations Ordner](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations Ordner")

    *Migrations Ordner*
3. Öffnen Sie die Datei **Configuration.cs** im Migrations Ordner. Suchen Sie den Klassenkonstruktor, und ändern Sie den Wert **automaticmigrationsenabled** in *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Öffnen Sie die Person-Klasse, und fügen Sie ein-Attribut für den Vornamen der Person hinzu. Mit diesem neuen Attribut ändern Sie das Modell.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. **Build auswählen | Erstellen** Sie die Projekt Mappe im Menü, um die Anwendung zu erstellen.

    ![Erstellen der Anwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Erstellen der Anwendung")

    *Erstellen der Anwendung*
6. Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Mit diesem Befehl wird nach Änderungen in den Datenobjekten gesucht, und dann werden die erforderlichen Befehle zum Ändern der Datenbank hinzugefügt.

    ![Hinzufügen eines mittleren namens](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Hinzufügen eines mittleren namens")

    *Hinzufügen eines mittleren namens*
7. Optionale Sie können den folgenden Befehl ausführen, um ein SQL-Skript mit dem differenziellen Update zu generieren. Auf diese Weise können Sie die Datenbank manuell aktualisieren (in diesem Fall nicht erforderlich) oder die Änderungen in anderen Datenbanken anwenden:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Erstellen eines SQL-Skripts](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generieren eines SQL-Skripts")

    *Erstellen eines SQL-Skripts*

    ![SQL-Skript Aktualisierung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL-Skript Aktualisierung")

    *SQL-Skript Aktualisierung*
8. Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein, um die Datenbank zu aktualisieren:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Aktualisieren der Datenbank](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Aktualisieren der Datenbank")

    *Aktualisieren der Datenbank*

    Dadurch wird die **MiddleName** -Spalte in der **People** -Tabelle hinzugefügt, um der aktuellen Definition der **Person** -Klasse zu entsprechen.
9. Nachdem die Datenbank aktualisiert wurde, klicken Sie mit der rechten Maustaste auf den Ordner Controller, und wählen Sie **Hinzufügen | Controller** zum erneuten Hinzufügen des Person-Controllers (mit denselben Werten vervollständigen). Dadurch werden die vorhandenen Methoden und Sichten aktualisiert, die das neue Attribut hinzufügen.

    ![Hinzufügen eines Controller Updates](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Hinzufügen eines Controller Updates")

    *Aktualisieren des Controllers*
10. Klicken Sie auf **Hinzufügen**. Wählen Sie dann die Werte **Überschreibungs PersonController.cs** und zugeordnete **Sichten überschreiben** aus, und klicken Sie auf **OK**.

   ![Hinzufügen eines Controller-überschreiben](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Aktualisieren des Controllers*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4-Ausführen der Anwendung

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Öffnen Sie **/Person**. Beachten Sie, dass die Daten beibehalten wurden, während die Spalte "Middle Name" hinzugefügt wurde.

    ![Mittlerer Name hinzugefügt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Mittlerer Name hinzugefügt")

    *Mittlerer Name hinzugefügt*
3. Wenn Sie auf **Bearbeiten**klicken, können Sie der aktuellen Person einen mittleren Namen hinzufügen.

    ![Edition des mittleren namens](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Edition des mittleren namens")

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser praktischen Übungseinheit haben Sie einfache Schritte zum Erstellen von CRUD-Vorgängen mit ASP.NET MVC 4-Gerüstbau mithilfe einer beliebigen Modell Klasse kennengelernt. Anschließend haben Sie gelernt, wie Sie ein End-to-End-Update in Ihrer Anwendung durchführen, und zwar von der Datenbank zu den Sichten, indem Sie Entity Framework Migrationen verwenden.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren. Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.

1. Wechseln Sie zu [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Windows Azure SDK</em>&quot;suchen.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.
3. Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.

    ![Installieren von Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Installationsfortschritt*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.

    ![Installation abgeschlossen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Installation abgeschlossen*
7. Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .
8. Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .

    ![VS Express für Web-Kachel](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Anhang B: Verwenden von Code Ausschnitten

Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen. Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")

*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*

***So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)***

1. Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.
2. Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).
3. Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.
4. Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).
5. Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.

![Beginnen Sie mit der Eingabe des Ausschnitt namens.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")

*Beginnen Sie mit der Eingabe des Ausschnitt namens.*

![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")

*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*

![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")

*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*

So ***fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)*** 1. Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.

1. Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.
2. Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.

![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")

*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*

![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")

*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*
