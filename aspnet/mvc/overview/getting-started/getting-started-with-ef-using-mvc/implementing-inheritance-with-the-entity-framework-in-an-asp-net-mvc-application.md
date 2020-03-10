---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: Implementieren der Vererbung mit EF in einer ASP.NET MVC 5-App'
description: In diesem Tutorial erfahren Sie, wie Sie die Vererbung in das Datenmodell implementieren können.
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 73a01ed47b0935a1a9734c197377470defb1fe36
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471063"
---
# <a name="tutorial-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a>Tutorial: Implementieren der Vererbung mit EF in einer ASP.NET MVC 5-App

Im vorherigen Tutorial haben Sie Parallelitäts Ausnahmen behandelt. In diesem Tutorial erfahren Sie, wie Sie die Vererbung in das Datenmodell implementieren können.

In der objektorientierten Programmierung können Sie die [Vererbung](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) verwenden, um die [Wiederverwendung von Code](http://en.wikipedia.org/wiki/Code_reuse)zu vereinfachen. In diesem Tutorial ändern Sie die Klassen `Instructor` und `Student` so, dass sie von einer `Person`-Basisklasse abgeleitet werden, die Eigenschaften wie `LastName` enthält. Diese Eigenschaften sind für Dozenten und Studenten gängig. Sie fügen keine Webseiten hinzu oder ändern diese, aber Sie werden Teile des Codes ändern. Diese Änderungen werden automatisch in der Datenbank widergespiegelt.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Weitere Informationen zum Zuordnen der Vererbung zu einer Datenbank
> * Erstellen der Klasse „Person“
> * Aktualisieren von „Instructor“ und „Student“
> * Person zum Modell hinzufügen
> * Erstellen und Aktualisieren von Migrationen
> * Testen der Implementierung
> * Bereitstellung in Azure

## <a name="prerequisites"></a>Voraussetzungen

* [Verarbeiten von Parallelitätsfehlern](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a>Zuordnen von Vererbung zu Datenbank

Die Klassen `Instructor` und `Student` im `School` Datenmodell verfügen über mehrere Eigenschaften, die identisch sind:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Angenommen, Sie möchten den redundanten Code für die Eigenschaften löschen, die von den Entitäten `Instructor` und `Student` gemeinsam genutzt werden. Oder Sie möchten einen Dienst schreiben, mit dem Namen formatiert werden können, ohne dass es eine Rolle spielt, ob der Name von einem Dozenten oder von einem Studenten stammt. Sie könnten eine `Person` Basisklasse erstellen, die nur die freigegebenen Eigenschaften enthält, und dann die `Instructor` und `Student` Entitäten von dieser Basisklasse erben, wie in der folgenden Abbildung dargestellt:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Es gibt mehrere Möglichkeiten, wie diese Vererbungsstruktur in der Datenbank dargestellt werden kann. Sie können über eine `Person` Tabelle verfügen, die Informationen zu den Studenten und Dozenten in einer einzelnen Tabelle enthält. Einige der Spalten können nur für Dozenten (`HireDate`) gelten, einige nur für Schüler/Studenten (`EnrollmentDate`), einige für beides (`LastName`, `FirstName`). Normalerweise verfügen Sie über eine *diskriminatorspalte* , um anzugeben, welcher Typ von den einzelnen Zeilen repräsentiert wird. So kann die Unterscheidungsspalte beispielsweise „Instructor“ für Dozenten und „Student“ für Studenten enthalten.

![Tabelle pro hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Dieses Muster zum Erstellen einer Entitäts Vererbungs Struktur aus einer einzelnen Datenbanktabelle wird als " *Tabelle pro Hierarchie* "-Vererbung (TPH) bezeichnet.

Alternativ kann die Datenbank so gestaltet werden, dass sie mehr wie die Vererbungsstruktur aussieht. Beispielsweise können Sie nur die namens Felder in der `Person` Tabelle und separate `Instructor` und `Student` Tabellen mit den Datumsfeldern haben.

![Tabelle pro type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Dieses Muster für die Erstellung einer Datenbanktabelle für jede Entitäts Klasse wird als " *Tabelle pro Typ* "-Vererbung (TPT) bezeichnet.

Eine weitere Möglichkeit besteht darin, individuellen Tabellen alle nicht abstrakten Typen zuzuordnen. Alle Eigenschaften einer Klasse, einschließlich der geerbten Eigenschaften, werden Spalten der entsprechenden Tabelle zugeordnet. Dieses Muster wird als TPC-Vererbung (TPC = Table per Concrete, Tabelle pro konkretem Typ) bezeichnet. Wenn Sie die TPC-Vererbung für die Klassen `Person`, `Student`und `Instructor` wie zuvor gezeigt implementiert haben, würden die Tabellen `Student` und `Instructor` nach der Implementierung der Vererbung nicht anders aussehen als zuvor.

TPC-und TPH-Vererbungsmuster bieten im Allgemeinen eine bessere Leistung in den Entity Framework als TPT-Vererbungsmuster, da TPT-Muster zu komplexen Verknüpfungs Abfragen führen können.

Dieses Tutorial veranschaulicht die Implementierung der TPH-Vererbung. TPH ist das Standard Vererbungsmuster in der Entity Framework. Sie müssen also lediglich eine `Person` Klasse erstellen, die `Instructor`-und `Student` Klassen ändern, um von `Person`abzuleiten, die neue Klasse der `DbContext`hinzufügen und eine Migration erstellen. (Informationen zum Implementieren der anderen Vererbungsmuster finden Sie unter [Mapping der TPT-Vererbung (Tabelle pro Typ)](https://msdn.microsoft.com/data/jj591617#2.5) und [Zuordnung der TPC-Vererbung (Tabelle pro konkrete Klasse](https://msdn.microsoft.com/data/jj591617#2.6) ) in der MSDN Entity Framework-Dokumentation.)

## <a name="create-the-person-class"></a>Erstellen der Klasse „Person“

Erstellen Sie im Ordner *Models* *Person.cs* , und ersetzen Sie den Vorlagen Code durch den folgenden Code:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a>Aktualisieren von „Instructor“ und „Student“

Aktualisieren Sie jetzt *Instructor.cs* und *Student.cs* , um Werte von *Person.SC*zu erben.

Leiten Sie in *Instructor.cs*die `Instructor`-Klasse von der `Person`-Klasse ab, und entfernen Sie die Schlüssel-und namens Felder. Der Code sieht aus wie im folgenden Beispiel:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Nehmen Sie ähnliche Änderungen an *Student.cs*vor. Die `Student`-Klasse sieht wie im folgenden Beispiel aus:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a>Person zum Modell hinzufügen

Fügen Sie in *SchoolContext.cs*eine `DbSet`-Eigenschaft für den `Person` Entitätstyp hinzu:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Das ist alles, was Entity Framework für die Konfiguration der „Tabelle pro Hierarchie“-Vererbung benötigt. Sie werden feststellen, dass bei der Aktualisierung der Datenbank eine `Person` Tabelle anstelle der Tabellen `Student` und `Instructor` vorhanden ist.

## <a name="create-and-update-migrations"></a>Erstellen und Aktualisieren von Migrationen

Geben Sie in der Paket-Manager-Konsole (PMC) den folgenden Befehl ein:

`Add-Migration Inheritance`

Führen Sie den `Update-Database` Befehl in der PMC aus. Der Befehl schlägt an dieser Stelle fehl, da wir über vorhandene Daten verfügen, die Migrationen nicht verarbeiten können. Sie erhalten eine Fehlermeldung wie die folgende:

> *Das Objekt ' dbo ' konnte nicht gelöscht werden. Instructor ', da durch eine FOREIGN KEY-Einschränkung auf Sie verwiesen wird.*

Öffnen Sie *Migrationen\&lt; Timestamp&gt;\_Inheritance.cs* , und ersetzen Sie die `Up`-Methode durch den folgenden Code:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Dieser Code übernimmt folgende Tasks für Datenbankaktualisierungen:

- Er entfernt Fremdschlüsseleinschränkungen und -indizes, die auf die Tabelle „Student“ verweisen.
- Er benennt die Tabelle „Instructor“ in „Person“ um und nimmt die Änderungen vor, die für das Speichern von Studentendaten erforderlich sind:

    - Er fügt das EnrollmentDate für Studenten hinzu, bei dem NULL-Werte zugelassen sind.
    - Er fügt eine Unterscheidungsspalte hinzu, um anzugeben, ob eine Zeile für einen Studenten oder für einen Dozenten bestimmt ist.
    - Er legt fest, dass bei HireDate NULL-Werte zugelassen sind, da die Zeilen für Studenten keine Einstellungsdaten enthalten.
    - Er fügt ein temporäres Feld hinzu, über das Fremdschlüssel aktualisiert werden sollen, die auf Studenten verweisen. Wenn Sie Schüler und Studenten in die Person-Tabelle kopieren, erhalten Sie neue Primärschlüssel Werte.
- Kopiert Daten aus der Tabelle „Student“ in die Tabelle „Person“. Dadurch werden Studenten neue Primärschlüsselwerte zugewiesen.
- Er legt Fremdschlüsselwerte fest, die auf Studenten verweisen.
- Er erstellt Fremdschlüsseleinschränkungen und -indizes neu, die dann auf die Tabelle „Person“ verweisen.

(Hätten Sie als Primärschlüsseltyp statt einem Integer die grafische Benutzeroberfläche verwendet haben, hätten die Primärschlüsselwerte für Studenten nicht geändert werden müssen, und mehrere dieser Schritte hätten ausgelassen werden können.)

Führen Sie den Befehl `update-database` erneut aus.

(In einem Produktionssystem würden Sie entsprechende Änderungen an der Down-Methode vornehmen, falls Sie es jemals verwenden müssten, um zur vorherigen Datenbankversion zurückzukehren. Für dieses Tutorial verwenden Sie nicht die Methode "Down".)

> [!NOTE]
> Beim Migrieren von Daten und vornehmen von Schema Änderungen können andere Fehler auftreten. Wenn Sie Migrations Fehler erhalten, die Sie nicht beheben können, können Sie mit dem Tutorial fortfahren, indem Sie die Verbindungs Zeichenfolge in der Datei " *Web. config* " ändern oder die Datenbank löschen. Der einfachste Ansatz besteht darin, die Datenbank in der Datei " *Web. config* " umzubenennen. Ändern Sie z. b. den Datenbanknamen in "contosouniversity2", wie im folgenden Beispiel gezeigt:
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> Bei einer neuen Datenbank gibt es keine zu migrierenden Daten, und der `update-database`-Befehl wird wahrscheinlich ohne Fehler ausgeführt. Anweisungen zum Löschen der Datenbank finden Sie unter Vorgehensweise beim Löschen [einer Datenbank aus Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Wenn Sie diesen Ansatz verwenden, um das Tutorial fortzusetzen, überspringen Sie den Bereitstellungs Schritt am Ende dieses Tutorials, oder stellen Sie eine neue Website und eine neue Datenbank bereit. Wenn Sie ein Update für denselben Standort bereitstellen, auf dem Sie bereits bereitgestellt haben, erhält EF denselben Fehler, wenn er Migrationen automatisch ausführt. Wenn Sie einen Migrations Fehler beheben möchten, ist die beste Ressource eine der Entity Framework Foren oder StackOverflow.com.

## <a name="test-the-implementation"></a>Testen der Implementierung

Führen Sie die Website aus, und testen Sie verschiedene Seiten. Alles funktioniert genauso wie vorher.

Erweitern Sie in **Server-Explorer** den Eintrag **Data connections\schoolcontext** und dann **Tables**, und Sie sehen, dass die Tabellen " **Student** " und " **Instructor** " durch eine **Person** -Tabelle ersetzt wurden. Erweitern Sie die Tabelle **Person** , und Sie sehen, dass Sie alle Spalten enthält, die in den Tabellen " **Student** " und " **Instructor** " verwendet wurden.

Klicken Sie mit der rechten Maustaste auf die Tabelle „Person“, und klicken Sie anschließend auf **Tabellendaten anzeigen**, um die Unterscheidungsspalte anzuzeigen.

Das folgende Diagramm veranschaulicht die Struktur der neuen Datenbank "School":

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Bereitstellung in Azure

Dieser Abschnitt erfordert, dass Sie den optionalen Abschnitt bereitstellen **der APP für Azure** in [Teil 3, sortieren, Filtern und Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) dieser tutorialreihe abgeschlossen haben. Wenn Migrations Fehler aufgetreten sind, die Sie durch Löschen der Datenbank im lokalen Projekt aufgelöst haben, überspringen Sie diesen Schritt. oder erstellen Sie eine neue Website und eine neue Datenbank, und stellen Sie Sie in der neuen Umgebung bereit.

1. Klicken Sie im **Projektmappen-Explorer** von Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie im Kontextmenü **Veröffentlichen** aus.

2. Klicken Sie auf **Veröffentlichen**.

    Die Web-App wird in Ihrem Standardbrowser geöffnet.

3. Testen Sie die Anwendung, um zu überprüfen, ob Sie funktioniert.

    Wenn Sie zum ersten Mal eine Seite ausführen, die auf die Datenbank zugreift, führt die Entity Framework alle Migrationen `Up` Methoden aus, die erforderlich sind, um die Datenbank mit dem aktuellen Datenmodell auf den neuesten Stand zu bringen.

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Links zu anderen Entity Framework Ressourcen finden Sie unter [ASP.NET Data Access-Empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

Weitere Informationen zu dieser und anderen Vererbungs Strukturen finden Sie unter [TPT-Vererbungsmuster](https://msdn.microsoft.com/data/jj618293) und [TPH-Vererbungsmuster](https://msdn.microsoft.com/data/jj618292) auf MSDN. Im nächsten Tutorial erfahren Sie, wie Sie eine Vielzahl von Entity Framework-Szenarios auf fortgeschrittenem Niveau verarbeiten können.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Es wurde gelernt, Vererbung der Datenbank zuzuordnen.
> * Klasse „Person“ wurde erstellt
> * „Instructor“ und „Student“ wurden aktualisiert
> * Person zum Modell hinzugefügt
> * Erstellen und Aktualisieren von Migrationen
> * Die Implementierung wurde getestet
> * Bereitstellung in Azure

Fahren Sie mit dem nächsten Artikel fort, um sich über die Grundlagen der Entwicklung von ASP.NET-Webanwendungen, die Entity Framework Code First verwenden, vertraut zu machen.
> [!div class="nextstepaction"]
> [Erweiterte Entity Framework-Szenarien](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
