---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 'Tutorial: Einstieg in Entity Framework 6 Code First mit MVC 5 | Microsoft-Dokumentation'
description: In dieser Reihe von Tutorials erfahren Sie, wie Sie eine ASP.NET MVC 5-Anwendung erstellen, die für den Datenzugriff Entity Framework 6 verwendet.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a00a5a3aa295c2584b90abbf931c258c9e1b58ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499407"
---
# <a name="tutorial-get-started-with-entity-framework-6-code-first-using-mvc-5"></a>Tutorial: Einstieg in Entity Framework 6 Code First mit MVC 5

> [!NOTE]
> Für eine neue Entwicklung wird empfohlen, [ASP.net Core Razor Pages](/aspnet/core/razor-pages) MVC-Controllern und-Sichten zu über ASP.net. Eine Reihe von tutorialreihen, die in diesem Beispiel Razor Pages, finden Sie unter [Tutorial: Einstieg in die Razor pages in ASP.net Core](/aspnet/core/tutorials/razor-pages/razor-pages-start). Das neue Tutorial:
> * Ist einfacher zu befolgen.
> * Bietet mehr bewährte Methoden für Entity Framework Core.
> * Verwendet effizientere Abfragen.
> * Passt besser zur aktuellen API.
> * Behandelt mehr Features.
> * Ist die bevorzugte Methode für die Entwicklung neuer Anwendungen.

In dieser Reihe von Tutorials erfahren Sie, wie Sie eine ASP.NET MVC 5-Anwendung erstellen, die für den Datenzugriff Entity Framework 6 verwendet. In diesem Tutorial wird der Code First Workflow verwendet. Informationen dazu, wie Sie zwischen Code First, Database First und Model First auswählen, finden Sie unter [Erstellen eines Modells](/ef/ef6/modeling/).

