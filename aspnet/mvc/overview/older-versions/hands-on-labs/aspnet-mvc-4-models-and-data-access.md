---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4-Modelle und Datenzugriff | Microsoft-Dokumentation
author: rick-anderson
description: 'Hinweis: in dieser praktischen Übungseinheit wird davon ausgegangen, dass Sie über grundlegende Kenntnisse von ASP.NET MVC verfügen. Wenn Sie ASP.NET MVC noch nicht verwendet haben, empfehlen wir, dass Sie ASP.NET MVC 4...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 90635b617930d0a9c126795f4c8790d542e33dc9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451467"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>ASP.NET MVC 4 – Modelle und Datenzugriff

vom [Web Camps-Team](https://twitter.com/webcamps)

[Webcamps-Trainingskit herunterladen](https://aka.ms/webcamps-training-kit)

Diese praktische Übung geht davon aus, dass Sie über grundlegende Kenntnisse von **ASP.NET MVC**verfügen. Wenn Sie **ASP.NET MVC** noch nicht verwendet haben, empfehlen wir Ihnen, **ASP.NET MVC 4-Grundlagen** praktische Übungseinheiten zu überspringen.

Diese Übungseinheit führt Sie durch die Verbesserungen und neuen Features, die Sie zuvor beschrieben haben, indem Sie kleinere Änderungen an einer im Quellordner bereitgestellten beispielweb Anwendung anwenden.

> [!NOTE]
> Der gesamte Beispielcode und die Code Ausschnitte sind im webcamps-Trainingskit enthalten, das unter [Microsoft-Web/webcamptrainingkit-Releases](https://aka.ms/webcamps-training-kit)verfügbar ist. Das für dieses Lab spezifische Projekt ist unter [ASP.NET MVC 4-Modelle und Datenzugriff](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess)verfügbar.

In der praktischen Übungseinheit **ASP.NET MVC-Grundlagen** haben Sie hart codierte Daten von den Controllern an die Ansichts Vorlagen übergeben. Um jedoch eine echte Webanwendung zu erstellen, empfiehlt es sich, eine echte Datenbank zu verwenden.

Diese praktische Übungseinheit zeigt Ihnen, wie Sie eine Datenbank-Engine zum Speichern und Abrufen der Daten verwenden, die für die Music Store-Anwendung benötigt werden. Um dies zu erreichen, beginnen Sie mit einer vorhandenen Datenbank, und erstellen Sie die Entity Data Model. In dieser Übungseinheit treffen Sie den **Database First** Ansatz und den Ansatz der **Code First** .

Sie können jedoch auch den **Model First** Ansatz verwenden, das gleiche Modell mit den Tools erstellen und anschließend die Datenbank daraus generieren.

![Database First im Vergleich zu Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First im Vergleich zu Model First")

*Database First im Vergleich zu Model First*

Nachdem Sie das Modell erstellt haben, nehmen Sie die richtigen Anpassungen in StoreController vor, um die Speicher Sichten mit den Daten aus der Datenbank bereitzustellen, anstatt hart codierte Daten zu verwenden. Sie müssen keine Änderungen an den Ansichts Vorlagen vornehmen, da der StoreController die gleichen ViewModels an die Ansichts Vorlagen zurückgibt, obwohl die Daten dieses Mal aus der Datenbank stammen.

**Der Code First Ansatz**

Mithilfe des Code First Ansatzes können wir das Modell aus dem Code definieren, ohne Klassen zu erstellen, die im Allgemeinen mit dem Framework gekoppelt sind.

In Code First werden Modell Objekte mit POCOS definiert, &quot;Plain Old CLR Objects&quot;. POCOS sind einfache einfache Klassen, die keine Vererbung aufweisen und keine Schnittstellen implementieren. Wir können die Datenbank automatisch aus Ihnen generieren, oder Sie können eine vorhandene Datenbank verwenden und die Klassen Zuordnung aus dem Code generieren.

Der Vorteil dieser Vorgehensweise besteht darin, dass das Modell unabhängig vom Persistenzframework (in diesem Fall Entity Framework) ist, da die POCOS-Klassen nicht mit dem Mapping-Framework gekoppelt sind.

> [!NOTE]
> Diese Übungseinheit basiert auf ASP.NET MVC 4 und einer Version der Music Store-Beispielanwendung, die so angepasst und minimiert ist, dass Sie nur den in dieser praktischen Übungseinheit gezeigten Features entspricht.
> 
> Wenn Sie die gesamte **Music Store** -Lernprogramm Anwendung erkunden möchten, finden Sie Sie in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

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

Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, können Sie den Anhang dieses Dokuments &quot;[Anhang C: Verwenden von Code Ausschnitten](#AppendixC)&quot;entnehmen.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exerzitien

Diese praktische Übungseinheit besteht aus den folgenden Übungen:

1. [Übung 1: Hinzufügen einer Datenbank](#Exercise1)
2. [Übung 2: Erstellen einer Datenbank mit Code First](#Exercise2)
3. [Übung 3: Abfragen der Datenbank mit Parametern](#Exercise3)

> [!NOTE]
> Jede Übung wird von einem **endordner** begleitet, der die sich ergebende Lösung enthält, die Sie nach Abschluss der Übungen erhalten. Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Durcharbeiten der Übungen benötigen.

Geschätzte Zeit bis zum Abschluss dieses Labs: **35 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Übung 1: Hinzufügen einer Datenbank

In dieser Übung erfahren Sie, wie Sie eine Datenbank mit den Tabellen der Anwendung "Musicstore" zur Projekt Mappe hinzufügen, um Ihre Daten zu nutzen. Nachdem die Datenbank mit dem Modell generiert und der Projekt Mappe hinzugefügt wurde, ändern Sie die StoreController-Klasse, um die Ansichts Vorlage mit den Daten aus der Datenbank bereitzustellen, anstatt hart codierte Werte zu verwenden.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Aufgabe 1: Hinzufügen einer Datenbank

In dieser Aufgabe fügen Sie der Projekt Mappe eine bereits erstellte Datenbank mit den Haupt Tabellen der "Musicstore"-Anwendung hinzu.

1. Öffnen Sie die Projekt Mappe " **Anfang** " unter **Quell/EX1-addingadatabasedbfirst/BEGIN/** Folder.

   1. Sie müssen einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Fügen Sie die **mvcmusicstore** -Datenbankdatei hinzu. In dieser praktischen Übungseinheit verwenden Sie eine bereits erstellte Datenbank mit dem Namen " **mvcmusicstore. mdf**". Klicken Sie hierzu mit der rechten Maustaste auf **App\_Daten** Ordner, zeigen Sie auf **Hinzufügen** , und klicken Sie dann auf **Vorhandenes Element**. Navigieren Sie zu **\source\assets** , und wählen Sie die Datei " **mvcmusicstore. mdf** " aus.

    ![Hinzufügen eines vorhandenen Elements](aspnet-mvc-4-models-and-data-access/_static/image2.png "Hinzufügen eines vorhandenen Elements")

    *Hinzufügen eines vorhandenen Elements*

    ![Mvcmusicstore. MDF-Datenbankdatei](aspnet-mvc-4-models-and-data-access/_static/image3.png "Mvcmusicstore. MDF-Datenbankdatei")

    *Mvcmusicstore. MDF-Datenbankdatei*

    Die Datenbank wurde dem Projekt hinzugefügt. Auch wenn sich die Datenbank in der Lösung befindet, können Sie Sie Abfragen und aktualisieren, da Sie auf einem anderen Datenbankserver gehostet wurde.

    ![Mvcmusicstore-Datenbank in Projektmappen-Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "Mvcmusicstore-Datenbank in Projektmappen-Explorer")

    *Mvcmusicstore-Datenbank in Projektmappen-Explorer*
3. Überprüfen Sie die Verbindung mit der Datenbank. Doppelklicken Sie hierzu auf " **mvcmusicstore. mdf** ", um eine Verbindung herzustellen.

    ![Verbindung mit "mvcmusicstore. mdf" wird hergestellt](aspnet-mvc-4-models-and-data-access/_static/image5.png "Verbindung mit "mvcmusicstore. mdf" wird hergestellt")

    *Verbindung mit "mvcmusicstore. mdf" wird hergestellt*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Aufgabe 2: Erstellen eines Datenmodells

In dieser Aufgabe erstellen Sie ein Datenmodell, um mit der Datenbank zu interagieren, die in der vorherigen Aufgabe hinzugefügt wurde.

1. Erstellen Sie ein Datenmodell, das die Datenbank darstellt. Klicken Sie hierzu in Projektmappen-Explorer klicken Sie mit der rechten Maustaste auf den Ordner **Modelle** , zeigen Sie auf **Hinzufügen** , und klicken Sie dann auf **Neues Element**. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Vorlage **Daten** aus, und klicken Sie dann auf **ADO.NET Entity Data Model** Element. Ändern Sie den Namen des Datenmodells in **storedb. edmx** , und klicken Sie auf **Hinzufügen**.

    ![Hinzufügen der storedb ADO.NET-Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Hinzufügen der storedb ADO.NET-Entity Data Model")

    *Hinzufügen der storedb ADO.NET-Entity Data Model*
2. Der **Entity Data Model-Assistent** wird angezeigt. Dieser Assistent führt Sie durch die Erstellung der Modell Ebene. Da das Modell basierend auf der zuvor hinzugefügten Datenbank erstellt werden sollte, wählen Sie **aus Datenbank generieren aus** , und klicken Sie auf **weiter**.

    ![Auswählen des Modell Inhalts](aspnet-mvc-4-models-and-data-access/_static/image7.png "Auswählen des Modell Inhalts")

    *Auswählen des Modell Inhalts*
3. Da Sie ein Modell aus einer Datenbank erstellen, müssen Sie die zu verwendende Verbindung angeben. Klicken Sie auf **neue Verbindung**.
4. Wählen Sie **Microsoft SQL Server Datenbankdatei** , und klicken Sie auf **weiter**

    ![Datenquelle auswählen](aspnet-mvc-4-models-and-data-access/_static/image8.png "Datenquelle auswählen")

    *Dialogfeld "Datenquelle auswählen"*
5. Klicken Sie auf **Durchsuchen** , und wählen Sie die Datenbank **mvcmusicstore. mdf** aus, die Sie im Ordner **App\_Data** finden, und klicken Sie auf **OK**

    ![Verbindungs Eigenschaften](aspnet-mvc-4-models-and-data-access/_static/image9.png "Verbindungseigenschaften")

    *Verbindungs Eigenschaften*
6. Die generierte Klasse muss den gleichen Namen wie die Entitäts Verbindungs Zeichenfolge haben. ändern Sie den Namen in " **musicstoreentities** ", und klicken Sie auf **weiter**.

    ![Auswählen der Datenverbindung](aspnet-mvc-4-models-and-data-access/_static/image10.png "Auswählen der Datenverbindung")

    *Auswählen der Datenverbindung*
7. Wählen Sie die zu verwendenden Datenbankobjekte aus. Wenn das Entitäts Modell nur die Tabellen der Datenbank verwendet, wählen Sie die Option **Tabellen** aus, und vergewissern Sie sich, dass die Optionen für die Option " **Fremdschlüssel Spalten in das Modell einbeziehen** " und "pluralisieren" bzw. "Singular" für **generierte Objektnamen** ebenfalls ausgewählt sind. Ändern Sie den Modell Namespace in **mvcmusicstore. Model** , und klicken Sie auf **Fertig**stellen.

    ![Auswählen der Datenbankobjekte](aspnet-mvc-4-models-and-data-access/_static/image11.png "Auswählen der Datenbankobjekte")

    *Auswählen der Datenbankobjekte*

    > [!NOTE]
    > Wenn ein Dialogfeld mit einer Sicherheitswarnung angezeigt wird, klicken Sie auf **OK** , um die Vorlage auszuführen und die Klassen für die Modell Entitäten zu generieren.
8. Ein Entitäts Diagramm für die Datenbank wird angezeigt, während eine separate Klasse erstellt wird, die jede Tabelle der Datenbank zuordnet. Beispielsweise wird die Tabelle **Alben** durch eine **Album** -Klasse dargestellt, wobei jede Spalte in der Tabelle einer Klassen Eigenschaft zugeordnet wird. Dies ermöglicht es Ihnen, Objekte abzufragen und zu bearbeiten, die Zeilen in der Datenbank darstellen.

    ![Entitäts Diagramm](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entitätsdiagramm")

    *Entitäts Diagramm*

    > [!NOTE]
    > Die T4-Vorlagen (. tt) führen Code aus, um die Entitäts Klassen zu generieren, und überschreiben die vorhandenen Klassen mit demselben Namen. In diesem Beispiel wurden die Klassen &quot;Album&quot;, &quot;Genre&quot; und &quot;Künstlerin&quot; mit dem generierten Code überschrieben.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Aufgabe 3: entwickeln der Anwendung

In dieser Aufgabe überprüfen Sie, dass das Projekt erfolgreich mithilfe der neuen Datenmodell Klassen erstellt wurde, obwohl die Modell Generierung die Modelle " **Album**", " **Genre** " und " **Artist** " entfernt hat.

1. Erstellen Sie das Projekt, indem Sie das Menü Element **Build** auswählen und dann **mvcmusicstore erstellen**.

    ![Projekt wird aufgebaut](aspnet-mvc-4-models-and-data-access/_static/image13.png "Projekt wird aufgebaut")

    *Projekt wird aufgebaut*
2. Das Projekt wurde erfolgreich erstellt. Warum funktioniert es noch? Dies funktioniert, da die Datenbanktabellen Felder enthalten, die die Eigenschaften enthalten, die Sie im **Album** "entfernte Klassen" und im **Genre**verwendet haben.

    ![Builds erfolgreich](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds erfolgreich")

    *Builds erfolgreich*
3. Während der Designer die Entitäten in einem Diagramm Format anzeigt, handelt es C# sich um Klassen. Erweitern Sie den Knoten **storedb. edmx** im Projektmappen-Explorer und dann **StoreDB.tt**, werden die neuen generierten Entitäten angezeigt.

    ![Generierte Dateien](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generierte Dateien")

    *Generierte Dateien*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Aufgabe 4: Abfragen der Datenbank

In dieser Aufgabe aktualisieren Sie die StoreController-Klasse, sodass anstelle von hart codierten Daten die Datenbank abgefragt wird, um die Informationen abzurufen.

1. Öffnen Sie " **controllers\storecontroller.cs** ", und fügen Sie der Klasse das folgende Feld hinzu, um eine Instanz der Klasse " **musicstoreentities** " mit dem Namen " **storedb**" zu speichern:

    (Code Ausschnitt- *Modelle und Datenzugriff-EX1 storedb*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. Die Klasse " **musicstoreentities** " macht eine Auflistungs Eigenschaft für jede Tabelle in der Datenbank verfügbar. Aktualisieren Sie die Methode zum **Durchsuchen** von Aktionen, um ein Genre mit allen **Alben**abzurufen.

    (Code Ausschnitt- *Modelle und Datenzugriff-EX1 Store durchsuchen*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > Sie verwenden eine Funktion von .net mit dem Namen **LINQ** (Language-Integrated Query), um stark typisierte Abfrage Ausdrücke für diese Auflistungen zu schreiben, die Code für die Datenbank ausführen und Objekte zurückgeben, für die Sie programmieren können.
    > 
    > Weitere Informationen zu LINQ finden Sie auf der [MSDN-Website](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Aktualisieren Sie die **Index** Aktionsmethode, um alle Genres abzurufen.

    (Code Ausschnitt- *Modelle und Datenzugriff-EX1-Speicher Index*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Aktualisieren Sie die **Index** Aktionsmethode, um alle Genres abzurufen und die Auflistung in eine Liste umzuwandeln.

    (Code Ausschnitt- *Modelle und Datenzugriff-EX1 Store genremenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe überprüfen Sie, ob auf der Seite Speicher Index nun die in der Datenbank gespeicherten Genres anstelle der hart codierten angezeigt werden. Es ist nicht erforderlich, die Ansichts Vorlage zu ändern, da **StoreController** dieselben Entitäten wie zuvor zurückgibt, obwohl die Daten dieses Zeitraums aus der Datenbank stammen.

1. Erstellen Sie die Lösung neu, und drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Vergewissern Sie sich, dass das Menü der **Genres** keine hart codierte Liste mehr ist und die Daten direkt aus der Datenbank abgerufen werden.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Durchsuchen von Genres aus der Datenbank*
3. Navigieren Sie nun zu einem beliebigen Genre, und überprüfen Sie, ob die Alben aus der Datenbank aufgefüllt

    ![Durchsuchen von Alben aus der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image17.png "Durchsuchen von Alben aus der Datenbank")

    *Durchsuchen von Alben aus der Datenbank*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Übung 2: Erstellen einer Datenbank mit Code First

In dieser Übung erfahren Sie, wie Sie den Code First Ansatz verwenden, um eine Datenbank mit den Tabellen der "Musicstore"-Anwendung zu erstellen und um auf die Daten zuzugreifen.

Nachdem das Modell generiert wurde, ändern Sie StoreController, um die Ansichts Vorlage mit den Daten aus der Datenbank bereitzustellen, anstatt hart codierte Werte zu verwenden.

> [!NOTE]
> Wenn Sie Übung 1 abgeschlossen haben und bereits mit dem Database First Ansatz gearbeitet haben, erfahren Sie nun, wie Sie die gleichen Ergebnisse mit einem anderen Prozess erhalten. Die Aufgaben, die in Übung 1 häufig ausgeführt werden, wurden gekennzeichnet, um das Lesen zu vereinfachen. Wenn Sie Übung 1 nicht abgeschlossen haben, aber den Code First Ansatz erlernen möchten, können Sie mit dieser Übung beginnen und eine vollständige Abdeckung des Themas erhalten.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Aufgabe 1: Auffüllen von Beispiel Daten

In dieser Aufgabe füllen Sie die Datenbank mit Beispiel Daten auf, wenn Sie anfänglich mit Code First erstellt werden.

1. Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Source/EX2-"-kreatingadatabasecoentfirst/BEGIN/** Folder ". Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Fügen Sie die Datei **SampleData.cs** dem Ordner **Models** hinzu. Klicken Sie hierzu mit der rechten Maustaste auf den Ordner **Modelle** , zeigen Sie auf **Hinzufügen** , und klicken Sie dann auf **Vorhandenes Element**. Navigieren Sie zu **\source\assets** , und wählen Sie die Datei **SampleData.cs** aus.

    ![Beispiel Daten Auffüllen von Code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Beispiel Daten Auffüllen von Code")

    *Beispiel Daten Auffüllen von Code*
3. Öffnen Sie die Datei **Global.asax.cs** , und fügen Sie die folgenden *using* -Anweisungen hinzu.

    (Code Ausschnitt- *Modelle und Datenzugriff EX2 globale asax*-using-Direktiven)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. Fügen Sie in der **Anwendung\_Start ()** -Methode die folgende Zeile hinzu, um den datenbankinitialisierer festzulegen.

    (Code Ausschnitt- *Modelle und Datenzugriff-EX2 Global asax setinitializer*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Aufgabe 2: Konfigurieren der Verbindung mit der Datenbank

Nachdem Sie dem Projekt bereits eine Datenbank hinzugefügt haben, schreiben Sie die Verbindungs Zeichenfolge in der Datei " **Web. config** ".

1. Fügen Sie eine Verbindungs Zeichenfolge in **Web. config**hinzu. Öffnen Sie hierzu " **Web. config** " unter "Projektstamm", und ersetzen Sie die Verbindungs Zeichenfolge "DefaultConnection" im Abschnitt " **&lt;connectionStrings&gt;** " durch diese Zeile:

    ![Speicherort der Datei Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "Speicherort der Datei Web. config")

    *Speicherort der Datei Web. config*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Aufgabe 3: Arbeiten mit dem Modell

Nachdem Sie die Verbindung mit der Datenbank bereits konfiguriert haben, verknüpfen Sie das Modell mit den Datenbanktabellen. In dieser Aufgabe erstellen Sie eine-Klasse, die mit Code First mit der-Datenbank verknüpft wird. Beachten Sie, dass eine vorhandene poco-Modell Klasse vorhanden ist, die geändert werden sollte.

> [!NOTE]
> Wenn Sie Übung 1 abgeschlossen haben, werden Sie feststellen, dass dieser Schritt von einem Assistenten durchgeführt wurde. Wenn Sie Code First, erstellen Sie manuell Klassen, die mit Daten Entitäten verknüpft werden.

1. Öffnen Sie das poco-Modellklassen **Genre** aus dem Projektordner **Models** , und fügen Sie eine ID ein. Verwenden Sie eine int-Eigenschaft mit dem Namen **GenreID**.

    (Code Ausschnitt- *Modelle und Datenzugriff-EX2 Code First Genre*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Zum Arbeiten mit Code First Konventionen muss das Klassen Genre über eine Primärschlüssel Eigenschaft verfügen, die automatisch erkannt wird.
    > 
    > Weitere Informationen zu Code First Konventionen finden Sie in diesem [MSDN-Artikel](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. Öffnen Sie nun das poco-Modellklassen- **Album** aus dem Projektordner **Models** , und schließen Sie die Fremdschlüssel ein, und erstellen Sie Eigenschaften mit den Namen **GenreID** und **artistId**. Diese Klasse verfügt bereits über den **GenreID** für den Primärschlüssel.

    (Code Ausschnitt- *Modelle und Datenzugriff-EX2 Code First Album*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Öffnen Sie die poco- **Modell Klasse,** und schließen Sie die **artistId-** Eigenschaft ein.

    (Code Ausschnitt- *Modelle und Datenzugriff-EX2 Code First Künstlerin*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Klicken Sie mit der rechten Maustaste auf den Projektordner **Modelle** , **und wählen Sie -Klasse**. Nennen Sie die Datei **MusicStoreEntities.cs**. Klicken Sie dann auf **hinzufügen.**

    ![Hinzufügen einer Klasse](aspnet-mvc-4-models-and-data-access/_static/image20.png "Hinzufügen einer Klasse")

    *Hinzufügen eines neuen Elements*

    ![Hinzufügen eines Klasse2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Hinzufügen eines Klasse2")

    *Hinzufügen einer Klasse*
5. Öffnen Sie die soeben erstellte Klasse **MusicStoreEntities.cs**, und fügen Sie die Namespaces **System. Data. Entity** und **System. Data. Entity. Infrastructure**ein.

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Ersetzen Sie die Klassen Deklaration, um die **dbcontext** -Klasse zu erweitern: Deklarieren Sie ein öffentliches **dbset** , und überschreiben Sie die **onmodelcreating** Nach diesem Schritt erhalten Sie eine Domänen Klasse, mit der das Modell mit dem Entity Framework verknüpft wird. Ersetzen Sie dazu den Klassen Code durch Folgendes:

    (Code Ausschnitt- *Modelle und Datenzugriff-EX2 Code First musicstoreentities*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> Mit Entity Framework **dbcontext** und **dbset** können Sie das poco-Klassen Genre Abfragen. Wenn Sie die **onmodelcreating** -Methode erweitern, geben Sie im **Code** an, wie das Genre einer Datenbanktabelle zugeordnet werden soll. Weitere Informationen zu dbcontext und dbset finden Sie in diesem MSDN-Artikel: [Link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Aufgabe 4: Abfragen der Datenbank

In dieser Aufgabe aktualisieren Sie die StoreController-Klasse so, dass Sie anstelle von hart codierten Daten aus der Datenbank abgerufen wird.

> [!NOTE]
> Diese Aufgabe wird in Übung 1 häufig durchführen.
> 
> Wenn Sie Übung 1 abgeschlossen haben, werden Sie feststellen, dass diese Schritte in beiden Ansätzen identisch sind (Database First oder Code First). Sie unterscheiden sich in der Art und Weise, wie die Daten mit dem Modell verknüpft sind, aber der Zugriff auf Daten Entitäten ist vom Controller aus noch transparent.

1. Öffnen Sie " **controllers\storecontroller.cs** ", und fügen Sie der Klasse das folgende Feld hinzu, um eine Instanz der Klasse " **musicstoreentities** " mit dem Namen " **storedb**" zu speichern:

    (Code Ausschnitt- *Modelle und Datenzugriff-EX1 storedb*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. Die Klasse " **musicstoreentities** " macht eine Auflistungs Eigenschaft für jede Tabelle in der Datenbank verfügbar. Aktualisieren Sie die Methode zum **Durchsuchen** von Aktionen, um ein Genre mit allen **Alben**abzurufen.

    (Code Ausschnitt- *Modelle und Datenzugriff-EX2 Store durchsuchen*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > Sie verwenden eine Funktion von .net mit dem Namen **LINQ** (Language-Integrated Query), um stark typisierte Abfrage Ausdrücke für diese Auflistungen zu schreiben, die Code für die Datenbank ausführen und Objekte zurückgeben, für die Sie programmieren können.
    > 
    > Weitere Informationen zu LINQ finden Sie auf der [MSDN-Website](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Aktualisieren Sie die **Index** Aktionsmethode, um alle Genres abzurufen.

    (Code Ausschnitt- *Modelle und Datenzugriff-EX2 Store-Index*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Aktualisieren Sie die **Index** Aktionsmethode, um alle Genres abzurufen und die Auflistung in eine Liste umzuwandeln.

    (Code Ausschnitt- *Modelle und Datenzugriff-EX2 Store genremenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Aufgabe 5: Ausführen der Anwendung

In dieser Aufgabe überprüfen Sie, ob auf der Seite Speicher Index nun die in der Datenbank gespeicherten Genres anstelle der hart codierten angezeigt werden. Die Ansichts Vorlage muss nicht geändert werden, da der **StoreController** das gleiche **storeindexviewmodel** zurückgibt wie zuvor, aber dieses Mal werden die Daten aus der Datenbank abgerufen.

1. Erstellen Sie die Lösung neu, und drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Vergewissern Sie sich, dass das Menü der **Genres** keine hart codierte Liste mehr ist und die Daten direkt aus der Datenbank abgerufen werden.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Durchsuchen von Genres aus der Datenbank*
3. Navigieren Sie nun zu einem beliebigen Genre, und überprüfen Sie, ob die Alben aus der Datenbank aufgefüllt

    ![Durchsuchen von Alben aus der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image23.png "Durchsuchen von Alben aus der Datenbank")

    *Durchsuchen von Alben aus der Datenbank*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Übung 3: Abfragen der Datenbank mit Parametern

In dieser Übung erfahren Sie, wie Sie die Datenbank mithilfe von Parametern Abfragen und wie Sie die Abfrageergebnis Strukturierung verwenden, ein Feature, mit dem die Anzahl der Datenbankzugriffe auf eine effizientere Weise abgerufen wird.

> [!NOTE]
> Weitere Informationen zur Abfrageergebnis Strukturierung finden Sie im folgenden [MSDN-Artikel](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Aufgabe 1: Ändern von StoreController zum Abrufen von Alben aus der Datenbank

In dieser Aufgabe ändern Sie die **StoreController** -Klasse so, dass Sie auf die Datenbank zugreift, um Alben aus einem bestimmten Genre abzurufen.

1. Öffnen Sie die Projekt Mappe " **Begin** " im Ordner " **source\ex3-queryingthedatabasewithparameterscodefirst\begin** ", wenn Sie den Code-First-Ansatz oder den Ordner " **source\ex3-queryingthedatabasewithparametersdbfirst\begin** " verwenden möchten, wenn Sie den Daten Bank ersten Ansatz verwenden möchten. Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.

   1. Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen. Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.
   2. Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.
   3. Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** ** | Projekt Mappe erstellen klicken**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern. Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen. Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.
2. Öffnen Sie die **StoreController** -Klasse, um die **Browse** -Aktionsmethode zu ändern. Erweitern Sie dazu im **Projektmappen-Explorer**den Ordner **Controller** , und doppelklicken Sie auf **StoreController.cs**.
3. Ändern Sie die Methode zum **Durchsuchen** von Aktionen, um Alben für ein bestimmtes Genre abzurufen. Ersetzen Sie hierzu den folgenden Code:

    (Code Ausschnitt- *Modelle und Datenzugriff-EX3 StoreController browsemethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> Um eine Auflistung der Entität aufzufüllen, müssen Sie die **include** -Methode verwenden, um anzugeben, dass auch die Alben abgerufen werden sollen. Sie können das verwenden. **Single ()** -Erweiterung in LINQ, da in diesem Fall nur ein Genre für ein Album erwartet wird. Die **Single ()** -Methode nimmt einen Lambda-Ausdruck als Parameter an, der in diesem Fall ein einzelnes Genre Objekt angibt, sodass der Name mit dem definierten Wert übereinstimmt.
> 
> Sie profitieren von einer Funktion, die es Ihnen ermöglicht, auch andere Verwandte Entitäten anzugeben, die Sie laden möchten, wenn das Genre Objekt abgerufen wird. Diese Funktion wird als **Abfrageergebnis Strukturierung**bezeichnet und ermöglicht es Ihnen, die Anzahl der zum Abrufen von Informationen benötigten Zeitpunkten für den Zugriff auf die Datenbank zu reduzieren. In diesem Szenario möchten Sie die Alben für das Genre, das Sie abrufen, vorab abrufen.
> 
> Die Abfrage enthält **Genres. include (&quot;Alben&quot;)** , um anzugeben, dass Sie auch Verwandte Alben benötigen. Dies führt zu einer effizienteren Anwendung, da Sie sowohl Genre-als auch Album Daten in einer einzelnen Daten Bank Anforderung abruft.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Aufgabe 2: Ausführen der Anwendung

In dieser Aufgabe werden Sie die Anwendung ausführen und Alben eines bestimmten Genres aus der Datenbank abrufen.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL in **/Store/Browse? Genre = Pop** , um zu überprüfen, ob die Ergebnisse aus der Datenbank abgerufen werden.

    ![Durchsuchen nach Genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Durchsuchen nach Genre")

    *Durchsuchen/Store/Browse? Genre = Pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Aufgabe 3: Zugreifen auf Alben nach ID

In dieser Aufgabe wiederholen Sie das vorherige Verfahren, um Alben anhand ihrer ID zu erhalten.

1. Schließen Sie den Browser bei Bedarf, um zu Visual Studio zurückzukehren. Öffnen Sie die **StoreController** -Klasse, um die **Detail** Aktionsmethode zu ändern. Erweitern Sie dazu im **Projektmappen-Explorer**den Ordner **Controller** , und doppelklicken Sie auf **StoreController.cs**.
2. Ändern Sie die **Detail** Aktionsmethode, um die Details der Alben basierend auf Ihrer **ID**abzurufen. Ersetzen Sie hierzu den folgenden Code:

    (Code Ausschnitt- *Modelle und Datenzugriff-EX3 StoreController detailsmethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Aufgabe 4: Ausführen der Anwendung

In dieser Aufgabe führen Sie die Anwendung in einem Webbrowser aus und erhalten die Details des Albums anhand ihrer ID.

1. Drücken Sie **F5** , um die Anwendung auszuführen.
2. Das Projekt wird auf der Startseite gestartet. Ändern Sie die URL in **/Store/Details/51** , oder Durchsuchen Sie die Genres, und wählen Sie ein Album aus, um sicherzustellen, dass die Ergebnisse aus der Datenbank abgerufen werden.

    ![Details zum Durchsuchen](aspnet-mvc-4-models-and-data-access/_static/image25.png "Details zum Durchsuchen")

    *Durchsuchen/Store/Details/51*

> [!NOTE]
> Außerdem können Sie diese Anwendung in Windows Azure-Websites bereitstellen, [indem Sie Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy verwenden](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Durch die Durchführung dieser praktischen Übungseinheit haben Sie die Grundlagen von ASP.NET MVC-Modellen und-Datenzugriff unter Verwendung des **Database First** Ansatzes und des **Code First** Ansatzes kennengelernt:

- Vorgehensweise beim Hinzufügen einer Datenbank zur Projekt Mappe, um Ihre Daten zu nutzen
- Aktualisieren von Controllern zum Bereitstellen von Ansichts Vorlagen mit den Daten, die aus der Datenbank entnommen werden, anstelle von hart codierten Daten
- Abfragen der Datenbank mithilfe von Parametern
- Verwendung der Abfrageergebnis Strukturierung, ein Feature, das die Anzahl der Datenbankzugriffe reduziert und Daten effizienter abruft
- Verwenden von Database First-und Code First Ansätzen in Microsoft Entity Framework zum Verknüpfen der Datenbank mit dem Modell

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren. Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.

1. Wechseln Sie zu [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Windows Azure SDK</em>&quot;suchen.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.
3. Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.

    ![Installieren von Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Installationsfortschritt*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.

    ![Installation abgeschlossen](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Installation abgeschlossen*
7. Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .
8. Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .

    ![VS Express für Web-Kachel](aspnet-mvc-4-models-and-data-access/_static/image30.png)

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

    ![Anmelden bei Windows Azure-Portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Anmelden bei Windows Azure-Portal")

    *Anmelden bei Windows Azure Verwaltungsportal*
2. Klicken Sie in der Befehlsleiste auf **neu** .

    ![Erstellen einer neuen Website](aspnet-mvc-4-models-and-data-access/_static/image32.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **Compute** - | **Website**. Wählen Sie dann **schneller** Fassung aus. Geben Sie eine verfügbare URL für die neue Website an, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Eine Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud ausgeführt wird und die Sie steuern und verwalten können. Mit der Option schneller Fassung können Sie eine abgeschlossene Webanwendung von außerhalb des Portals auf der Windows Azure-Website bereitstellen. Sie enthält keine Schritte zum Einrichten einer Datenbank.

    ![Erstellen einer neuen Website mithilfe der schneller Fassung](aspnet-mvc-4-models-and-data-access/_static/image33.png "Erstellen einer neuen Website mithilfe der schneller Fassung")

    *Erstellen einer neuen Website mithilfe der schneller Fassung*
4. Warten Sie, bis die neue **Website** erstellt wurde.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** -Spalte. Überprüfen Sie, ob die neue Website funktioniert.

    ![Navigieren zur neuen Website](aspnet-mvc-4-models-and-data-access/_static/image34.png "Navigieren zur neuen Website")

    *Navigieren zur neuen Website*

    ![Website wird ausgeführt](aspnet-mvc-4-models-and-data-access/_static/image35.png "Website wird ausgeführt")

    *Website wird ausgeführt*
6. Wechseln Sie zurück zum Portal, und klicken Sie in der Spalte **Name** auf den Namen der Website, um die Verwaltungs Seiten anzuzeigen.

    ![Öffnen der Website-Verwaltungs Seiten](aspnet-mvc-4-models-and-data-access/_static/image36.png "Öffnen der Website-Verwaltungs Seiten")

    *Öffnen der Website-Verwaltungs Seiten*
7. Klicken Sie auf der Seite **Dashboard** im Abschnitt **kurzer Blick** auf den Link **Veröffentlichungs Profil herunterladen** .

    > [!NOTE]
    > Das *Veröffentlichungs Profil* enthält alle Informationen, die zum Veröffentlichen einer Webanwendung auf einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungs Methoden erforderlich sind. Das Veröffentlichungs Profil enthält die URLs, Benutzer Anmelde Informationen und Daten Bank Zeichenfolgen, die erforderlich sind, um eine Verbindung mit den einzelnen Endpunkten herzustellen und diese zu authentifizieren, für die eine Veröffentlichungs Methode aktiviert ist. **Microsoft webmatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** unterstützen das Lesen von Veröffentlichungs Profilen, um die Konfiguration dieser Programme für das Veröffentlichen von Webanwendungen auf Windows Azure-Websites zu automatisieren.

    ![Das Website-Veröffentlichungs Profil wird heruntergeladen.](aspnet-mvc-4-models-and-data-access/_static/image37.png "Das Website-Veröffentlichungs Profil wird heruntergeladen.")

    *Das Website-Veröffentlichungs Profil wird heruntergeladen.*
8. Laden Sie die Veröffentlichungs Profil Datei an einen bekannten Speicherort herunter. In dieser Übung erfahren Sie, wie Sie diese Datei zum Veröffentlichen einer Webanwendung auf Windows Azure-Websites in Visual Studio verwenden.

    ![Die Veröffentlichungs Profil Datei wird gespeichert.](aspnet-mvc-4-models-and-data-access/_static/image38.png "Das Veröffentlichungs Profil wird gespeichert.")

    *Die Veröffentlichungs Profil Datei wird gespeichert.*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: Konfigurieren des Datenbankservers

Wenn Ihre Anwendung SQL Server Datenbanken nutzt, müssen Sie einen SQL-Daten Bank Server erstellen. Wenn Sie eine einfache Anwendung bereitstellen möchten, die SQL Server nicht verwendet, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank. Sie können die SQL-Datenbankserver aus Ihrem Abonnement im Windows Azure-Verwaltungs Portal unter **SQL-Datenbanken** | **Server** | **Dashboard des Servers**anzeigen. Wenn Sie keinen Server erstellt haben, können Sie ihn mithilfe der Schaltfläche **Hinzufügen** auf der Befehlsleiste erstellen. Notieren Sie sich den **Servernamen und die URL, den Administrator Anmelde Namen und das Kennwort**, da Sie Sie in den nächsten Aufgaben verwenden werden. Erstellen Sie die Datenbank noch nicht, da Sie in einer späteren Phase erstellt wird.

    ![Dashboard des SQL-Datenbankservers](aspnet-mvc-4-models-and-data-access/_static/image39.png "Dashboard des SQL-Datenbankservers")

    *Dashboard des SQL-Datenbankservers*
2. In der nächsten Aufgabe testen Sie die Datenbankverbindung aus Visual Studio. aus diesem Grund müssen Sie Ihre lokale IP-Adresse in die Liste der **zulässigen IP-Adressen**des Servers einschließen. Klicken Sie hierzu auf **Konfigurieren**, wählen Sie die IP-Adresse der **aktuellen Client-IP-Adresse** aus, und fügen Sie Sie in die Textfelder Start-IP- **Adresse** und End- **IP-Adresse** ein, und klicken Sie auf die Schaltfläche ![Add-Client-IP-Address-OK-Button](aspnet-mvc-4-models-and-data-access/_static/image40.png)

    ![Client-IP-Adresse](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Client-IP-Adresse*
3. Sobald die **Client-IP-Adresse** der Liste zulässige IP-Adressen hinzugefügt wurde, klicken Sie auf **Speichern** , um die Änderungen zu bestätigen.

    ![Änderungen bestätigen](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Änderungen bestätigen*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy

1. Kehren Sie zur ASP.NET MVC 4-Lösung zurück. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Website Projekt, und wählen Sie **veröffentlichen**aus.

    ![Veröffentlichen der Anwendung](aspnet-mvc-4-models-and-data-access/_static/image43.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungs Profil, das Sie in der ersten Aufgabe gespeichert haben.

    ![Importieren des Veröffentlichungs Profils](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importieren des Veröffentlichungs Profils")

    *Veröffentlichungs Profil wird importiert.*
3. Klicken Sie auf **Verbindung**überprüfen. Klicken Sie nach Abschluss der Überprüfung auf **weiter**.

    > [!NOTE]
    > Die Überprüfung ist fertiggestellt, sobald ein grünes Häkchen neben der Schaltfläche Verbindung überprüfen angezeigt wird.

    ![Die Verbindung wird überprüft.](aspnet-mvc-4-models-and-data-access/_static/image45.png "Die Verbindung wird überprüft.")

    *Die Verbindung wird überprüft.*
4. Klicken Sie auf der Seite **Einstellungen** unter dem Abschnitt **Datenbanken** auf die Schaltfläche neben dem Textfeld der Datenbankverbindung (z. b. **DefaultConnection**).

    ![Webbereitstellungs Konfiguration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Webbereitstellungs Konfiguration")

    *Webbereitstellungs Konfiguration*
5. Konfigurieren Sie die Datenbankverbindung wie folgt:

   - Geben Sie unter **Server Name** die URL des SQL-Datenbankservers unter Verwendung des *TCP:* -Präfix ein.
   - Geben Sie unter **Benutzername** den Anmelde Namen des Server Administrators ein.
   - Geben Sie unter **Kennwort** Ihren Server Administrator-Anmelde Kennwort ein.
   - Geben Sie einen neuen Datenbanknamen ein.

     ![Konfigurieren der Ziel Verbindungs Zeichenfolge](aspnet-mvc-4-models-and-data-access/_static/image47.png "Konfigurieren der Ziel Verbindungs Zeichenfolge")

     *Konfigurieren der Ziel Verbindungs Zeichenfolge*
6. Klicken Sie dann auf **OK**. Wenn Sie zum Erstellen der Datenbank aufgefordert werden, klicken Sie auf **Ja**.

    ![Erstellen der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image48.png "Erstellen der Daten Bank Zeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungs Zeichenfolge, die Sie zum Herstellen einer Verbindung mit der SQL-Datenbank in Windows Azure verwenden, wird im Textfeld Standardverbindung angezeigt. Klicken Sie dann auf **Weiter**.

    ![Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank](aspnet-mvc-4-models-and-data-access/_static/image49.png "Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank")

    *Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank*
8. Klicken Sie auf der Seite **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](aspnet-mvc-4-models-and-data-access/_static/image50.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Nachdem der Veröffentlichungs Vorgang abgeschlossen ist, wird die veröffentlichte Website in Ihrem Standardbrowser geöffnet.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Anhang C: Verwenden von Code Ausschnitten

Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen. Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-models-and-data-access/_static/image51.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")

*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*

***So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)***

1. Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.
2. Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).
3. Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.
4. Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).
5. Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.

![Beginnen Sie mit der Eingabe des Ausschnitt namens.](aspnet-mvc-4-models-and-data-access/_static/image52.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")

*Beginnen Sie mit der Eingabe des Ausschnitt namens.*

![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](aspnet-mvc-4-models-and-data-access/_static/image53.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")

*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*

![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](aspnet-mvc-4-models-and-data-access/_static/image54.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")

*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*

So ***fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)*** 1. Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.

1. Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.
2. Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.

![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](aspnet-mvc-4-models-and-data-access/_static/image55.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")

*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*

![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](aspnet-mvc-4-models-and-data-access/_static/image56.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")

*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*
