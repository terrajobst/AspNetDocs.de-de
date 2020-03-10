---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: Aktualisieren verwandter Daten mit EF in einer ASP.NET MVC-App'
description: In diesem Tutorial aktualisieren Sie verwandte Daten. Für die meisten Beziehungen kann dies durch Aktualisieren von Fremdschlüssel Feldern oder Navigations Eigenschaften erreicht werden.
author: tdykstra
ms.author: riande
ms.date: 01/19/2019
ms.topic: tutorial
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4d5f6447fdccefdcdf9497a9e94f23243302a0e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499299"
---
# <a name="tutorial-update-related-data-with-ef-in-an-aspnet-mvc-app"></a>Tutorial: Aktualisieren verwandter Daten mit EF in einer ASP.NET MVC-App

Im vorherigen Tutorial haben Sie verwandte Daten angezeigt. In diesem Tutorial aktualisieren Sie verwandte Daten. Für die meisten Beziehungen kann dies durch Aktualisieren von Fremdschlüssel Feldern oder Navigations Eigenschaften erreicht werden. Bei m:n-Beziehungen wird die jointabelle nicht direkt von der Entity Framework verfügbar gemacht, sodass Sie Entitäten zu und aus den entsprechenden Navigations Eigenschaften hinzufügen und daraus entfernen.