In dieser tutorialreihe wird erläutert, wie Sie die Beispielanwendung "" der Anwendung "" erstellen. Die Beispielanwendung ist eine einfache University-Website. Mit diesem Dienst können Sie Informationen zu Studenten, Kurs und Dozenten anzeigen und aktualisieren. Hier sind zwei der Bildschirme, die Sie erstellen:

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Student bearbeiten](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erstellen einer MVC-Web-App
> * Einrichten des Websitestils
> * Installieren von Entity Framework 6
> * Erstellen des Datenmodells
> * Erstellen des Datenbankkontexts
> * Initialisieren Sie die Datenbank mit Testdaten
> * Einrichten von EF 6 für die Verwendung von localdb
> * Erstellen Sie Controller und Ansichten
> * Zeigen Sie die Datenbank an

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

## <a name="create-an-mvc-web-app"></a>Erstellen einer MVC-Web-App

1. Öffnen Sie Visual Studio, und C# erstellen Sie ein Webprojekt mithilfe der Vorlage **ASP.NET-Webanwendung (.NET Framework)** . Nennen Sie das Projekt *conjeuniversity* , und wählen Sie **OK**aus.

   ![Dialogfeld "Neues Projekt" in Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

1. Wählen Sie in der **neuen ASP.NET-Webanwendung-conprosouniversity**die Option **MVC**aus.

   ![Dialogfeld "neue Web-App" in Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

    > [!NOTE]
    > Standardmäßig ist die **Authentifizierungs** Option auf **keine Authentifizierung**festgelegt. Für dieses Tutorial benötigt die Web-App keine Benutzeranmeldung. Außerdem wird der Zugriff nicht basierend auf der Anmeldung eingeschränkt.

1. Klicken Sie auf **OK**, um das Projekt zu erstellen.

## <a name="set-up-the-site-style"></a>Einrichten des Websitestils

Sie können das Websitemenü, das Layout und die Startseite über einige Änderungen einrichten.

1. Öffnen Sie *views\shared\\_Layout. cshtml*, und nehmen Sie die folgenden Änderungen vor:

   - Ändern Sie jedes Vorkommen von "My ASP.NET Application" und "Application Name" in "Configuration Manager".
   - Fügen Sie Menüeinträge für Schüler und Studenten, Kurse, Dozenten und Abteilungen hinzu, und löschen Sie den Kontakt Eintrag.

   Die Änderungen werden im folgenden Code Ausschnitt hervorgehoben:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. Ersetzen Sie in *views\home\index.cshtml*den Inhalt der Datei durch den folgenden Code, um den Text zu ASP.net und MVC durch Text zu dieser Anwendung zu ersetzen:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. Drücken Sie STRG + F5, um die Website auszuführen. Die Startseite wird mit dem Hauptmenü angezeigt.

## <a name="install-entity-framework-6"></a>Installieren von Entity Framework 6

1. Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.

2. Geben Sie im Fenster **Paket-Manager-Konsole** den folgenden Befehl ein:

   ```text
   Install-Package EntityFramework
   ```

Bei diesem Schritt handelt es sich um einen der folgenden Schritte, die Sie manuell durchführen können. Dies kann jedoch automatisch durch die ASP.NET MVC-Gerüstbau Funktion durchgeführt worden sein. Sie führen Sie manuell aus, sodass Sie die erforderlichen Schritte zum Verwenden von Entity Framework (EF) sehen können. Später verwenden Sie Gerüstbau, um den MVC-Controller und Ansichten zu erstellen. Eine Alternative besteht darin, Gerüstbau das nuget-Paket von EF automatisch zu installieren, die Daten Bank Kontext Klasse zu erstellen und die Verbindungs Zeichenfolge zu erstellen. Wenn Sie auf diese Weise arbeiten möchten, müssen Sie lediglich diese Schritte überspringen und den MVC-Controller einrichten, nachdem Sie die Entitäts Klassen erstellt haben.

## <a name="create-the-data-model"></a>Erstellen des Datenmodells

Als nächstes erstellen Sie Entitätsklassen für die Contoso University-Anwendung. Beginnen Sie mit den folgenden drei Entitäten:

**Kurs** <-> **Registrierung** <-> **Student**

| Entitäten | Beziehung |
| -------- | ------------ |
| Kurs zur Registrierung | 1:n |
| Student zur Registrierung | 1:n |

Es besteht eine 1:n-Beziehung zwischen den Entitäten `Student` und `Enrollment`. Außerdem besteht eine 1:n-Beziehung zwischen den Entitäten `Course` und `Enrollment`. Das bedeutet, dass ein Student für beliebig viele Kurse angemeldet sein kann und sich für jeden Kurs eine beliebige Anzahl von Studenten anmelden kann.

In den folgenden Abschnitten erstellen Sie eine Klasse für jede dieser Entitäten.

> [!NOTE]
> Wenn Sie versuchen, das Projekt zu kompilieren, bevor Sie alle diese Entitäts Klassen erstellt haben, erhalten Sie Compilerfehler.

### <a name="the-student-entity"></a>Die Entität „Student“

- Erstellen Sie im Ordner *Models* eine Klassendatei mit dem Namen *Student.cs* , indem Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner klicken und > **Klasse** **Hinzufügen** auswählen. Ersetzen Sie den Vorlagencode durch den folgenden Code:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

Die `ID`-Eigenschaft fungiert als Primärschlüsselspalte der Datenbanktabelle, die dieser Klasse entspricht. Standardmäßig interpretiert Entity Framework eine Eigenschaft mit dem Namen `ID` oder *className* `ID` als Primärschlüssel.

Die `Enrollments`-Eigenschaft ist eine *Navigationseigenschaft*. Navigationseigenschaften enthalten andere Entitäten, die dieser Entität zugehörig sind. In diesem Fall enthält die `Enrollments`-Eigenschaft einer `Student`-Entität alle `Enrollment`-Entitäten, die mit dieser `Student` Entität verknüpft sind. Anders ausgedrückt: Wenn eine angegebene `Student` Zeile in der Datenbank über zwei verknüpfte `Enrollment` Zeilen verfügt (Zeilen, die den Primärschlüssel Wert dieses Schülers in der `StudentID` Fremdschlüssel Spalte enthalten), enthält die `Enrollments` Navigations Eigenschaft der `Student` Entität diese beiden `Enrollment` Entitäten.

Navigations Eigenschaften werden in der Regel als `virtual` definiert, damit Sie von bestimmten Entity Framework Funktionen wie *Lazy Loading*profitieren können. (Lazy Load wird später im Lernprogramm zum [Lesen verwandter Daten](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) weiter unten in dieser Reihe erläutert.)

Wenn eine Navigationseigenschaft mehrere Entitäten enthalten kann (wie bei m:n- oder 1:n-Beziehungen), muss dessen Typ aus einer Liste bestehen, in der Einträge hinzugefügt, gelöscht und aktualisiert werden können – z.B.: `ICollection`.

### <a name="the-enrollment-entity"></a>Die Entität „Enrollment“

- Erstellen Sie im Ordner *Models* (Modelle) die Datei *Enrollment.cs*, und ersetzen Sie den vorhandenen Code durch folgenden Code:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Die `EnrollmentID`-Eigenschaft ist der Primärschlüssel. diese Entität verwendet das *className* -`ID` Muster anstelle `ID`, wie Sie es in der `Student`-Entität gesehen haben. Normalerweise würden Sie nur ein Muster auswählen und dieses für das gesamte Datenmodell verwenden. Diese Variation soll verdeutlichen, dass Sie ein beliebiges Muster erstellen können. In einem späteren Tutorial erfahren Sie, wie Sie mit `ID` ohne `classname` die Vererbung im Datenmodell leichter implementieren können.

Die `Grade`-Eigenschaft ist eine [Enumeration](/ef/ef6/modeling/code-first/data-types/enums). Das Fragezeichen nach der `Grade`-Typdeklaration gibt an, dass die `Grade`-Eigenschaft [NULL-Werte zulässt](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types). Ein Grade, der NULL ist, unterscheidet sich von 0 (null) – NULL bedeutet, dass eine Klasse noch nicht bekannt ist oder noch nicht zugewiesen wurde.

Bei der `StudentID`-Eigenschaft handelt es sich um einen Fremdschlüssel, und die zugehörige Navigationseigenschaft lautet `Student`. Eine `Enrollment`-Entität wird einer `Student`-Entität zugeordnet, damit die Eigenschaft nur eine `Student`-Entität enthalten kann. Dies steht im Gegensatz zu der bereits erläuterten `Student.Enrollments`-Navigationseigenschaft, die mehrere `Enrollment`-Entitäten enthalten kann.

Bei der `CourseID`-Eigenschaft handelt es sich um einen Fremdschlüssel, und die zugehörige Navigationseigenschaft lautet `Course`. Die `Enrollment`-Entität wird einer `Course`-Entität zugeordnet.

Entity Framework interpretiert eine Eigenschaft als Fremdschlüssel Eigenschaft, wenn Sie *&lt;Navigations Eigenschaftsnamen&gt;&lt;Name der Primärschlüssel Eigenschaft*&gt;hat (z. b. `StudentID` für die Navigations Eigenschaft `Student`, da der Primärschlüssel der `Student`-Entität `ID`ist). Fremdschlüssel Eigenschaften können auch einfach *&lt;Namen der Primärschlüssel Eigenschaft&gt;* benannt werden (z. b. `CourseID`, weil der Primärschlüssel der `Course` Entität `CourseID`ist).

### <a name="the-course-entity"></a>Die Entität „Course“

- Erstellen Sie im Ordner *Models* *Course.cs*, und ersetzen Sie den Vorlagen Code durch den folgenden Code:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Die `Enrollments`-Eigenschaft ist eine Navigationseigenschaft. `Course`-Entitäten können sich auf jede beliebige Anzahl von `Enrollment`-Entitäten beziehen.

In einem späteren Tutorial dieser Reihe werden weitere Informationen zum <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute>-Attribut angezeigt. Im Grunde können Sie über dieses Attribut den Primärschlüssel für den Kurs angeben, anstatt ihn von der Datenbank generieren zu lassen.

## <a name="create-the-database-context"></a>Erstellen des Datenbankkontexts

Die Hauptklasse, die Entity Framework Funktionen für ein bestimmtes Datenmodell koordiniert, ist die *Daten Bank Kontext* Klasse. Sie erstellen diese Klasse durch Ableiten von der [System. Data. Entity. dbcontext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.113).aspx) -Klasse. Im Code geben Sie an, welche Entitäten im Datenmodell enthalten sind. Außerdem können Sie bestimmte Entity Framework-Verhalten anpassen. In diesem Projekt heißt die Klasse `SchoolContext`.

- Zum Erstellen eines Ordners im Projekt conjesouniversity klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **neuer Ordner**. Benennen Sie den neuen Ordner *dal* (für Datenzugriffs Ebene). Erstellen Sie in diesem Ordner eine neue Klassendatei mit dem Namen *SchoolContext.cs*, und ersetzen Sie den Vorlagen Code durch den folgenden Code:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specify-entity-sets"></a>Entitätenmengen angeben

Mit diesem Code wird eine [dbset](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.113).aspx) -Eigenschaft für jede Entitätenmenge erstellt. In Entity Framework-Terminologie entspricht eine *Entitätenmenge* in der Regel einer Datenbanktabelle, und eine *Entität* entspricht einer Zeile in der Tabelle.

> [!NOTE]
>
> Sie können die Anweisungen `DbSet<Enrollment>` und `DbSet<Course>` weglassen und funktionieren gleich. Entity Framework würden Sie implizit einschließen, da die Entität `Student` auf die `Enrollment` Entität verweist und die `Enrollment` Entität auf die `Course` Entität verweist.

### <a name="specify-the-connection-string"></a>Angeben der Verbindungs Zeichenfolge

Der Name der Verbindungs Zeichenfolge (die Sie später der Datei "Web. config" hinzufügen) wird an den-Konstruktor übergeben.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Sie können auch die Verbindungs Zeichenfolge selbst übergeben, anstelle des Namens eines in der Datei Web. config gespeicherten namens. Weitere Informationen zu den Optionen zum Angeben der zu verwendenden Datenbank finden Sie unter Verbindungs Zeichenfolgen [und Modelle](/ef/ef6/fundamentals/configuring/connection-strings).

Wenn Sie keine Verbindungs Zeichenfolge oder den Namen explizit angeben, wird Entity Framework angenommen, dass der Name der Verbindungs Zeichenfolge mit dem Klassennamen identisch ist. Der Standardname der Verbindungs Zeichenfolge in diesem Beispiel wäre dann `SchoolContext`, und zwar genau so, wie Sie dies explizit angeben.

### <a name="specify-singular-table-names"></a>Angeben von Namen für die Singular Tabelle

Die `modelBuilder.Conventions.Remove`-Anweisung in der [onmodelcreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) -Methode verhindert, dass Tabellennamen pluralisiert werden. Wenn Sie dies nicht getan haben, werden die generierten Tabellen in der Datenbank `Students`, `Courses`und `Enrollments`benannt. Stattdessen werden die Tabellennamen `Student`, `Course`und `Enrollment`. Entwickler sind sich uneinig darüber, ob Tabellennamen im Plural stehen sollten oder nicht. In diesem Tutorial wird das Singular-Formular verwendet, aber es ist wichtig, dass Sie das gewünschte Formular auswählen können, indem Sie diese Codezeile einschließen oder weglassen.

## <a name="initialize-db-with-test-data"></a>Initialisieren Sie die Datenbank mit Testdaten

Entity Framework können automatisch eine Datenbank erstellen (oder löschen und neu erstellen), wenn die Anwendung ausgeführt wird. Sie können angeben, dass dies jedes Mal erfolgen soll, wenn die Anwendung ausgeführt wird, oder nur, wenn das Modell nicht mit der vorhandenen Datenbank synchronisiert ist. Sie können auch eine `Seed` Methode schreiben, die nach dem Erstellen der Datenbank automatisch aufruft Entity Framework, um diese mit Testdaten aufzufüllen.

Das Standardverhalten besteht darin, eine Datenbank nur dann zu erstellen, wenn Sie nicht vorhanden ist (und eine Ausnahme auslöst, wenn sich das Modell geändert hat und die Datenbank bereits vorhanden ist). In diesem Abschnitt geben Sie an, dass die Datenbank immer dann gelöscht und neu erstellt werden soll, wenn das Modell geändert wird. Das Löschen der Datenbank führt zum Verlust all Ihrer Daten. Dies ist im Allgemeinen bei der Entwicklung von Ordnung, da die `Seed`-Methode bei der erneuten Erstellung der Datenbank ausgeführt wird und die Testdaten neu erstellt. In der Produktionsumgebung möchten Sie jedoch im Allgemeinen nicht alle Ihre Daten verlieren, wenn Sie das Datenbankschema ändern müssen. Später erfahren Sie, wie Sie Modelländerungen mithilfe Code First-Migrationen bearbeiten, um das Datenbankschema zu ändern, anstatt die Datenbank zu löschen und neu zu erstellen.

1. Erstellen Sie im Ordner dal eine neue Klassendatei mit dem Namen *SchoolInitializer.cs* , und ersetzen Sie den Vorlagen Code durch den folgenden Code, der bewirkt, dass eine Datenbank bei Bedarf erstellt wird und Testdaten in die neue Datenbank geladen werden.

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

   Die `Seed`-Methode nimmt das Daten Bank Kontext Objekt als Eingabeparameter an, und der Code in der-Methode verwendet dieses Objekt, um der Datenbank neue Entitäten hinzuzufügen. Für jeden Entitätstyp erstellt der Code eine Auflistung von neuen Entitäten, fügt Sie der entsprechenden `DbSet`-Eigenschaft hinzu und speichert die Änderungen dann in der Datenbank. Es ist nicht erforderlich, die `SaveChanges`-Methode nach jeder entitätenentität wie hier beschrieben aufzurufen. Dadurch können Sie jedoch die Ursache eines Problems finden, wenn eine Ausnahme auftritt, während der Code in die Datenbank schreibt.

2. Wenn Sie Entity Framework möchten, dass die Initialisiererklasse verwendet werden soll, fügen Sie dem `entityFramework`-Element in der *Web. config* -Datei der Anwendung (die im Stamm Projektordner) ein-Element hinzu, wie im folgenden Beispiel gezeigt:

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

   Der `context type` gibt den voll qualifizierten Kontext Klassennamen und die Assembly an, in der er sich befindet, und die `databaseinitializer type` gibt den voll qualifizierten Namen der Initialisiererklasse und die Assembly an, in der Sie sich befindet. (Wenn Sie nicht möchten, dass EF den Initialisierer verwendet, können Sie ein Attribut für das `context` Element festlegen: `disableDatabaseInitialization="true"`.) Weitere Informationen finden Sie unter [Einstellungen für die Konfigurationsdatei](/ef/ef6/fundamentals/configuring/config-file).

   Eine Alternative zum Festlegen des Initialisierers in der Datei *Web. config* besteht darin, den Initialisierer in Code zu verwenden, indem der `Application_Start`-Methode in der *Global.asax.cs* -Datei eine `Database.SetInitializer`-Anweisung hinzugefügt wird. Weitere Informationen finden Sie Untergrund Legendes zu [datenbankinitialisierern in Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

Die Anwendung ist nun so eingerichtet, dass beim ersten Zugriff auf die Datenbank in einer bestimmten Anwendungs Anwendung Entity Framework die Datenbank mit dem Modell (ihrer `SchoolContext`-und Entitäts Klassen) vergleicht. Wenn es einen Unterschied gibt, löscht die Anwendung die Datenbank und erstellt sie neu.

> [!NOTE]
> Wenn Sie eine Anwendung auf einem produktionsweb Server bereitstellen, müssen Sie Code entfernen oder deaktivieren, der die Datenbank löscht und neu erstellt. Dies wird in einem späteren Tutorial dieser Reihe durchführen.

## <a name="set-up-ef-6-to-use-localdb"></a>Einrichten von EF 6 für die Verwendung von localdb

[Localdb](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017) ist eine vereinfachte Version der SQL Server Express Datenbank-Engine. Es ist einfach zu installieren und zu konfigurieren, bei Bedarf gestartet und im Benutzermodus ausgeführt. Localdb wird in einem speziellen Ausführungs Modus von SQL Server Express ausgeführt, der es Ihnen ermöglicht, mit Datenbanken als *MDF* -Dateien zu arbeiten. Sie können localdb-Datenbankdateien im *App-\_Daten* Ordner eines Webprojekts ablegen, wenn Sie in der Lage sein möchten, die Datenbank mit dem Projekt zu kopieren. Mit der benutzerinstanzfunktion in SQL Server Express können Sie auch mit *MDF* -Dateien arbeiten, aber die benutzerinstanzfunktion ist veraltet. Daher wird localdb zum Arbeiten mit *MDF* -Dateien empfohlen. Localdb wird standardmäßig mit Visual Studio installiert.

In der Regel wird SQL Server Express nicht für produktionsweb Anwendungen verwendet. Localdb wird insbesondere für den Einsatz in der Produktion mit einer Webanwendung nicht empfohlen, da es nicht für die Verwendung mit IIS konzipiert ist.

- In diesem Tutorial arbeiten Sie mit localdb. Öffnen Sie die Datei " *Web. config* " der Anwendung, und fügen Sie ein `connectionStrings` Element vor dem `appSettings`-Element hinzu, wie im folgenden Beispiel gezeigt. (Stellen Sie sicher, dass Sie die Datei " *Web. config* " im Stamm Projektordner aktualisieren. Es gibt auch eine *Web. config* -Datei im Unterordner *views* , die Sie nicht aktualisieren müssen.)

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Die Verbindungs Zeichenfolge, die Sie hinzugefügt haben, gibt an, dass Entity Framework eine localdb-Datenbank namens *ContosoUniversity1. mdf*verwendet. (Die Datenbank ist noch nicht vorhanden, Sie wird jedoch von EF erstellt.) Wenn Sie die Datenbank in Ihrem *App-\_Daten* Ordner erstellen möchten, können Sie der Verbindungs Zeichenfolge `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` hinzufügen. Weitere Informationen zu Verbindungs Zeichenfolgen finden Sie unter SQL Server Verbindungs Zeichenfolgen [für ASP.NET-Webanwendungen](/previous-versions/aspnet/jj653752(v=vs.110)).

Sie benötigen keine Verbindungs Zeichenfolge in der Datei " *Web. config* ". Wenn Sie keine Verbindungs Zeichenfolge angeben, verwendet Entity Framework eine Standard Verbindungs Zeichenfolge, die auf der Kontext Klasse basiert. Weitere Informationen finden Sie unter [Code First zu einer neuen Datenbank](/ef/ef6/modeling/code-first/workflows/new-database).

## <a name="create-controller-and-views"></a>Erstellen Sie Controller und Ansichten

Nun erstellen Sie eine Webseite, auf der Daten angezeigt werden. Durch den Prozess der Daten Anforderung wird die Erstellung der Datenbank automatisch ausgelöst. Zunächst erstellen Sie einen neuen Controller. Aber bevor Sie dies tun, erstellen Sie das Projekt, um die Modell-und Kontext Klassen für das MVC-Controller Gerüst verfügbar zu machen.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Controller** , wählen Sie **Hinzufügen**aus, und klicken Sie dann auf **Neues Gerüst Element**.
2. Wählen Sie im Dialogfeld **Gerüst hinzufügen** die Option **MVC 5-Controller mit Ansichten, verwenden Sie Entity Framework**, und wählen Sie dann **Hinzufügen**aus.

     ![Dialogfeld "Gerüst hinzufügen" in Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. Nehmen Sie im Dialogfeld **Controller hinzufügen** die folgenden Optionen vor, und wählen Sie dann **Hinzufügen**aus:

   - Modell Klasse: **Student (condesouniversity. Models)** . (Wenn diese Option nicht in der Dropdown Liste angezeigt wird, erstellen Sie das Projekt, und wiederholen Sie den Vorgang.)
   - Datenkontext Klasse: **schoolContext (condesouniversity. DAL)** .
   - Controller Name: **studentcontroller** (nicht studentscontroller).
   - Überlassen Sie die Standardwerte für die anderen Felder.

     Wenn Sie auf **Hinzufügen**klicken, erstellt das Gerüst eine *StudentController.cs* -Datei und einen Satz von Sichten (*cshtml* -Dateien), die mit dem Controller zusammenarbeiten. Wenn Sie Projekte erstellen, die Entity Framework verwenden, können Sie in Zukunft auch einige zusätzliche Funktionen des gerüsters nutzen: Erstellen Sie die erste Modell Klasse, erstellen Sie keine Verbindungs Zeichenfolge, und geben Sie dann im Feld **Controller hinzufügen** einen **neuen Datenkontext** an, indem Sie die Schaltfläche **+** neben **Datenkontext Klasse**auswählen. Das Gerüst Fenster erstellt sowohl die `DbContext` Klasse als auch die Verbindungs Zeichenfolge sowie den Controller und die Ansichten.
4. Visual Studio öffnet die Datei " *controllers\studentcontroller.cs* ". Sie sehen, dass eine Klassen Variable erstellt wurde, die ein Daten Bank Kontext Objekt instanziiert:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     Die `Index` Aktionsmethode Ruft eine Liste von Schülern aus der Entitätenmenge " *Students* " ab, indem die `Students`-Eigenschaft der Daten Bank Kontext Instanz gelesen wird:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     In der Ansicht *student\index.cshtml* wird diese Liste in einer Tabelle angezeigt:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Drücken Sie STRG+F5, um das Projekt auszuführen. (Wenn der Fehler "Schatten Kopie kann nicht erstellt werden" angezeigt wird, schließen Sie den Browser, und wiederholen Sie den Vorgang.)

     Klicken Sie auf die Registerkarte **Studenten** , um die Testdaten anzuzeigen, die von der `Seed` Methode eingefügt wurden. Je nachdem, wie eng das Browserfenster ist, wird der Link "Student Registerkarte" in der oberen Adressleiste angezeigt, oder Sie müssen auf die obere rechte Ecke klicken, um den Link anzuzeigen.

     ![Menü Schaltfläche](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

## <a name="view-the-database"></a>Zeigen Sie die Datenbank an

Wenn Sie die Seite "Studenten" ausgeführt haben und die Anwendung versucht hat, auf die Datenbank zuzugreifen, stellte EF fest, dass keine Datenbank vorhanden war und erstellt wurde. EF hat dann die Seed-Methode ausgeführt, um die Datenbank mit Daten aufzufüllen.

Sie können entweder **Server-Explorer** oder **SQL Server-Objekt-Explorer** (ssox) verwenden, um die Datenbank in Visual Studio anzuzeigen. In diesem Tutorial verwenden Sie **Server-Explorer**.

1. Schließen Sie den Browser.
2. Erweitern Sie in **Server-Explorer**den Knoten **Datenverbindungen** (Sie müssen möglicherweise zuerst die Schaltfläche "Aktualisieren" auswählen), den Eintrag " **School Context" (contosouniversity)** und dann " **Tables** ", um die Tabellen in der neuen Datenbank anzuzeigen.

3. Klicken Sie mit der rechten Maustaste auf die Tabelle **Student** , und klicken Sie auf **Tabellendaten anzeigen** , um die erstellten Spalten und die in die Tabelle eingefügten Zeilen anzuzeigen.

4. Schließen Sie die **Server-Explorer** Verbindung.

Die Datenbankdateien *ContosoUniversity1. mdf* und *. ldf* befinden sich im Ordner *% User Profile%* .

Da Sie den `DropCreateDatabaseIfModelChanges` Initialisierer verwenden, können Sie jetzt eine Änderung an der `Student` Klasse vornehmen, die Anwendung erneut ausführen, und die Datenbank wird automatisch neu erstellt, damit Sie ihrer Änderung entspricht. Wenn Sie z. b. der `Student` Klasse eine `EmailAddress`-Eigenschaft hinzufügen, die Seite "Students" erneut ausführen und dann die Tabelle erneut anzeigen, wird eine neue `EmailAddress` Spalte angezeigt.

## <a name="conventions"></a>Konventionen

Die Menge an Code, den Sie schreiben müssen, damit Entity Framework eine komplette Datenbank erstellen können, ist aufgrund von *Konventionen*oder Annahmen, die Entity Framework vornimmt, minimal. Einige von Ihnen wurden bereits notiert oder verwendet, ohne dass Sie Sie kennen:

- Die pluralisierten Formen von Entitäts Klassennamen werden als Tabellennamen verwendet.
- Eigenschaftennamen von Entitäten werden als Spaltennamen verwendet.
- Entitäts Eigenschaften, die `ID` oder *className* `ID` benannt werden, werden als Primärschlüssel Eigenschaften erkannt.
- Eine Eigenschaft wird als Fremdschlüssel Eigenschaft interpretiert, wenn Sie *&lt;Name der Navigations Eigenschaft&gt;&lt;Name der Primärschlüssel Eigenschaft&gt;* (z. b. `StudentID` für die Navigations Eigenschaft `Student`, da der Primärschlüssel der `Student`-Entität `ID`ist). Fremdschlüssel Eigenschaften können auch einfach &lt;Namen der Primärschlüssel Eigenschaft&gt; benannt werden (z. b. `EnrollmentID`, weil der Primärschlüssel der `Enrollment` Entität `EnrollmentID`ist).

Sie haben gesehen, dass Konventionen überschrieben werden können. Sie haben z. b. angegeben, dass Tabellennamen nicht pluralisiert werden sollen, und später sehen Sie, wie eine Eigenschaft explizit als Fremdschlüssel Eigenschaft markiert wird.

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen zu EF 6 finden Sie in den folgenden Artikeln:

* [ASP.NET: Datenzugriff – Empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md)

* [Code First Konventionen](/ef/ef6/modeling/code-first/conventions/built-in)

* [Erstellen eines komplexeren Datenmodells](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erstellt eine MVC-Web-App.
> * Einrichten des Websitestils
> * Installiert Entity Framework 6
> * Haben Sie das Datenmodell erstellt
> * Haben Sie den Datenbankkontext erstellt
> * Haben Sie die Datenbank mit Testdaten initialisiert
> * Einrichten von EF 6 für die Verwendung von localdb
> * Haben Sie Controller und Ansichten erstellt
> * Haben Sie die Datenbank angezeigt

Im nächsten Artikel erfahren Sie, wie Sie den Code zum Erstellen, lesen, aktualisieren und löschen (CRUD) in ihren Controllern und Ansichten überprüfen und anpassen.
> [!div class="nextstepaction"]
> [Implementieren von grundlegenden CRUD-Funktionen](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)