---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Aktualisieren verwandter Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung (6 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio erstellt werden...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d29cb172d642b67947b461d1a7e55d01872bb8c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468285"
---
# <a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Aktualisieren verwandter Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung (6 von 10)

von [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio 2012 erstellt werden. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe von Anfang an starten oder [ein Starter-Projekt für dieses Kapitel herunterladen](building-the-ef5-mvc4-chapter-downloads.md) und hier beginnen.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem stoßen, das Sie nicht beheben können, [Laden Sie das abgeschlossene Kapitel herunter](building-the-ef5-mvc4-chapter-downloads.md) , und versuchen Sie, das Problem zu reproduzieren. Im Allgemeinen können Sie die Lösung für das Problem finden, indem Sie Ihren Code mit dem abgeschlossenen Code vergleichen. Informationen zu häufigen Fehlern und deren Lösung finden Sie unter [Fehler und](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors) Problem Umgehungen.

Im vorherigen Tutorial haben Sie verwandte Daten angezeigt. in diesem Tutorial aktualisieren Sie verwandte Daten. Für die meisten Beziehungen kann dies durch Aktualisieren der entsprechenden Fremdschlüssel Felder erreicht werden. Bei m:n-Beziehungen macht der Entity Framework die jointabelle nicht direkt verfügbar. Daher müssen Sie Entitäten explizit zu den entsprechenden Navigations Eigenschaften hinzufügen und daraus entfernen.

Die folgenden Abbildungen zeigen die Seiten, mit den Sie arbeiten werden.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Anpassen der Seiten „Erstellen“ und „Bearbeiten“ für Kurse

Wenn eine neue Kursentität erstellt wird, muss diese in Beziehung zu einer vorhandenen Abteilung stehen. Um dies zu vereinfachen, enthält der Gerüstcode Controllermethoden und Ansichten zum „Erstellen“ und „Bearbeiten“, die eine Dropdownliste enthalten, aus denen der Fachbereich ausgewählt werden kann. In der Dropdown Liste wird die `Course.DepartmentID` Fremdschlüssel Eigenschaft festgelegt, und das ist alles Entity Framework Anforderungen, um die `Department` Navigations Eigenschaft mit der entsprechenden `Department` Entität zu laden. Verwenden Sie den Gerüstcode, aber nehmen Sie kleine Änderungen vor, um die Fehlerbehandlung hinzuzufügen und die Dropdownliste zu sortieren.

Löschen Sie in *CourseController.cs*die vier Methoden `Edit` und `Create`, und ersetzen Sie Sie durch den folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

Die `PopulateDepartmentsDropDownList`-Methode ruft eine Liste aller Abteilungen nach Namen sortiert ab, erstellt eine `SelectList` Auflistung für eine Dropdown Liste und übergibt die Auflistung an die Ansicht in einer `ViewBag`-Eigenschaft. Die Methode akzeptiert den optionalen `selectedDepartment`-Parameter, über den der Code das Element angeben kann, das ausgewählt wird, wenn die Dropdownliste gerendert wird. Die Ansicht übergibt den Namen `DepartmentID` an [das `DropDownList`](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)-Hilfsprogramm, und das Hilfsprogramm weiß dann, dass das `ViewBag` Objekt nach einem `SelectList` mit dem Namen `DepartmentID`durchsucht werden soll.

Die `HttpGet` `Create` Methode ruft die `PopulateDepartmentsDropDownList` Methode auf, ohne das ausgewählte Element festzulegen, da die Abteilung für einen neuen Kurs noch nicht eingerichtet wurde:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Mit der `HttpGet` `Edit`-Methode wird das ausgewählte Element auf Grundlage der ID der Abteilung festgelegt, die dem gerade bearbeiteten Kurs bereits zugewiesen ist:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Die `HttpPost`-Methoden für `Create` und `Edit` enthalten außerdem Code, der das ausgewählte Element festlegt, wenn die Seite nach einem Fehler erneut angezeigt wird:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Mit diesem Code wird sichergestellt, dass wenn die Seite erneut angezeigt wird, um die Fehlermeldung anzuzeigen, die ausgewählte Abteilung ausgewählt bleibt.

Fügen Sie in *views\course\kreate.cshtml*den hervorgehobenen Code hinzu, um ein neues Kurs Nummern Feld vor dem Feld **Titel** zu erstellen. Wie in einem früheren Tutorial erläutert, werden Primärschlüssel Felder standardmäßig nicht als Gerüst festgelegt, aber der Primärschlüssel ist sinnvoll, sodass der Benutzer in der Lage sein soll, den Schlüsselwert einzugeben.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

Fügen Sie in " *views\cours\edit.cshtml*", " *views\cours\delete.cshtml*" und " *views\cours \Details.cshtml*" vor dem Feld " **Title** " ein Feld für die Kursnummer hinzu. Da es sich um den Primärschlüssel handelt, wird er angezeigt, kann aber nicht geändert werden.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Führen Sie die Seite **Erstellen** aus (zeigen Sie die Kursindex Seite an, klicken Sie auf **neu erstellen**), und geben Sie Daten für einen neuen Kurs ein:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Klicken Sie auf **Erstellen**. Die Seite Index Seite wird mit dem neuen Kurs angezeigt, der der Liste hinzugefügt wird. Der Fachbereichsname in der Indexseitenliste wurde der Navigationseigenschaft entnommen und deutet darauf hin, dass die Beziehung ordnungsgemäß festgelegt wurde.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Führen Sie die **Bearbeitungs** Seite aus (zeigen Sie die Kursindex Seite an, und klicken Sie auf Kurs **Bearbeiten** ).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Ändern Sie die Daten auf der Seite, und klicken Sie auf **Speichern**. Die Kursindex Seite wird mit den aktualisierten Kursdaten angezeigt.

## <a name="adding-an-edit-page-for-instructors"></a>Hinzufügen einer Bearbeitungsseite für Dozenten

Bei der Bearbeitung eines Dozentendatensatzes sollten Sie auch die Bürozuweisung des Dozenten aktualisieren. Die `Instructor`-Entität verfügt über eine 1:0-oder 1-Beziehung mit der `OfficeAssignment` Entität, was bedeutet, dass Sie die folgenden Situationen behandeln müssen:

- Wenn der Benutzer die Office-Zuweisung löscht und ursprünglich einen Wert enthielt, müssen Sie die `OfficeAssignment` Entität entfernen und löschen.
- Wenn der Benutzer einen Office-Zuweisungs Wert eingibt und dieser ursprünglich leer war, müssen Sie eine neue `OfficeAssignment` Entität erstellen.
- Wenn der Benutzer den Wert einer Office-Zuweisung ändert, müssen Sie den Wert in einer vorhandenen `OfficeAssignment` Entität ändern.

Öffnen Sie *InstructorController.cs* , und sehen Sie sich die `HttpGet` `Edit`-Methode an:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Der Gerüst Code hier ist nicht das, was Sie möchten. Es ist das Einrichten von Daten für eine Dropdown Liste, aber Sie benötigen ein Textfeld. Ersetzen Sie diese Methode durch den folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Mit diesem Code wird die `ViewBag`-Anweisung gelöscht und Eager Loading für die zugeordnete `OfficeAssignment` Entität hinzugefügt. Sie können Eager Loading nicht mit der `Find`-Methode ausführen, sodass die Methoden `Where` und `Single` stattdessen verwendet werden, um den Dozenten auszuwählen.

Ersetzen Sie die `HttpPost` `Edit`-Methode durch den folgenden Code. die Office-Zuweisungs Updates behandelt:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Im Code werden folgende Schritte ausgeführt:

- Ruft die aktuelle Entität `Instructor` von der Datenbank über Eager Loading für die Navigationseigenschaft `OfficeAssignment` ab. Dies entspricht dem, was Sie in der `HttpGet` `Edit`-Methode getan haben.
- Aktualisiert die abgerufene Entität `Instructor` mit Werten aus der Modellbindung. Mit der verwendeten [tryupdatemodel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) -Überladung können Sie die Eigenschaften in die *Whitelist* aufnehmen, die Sie einschließen möchten. Dies verhindert eine Übertragung, wie im [zweiten Tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)erläutert wird.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Wenn der Bürostandort leer ist, wird die `Instructor.OfficeAssignment`-Eigenschaft auf NULL festgelegt, damit die verknüpfte Zeile in der `OfficeAssignment` Tabelle gelöscht wird.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Speichert die Änderungen in der Datenbank.

Fügen Sie in *views\instructor\edit.cshtml*nach den `div` Elementen für das Einstellungs **Datum** ein neues Feld zum Bearbeiten des Office-Speicher Orts hinzu:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Führen Sie die Seite aus (Wählen Sie die Registerkarte **Dozenten** aus, und klicken Sie dann auf auf einem Dozenten **Bearbeiten** ) Ändern Sie den **Standort des Büros**, und klicken Sie auf **Speichern**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Hinzufügen von Kurs Zuweisungen zur Dozenten Bearbeitungsseite

Dozenten können eine beliebige Anzahl von Kursen unterrichten. Jetzt soll die Dozentenseite „Bearbeiten“ verbessert werden, indem es ermöglicht wird, Kurszuweisungen über eine Reihe von Kontrollkästchen zu verändern. Dies wird im folgenden Screenshot dargestellt:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Die Beziehung zwischen den Entitäten `Course` und `Instructor` ist m:n. das bedeutet, dass Sie keinen direkten Zugriff auf die jointabelle haben. Stattdessen fügen Sie Entitäten zu und aus der `Instructor.Courses` Navigations Eigenschaft hinzu und entfernen Sie daraus.

Die Benutzeroberfläche, über die Sie ändern können, welchen Kursen ein Dozent zugewiesen ist, besteht aus einer Reihe von Kontrollkästchen. Für jeden Kurs in der Datenbank wird ein Kontrollkästchen angezeigt. Die Kontrollkästchen, denen der Dozent zu diesem Zeitpunkt zugewiesen ist, sind aktiviert. Der Benutzer kann Kontrollkästchen aktivieren oder deaktivieren, um Kurszuweisungen zu ändern. Wenn die Anzahl der Kurse viel größer ist, sollten Sie wahrscheinlich eine andere Methode zum Darstellen der Daten in der Ansicht verwenden, aber Sie verwenden dieselbe Methode zum Bearbeiten von Navigations Eigenschaften, um Beziehungen zu erstellen oder zu löschen.

Verwenden Sie eine Ansichtsmodellklasse, um Daten für die Ansicht bereitzustellen, um eine Liste mit Kontrollkästchen zu erstellen. Erstellen Sie im Ordner " *ViewModels* " *AssignedCourseData.cs* , und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Ersetzen Sie in *InstructorController.cs*die `HttpGet` `Edit`-Methode durch den folgenden Code. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Über den Code wird für die `Courses`-Navigationseigenschaft Eager Loading hinzugefügt und die neue `PopulateAssignedCourseData`-Methode aufgerufen, um über die Ansichtsmodellklasse `AssignedCourseData` Informationen für das Kontrollkästchenarray zur Verfügung zu stellen.

Der Code in der `PopulateAssignedCourseData`-Methode liest alle `Course` Entitäten, um eine Liste von Kursen mithilfe der Ansichts Modell Klasse zu laden. Für jeden Kurs überprüft der Code, ob dieser in der `Courses`-Navigationseigenschaft des Dozenten vorhanden ist. Zum Erstellen einer effizienten Suche bei der Überprüfung, ob ein Kurs dem Dozenten zugewiesen ist, werden die Kurse, die dem Dozenten zugewiesen sind, in eine [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) -Auflistung eingefügt. Die `Assigned`-Eigenschaft wird für Kurse, denen der Dozenten zugewiesen ist, auf `true` festgelegt. Die Ansicht verwendet diese Eigenschaft, um zu bestimmen, welche Kontrollkästchen als aktiviert angezeigt werden sollen. Schließlich wird die Liste an die Ansicht in einer `ViewBag`-Eigenschaft übermittelt.

Fügen Sie als nächstes den Code hinzu, der ausgeführt wird, wenn der Benutzer auf **Speichern** klickt. Ersetzen Sie die `HttpPost` `Edit`-Methode durch den folgenden Code, der eine neue Methode aufruft, die die `Courses`-Navigations Eigenschaft der `Instructor` Entität aktualisiert. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Da die Sicht nicht über eine Auflistung von `Course` Entitäten verfügt, kann der Modell Binder die `Courses` Navigations Eigenschaft nicht automatisch aktualisieren. Anstatt den Modell Binder zum Aktualisieren der Kurse-Navigations Eigenschaft zu verwenden, führen Sie dies in der neuen `UpdateInstructorCourses`-Methode aus. Aus diesem Grund müssen Sie die `Courses`-Eigenschaft von der Modellbindung ausschließen. Dies erfordert keine Änderung des Codes, der [tryupdatemodel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) aufruft, da Sie die *Whitelists* -Überladung verwenden und `Courses` nicht in der Include-Liste enthalten ist.

Wenn keine Kontrollkästchen ausgewählt wurden, initialisiert der Code in `UpdateInstructorCourses` die `Courses` Navigations Eigenschaft mit einer leeren Auflistung:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Der Code führt dann eine Schleife durch alle Kurse in der Datenbank aus und überprüft jeden Kurs im Hinblick auf die Kurse, die zu diesem Zeitpunkt dem Dozenten zugewiesen sind, und denen, die in der Ansicht aktiviert wurden. Die beiden letzten Auflistungen werden in `HashSet`-Objekten gespeichert, um Suchvorgänge effizienter zu gestalten.

Wenn das Kontrollkästchen für einen Kurs aktiviert ist, dieser Kurs jedoch nicht in der `Instructor.Courses`-Navigationseigenschaft vorhanden ist, wird dieser der Auflistung in der Navigationseigenschaft hinzugefügt.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Wenn das Kontrollkästchen für einen Kurs aktiviert ist, dieser Kurs jedoch nicht in der `Instructor.Courses`-Navigationseigenschaft vorhanden ist, wird dieser aus der Auflistung in der Navigationseigenschaft gelöscht.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Fügen Sie in *views\instructor\edit.cshtml*ein Feld " **Courses** " mit einem Array von Kontrollkästchen hinzu, indem Sie den folgenden hervorgehobenen Code direkt nach den `div` Elementen für das `OfficeAssignment` Feld hinzufügen:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Dieser Code erstellt eine HTML-Tabelle mit drei Spalten. Jede Spalte enthält ein Kontrollkästchen gefolgt von einem Titel, der aus der Kursnummer und dem Kurstitel besteht. Die Kontrollkästchen haben alle den gleichen Namen ("selectedcourses"), der dem Modell Binder mitteilt, dass er als Gruppe behandelt werden soll. Das `value`-Attribut jedes Kontrollkästchens ist auf den Wert `CourseID.` festgelegt, wenn die Seite gepostet wird, übergibt die Modell Bindung ein Array an den Controller, der aus den `CourseID` Werten für die ausgewählten Kontrollkästchen besteht.

Wenn die Kontrollkästchen anfänglich gerendert werden, verfügen die für Kurse, die dem Dozenten zugewiesen sind, über `checked` Attribute, die diese auswählen (diese werden als aktiviert angezeigt).

Nachdem Sie Kurs Zuweisungen geändert haben, sollten Sie in der Lage sein, die Änderungen zu überprüfen, wenn die Website auf die `Index` Seite zurückkehrt. Daher müssen Sie der Tabelle auf dieser Seite eine Spalte hinzufügen. In diesem Fall müssen Sie das `ViewBag`-Objekt nicht verwenden, da die anzuzeigenden Informationen bereits in der `Courses`-Navigations Eigenschaft der `Instructor`-Entität vorhanden sind, die Sie als Modell an die Seite übergeben.

Fügen Sie in *views\instructor \index.cshtml*eine **Kurs** Überschrift direkt nach der **Office** -Überschrift hinzu, wie im folgenden Beispiel gezeigt:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Fügen Sie dann eine neue Detail Zelle direkt nach der Detail Zelle Bürostandort hinzu:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Führen Sie die Seite " **Instructor Index** " aus, um die den einzelnen Dozenten zugewiesenen Kurse anzuzeigen:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Klicken Sie in einem Dozenten auf **Bearbeiten** , um die Seite bearbeiten anzuzeigen.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Ändern Sie einige Kurs Zuweisungen, und klicken Sie auf **Speichern**. Die Änderungen werden auf der Indexseite angezeigt.

 Hinweis: der Ansatz zum Bearbeiten von Kurs Daten für Dozenten funktioniert gut, wenn eine begrenzte Anzahl von Kursen vorliegt. Bei umfangreicheren Auflistungen wären eine andere Benutzeroberfläche und eine andere Aktualisierungsmethode erforderlich.  

## <a name="update-the-delete-method"></a>Aktualisieren der Delete-Methode

Ändern Sie den Code in der HttpPost-Delete-Methode, sodass der Office-Zuweisungs Daten Satz (falls vorhanden) gelöscht wird, wenn der Dozenten gelöscht wird:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]

Wenn Sie versuchen, einen Dozenten zu löschen, der einer Abteilung als Administrator zugewiesen ist, erhalten Sie einen Fehler bei der referenziellen Integrität. In [der aktuellen Version dieses Tutorials](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) finden Sie zusätzlichen Code, mit dem der Dozenten automatisch aus einer Abteilung entfernt wird, in der der Dozenten als Administrator zugewiesen ist.

## <a name="summary"></a>Zusammenfassung

Nun haben Sie diese Einführung in die Arbeit mit verwandten Daten abgeschlossen. Bisher haben Sie in diesen Tutorials eine vollständige Palette von CRUD-Vorgängen durchgeführt, aber Sie haben keine Parallelitäts Probleme behandelt. Im nächsten Tutorial wird das Thema neben läufigkeits Optionen erläutert, die Optionen für die Verarbeitung erläutert und die Parallelitäts Behandlung für den CRUD-Code hinzugefügt, den Sie bereits für einen Entitätstyp geschrieben haben.

Links zu anderen Entity Framework Ressourcen finden Sie am Ende des [letzten Tutorials in dieser Reihe](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Zurück](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