In den folgenden Abbildungen werden die Seiten dargestellt, mit denen Sie arbeiten werden.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Dozenten Bearbeitung mit Kursen](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Anpassen von Kursseiten
> * Seite "Office zu Dozenten hinzufügen"
> * Hinzufügen von Kursen zur Dozenten Seite
> * Aktualisieren von deleteconfirmed
> * Hinzufügen von einem Bürostandort und von Kursen zu der Seite „Erstellen“

## <a name="prerequisites"></a>Voraussetzungen

* [Lesen von relevanten Daten](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-courses-pages"></a>Anpassen von Kursseiten

Wenn eine neue Kursentität erstellt wird, muss diese in Beziehung zu einer vorhandenen Abteilung stehen. Um dies zu vereinfachen, enthält der Gerüstcode Controllermethoden und Ansichten zum „Erstellen“ und „Bearbeiten“, die eine Dropdownliste enthalten, aus denen der Fachbereich ausgewählt werden kann. In der Dropdown Liste wird die `Course.DepartmentID` Fremdschlüssel Eigenschaft festgelegt, und das ist alles Entity Framework Anforderungen, um die `Department` Navigations Eigenschaft mit der entsprechenden `Department` Entität zu laden. Verwenden Sie den Gerüstcode, aber nehmen Sie kleine Änderungen vor, um die Fehlerbehandlung hinzuzufügen und die Dropdownliste zu sortieren.

Löschen Sie in *CourseController.cs*die vier Methoden `Create` und `Edit`, und ersetzen Sie Sie durch den folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Fügen Sie am Anfang der Datei die folgende `using`-Anweisung hinzu:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Die `PopulateDepartmentsDropDownList`-Methode ruft eine Liste aller Abteilungen nach Namen sortiert ab, erstellt eine `SelectList` Auflistung für eine Dropdown Liste und übergibt die Auflistung an die Ansicht in einer `ViewBag`-Eigenschaft. Die Methode akzeptiert den optionalen `selectedDepartment`-Parameter, über den der Code das Element angeben kann, das ausgewählt wird, wenn die Dropdownliste gerendert wird. Die Ansicht übergibt den Namen `DepartmentID` an das [Dropdown List](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) -Hilfsprogramm, und das Hilfsprogramm weiß dann, dass das `ViewBag` Objekt nach einem `SelectList` namens `DepartmentID`suchen soll.

Die `HttpGet` `Create` Methode ruft die `PopulateDepartmentsDropDownList` Methode auf, ohne das ausgewählte Element festzulegen, da die Abteilung für einen neuen Kurs noch nicht eingerichtet wurde:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Mit der `HttpGet` `Edit`-Methode wird das ausgewählte Element auf Grundlage der ID der Abteilung festgelegt, die dem gerade bearbeiteten Kurs bereits zugewiesen ist:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

Die `HttpPost`-Methoden für `Create` und `Edit` enthalten außerdem Code, der das ausgewählte Element festlegt, wenn die Seite nach einem Fehler erneut angezeigt wird:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Mit diesem Code wird sichergestellt, dass wenn die Seite erneut angezeigt wird, um die Fehlermeldung anzuzeigen, die ausgewählte Abteilung ausgewählt bleibt.

In den Kurs Ansichten wird bereits ein Gerüst mit Dropdown Listen für das Feld "Department" angezeigt, aber Sie möchten die Beschriftung "DepartmentID" für dieses Feld nicht. nehmen Sie daher die folgende hervorgehobene Änderung an der Datei " *views\courcshtml* " vor, um die Beschriftung zu ändern.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Nehmen Sie die gleiche Änderung in *views\cours\edit.cshtml*vor.

Normalerweise erstellt das Gerüst einen Primärschlüssel nicht, da der Schlüsselwert von der Datenbank generiert wird und nicht geändert werden kann und kein sinnvoller Wert ist, der Benutzern angezeigt werden soll. Für Entitäten enthält das Gerüst ein Textfeld für das `CourseID` Feld, da es versteht, dass das Attribut "`DatabaseGeneratedOption.None`" bedeutet, dass der Benutzer den Primärschlüssel Wert eingeben kann. Es ist aber nicht zu verstehen, dass die Anzahl aussagekräftig ist, die Sie in den anderen Ansichten sehen möchten, sodass Sie Sie manuell hinzufügen müssen.

Fügen Sie in *views\cours\edit.cshtml*vor dem Feld **Titel** ein Feld für die Kursnummer hinzu. Da es sich um den Primärschlüssel handelt, wird er angezeigt, kann aber nicht geändert werden.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

In der Bearbeitungs Ansicht ist bereits ein ausgeblendetes Feld (`Html.HiddenFor`-Hilfsprogramm) für die Kursnummer vorhanden. Durch das Hinzufügen eines *HTML. LabelFor* -Hilfsprogramms entfällt das ausgeblendete Feld, da es nicht bewirkt, dass die Kursnummer in die veröffentlichten Daten eingeschlossen wird, wenn der Benutzer auf der Seite bearbeiten auf **Speichern** klickt.

Ändern Sie in " *views\cour\delete.cshtml* " und " *views\cours\details.cshtml*" die Bezeichnung "Abteilungs Name" von "Name" in "Department", und fügen Sie ein Feld für die Kursnummer vor dem Feld " **Titel** " hinzu.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Führen Sie die Seite **Erstellen** aus (zeigen Sie die Kursindex Seite an, klicken Sie auf **neu erstellen**), und geben Sie Daten für einen neuen Kurs ein:

| value | Einstellung |
| ----- | ------- |
| Number | Geben Sie *1000*ein. |
| Titel | Geben Sie *Algebra*ein. |
| Guthaben | Geben Sie *4*ein. |
|Department | Wählen Sie **Mathematik**aus. |

Klicken Sie auf **Erstellen**. Die Seite Index Seite wird mit dem neuen Kurs angezeigt, der der Liste hinzugefügt wird. Der Fachbereichsname in der Indexseitenliste wurde der Navigationseigenschaft entnommen und deutet darauf hin, dass die Beziehung ordnungsgemäß festgelegt wurde.

Führen Sie die **Bearbeitungs** Seite aus (zeigen Sie die Kursindex Seite an, und klicken Sie auf Kurs **Bearbeiten** ).

Ändern Sie die Daten auf der Seite, und klicken Sie auf **Speichern**. Die Kursindex Seite wird mit den aktualisierten Kursdaten angezeigt.

## <a name="add-office-to-instructors-page"></a>Seite "Office zu Dozenten hinzufügen"

Bei der Bearbeitung eines Dozentendatensatzes sollten Sie auch die Bürozuweisung des Dozenten aktualisieren. Die `Instructor`-Entität verfügt über eine 1:0-oder 1-Beziehung mit der `OfficeAssignment` Entität, was bedeutet, dass Sie die folgenden Situationen behandeln müssen:

- Wenn der Benutzer die Office-Zuweisung löscht und ursprünglich einen Wert enthielt, müssen Sie die `OfficeAssignment` Entität entfernen und löschen.
- Wenn der Benutzer einen Office-Zuweisungs Wert eingibt und dieser ursprünglich leer war, müssen Sie eine neue `OfficeAssignment` Entität erstellen.
- Wenn der Benutzer den Wert einer Office-Zuweisung ändert, müssen Sie den Wert in einer vorhandenen `OfficeAssignment` Entität ändern.

Öffnen Sie *InstructorController.cs* , und sehen Sie sich die `HttpGet` `Edit`-Methode an:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Der Gerüst Code hier ist nicht das, was Sie möchten. Es ist das Einrichten von Daten für eine Dropdown Liste, aber Sie benötigen ein Textfeld. Ersetzen Sie diese Methode durch den folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Mit diesem Code wird die `ViewBag`-Anweisung gelöscht und Eager Loading für die zugeordnete `OfficeAssignment` Entität hinzugefügt. Sie können Eager Loading nicht mit der `Find`-Methode ausführen, sodass die Methoden `Where` und `Single` stattdessen verwendet werden, um den Dozenten auszuwählen.

Ersetzen Sie die `HttpPost` `Edit`-Methode durch den folgenden Code. die Office-Zuweisungs Updates behandelt:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Für den Verweis auf `RetryLimitExceededException` ist eine `using` Anweisung erforderlich. um es hinzuzufügen, zeigen Sie mit der Maus auf `RetryLimitExceededException`. Die folgende Meldung wird angezeigt: ![ Wiederholungs Fehlermeldung](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Wählen Sie **mögliche Korrekturen anzeigen**aus, und verwenden Sie dann **System. Data. Entity. Infrastructure.**

![Wiederholungs Ausnahme auflösen](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Im Code werden folgende Schritte ausgeführt:

- Ändert den Methodennamen in `EditPost`, da die Signatur jetzt mit der `HttpGet`-Methode identisch ist (das `ActionName`-Attribut gibt an, dass die/Edit/-URL noch verwendet wird).
- Ruft die aktuelle Entität `Instructor` von der Datenbank über Eager Loading für die Navigationseigenschaft `OfficeAssignment` ab. Dies entspricht dem, was Sie in der `HttpGet` `Edit`-Methode getan haben.
- Aktualisiert die abgerufene Entität `Instructor` mit Werten aus der Modellbindung. Mit der verwendeten [tryupdatemodel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) -Überladung können Sie die Eigenschaften in die *Whitelist* aufnehmen, die Sie einschließen möchten. Dies verhindert eine Übertragung, wie im [zweiten Tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)erläutert wird.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Wenn der Bürostandort leer ist, wird die `Instructor.OfficeAssignment`-Eigenschaft auf NULL festgelegt, damit die verknüpfte Zeile in der `OfficeAssignment` Tabelle gelöscht wird.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Speichert die Änderungen in der Datenbank.

Fügen Sie in *views\instructor\edit.cshtml*nach den `div` Elementen für das Einstellungs **Datum** ein neues Feld zum Bearbeiten des Office-Speicher Orts hinzu:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Führen Sie die Seite aus (Wählen Sie die Registerkarte **Dozenten** aus, und klicken Sie dann auf auf einem Dozenten **Bearbeiten** ) Ändern Sie den **Standort des Büros**, und klicken Sie auf **Speichern**.

## <a name="add-courses-to-instructors-page"></a>Hinzufügen von Kursen zur Dozenten Seite

Dozenten können eine beliebige Anzahl von Kursen unterrichten. Nun verbessern Sie die Bearbeitungsseite für Dozenten, indem Sie die Möglichkeit zum Ändern von Kurs Zuweisungen mithilfe einer Gruppe von Kontrollkästchen hinzufügen.

Die Beziehung zwischen den Entitäten `Course` und `Instructor` ist m:n. das bedeutet, dass Sie keinen direkten Zugriff auf die Fremdschlüssel Eigenschaften haben, die in der jointabelle enthalten sind. Stattdessen fügen Sie Entitäten zu und aus der `Instructor.Courses` Navigations Eigenschaft hinzu und entfernen Sie daraus.

Die Benutzeroberfläche, über die Sie ändern können, welchen Kursen ein Dozent zugewiesen ist, besteht aus einer Reihe von Kontrollkästchen. Für jeden Kurs in der Datenbank wird ein Kontrollkästchen angezeigt. Die Kontrollkästchen, denen der Dozent zu diesem Zeitpunkt zugewiesen ist, sind aktiviert. Der Benutzer kann Kontrollkästchen aktivieren oder deaktivieren, um Kurszuweisungen zu ändern. Wenn die Anzahl der Kurse viel größer ist, sollten Sie wahrscheinlich eine andere Methode zum Darstellen der Daten in der Ansicht verwenden, aber Sie verwenden dieselbe Methode zum Bearbeiten von Navigations Eigenschaften, um Beziehungen zu erstellen oder zu löschen.

Verwenden Sie eine Ansichtsmodellklasse, um Daten für die Ansicht bereitzustellen, um eine Liste mit Kontrollkästchen zu erstellen. Erstellen Sie im Ordner " *ViewModels* " *AssignedCourseData.cs* , und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Ersetzen Sie in *InstructorController.cs*die `HttpGet` `Edit`-Methode durch den folgenden Code. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

Über den Code wird für die `Courses`-Navigationseigenschaft Eager Loading hinzugefügt und die neue `PopulateAssignedCourseData`-Methode aufgerufen, um über die Ansichtsmodellklasse `AssignedCourseData` Informationen für das Kontrollkästchenarray zur Verfügung zu stellen.

Der Code in der `PopulateAssignedCourseData`-Methode liest alle `Course` Entitäten, um eine Liste von Kursen mithilfe der Ansichts Modell Klasse zu laden. Für jeden Kurs überprüft der Code, ob dieser in der `Courses`-Navigationseigenschaft des Dozenten vorhanden ist. Zum Erstellen einer effizienten Suche bei der Überprüfung, ob ein Kurs dem Dozenten zugewiesen ist, werden die Kurse, die dem Dozenten zugewiesen sind, in eine [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) -Auflistung eingefügt. Die `Assigned`-Eigenschaft wird für Kurse, denen der Dozenten zugewiesen ist, auf `true` festgelegt. Die Ansicht verwendet diese Eigenschaft, um zu bestimmen, welche Kontrollkästchen als aktiviert angezeigt werden sollen. Schließlich wird die Liste an die Ansicht in einer `ViewBag`-Eigenschaft übermittelt.

Fügen Sie als nächstes den Code hinzu, der ausgeführt wird, wenn der Benutzer auf **Speichern** klickt. Ersetzen Sie die `EditPost`-Methode durch den folgenden Code, der eine neue Methode aufruft, die die `Courses`-Navigations Eigenschaft der `Instructor`-Entität aktualisiert. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

Die Methoden Signatur unterscheidet sich nun von der `HttpGet` `Edit`-Methode, sodass der Methodenname von `EditPost` zurück in `Edit`geändert wird.

Da die Sicht nicht über eine Auflistung von `Course` Entitäten verfügt, kann der Modell Binder die `Courses` Navigations Eigenschaft nicht automatisch aktualisieren. Anstatt den Modell Binder zu verwenden, um die `Courses` Navigations Eigenschaft zu aktualisieren, führen Sie dies in der neuen `UpdateInstructorCourses`-Methode aus. Aus diesem Grund müssen Sie die `Courses`-Eigenschaft von der Modellbindung ausschließen. Dies erfordert keine Änderung des Codes, der [tryupdatemodel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) aufruft, da Sie die *Whitelists* -Überladung verwenden und `Courses` nicht in der Include-Liste enthalten ist.

Wenn keine Kontrollkästchen ausgewählt wurden, initialisiert der Code in `UpdateInstructorCourses` die `Courses` Navigations Eigenschaft mit einer leeren Auflistung:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Der Code führt dann eine Schleife durch alle Kurse in der Datenbank aus und überprüft jeden Kurs im Hinblick auf die Kurse, die zu diesem Zeitpunkt dem Dozenten zugewiesen sind, und denen, die in der Ansicht aktiviert wurden. Die beiden letzten Auflistungen werden in `HashSet`-Objekten gespeichert, um Suchvorgänge effizienter zu gestalten.

Wenn das Kontrollkästchen für einen Kurs aktiviert ist, dieser Kurs jedoch nicht in der `Instructor.Courses`-Navigationseigenschaft vorhanden ist, wird dieser der Auflistung in der Navigationseigenschaft hinzugefügt.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Wenn das Kontrollkästchen für einen Kurs aktiviert ist, dieser Kurs jedoch nicht in der `Instructor.Courses`-Navigationseigenschaft vorhanden ist, wird dieser aus der Auflistung in der Navigationseigenschaft gelöscht.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Fügen Sie in *views\instructor\edit.cshtml*ein Feld " **Courses** " mit einem Array von Kontrollkästchen hinzu, indem Sie den folgenden Code direkt nach den `div` Elementen für das `OfficeAssignment` Feld und vor dem `div`-Element für die Schaltfläche " **Speichern** " hinzufügen:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Wenn Zeilenumbrüche und Einzüge nicht so aussehen wie hier, wenn Sie den Code einfügen, müssen Sie alle Elemente manuell korrigieren, damit Sie so aussehen, wie Sie hier sehen. Der Einzug muss nicht perfekt sein, die Zeilen `@</tr><tr>`, `@:<td>`, `@:</td>` und `@</tr>` müssen jedoch, wie dargestellt, jeweils in einer einzelnen Zeile stehen. Ansonsten wird ein Laufzeitfehler ausgelöst.

Dieser Code erstellt eine HTML-Tabelle mit drei Spalten. Jede Spalte enthält ein Kontrollkästchen gefolgt von einem Titel, der aus der Kursnummer und dem Kurstitel besteht. Die Kontrollkästchen haben alle den gleichen Namen ("selectedcourses"), der dem Modell Binder mitteilt, dass er als Gruppe behandelt werden soll. Das `value`-Attribut jedes Kontrollkästchens ist auf den Wert `CourseID.` festgelegt, wenn die Seite gepostet wird, übergibt die Modell Bindung ein Array an den Controller, der aus den `CourseID` Werten für die ausgewählten Kontrollkästchen besteht.

Wenn die Kontrollkästchen anfänglich gerendert werden, verfügen die für Kurse, die dem Dozenten zugewiesen sind, über `checked` Attribute, die diese auswählen (diese werden als aktiviert angezeigt).

Nachdem Sie Kurs Zuweisungen geändert haben, sollten Sie in der Lage sein, die Änderungen zu überprüfen, wenn die Website auf die `Index` Seite zurückkehrt. Daher müssen Sie der Tabelle auf dieser Seite eine Spalte hinzufügen. In diesem Fall müssen Sie das `ViewBag`-Objekt nicht verwenden, da die anzuzeigenden Informationen bereits in der `Courses`-Navigations Eigenschaft der `Instructor`-Entität vorhanden sind, die Sie als Modell an die Seite übergeben.

Fügen Sie in *views\instructor \index.cshtml*eine **Kurs** Überschrift direkt nach der **Office** -Überschrift hinzu, wie im folgenden Beispiel gezeigt:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Fügen Sie dann eine neue Detail Zelle direkt nach der Detail Zelle Bürostandort hinzu:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Führen Sie die Seite **Instructor Index** aus, um die Kurse anzuzeigen, die den einzelnen Dozenten zugewiesen sind.

Klicken Sie in einem Dozenten auf **Bearbeiten** , um die Seite bearbeiten anzuzeigen.

Ändern Sie einige Kurs Zuweisungen, und klicken Sie auf **Speichern**. Die Änderungen werden auf der Indexseite angezeigt.

 Hinweis: der hier vorgenommene Ansatz zum Bearbeiten von Kurs Daten für Dozenten funktioniert gut, wenn eine begrenzte Anzahl von Kursen vorliegt. Bei umfangreicheren Auflistungen wären eine andere Benutzeroberfläche und eine andere Aktualisierungsmethode erforderlich.

## <a name="update-deleteconfirmed"></a>Aktualisieren von deleteconfirmed

Löschen Sie in *InstructorController.cs*die `DeleteConfirmed`-Methode, und fügen Sie den folgenden Code an der Stelle ein.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Mit diesem Code wird die folgende Änderung vorgenommen:

- Wenn der Dozenten als Administrator einer beliebigen Abteilung zugewiesen ist, entfernt die Dozenten Zuweisung aus dieser Abteilung. Ohne diesen Code erhalten Sie einen Fehler bei der referenziellen Integrität, wenn Sie versuchen, einen Dozenten zu löschen, der als Administrator für eine Abteilung zugewiesen wurde.

Dieser Code behandelt nicht das Szenario eines Dozenten, der als Administrator für mehrere Abteilungen zugewiesen ist. Im letzten Tutorial fügen Sie Code hinzu, der das Auftreten dieses Szenarios verhindert.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Hinzufügen von einem Bürostandort und von Kursen zu der Seite „Erstellen“

Löschen Sie in *InstructorController.cs*die `HttpGet`-und `HttpPost` `Create`-Methoden, und fügen Sie dann den folgenden Code an ihrem Speicherort ein:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Dieser Code ähnelt dem, was Sie für die Bearbeitungsmethoden gesehen haben, mit dem Unterschied, dass anfänglich keine Kurse ausgewählt werden. Die `HttpGet` `Create`-Methode ruft die `PopulateAssignedCourseData`-Methode nicht auf, da möglicherweise Kurse ausgewählt sind, aber eine leere Auflistung für die `foreach` Schleife in der Sicht bereitgestellt wird (andernfalls würde der Ansichts Code eine NULL-Verweis Ausnahme auslösen).

Die HttpPost-Methode "Create" fügt jeden ausgewählten Kurs der Navigations Eigenschaft "Kurse" vor dem Vorlagen Code hinzu, der auf Validierungs Fehler überprüft und der Datenbank den neuen Dozenten hinzufügt. Kurse werden auch dann hinzugefügt, wenn es Modell Fehler gibt, sodass beim Auftreten von Modell Fehlern (z. b. wenn der Benutzer ein ungültiges Datum eingegeben hat) eine automatische Wiederherstellung der Seite durchgeführt wird, wenn die Seite erneut mit einer Fehlermeldung angezeigt wird.

Beachten Sie, dass Sie die `Courses`-Navigationseigenschaft als leere Auflistung initialisieren müssen, wenn Sie dieser Kurse hinzufügen möchten:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Wenn Sie dies nicht im Controllercode durchführen möchten, können Sie dies auch im Dozentenmodell tun, indem Sie den Eigenschaftengetter ändern, um falls nötig automatisch die Auflistung zu erstellen. Dies wird im folgenden Code dargestellt:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Wenn Sie die `Courses`-Eigenschaft auf diese Weise ändern, können Sie den expliziten Code zum Initialisieren der Eigenschaft aus dem Controller entfernen.

Fügen Sie in " *views\instructor\kreate.cshtml*" nach dem Feld "Einstellungs Datum" und vor der Schaltfläche " **senden** " ein Textfeld und einen Kurs für das Büro hinzu.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Nachdem Sie den Code eingefügt haben, korrigieren Sie Zeilenumbrüche und Einzug wie zuvor bei der Seite bearbeiten.

Führen Sie die Seite erstellen aus, und fügen Sie einen Dozenten hinzu.

<a id="transactions"></a>

## <a name="handling-transactions"></a>Behandeln von Transaktionen

Wie im Lernprogramm [grundlegende CRUD-Funktionalität](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)erläutert, implementiert die Entity Framework standardmäßig Transaktionen. Szenarios, in denen Sie mehr Kontrolle benötigen, z. b. Wenn Sie Vorgänge einschließen möchten, die außerhalb der Entity Framework in einer Transaktion ausgeführt werden, finden Sie unter [Arbeiten mit Transaktionen](https://msdn.microsoft.com/data/dn456843) auf MSDN.

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Links zu anderen Entity Framework Ressourcen finden Sie unter [ASP.NET Data Access-Empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-step"></a>Nächster Schritt

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Angepasste Kursseiten
> * Seite "Office zu Dozenten hinzugefügt"
> * Kurse zur Seite "Dozenten" hinzugefügt
> * Aktualisierte deleteconfirmed
> * Hinzufügen von Bürostandort und-Kursen zur Seite "erstellen"

Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie ein asynchrones Programmiermodell implementiert wird.
> [!div class="nextstepaction"]
> [Asynchrones Programmiermodell](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
