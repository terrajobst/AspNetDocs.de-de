---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Einführung in Entity Framework 4,0 Database First und ASP.NET 4 Web Forms-Part 6 | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie mithilfe der Entity Framework Web Forms Anwendungen erstellen. Die Beispielanwendung ist...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 8bfbe74f90eb03ea9aab7610842ef2578e80d113
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454917"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Die ersten Schritte mit Entity Framework 4,0 Database First und ASP.NET 4 Web Forms-Teil 6

von [Tom Dykstra](https://github.com/tdykstra)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie ASP.net-Web Forms Anwendungen mithilfe von Entity Framework 4,0 und Visual Studio 2010 erstellen. Weitere Informationen zur tutorialreihe finden Sie [im ersten Tutorial der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md) .

## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementieren der "Tabelle pro Hierarchie"-Vererbung

Im vorherigen Tutorial haben Sie mit verknüpften Daten gearbeitet, indem Sie Beziehungen hinzufügen und löschen und eine neue Entität hinzufügen, die eine Beziehung zu einer vorhandenen Entität enthielt. In diesem Tutorial erfahren Sie, wie Sie die Vererbung in das Datenmodell implementieren können.

Bei der objektorientierten Programmierung können Sie die Vererbung verwenden, um die Arbeit mit verknüpften Klassen zu vereinfachen. Beispielsweise können Sie `Instructor`-und `Student` Klassen erstellen, die von einer `Person`-Basisklasse abgeleitet werden. Sie können die gleichen Arten von Vererbungs Strukturen zwischen Entitäten in der Entity Framework erstellen.

In diesem Teil des Tutorials erstellen Sie keine neuen Webseiten. Stattdessen werden dem Datenmodell abgeleitete Entitäten hinzugefügt und vorhandene Seiten geändert, sodass die neuen Entitäten verwendet werden.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>"Tabelle pro Hierarchie" und "Tabelle pro Typ"-Vererbung

In einer Datenbank können Informationen zu verknüpften Objekten in einer Tabelle oder in mehreren Tabellen gespeichert werden. Beispielsweise enthält die `Person` Tabelle in der `School`-Datenbank Informationen zu den Studenten und Dozenten in einer einzelnen Tabelle. Einige der Spalten gelten nur für Dozenten (`HireDate`), einige nur für Schüler/Studenten (`EnrollmentDate`) und einige für beide (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Sie können die Entity Framework so konfigurieren, dass `Instructor` und `Student` Entitäten erstellt werden, die von der `Person` Entität erben. Dieses Muster zum Erstellen einer Entitäts Vererbungs Struktur aus einer einzelnen Datenbanktabelle wird als " *Tabelle pro Hierarchie* "-Vererbung (TPH) bezeichnet.

Für Kurse verwendet die `School` Datenbank ein anderes Muster. Online Kurse und Kurse vor Ort werden in separaten Tabellen gespeichert, von denen jeder über einen Fremdschlüssel verfügt, der auf die `Course` Tabelle verweist. Informationen, die für beide Kurs Typen gemeinsam sind, werden nur in der `Course` Tabelle gespeichert.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Sie können das Entity Framework Datenmodell so konfigurieren, dass `OnlineCourse` und `OnsiteCourse` Entitäten von der `Course` Entität erben. Dieses Muster zum Erstellen einer Entitäts Vererbungs Struktur aus separaten Tabellen für jeden Typ, wobei jede separate Tabelle auf eine Tabelle verweist, in der die Daten gespeichert werden, die für alle Typen gemeinsam sind, werden als *Tabelle pro Typ* (TPT) bezeichnet.

TPH-Vererbungsmuster bieten im Allgemeinen eine bessere Leistung in den Entity Framework als TPT-Vererbungsmuster, da TPT-Muster zu komplexen Verknüpfungs Abfragen führen können. Diese exemplarische Vorgehensweise veranschaulicht die Implementierung der TPH-Vererbung. Führen Sie dazu die folgenden Schritte aus:

- Erstellen Sie `Instructor` und `Student` Entitäts Typen, die von `Person`abgeleitet werden.
- Verschieben von Eigenschaften, die sich auf die abgeleiteten Entitäten aus der `Person` Entität beziehen, in die abgeleiteten Entitäten
- Legen Sie Einschränkungen für Eigenschaften in den abgeleiteten Typen fest.
- Erstellen Sie die `Person`-Entität als abstrakte Entität.
- Ordnen Sie jede abgeleitete Entität der `Person` Tabelle mit einer Bedingung zu, die angibt, wie bestimmt wird, ob eine `Person` Zeile diesen abgeleiteten Typ darstellt.

## <a name="adding-instructor-and-student-entities"></a>Hinzufügen von Dozenten und Studenten Entitäten

Öffnen Sie die Datei <em>School Model. edmx</em> , klicken Sie mit der rechten Maustaste auf einen nicht belegten Bereich im Designer, wählen Sie <strong>Hinzufügen</strong>und dann <strong>Entität</strong>aus<em>.</em>

[![Image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

Benennen Sie die Entität im Dialogfeld **Entität hinzufügen** `Instructor` und legen Sie die **Basistyp** Option auf `Person`fest.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Klicken Sie auf **OK**. Der Designer erstellt eine `Instructor` Entität, die von der `Person` Entität abgeleitet ist. Die neue Entität hat noch keine Eigenschaften.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Wiederholen Sie das Verfahren zum Erstellen einer `Student` Entität, die ebenfalls von `Person`abgeleitet ist.

Nur Dozenten haben Einstellungsdaten, daher müssen Sie diese Eigenschaft von der `Person`-Entität in die `Instructor` Entität verschieben. Klicken Sie in der `Person` Entität mit der rechten Maustaste auf die Eigenschaft `HireDate`, und klicken Sie auf **Ausschneiden** Klicken Sie dann mit der rechten Maustaste auf **Eigenschaften** in der `Instructor`-Entität, und **Klicken Sie**

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

Das Einstellungs Datum einer `Instructor` Entität darf nicht NULL sein. Klicken Sie mit der rechten Maustaste auf die `HireDate`-Eigenschaft, klicken Sie auf **Eigenschaften**, und ändern Sie dann im **Eigenschaften** Fenster `Nullable` in `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Wiederholen Sie die Prozedur, um die `EnrollmentDate`-Eigenschaft von der `Person`-Entität in die `Student` Entität zu verschieben Stellen Sie sicher, dass Sie für die Eigenschaft `EnrollmentDate` auch `Nullable` auf `False` festlegen.

Nun, da eine `Person` Entität nur die Eigenschaften hat, die `Instructor` und `Student` Entitäten gemeinsam sind (abgesehen von den Navigations Eigenschaften, die Sie nicht verschieben), kann die Entität nur als Basis Entität in der Vererbungs Struktur verwendet werden. Daher müssen Sie sicherstellen, dass die Entität nie als unabhängige Entität behandelt wird. Klicken Sie mit der rechten Maustaste auf die `Person` Entität, wählen Sie **Eigenschaften**aus, und ändern Sie dann im **Eigenschaften** Fenster den Wert der Eigenschaft **abstract** in **true**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Mapping von Instructor-und Student-Entitäten zur Person-Tabelle

Nun müssen Sie dem Entity Framework mitteilen, wie zwischen `Instructor` und `Student` Entitäten in der Datenbank unterschieden werden kann.

Klicken Sie mit der rechten Maustaste auf die Entität `Instructor` und wählen Sie **Tabellen Zuordnung** Klicken Sie im Fenster **Mappingdetails** auf **Tabelle oder Sicht hinzufügen** , und wählen Sie **Person**aus.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Klicken Sie auf **Bedingung hinzufügen**, und wählen Sie dann **HireDate**aus.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

**Operator** ändern in **ist** und **Wert/Eigenschaft** auf **not NULL**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Wiederholen Sie die Prozedur für die `Students` Entität, und geben Sie an, dass diese Entität der `Person` Tabelle zugeordnet werden soll, wenn die `EnrollmentDate` Spalte nicht NULL ist Speichern und schließen Sie dann das Datenmodell.

Erstellen Sie das Projekt, um die neuen Entitäten als Klassen zu erstellen und Sie im Designer verfügbar zu machen.

## <a name="using-the-instructor-and-student-entities"></a>Verwenden der Dozenten-und Student-Entitäten

Wenn Sie die Webseiten erstellt haben, die mit den Daten von Student und Instructor arbeiten, haben Sie die Daten an die `Person` Entitätenmenge gebunden, und Sie haben die `HireDate` oder `EnrollmentDate` Eigenschaft gefiltert, um die zurückgegebenen Daten auf Studenten oder Dozenten zu beschränken. Wenn Sie jedoch jedes Datenquellen-Steuerelement an die `Person` Entitätenmenge binden, können Sie angeben, dass nur `Student` oder `Instructor` Entitäts Typen ausgewählt werden sollen. Da der Entity Framework weiß, wie Studenten und Dozenten in der `Person` Entitätenmenge unterschieden werden können, können Sie die `Where` Eigenschaften Einstellungen entfernen, die Sie manuell eingegeben haben.

Im Visual Studio-Designer können Sie den Entitätstyp angeben, den ein `EntityDataSource` Steuerelement im Dropdown Feld **EntityTypeFilter** des `Configure Data Source`-Assistenten auswählen soll, wie im folgenden Beispiel gezeigt.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

Im **Eigenschaften** Fenster können Sie `Where`-Klauselwerte entfernen, die nicht mehr benötigt werden, wie im folgenden Beispiel gezeigt.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Da Sie jedoch das Markup für `EntityDataSource`-Steuerelemente geändert haben, sodass das `ContextTypeName`-Attribut verwendet werden kann, können Sie den Assistenten zum **Konfigurieren von Datenquellen** nicht auf `EntityDataSource`-Steuerelementen ausführen, die Sie bereits erstellt haben. Daher nehmen Sie die erforderlichen Änderungen vor, indem Sie stattdessen das Markup ändern.

Öffnen Sie die Seite *students. aspx* . Entfernen Sie im `StudentsEntityDataSource`-Steuerelement das `Where`-Attribut, und fügen Sie ein `EntityTypeFilter="Student"`-Attribut hinzu. Das Markup ähnelt nun dem folgenden Beispiel:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Durch Festlegen des `EntityTypeFilter` Attributs wird sichergestellt, dass das `EntityDataSource` Steuerelement nur den angegebenen Entitätstyp auswählt. Wenn Sie sowohl `Student` als auch `Instructor` Entitäts Typen abrufen möchten, legen Sie dieses Attribut nicht fest. (Sie haben die Möglichkeit, mehrere Entitäts Typen mit nur einem `EntityDataSource` Steuerelement abzurufen, wenn Sie das-Steuerelement für schreibgeschützten Datenzugriff verwenden. Wenn Sie ein `EntityDataSource`-Steuerelement verwenden, um Entitäten einzufügen, zu aktualisieren oder zu löschen, und wenn die Entitätenmenge, an die Sie gebunden ist, mehrere Typen enthalten kann, können Sie nur mit einem Entitätstyp arbeiten, und Sie müssen dieses Attribut festlegen.)

Wiederholen Sie die Prozedur für das `SearchEntityDataSource`-Steuerelement, mit der Ausnahme, dass Sie nur den Teil des `Where` Attributs entfernen, der `Student` Entitäten auswählt, anstatt die-Eigenschaft Das öffnende Tag des-Steuer Elements ähnelt nun dem folgenden Beispiel:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Führen Sie die Seite aus, um zu überprüfen, ob Sie wie zuvor funktioniert.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Aktualisieren Sie die folgenden Seiten, die Sie in den vorherigen Tutorials erstellt haben, damit Sie die neuen `Student` und `Instructor` Entitäten anstelle von `Person` Entitäten verwenden, und führen Sie Sie dann aus, um sicherzustellen, dass Sie wie zuvor funktionieren:

- Fügen Sie in *studentsadd. aspx*`EntityTypeFilter="Student"` dem `StudentsEntityDataSource`-Steuerelement hinzu. Das Markup ähnelt nun dem folgenden Beispiel: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- Fügen Sie in " *about. aspx*" `EntityTypeFilter="Student"` dem `StudentStatisticsEntityDataSource`-Steuerelement hinzu, und entfernen Sie `Where="it.EnrollmentDate is not null"`. Das Markup ähnelt nun dem folgenden Beispiel: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- Fügen Sie in " *Dozenten. aspx* " und " *Instructor scourses. aspx*" `EntityTypeFilter="Instructor"` dem `InstructorsEntityDataSource`-Steuerelement hinzu, und entfernen Sie `Where="it.HireDate is not null"`. Das Markup in " *Dozenten. aspx* " ähnelt nun dem folgenden Beispiel: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Das Markup in " *Instructor Courses. aspx* " ähnelt nun dem folgenden Beispiel:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

Aufgrund dieser Änderungen haben Sie die Wartbarkeit der Anwendung der Anwendung "der Anwendung" auf verschiedene Weise verbessert. Sie haben die Auswahl-und Validierungs Logik aus der UI-Schicht (*aspx* -Markup) verschoben und als integralen Bestandteil der Datenzugriffs Ebene festgestellt. Dies trägt dazu bei, ihren Anwendungscode von Änderungen zu isolieren, die Sie in Zukunft für das Datenbankschema oder das Datenmodell vornehmen können. Sie könnten z. b. entscheiden, dass Schüler/Studenten als Mitarbeiter Hilfen eingestellt werden und daher ein Einstellungs Datum erhalten würde. Anschließend können Sie eine neue Eigenschaft hinzufügen, um Schüler und Studenten von Dozenten zu unterscheiden und das Datenmodell zu aktualisieren. Es muss kein Code in der Webanwendung geändert werden, es sei denn, Sie möchten ein Einstellungs Datum für Schüler und Studenten anzeigen. Ein weiterer Vorteil beim Hinzufügen von `Instructor`-und `Student` Entitäten besteht darin, dass Ihr Code leichter verständlich ist als bei `Person` Objekten, die eigentlich Studenten oder Dozenten waren.

Sie haben nun eine Möglichkeit gesehen, ein Vererbungsmuster in der Entity Framework zu implementieren. Im folgenden Tutorial erfahren Sie, wie Sie gespeicherte Prozeduren verwenden, um mehr Kontrolle darüber zu erhalten, wie die Entity Framework auf die Datenbank zugreift.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-7.md)
