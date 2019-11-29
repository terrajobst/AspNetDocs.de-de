---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Abrufen und Anzeigen von Daten mit Modell Bindung und Web Forms | Microsoft-Dokumentation
author: Rick-Anderson
description: In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung führt zu einer geraden Daten Interaktion-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 81cca22cb4752d071d2a68986ae9ac2bed737594
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633175"
---
# <a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Abrufen und Anzeigen von Daten mit Modell Bindung und Web Forms

> In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung sorgt für eine genauere Daten Interaktion als bei der Verarbeitung von Datenquellen Objekten (z. b. ObjectDataSource oder SqlDataSource). Diese Serie beginnt mit Einführungs Material und wechselt in spätere Tutorials zu erweiterten Konzepten.
> 
> Das Modell bindungsmuster funktioniert mit jeder Datenzugriffs Technologie. In diesem Tutorial verwenden Sie Entity Framework, aber Sie können die Datenzugriffs Technologie verwenden, die Ihnen am meisten vertraut ist. Über ein Daten gebundenes Server Steuerelement, z. b. ein GridView-, ListView-, DetailsView-oder FormView-Steuerelement, geben Sie die Namen der Methoden an, die zum auswählen, aktualisieren, löschen und Erstellen von Daten verwendet werden sollen. In diesem Tutorial geben Sie einen Wert für die SelectMethod-Methode an. 
> 
> Innerhalb dieser Methode stellen Sie die Logik zum Abrufen der Daten bereit. Im nächsten Tutorial legen Sie Werte für UpdateMethod, DeleteMethod und InsertMethod fest.
>
> Sie können das gesamte Projekt in C# oder Visual Basic [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) . Der herunterladbare Code funktioniert mit Visual Studio 2012 und höher. Dabei wird die Vorlage Visual Studio 2012 verwendet, die sich geringfügig von der in diesem Tutorial gezeigten Vorlage in Visual Studio 2017 unterscheidet.
> 
> In diesem Tutorial führen Sie die Anwendung in Visual Studio aus. Sie können die Anwendung auch für einen Hostinganbieter bereitstellen und über das Internet verfügbar machen. Microsoft bietet kostenloses Webhosting für bis zu 10 Websites in einer  
> [Kostenloses Azure-Testkonto](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Informationen zum Bereitstellen eines Visual Studio-Webprojekts für die Azure App Service von Web-Apps finden Sie unter [ASP.net Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) Series. In diesem Tutorial erfahren Sie außerdem, wie Sie Entity Framework Code First-Migrationen zum Bereitstellen Ihrer SQL Server-Datenbank in Azure SQL-Datenbank verwenden.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> - Microsoft Visual Studio 2017 oder Microsoft Visual Studio Community 2017
>   
> Dieses Tutorial funktioniert auch mit Visual Studio 2012 und Visual Studio 2013. es gibt jedoch einige Unterschiede in der Benutzeroberfläche und der Projektvorlage.

## <a name="what-youll-build"></a>Was Sie erstellen

In diesem Tutorial gehen Sie wie folgt vor:

* Erstellen von Datenobjekten, die eine Universität mit Studenten widerspiegeln, die in Kursen angemeldet sind
* Erstellen von Datenbanktabellen aus den Objekten
* Auffüllen der Datenbank mit Testdaten
* Anzeigen von Daten in einem Webformular

## <a name="create-the-project"></a>Erstellen eines Projekts

1. Erstellen Sie in Visual Studio 2017 ein Projekt mit dem Namen " **contosouniversitymodelbinding**" der **ASP.NET-Webanwendung (.NET Framework)** .

   ![Projekt erstellen](retrieving-data/_static/image19.png)

2. Klicken Sie auf **OK**. Das Dialogfeld zum Auswählen einer Vorlage wird angezeigt.

   ![Auswählen von Web Forms](retrieving-data/_static/image3.png)

3. Wählen Sie die **Web Forms** Vorlage aus. 

4. Ändern Sie ggf. die Authentifizierung in **einzelne Benutzerkonten**. 

5. Klicken Sie auf **OK**, um das Projekt zu erstellen.

## <a name="modify-site-appearance"></a>Website Darstellung ändern

   Nehmen Sie einige Änderungen vor, um das Erscheinungsbild der Website anzupassen. 
   
   1. Öffnen Sie die Datei Site. Master.
   
   2. Ändern Sie den Titel, um die "The **University** " und nicht die **ASP.NET-Anwendung**anzuzeigen.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. Ändern Sie den Header Text vom **Anwendungsnamen** in die **Datei**"Configuration Manager".

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. Ändern Sie die Navigations Header Links zu den entsprechenden Websites. 
   
      Entfernen Sie die Links für " **about** " und " **Contact** ", und verknüpfen Sie die Seite mit der Seite " **Studenten** ", die Sie erstellen möchten.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. Speichern Sie Site. Master.

## <a name="add-a-web-form-to-display-student-data"></a>Hinzufügen eines Webformulars zum Anzeigen von Studenten Daten

   1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen** und dann **Neues Element**aus. 
   
   2. Wählen Sie im Dialogfeld **Neues Element hinzufügen** das **Webformular mit der Vorlage Master Seite** aus, und nennen Sie es **students. aspx**.

      ![Seite erstellen](retrieving-data/_static/image5.png)

   3. Wählen Sie **Hinzufügen** aus.
   
   4. Wählen Sie für die Master Seite des Webformulars die Option **Site. Master**aus.
   
   5. Klicken Sie auf **OK**.

## <a name="add-the-data-model"></a>Hinzufügen des Datenmodells

Fügen Sie im Ordner **Models** eine Klasse mit dem Namen **UniversityModels.cs**hinzu.

   1. Klicken Sie mit der rechten Maustaste auf **Modelle**, und wählen Sie **Hinzufügen**und **Neues Element**aus. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.

   2. Klicken Sie im linken Navigationsmenü auf **Code**und dann auf **Klasse**.

      ![Modell Klasse erstellen](retrieving-data/_static/image20.png)

   3. Benennen Sie die Klasse **UniversityModels.cs** , und wählen Sie **Hinzufügen**.

      Definieren Sie in dieser Datei die Klassen `SchoolContext`, `Student`, `Enrollment`und `Course` wie folgt:

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      Die `SchoolContext`-Klasse wird von `DbContext`abgeleitet, die die Datenbankverbindung und die Änderungen an den Daten verwaltet.

      Beachten Sie in der `Student`-Klasse die Attribute, die auf die Eigenschaften `FirstName`, `LastName`und `Year` angewendet werden. In diesem Tutorial werden diese Attribute für die Datenüberprüfung verwendet. Um den Code zu vereinfachen, werden nur diese Eigenschaften mit Daten Validierungs Attributen gekennzeichnet. In einem echten Projekt würden Sie Validierungs Attribute auf alle Eigenschaften anwenden, die überprüft werden müssen.

   4. Speichern Sie UniversityModels.cs.

## <a name="set-up-the-database-based-on-classes"></a>Einrichten der Datenbank auf der Grundlage von Klassen

In diesem Tutorial werden [Code First-Migrationen](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) zum Erstellen von Objekten und Datenbanktabellen verwendet. In diesen Tabellen werden Informationen zu den Studenten und deren Kursen gespeichert.

   1. Wählen **Sie** Extras > **nuget-Paket-Manager** > Paket-Manager- **Konsole**aus.

   2. Führen Sie in der **Paket-Manager-Konsole**den folgenden Befehl aus:  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      Wenn der Befehl erfolgreich abgeschlossen wurde, wird eine Meldung angezeigt, die besagt, dass Migrationen aktiviert wurden.

      ![Migrationen aktivieren](retrieving-data/_static/image8.png)

      Beachten Sie, dass eine Datei mit dem Namen *Configuration.cs* erstellt wurde. Die `Configuration`-Klasse verfügt über eine `Seed`-Methode, mit der die Datenbanktabellen mit Testdaten vorab aufgefüllt werden können.

## <a name="pre-populate-the-database"></a>Vorab Auffüllen der Datenbank

   1. Öffnen Sie Configuration.cs.
   
   2. Fügen Sie der `Seed` -Methode folgenden Code hinzu. Fügen Sie außerdem eine `using`-Anweisung für den `ContosoUniversityModelBinding. Models`-Namespace hinzu.

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. Speichern Sie Configuration.cs.

   4. Führen Sie in der Paket-Manager-Konsole den Befehl **Add-Migration Initial**aus.

   5. Führen Sie den Befehl **Update-Database**aus.

      Wenn Sie beim Ausführen dieses Befehls eine Ausnahme erhalten, können sich die Werte für `StudentID` und `CourseID` von den Werten der `Seed`-Methode unterscheiden. Öffnen Sie diese Datenbanktabellen, und suchen Sie nach vorhandenen Werten für `StudentID` und `CourseID`. Fügen Sie diese Werte dem Code für das Seeding der `Enrollments` Tabelle hinzu.

## <a name="add-a-gridview-control"></a>Hinzufügen eines GridView-Steuer Elements

Mit aufgefüllten Datenbankdaten sind Sie nun bereit, diese Daten abzurufen und anzuzeigen. 

1. Öffnen Sie students. aspx.

2. Suchen Sie den `MainContent` Platzhalter. Fügen Sie innerhalb dieses Platzhalters ein **GridView** -Steuerelement hinzu, das diesen Code enthält.

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   Beachten Sie Folgendes:
   * Beachten Sie den Wert, der für die `SelectMethod`-Eigenschaft im GridView-Element festgelegt wurde. Dieser Wert gibt die Methode an, die zum Abrufen von GridView-Daten verwendet wird, die Sie im nächsten Schritt erstellen. 
   
   * Die `ItemType`-Eigenschaft wird auf die zuvor erstellte `Student` Klasse festgelegt. Diese Einstellung ermöglicht es Ihnen, auf Klasseneigenschaften im Markup zu verweisen. Die `Student`-Klasse verfügt beispielsweise über eine Auflistung mit dem Namen `Enrollments`. Sie können `Item.Enrollments` verwenden, um diese Auflistung abzurufen, und dann die [LINQ-Syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) verwenden, um die registrierten Gutschriften des Studenten abzurufen.
   
3. Speichern Sie students. aspx.

## <a name="add-code-to-retrieve-data"></a>Hinzufügen von Code zum Abrufen von Daten

   Fügen Sie in der Code Behind-Datei students. aspx die für den `SelectMethod` Wert angegebene Methode hinzu. 
   
   1. Öffnen Sie students.aspx.cs.
   
   2. Fügen Sie `using`-Anweisungen für die `ContosoUniversityModelBinding. Models`-und `System.Data.Entity`-Namespaces hinzu.

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. Fügen Sie die für `SelectMethod`angegebene Methode hinzu:

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      Die `Include`-Klausel verbessert die Abfrageleistung, ist aber nicht erforderlich. Ohne die `Include`-Klausel werden die Daten mithilfe [*Lazy Loading*](https://en.wikipedia.org/wiki/Lazy_loading)abgerufen. Dies umfasst das Senden einer separaten Abfrage an die Datenbank, wenn verknüpfte Daten abgerufen werden. Mit der `Include`-Klausel werden Daten mithilfe *Eager Loading*abgerufen, was bedeutet, dass eine einzelne Datenbankabfrage alle zugehörigen Daten abruft. Wenn verwandte Daten nicht verwendet werden, ist Eager Loading weniger effizient, da mehr Daten abgerufen werden. In diesem Fall bietet Eager Loading jedoch die beste Leistung, da die zugehörigen Daten für jeden Datensatz angezeigt werden.

      Weitere Informationen zu Leistungs Überlegungen beim Laden von verknüpften Daten finden Sie im Artikel **Lazy, eifrig und Explizites Laden verwandter Daten** im Artikel [Lesen verwandter Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) .

      Standardmäßig werden die Daten nach den Werten der als Schlüssel markierten Eigenschaft sortiert. Sie können eine `OrderBy`-Klausel hinzufügen, um einen anderen Sortier Wert anzugeben. In diesem Beispiel wird die standardmäßige `StudentID`-Eigenschaft zum Sortieren verwendet. Im Artikel [Sortieren, Paging und Filtern von Daten](sorting-paging-and-filtering-data.md) ist der Benutzer in der Lage, eine Spalte für die Sortierung auszuwählen.
 
   4. Speichern Sie students.aspx.cs.

## <a name="run-your-application"></a>Ausführen der Anwendung 

Führen Sie Ihre Webanwendung (**F5**) aus, und navigieren Sie zur Seite " **Studenten** ", auf der Folgendes angezeigt wird:

   ![Daten anzeigen](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Automatische Generierung von Modell Bindungsmethoden

Wenn Sie diese tutorialreihe durcharbeiten, können Sie einfach den Code aus dem Tutorial in Ihr Projekt kopieren. Ein Nachteil dieses Ansatzes ist jedoch, dass Sie die von Visual Studio bereitgestellte Funktion möglicherweise nicht kennen, um automatisch Code für Modell Bindungsmethoden zu generieren. Wenn Sie an Ihren eigenen Projekten arbeiten, können Sie mithilfe der automatischen Codegenerierung Zeit sparen und einen Eindruck davon gewinnen, wie ein Vorgang implementiert wird. In diesem Abschnitt wird das Feature zur automatischen Codegenerierung beschrieben. Dieser Abschnitt dient nur zu Informationszwecken und enthält keinen Code, den Sie in Ihrem Projekt implementieren müssen. 

Wenn Sie einen Wert für die Eigenschaften `SelectMethod`, `UpdateMethod`, `InsertMethod`oder `DeleteMethod` im Markup Code festlegen, können Sie die Option **neue Methode erstellen** auswählen.

![Erstellen einer Methode](retrieving-data/_static/image18.png)

Visual Studio erstellt nicht nur eine Methode im Code-Behind mit der richtigen Signatur, sondern generiert auch Implementierungs Code, um den Vorgang auszuführen. Wenn Sie zuerst die `ItemType`-Eigenschaft festlegen, bevor Sie die Funktion zur automatischen Codegenerierung verwenden, verwendet der generierte Code diesen Typ für die Vorgänge. Wenn Sie z. b. die `UpdateMethod`-Eigenschaft festlegen, wird der folgende Code automatisch generiert:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Dieser Code muss dem Projekt nicht hinzugefügt werden. Im nächsten Tutorial implementieren Sie Methoden zum Aktualisieren, löschen und Hinzufügen neuer Daten.

## <a name="summary"></a>Summary

In diesem Tutorial haben Sie Datenmodell Klassen erstellt und eine Datenbank aus diesen Klassen generiert. Sie haben die Datenbanktabellen mit Testdaten ausgefüllt. Sie haben die Modell Bindung verwendet, um Daten aus der Datenbank abzurufen, und dann die Daten in einer GridView angezeigt.

Im nächsten [Tutorial](updating-deleting-and-creating-data.md) dieser Reihe aktivieren Sie das Aktualisieren, löschen und Erstellen von Daten.

> [!div class="step-by-step"]
> [Nächste](updating-deleting-and-creating-data.md)
