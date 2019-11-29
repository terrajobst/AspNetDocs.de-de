---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementieren der Vererbung mit dem Entity Framework in einer ASP.NET MVC-Anwendung (8 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio erstellt werden...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9507cba71b976825257cc9948d54f2651355959d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595321"
---
# <a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Implementieren der Vererbung mit dem Entity Framework in einer ASP.NET MVC-Anwendung (8 von 10)

von [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio 2012 erstellt werden. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe von Anfang an starten oder [ein Starter-Projekt für dieses Kapitel herunterladen](building-the-ef5-mvc4-chapter-downloads.md) und hier beginnen.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem stoßen, das Sie nicht beheben können, [Laden Sie das abgeschlossene Kapitel herunter](building-the-ef5-mvc4-chapter-downloads.md) , und versuchen Sie, das Problem zu reproduzieren. Im Allgemeinen können Sie die Lösung für das Problem finden, indem Sie Ihren Code mit dem abgeschlossenen Code vergleichen. Informationen zu häufigen Fehlern und deren Lösung finden Sie unter [Fehler und](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors) Problem Umgehungen.

Im vorherigen Tutorial haben Sie Parallelitäts Ausnahmen behandelt. In diesem Tutorial erfahren Sie, wie Sie die Vererbung in das Datenmodell implementieren können.

Bei der objektorientierten Programmierung können Sie die Vererbung verwenden, um redundanten Code auszuschließen. In diesem Tutorial ändern Sie die Klassen `Instructor` und `Student` so, dass sie von einer `Person`-Basisklasse abgeleitet werden, die Eigenschaften wie `LastName` enthält. Diese Eigenschaften sind für Dozenten und Studenten gängig. Sie fügen keine Webseiten hinzu oder ändern diese, aber Sie werden Teile des Codes ändern. Diese Änderungen werden automatisch in der Datenbank widergespiegelt.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>"Tabelle pro Hierarchie" und "Tabelle pro Typ"-Vererbung

Bei der objektorientierten Programmierung können Sie die Vererbung verwenden, um die Arbeit mit verknüpften Klassen zu vereinfachen. Beispielsweise verwenden die Klassen `Instructor` und `Student` im `School` Datenmodell mehrere Eigenschaften, die zu redundantem Code führen:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Angenommen, Sie möchten den redundanten Code für die Eigenschaften löschen, die von den Entitäten `Instructor` und `Student` gemeinsam genutzt werden. Sie könnten eine `Person` Basisklasse erstellen, die nur die freigegebenen Eigenschaften enthält, und dann die `Instructor` und `Student` Entitäten von dieser Basisklasse erben, wie in der folgenden Abbildung dargestellt:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Es gibt mehrere Möglichkeiten, wie diese Vererbungsstruktur in der Datenbank dargestellt werden kann. Sie können über eine `Person` Tabelle verfügen, die Informationen zu den Studenten und Dozenten in einer einzelnen Tabelle enthält. Einige der Spalten können nur für Dozenten (`HireDate`) gelten, einige nur für Schüler/Studenten (`EnrollmentDate`), einige für beides (`LastName`, `FirstName`). Normalerweise verfügen Sie über eine *diskriminatorspalte* , um anzugeben, welcher Typ von den einzelnen Zeilen repräsentiert wird. So kann die Unterscheidungsspalte beispielsweise „Instructor“ für Dozenten und „Student“ für Studenten enthalten.

![Tabelle pro hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Dieses Muster zum Erstellen einer Entitäts Vererbungs Struktur aus einer einzelnen Datenbanktabelle wird als " *Tabelle pro Hierarchie* "-Vererbung (TPH) bezeichnet.

Alternativ kann die Datenbank so gestaltet werden, dass sie mehr wie die Vererbungsstruktur aussieht. Beispielsweise können Sie nur die namens Felder in der `Person` Tabelle und separate `Instructor` und `Student` Tabellen mit den Datumsfeldern haben.

![Tabelle pro type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Dieses Muster für die Erstellung einer Datenbanktabelle für jede Entitäts Klasse wird als " *Tabelle pro Typ* "-Vererbung (TPT) bezeichnet.

TPH-Vererbungsmuster bieten im Allgemeinen eine bessere Leistung in den Entity Framework als TPT-Vererbungsmuster, da TPT-Muster zu komplexen Verknüpfungs Abfragen führen können. Dieses Tutorial veranschaulicht die Implementierung der TPH-Vererbung. Führen Sie dazu die folgenden Schritte aus:

- Erstellen Sie eine `Person`-Klasse, und ändern Sie die Klassen `Instructor` und `Student`, sodass Sie von `Person`abgeleitet werden.
- Fügen Sie der Daten Bank Kontext Klasse Modell-zu-Datenbank-Zuordnungscode hinzu.
- Ändern Sie `InstructorID` und `StudentID` Verweise im gesamten Projekt auf `PersonID`.

## <a name="creating-the-person-class"></a>Erstellen der Person-Klasse

 Hinweis: Sie können das Projekt nach dem Erstellen der nachfolgenden Klassen nicht kompilieren, bis Sie die Controller aktualisieren, von denen diese Klassen verwendet werden. 

Erstellen Sie im Ordner *Models* *Person.cs* , und ersetzen Sie den Vorlagen Code durch den folgenden Code:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Leiten Sie in *Instructor.cs*die `Instructor`-Klasse von der `Person`-Klasse ab, und entfernen Sie die Schlüssel-und namens Felder. Der Code sieht aus wie im folgenden Beispiel:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Nehmen Sie ähnliche Änderungen an *Student.cs*vor. Die `Student`-Klasse sieht wie im folgenden Beispiel aus:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Hinzufügen des Entitäts Typs Person zum Modell

Fügen Sie in *SchoolContext.cs*eine `DbSet`-Eigenschaft für den `Person` Entitätstyp hinzu:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Das ist alles, was Entity Framework für die Konfiguration der „Tabelle pro Hierarchie“-Vererbung benötigt. Sie werden feststellen, dass bei der erneuten Erstellung der Datenbank eine `Person` Tabelle anstelle der Tabellen `Student` und `Instructor` vorhanden ist.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Ändern von "InstructorID" und "StudentID" in PersonID

Ändern Sie in *SchoolContext.cs*in der Anweisung Instructor Kurs Mapping `MapRightKey("InstructorID")` in `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Diese Änderung ist nicht erforderlich. der Name der Instructions-ID-Spalte wird lediglich in der m:n-jointabelle geändert. Wenn Sie den Namen als InstructorID belassen, funktioniert die Anwendung trotzdem ordnungsgemäß. Hier ist die abgeschlossene *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Als nächstes müssen Sie `InstructorID` in `PersonID` ändern und `StudentID`, um im gesamten Projekt zu `PersonID`, ***außer*** in den Dateien mit Zeitstempel Dateien im *Migrations* Ordner. Zu diesem Zweck können Sie nur die Dateien suchen und öffnen, die geändert werden müssen, und dann eine globale Änderung der geöffneten Dateien durchführen. Die einzige Datei im *Migrations* Ordner, die Sie ändern sollten, ist *migrations\configuration.cs.*

1. > [!IMPORTANT]
   > Schließen Sie zunächst alle geöffneten Dateien in Visual Studio.
2. Klicken Sie auf Suchen **und ersetzen--alle Dateien** im Menü **Bearbeiten** suchen, und suchen Sie dann nach allen Dateien im Projekt, die `InstructorID`enthalten.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Öffnen Sie jede Datei im Fenster Suchergebnisse ***mit Ausnahme*** des &lt;Zeitstempels&gt; *\_. cs* -Migrations Dateien im Ordner *Migrationen* , indem Sie für jede Datei auf eine Zeile doppelklicken.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Öffnen Sie das Dialogfeld **in Dateien ersetzen** , und ändern Sie **Suchen in** **alle geöffneten Dokumente**.
5. Verwenden Sie das Dialogfeld **in Dateien ersetzen** , um alle `InstructorID` in zu ändern `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Suchen Sie alle Dateien im Projekt, die `StudentID`enthalten.
7. Öffnen Sie jede Datei im Fenster Suchergebnisse ***mit Ausnahme*** des &lt;Zeitstempels&gt; *\_\*. cs* -Migrations Dateien im Ordner *Migrationen* , indem Sie für jede Datei auf eine Zeile doppelklicken.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Öffnen Sie das Dialogfeld **in Dateien ersetzen** , und ändern Sie **Suchen in** **alle geöffneten Dokumente**.
9. Verwenden Sie das Dialogfeld **in Dateien ersetzen** , um alle `StudentID` in `PersonID`zu ändern.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Erstellen Sie das Projekt.

(Beachten Sie, dass dies den *Nachteil* des `classnameID` Musters zum Benennen von primär Schlüsseln veranschaulicht. Wenn Sie den Namen Primary Keys ID haben, ohne den Klassennamen vorzugeben, wäre jetzt *keine* Umbenennung erforderlich.)

## <a name="create-and-update-a-migrations-file"></a>Erstellen und Aktualisieren einer Migrations Datei

Geben Sie in der Paket-Manager-Konsole (PMC) den folgenden Befehl ein:

`Add-Migration Inheritance`

Führen Sie den `Update-Database` Befehl in der PMC aus. Der Befehl schlägt an dieser Stelle fehl, da wir über vorhandene Daten verfügen, die Migrationen nicht verarbeiten können. Sie erhalten die folgende Fehlermeldung:

*Die ALTER TABLE-Anweisung steht in Konflikt mit der FOREIGN KEY-Einschränkung "FK\_dbo. Abteilung\_dbo. Person\_PersonID ". Der Konflikt trat in der Datenbank "Conto souniversity", Tabelle "dbo" auf. Person ", Spalte" PersonID ".*

Öffnen Sie *Migrationen\&lt; Timestamp&gt;\_Inheritance.cs* , und ersetzen Sie die `Up`-Methode durch den folgenden Code:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Führen Sie den `update-database` Befehl erneut aus.

> [!NOTE]
> Beim Migrieren von Daten und vornehmen von Schema Änderungen können andere Fehler auftreten. Wenn Sie Migrations Fehler erhalten, die Sie nicht beheben können, können Sie mit dem Tutorial fortfahren, indem Sie die Verbindungs Zeichenfolge in der Datei " *Web. config* " ändern oder die Datenbank löschen. Der einfachste Ansatz besteht darin, die Datenbank in der Datei " *Web. config* " umzubenennen. Ändern Sie z. b. den Datenbanknamen in Cu\_Test, wie im folgenden Beispiel gezeigt:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Bei einer neuen Datenbank gibt es keine zu migrierenden Daten, und der `update-database`-Befehl wird wahrscheinlich ohne Fehler ausgeführt. Anweisungen zum Löschen der Datenbank finden Sie unter Vorgehensweise beim Löschen [einer Datenbank aus Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Wenn Sie diesen Ansatz verwenden, um das Tutorial fortzusetzen, überspringen Sie den Bereitstellungs Schritt am Ende dieses Tutorials, da die bereitgestellte Site beim automatischen Ausführen von Migrationen denselben Fehler erhalten würde. Wenn Sie einen Migrations Fehler beheben möchten, ist die beste Ressource eine der Entity Framework Foren oder StackOverflow.com.

## <a name="testing"></a>Test

Führen Sie die Website aus, und testen Sie verschiedene Seiten. Alles funktioniert genauso wie vorher.

Erweitern Sie in **Server-Explorer** den Eintrag **schoolContext** und dann **Tables**, und Sie sehen, dass die Tabellen " **Student** " und " **Instructor** " durch eine **Person** -Tabelle ersetzt wurden. Erweitern Sie die Tabelle **Person** , und Sie sehen, dass Sie alle Spalten enthält, die in den Tabellen " **Student** " und " **Instructor** " verwendet wurden.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Klicken Sie mit der rechten Maustaste auf die Tabelle „Person“, und klicken Sie anschließend auf **Tabellendaten anzeigen**, um die Unterscheidungsspalte anzuzeigen.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Das folgende Diagramm veranschaulicht die Struktur der neuen Datenbank "School":

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Summary

Die "Tabelle pro Hierarchie"-Vererbung wurde nun für die Klassen `Person`, `Student`und `Instructor` implementiert. Weitere Informationen zu dieser und anderen Vererbungs Strukturen finden Sie unter [Vererbungs-Mapping-Strategien](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) im Blog von Morteza manavi. Im nächsten Tutorial sehen Sie einige Möglichkeiten, das Repository und Arbeitseinheiten Muster zu implementieren.

Links zu anderen Entity Framework Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
