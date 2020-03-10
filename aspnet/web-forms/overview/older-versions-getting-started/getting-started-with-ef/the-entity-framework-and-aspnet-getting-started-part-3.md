---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Einführung in Entity Framework 4,0 Database First und ASP.NET 4 Web Forms-Part 3 | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie mithilfe der Entity Framework Web Forms Anwendungen erstellen. Die Beispielanwendung ist...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 88debb11a9157dce9ff000b1cb459b876dbceaf3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78522639"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Einführung in Entity Framework 4,0 Database First und ASP.NET 4 Web Forms-Part 3

von [Tom Dykstra](https://github.com/tdykstra)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie ASP.net-Web Forms Anwendungen mithilfe von Entity Framework 4,0 und Visual Studio 2010 erstellen. Weitere Informationen zur tutorialreihe finden Sie [im ersten Tutorial der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md) .

## <a name="filtering-ordering-and-grouping-data"></a>Filtern, anordnen und Gruppieren von Daten

Im vorherigen Tutorial haben Sie das `EntityDataSource`-Steuerelement verwendet, um Daten anzuzeigen und zu bearbeiten. In diesem Tutorial filtern, Sortieren und gruppieren Sie Daten. Wenn Sie dazu Eigenschaften des `EntityDataSource`-Steuer Elements festlegen, unterscheidet sich die Syntax von anderen Datenquellen-Steuerelementen. Wie Sie sehen werden, können Sie jedoch das `QueryExtender`-Steuerelement verwenden, um diese Unterschiede zu minimieren.

Sie ändern die Seite " *students. aspx* ", um nach Schülern zu filtern, nach Name zu sortieren und nach Name zu suchen. Außerdem ändern Sie die Seite " *Courses. aspx* ", um Kurse für die ausgewählte Abteilung anzuzeigen und nach Kursen nach Namen zu suchen. Schließlich fügen Sie der Seite "Info *. aspx* " Studenten Statistiken hinzu.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Verwenden der EntityDataSource-Eigenschaft "Where" zum Filtern von Daten

Öffnen Sie die Seite " *students. aspx* ", die Sie im vorherigen Tutorial erstellt haben. Wie derzeit konfiguriert, zeigt das `GridView`-Steuerelement auf der Seite alle Namen aus der `People` Entitätenmenge an. Sie können jedoch nur die Schüler/Studenten anzeigen, die Sie finden, indem Sie `Person` Entitäten auswählen, die Registrierungsdaten ungleich NULL aufweisen.

Wechseln Sie zur **Entwurfs** Ansicht, und wählen Sie das `EntityDataSource` Steuerelement aus. Legen Sie im Fenster **Eigenschaften** die Eigenschaft `Where` auf `it.EnrollmentDate is not null`fest.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

Die Syntax, die Sie in der `Where`-Eigenschaft des `EntityDataSource`-Steuer Elements verwenden, ist Entity SQL. Entity SQL ähnelt Transact-SQL, ist jedoch für die Verwendung mit Entitäten und nicht von Datenbankobjekten angepasst. Im Ausdrucks `it.EnrollmentDate is not null`stellt das Wort `it` einen Verweis auf die von der Abfrage zurückgegebene Entität dar. Daher verweist `it.EnrollmentDate` auf die `EnrollmentDate`-Eigenschaft der `Person`-Entität, die vom `EntityDataSource` Steuerelement zurückgegeben wird.

Führen Sie die Seite aus. Die Liste "Students" enthält jetzt nur Studenten. (Es werden keine Zeilen angezeigt, in denen kein Registrierungsdatum vorhanden ist.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Verwenden der EntityDataSource-Eigenschaft "OrderBy" zum Sortieren von Daten

Außerdem soll diese Liste in der Reihenfolge der Namen angezeigt werden, wenn Sie zum ersten Mal angezeigt wird. Wenn die Seite " *students. aspx* " in der **Entwurfs** Ansicht weiterhin geöffnet ist und das `EntityDataSource`-Steuerelement noch ausgewählt ist, legen Sie im **Eigenschaften** Fenster die **OrderBy** -Eigenschaft auf `it.LastName`fest.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Führen Sie die Seite aus. Die Liste der Schüler/Studenten liegt nun in der Reihenfolge nach dem Nachnamen.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Verwenden eines Control-Parameters zum Festlegen der Where-Eigenschaft

Wie bei anderen Datenquellen-Steuerelementen können Sie Parameterwerte an die `Where`-Eigenschaft übergeben. Auf der Seite " *Courses. aspx* ", die Sie in Teil 2 des Tutorials erstellt haben, können Sie diese Methode zum Anzeigen von Kursen verwenden, die mit der Abteilung verknüpft sind, die ein Benutzer aus der Dropdown Liste auswählt.

Öffnen Sie " *Courses. aspx* ", und wechseln Sie zur **Entwurfs** Ansicht. Fügen Sie der Seite ein zweites `EntityDataSource`-Steuerelement hinzu, und benennen Sie es `CoursesEntityDataSource`. Stellen Sie eine Verbindung mit dem `SchoolEntities` Modell her, und wählen Sie `Courses` als **entitySetName** -Wert aus.

Klicken Sie im **Eigenschaften** Fenster auf die Auslassungs Punkte im Feld **Where** -Eigenschaft. (Stellen Sie sicher, dass das `CoursesEntityDataSource` Steuerelement immer noch ausgewählt ist, bevor Sie das **Eigenschaften** Fenster verwenden.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

Das Dialogfeld **Ausdrucks-Editor** wird angezeigt. Wählen Sie in diesem Dialogfeld **den WHERE-Ausdruck basierend auf den angegebenen Parametern automatisch generieren**aus, und klicken Sie dann auf **Parameter hinzufügen**. Benennen Sie den Parameter `DepartmentID`, wählen Sie **Steuer** Element als **Parameter Quellwert** aus, und wählen Sie **departmentsdropdownlist** als **ControlID** -Wert aus.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Klicken Sie auf **Erweiterte Eigenschaften anzeigen**, und ändern Sie im **Eigenschaften** Fenster des Dialog Felds **Ausdrucks-Editor** die `Type`-Eigenschaft in `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Wenn Sie fertig sind, klicken Sie auf **OK**.

Fügen Sie der Seite unterhalb der Dropdown Liste ein `GridView`-Steuerelement hinzu, und benennen Sie es `CoursesGridView`. Verbinden Sie ihn mit dem `CoursesEntityDataSource` Datenquellen-Steuerelement, klicken Sie auf **Schema aktualisieren**, klicken Sie auf **Spalten bearbeiten**, und entfernen Sie die Spalte `DepartmentID`. Das `GridView`-Steuerelement Markup ähnelt dem folgenden Beispiel.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Wenn der Benutzer die ausgewählte Abteilung in der Dropdown Liste ändert, soll die Liste der zugeordneten Kurse automatisch geändert werden. Um dies zu erreichen, wählen Sie die Dropdown Liste aus, und legen Sie im **Eigenschaften** Fenster die `AutoPostBack`-Eigenschaft auf `True`fest.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Nachdem Sie nun mit der Verwendung des Designers fertig sind, wechseln Sie zur **Quell** Ansicht, und ersetzen Sie die Eigenschaften `ConnectionString` und `DefaultContainer` Name des `CoursesEntityDataSource`-Steuer Elements durch das `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`-Attribut. Wenn Sie fertig sind, wird das Markup für das Steuerelement wie im folgenden Beispiel aussehen.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Führen Sie die Seite aus, und wählen Sie die Dropdown Liste aus, um verschiedene Abteilungen auszuwählen. Im `GridView` Steuerelement werden nur Kurse angezeigt, die von der ausgewählten Abteilung angeboten werden.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Verwenden der EntityDataSource-Eigenschaft "GroupBy" zum Gruppieren von Daten

Nehmen wir an, dass die IT-Organisation auf der Seite "Info" auf der Seite "Info" eine Reihe von Statistiken Insbesondere möchte ich eine Aufschlüsselung der Anzahl von Studenten nach dem Datum anzeigen, an dem Sie sich angemeldet haben.

Öffnen Sie *about. aspx*, und ersetzen Sie in der **Quell** Ansicht den vorhandenen Inhalt des `BodyContent`-Steuer Elements durch "Student Body Statistics" between `h2` Tags:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Fügen Sie nach der Überschrift ein `EntityDataSource`-Steuerelement hinzu, und benennen Sie es `StudentStatisticsEntityDataSource`. Verbinden Sie ihn mit `SchoolEntities`, wählen Sie die `People` Entitätenmenge aus, und lassen Sie das Feld **auswählen** im Assistenten unverändert. Legen Sie im **Eigenschaften** Fenster die folgenden Eigenschaften fest:

- Legen Sie die `Where`-Eigenschaft auf `it.EnrollmentDate is not null`fest, um nur nach Schülern zu filtern.
- Um die Ergebnisse nach dem anmelderungs Datum zu gruppieren, legen Sie die `GroupBy`-Eigenschaft auf `it.EnrollmentDate`fest.
- Legen Sie die `Select`-Eigenschaft auf `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`fest, um das anmelddatum und die Anzahl der Schüler/Studenten auszuwählen.
- Um die Ergebnisse nach dem anmelderungs Datum zu sortieren, legen Sie die `OrderBy`-Eigenschaft auf `it.EnrollmentDate`fest.

Ersetzen Sie in der **Quell** Ansicht die Eigenschaften `ConnectionString` und `DefaultContainer` Name durch eine `ContextTypeName`-Eigenschaft. Das `EntityDataSource`-Steuerelement Markup ähnelt nun dem folgenden Beispiel.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

Die Syntax der Eigenschaften `Select`, `GroupBy`und `Where` ähnelt Transact-SQL, mit Ausnahme des `it` Schlüsselworts, das die aktuelle Entität angibt.

Fügen Sie das folgende Markup hinzu, um ein `GridView`-Steuerelement zum Anzeigen der Daten zu erstellen.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Führen Sie die Seite aus, um eine Liste mit der Anzahl der Schüler/Studenten nach anmelderungs Datum anzuzeigen.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Verwenden des QueryExtender-Steuer Elements zum Filtern und anordnen

Das `QueryExtender`-Steuerelement bietet eine Möglichkeit, das Filtern und Sortieren im Markup anzugeben. Die Syntax ist unabhängig vom verwendeten Datenbankverwaltungssystem (Database Management System, DBMS). Sie ist auch in der Regel unabhängig vom Entity Framework, mit der Ausnahme, dass die Syntax, die Sie für Navigations Eigenschaften verwenden, für die Entity Framework eindeutig ist.

In diesem Teil des Tutorials verwenden Sie ein `QueryExtender`-Steuerelement, um Daten zu filtern und zu sortieren, und eines der Order-by-Felder ist eine Navigations Eigenschaft.

(Wenn Sie lieber Code anstelle von Markup verwenden möchten, um die Abfragen zu erweitern, die automatisch vom `EntityDataSource`-Steuerelement generiert werden, können Sie dies erreichen, indem Sie das `QueryCreated`-Ereignis behandeln. Auf diese Weise erweitert das `QueryExtender`-Steuerelement auch `EntityDataSource` Steuerelement Abfragen.)

Öffnen Sie die Seite " *Courses. aspx* ", und fügen Sie unter dem zuvor hinzugefügten Markup das folgende Markup ein, um eine Überschrift, ein Textfeld zum Eingeben von Such Zeichenfolgen, eine Such Schaltfläche und ein `EntityDataSource` Steuerelement, das an die `Courses` Entitätenmenge gebunden ist

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Beachten Sie, dass die `Include`-Eigenschaft des `EntityDataSource`-Steuer Elements auf `Department`festgelegt ist. In der-Datenbank enthält die `Course` Tabelle nicht den Abteilungsnamen; Sie enthält eine `DepartmentID` Fremdschlüssel Spalte. Wenn Sie die Datenbank direkt Abfragen, um den Abteilungsnamen zusammen mit den Kurs Daten zu erhalten, müssten Sie den `Course`-und `Department` Tabellen beitreten. Wenn Sie die `Include`-Eigenschaft auf `Department`festlegen, geben Sie an, dass die Entity Framework das Abrufen der zugehörigen `Department` Entität durchführen soll, wenn eine `Course` Entität abgerufen wird. Die `Department`-Entität wird dann in der `Department`-Navigations Eigenschaft der `Course`-Entität gespeichert. (Standardmäßig ruft die `SchoolEntities`-Klasse, die vom Datenmodell-Designer generiert wurde, zugehörige Daten ab, wenn Sie benötigt werden, und Sie haben das Datenquellen-Steuerelement an diese Klasse gebunden, sodass das Festlegen der `Include`-Eigenschaft nicht erforderlich ist. Durch das festlegen wird die Leistung der Seite verbessert, da andernfalls die Entity Framework separate Aufrufe der Datenbank vornehmen würde, um Daten für die `Course` Entitäten und für die zugehörigen `Department` Entitäten abzurufen.)

Fügen Sie nach dem soeben erstellten `EntityDataSource` Steuerelement das folgende Markup ein, um ein `QueryExtender` Steuerelement zu erstellen, das an dieses `EntityDataSource` Steuerelement gebunden ist.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

Das `SearchExpression`-Element gibt an, dass Kurse ausgewählt werden sollen, deren Titel mit dem im Textfeld eingegebenen Wert verglichen werden. Nur so viele Zeichen, wie im Textfeld eingegeben werden, werden verglichen, da die `SearchType`-Eigenschaft `StartsWith`angibt.

Das `OrderByExpression`-Element gibt an, dass das Resultset nach Kurstitel innerhalb des Abteilungs namens geordnet wird. Beachten Sie, dass der Abteilungs Name angegeben ist: `Department.Name`. Da die Zuordnung zwischen der `Course`-Entität und der `Department`-Entität eins-zu-eins ist, enthält die `Department`-Navigations Eigenschaft eine `Department` Entität. (Wenn es sich um eine 1: n-Beziehung handelt, würde die-Eigenschaft eine-Auflistung enthalten.) Um den Abteilungsnamen zu erhalten, müssen Sie die `Name`-Eigenschaft der `Department` Entität angeben.

Fügen Sie abschließend ein `GridView`-Steuerelement hinzu, um die Liste der Kurse anzuzeigen:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

Die erste Spalte ist ein Vorlagen Feld, in dem der Abteilungs Name angezeigt wird. Der Datenbindung-Ausdruck gibt `Department.Name`genau so an, wie Sie im `QueryExtender`-Steuerelement gesehen haben.

Führen Sie die Seite aus. Die erste Anzeige zeigt eine Liste aller Kurse nach Abteilung und dann nach Kurs Titel an.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Geben Sie "m" ein, und klicken Sie auf **Suchen** , um alle Kurse anzuzeigen, deren Titel mit "m" beginnen (bei der Suche wird die Groß-/Kleinschreibung nicht beachtet

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Verwenden des "like"-Operators zum Filtern von Daten

Sie können eine ähnliche Wirkung wie die `StartsWith`-, `Contains`-und `EndsWith` Suchtypen des `QueryExtender`-Steuer Elements erzielen, indem Sie einen `Like`-Operator in der `EntityDataSource`-Eigenschaft des `Where`-Steuer Elements verwenden. In diesem Teil des Tutorials erfahren Sie, wie Sie den `Like`-Operator verwenden, um nach einem Schüler/Student nach dem Namen zu suchen.

Öffnen Sie " *students. aspx* " in der **Quell** Ansicht. Fügen Sie nach dem `GridView`-Steuerelement das folgende Markup hinzu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Dieses Markup ähnelt dem, was Sie zuvor gesehen haben, mit Ausnahme des `Where`-Eigenschafts Werts. Der zweite Teil des `Where` Ausdrucks definiert eine Teil Zeichenfolgen-Suche (`LIKE %FirstMidName% or LIKE %LastName%`), die sowohl den Vornamen als auch den Nachnamen nach dem im Textfeld eingegebenen Namen durchsucht.

Führen Sie die Seite aus. Anfänglich werden alle Studenten angezeigt, da der Standardwert für den `StudentName`-Parameter "%" ist.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Geben Sie den Buchstaben "g" in das Textfeld ein, und klicken Sie auf **Suchen**. Sie sehen eine Liste von Schülern mit dem Namen "g" im vor-oder Nachnamen.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Sie haben nun Daten aus einzelnen Tabellen angezeigt, aktualisiert, gefiltert, sortiert und gruppiert. Im nächsten Tutorial beginnen Sie mit der Arbeit mit verwandten Daten (Master/Detail-Szenarien).

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-4.md)
