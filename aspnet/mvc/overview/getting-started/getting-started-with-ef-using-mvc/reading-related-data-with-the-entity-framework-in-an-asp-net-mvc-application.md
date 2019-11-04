---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: Lesen verwandter Daten mit EF in einer ASP.NET MVC-App'
description: In diesem Tutorial werden verwandte Daten gelesen und angezeigt – d. h. Daten, die vom Entity Framework in Navigations Eigenschaften geladen werden.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d804c8dd45ad131949260c85d9d9c6683bfe9646
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445659"
---
# <a name="tutorial-read-related-data-with-ef-in-an-aspnet-mvc-app"></a>Tutorial: Lesen verwandter Daten mit EF in einer ASP.NET MVC-App

Im vorherigen Tutorial haben Sie das Datenmodell "School" abgeschlossen. In diesem Tutorial werden verwandte Daten gelesen und angezeigt – d. h. Daten, die vom Entity Framework in Navigations Eigenschaften geladen werden.

Die folgenden Abbildungen zeigen die Seiten, mit den Sie arbeiten werden.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 5-Anwendungen mithilfe der Entity Framework 6 Code First und Visual Studio erstellt werden. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

In diesem Tutorial:

> [!div class="checklist"]
> * Erfahren Sie, wie verwandte Daten geladen werden.
> * Erstellen Sie eine Seite „Kurse“
> * Erstellen Sie eine Seite „Instructors“

## <a name="prerequisites"></a>Erforderliche Voraussetzungen

* [Erstellen eines komplexeren Datenmodells](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="learn-how-to-load-related-data"></a>Erfahren Sie, wie verwandte Daten geladen werden.

Es gibt mehrere Möglichkeiten, wie die Entity Framework verknüpfte Daten in die Navigations Eigenschaften einer Entität laden kann:

- *Lazy Loading (verzögertes Laden)* . Wenn die Entität zuerst gelesen wird, werden verwandte Daten nicht abgerufen. Wenn Sie jedoch zum ersten Mal versuchen, auf eine Navigationseigenschaft zuzugreifen, werden die für diese Navigationseigenschaft erforderlichen Daten automatisch abgerufen. Dies führt dazu, dass mehrere Abfragen an die Datenbank gesendet werden – eine für die Entität selbst und eine, wenn verknüpfte Daten für die Entität abgerufen werden müssen. Die `DbContext`-Klasse aktiviert standardmäßig Lazy Loading.

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Eager Loading (vorzeitiges Laden)* . Wenn die Entität gelesen wird, werden ihre verwandten Daten mit ihr abgerufen. Dies führt normalerweise zu einer einzelnen Joinabfrage, die alle Daten abruft, die erforderlich sind. Sie geben Eager Loading mithilfe der `Include`-Methode an.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Explizites Laden*. Dies ähnelt Lazy Loading, mit dem Unterschied, dass Sie die zugehörigen Daten explizit im Code abrufen. Dies geschieht nicht automatisch, wenn Sie auf eine Navigations Eigenschaft zugreifen. Sie laden verknüpfte Daten manuell, indem Sie den Eintrag Objekt Zustands-Manager für eine Entität abrufen und die [Collection. Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) -Methode für Auflistungen oder die [Reference. Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) -Methode für Eigenschaften aufrufen, die eine einzelne Entität enthalten. (Im folgenden Beispiel würden Sie, wenn Sie die Administrator Navigations Eigenschaft laden möchten, `Collection(x => x.Courses)` durch `Reference(x => x.Administrator)`ersetzen.) In der Regel verwenden Sie Explizites Laden nur dann, wenn Sie Lazy Loading deaktiviert haben.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Da Sie die Eigenschaftswerte nicht sofort abrufen, werden Lazy Loading und explizites laden auch als *Verzögertes Laden*bezeichnet.

### <a name="performance-considerations"></a>Überlegungen zur Leistung

Wenn Sie wissen, dass Sie für jede abgerufene Entität verwandte Daten benötigen, bietet Eager Loading häufig die beste Leistung, da eine einzelne Abfrage, die an die Datenbank gesendet wird, in der Regel effizienter ist als separate Abfragen für jede abgerufene Entität. Nehmen Sie beispielsweise in den obigen Beispielen an, dass jede Abteilung zehn Verwandte Kurse hat. Das Eager Loading Beispiel würde zu einer einzelnen (Join-) Abfrage und einem einzelnen Roundtrip zur Datenbank führen. Die Lazy Loading-und expliziten Lade Beispiele führen zu elf Abfragen und elf Roundtrips zur Datenbank. Die zusätzlichen Roundtrips zur Datenbank beeinträchtigen die Leistung besonders bei hoher Latenz.

In einigen Szenarios ist Lazy Loading hingegen effizienter. Das unverzügliches Laden kann dazu führen, dass ein sehr komplexer Join generiert wird, der SQL Server nicht effizient verarbeiten kann. Wenn Sie nur für eine Teilmenge einer Gruppe von Entitäten, die Sie verarbeiten, auf die Navigations Eigenschaften einer Entität zugreifen müssen, Lazy Loading möglicherweise eine bessere Leistung, da Eager Loading mehr Daten abrufen, als Sie benötigen. Wenn die Leistung wichtig ist, empfiehlt es sich, die Leistung mit beiden Möglichkeiten zu testen, um die beste Wahl treffen zu können.

Lazy Load kann Code maskieren, der Leistungsprobleme verursacht. Beispielsweise kann es vorkommen, dass Code, der nicht das sorgfältige oder explizite laden angibt, aber eine große Menge an Entitäten verarbeitet und in jeder Iterationen mehrere Navigations Eigenschaften verwendet, möglicherweise sehr ineffizient ist (aufgrund vieler Roundtrips zur Datenbank). Eine Anwendung, die eine gute Leistung bei der Entwicklung mit einer lokalen SQL Server-Datenbank erzielt, kann bei der Umstellung auf die Azure SQL-Datenbank aufgrund erhöhter Latenz und Lazy Loading Leistungsprobleme verursachen. Durch die Profilerstellung für die Datenbankabfragen mit einem realistischen Test Ladevorgang können Sie ermitteln, ob Lazy Loading geeignet ist. Weitere Informationen finden Sie unter [Demystifying Entity Framework Strategien: Laden](https://msdn.microsoft.com/magazine/hh205756.aspx) von verknüpften Daten und [Verwenden des Entity Framework, um die Netzwerk Latenz zu SQL Azure zu verringern](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Deaktivieren von Lazy Loading vor der Serialisierung

Wenn Sie Lazy Loading während der Serialisierung aktiviert lassen, können Sie am Ende deutlich mehr Daten Abfragen als beabsichtigt. Die Serialisierung funktioniert in der Regel durch den Zugriff auf jede Eigenschaft einer Instanz eines Typs. Eigenschafts Zugriffs Trigger Lazy Loading, und diese Lazy Loaded-Entitäten werden serialisiert. Der Serialisierungsprozess greift dann auf jede Eigenschaft der verzögert geladenen Entitäten zu, was möglicherweise noch mehr Lazy Loading und die Serialisierung verursacht. Um dies zu verhindern, sollten Sie Lazy Loading deaktivieren, bevor Sie eine Entität serialisieren.

Die Serialisierung kann auch durch die vom Entity Framework verwendeten Proxy Klassen kompliziert werden, wie im [Tutorial Advanced Szenarios](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies)erläutert.

Eine Möglichkeit, um Serialisierungsprobleme zu vermeiden, ist das Serialisieren von DTOs (Data Transfer Objects) anstelle von Entitäts Objekten, wie im Tutorial [using Web API with Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) gezeigt.

Wenn Sie keine DTOs verwenden, können Sie Lazy Loading deaktivieren und Proxy Probleme vermeiden, indem Sie die [Proxy Erstellung deaktivieren](https://msdn.microsoft.com/data/jj592886.aspx).

Im folgenden finden [Sie weitere Möglichkeiten, Lazy Loading zu deaktivieren](https://msdn.microsoft.com/data/jj574232):

- Beim Deklarieren der Eigenschaft können Sie für bestimmte Navigations Eigenschaften das `virtual` Schlüsselwort weglassen.
- Legen Sie für alle Navigations Eigenschaften `LazyLoadingEnabled` auf `false`fest, und fügen Sie den folgenden Code in den Konstruktor der Kontext Klasse ein:

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page"></a>Erstellen Sie eine Seite „Kurse“

Die `Course`-Entität enthält eine Navigations Eigenschaft, die die `Department` Entität der Abteilung enthält, der der Kurs zugewiesen ist. Um den Namen der zugewiesenen Abteilung in einer Liste von Kursen anzuzeigen, müssen Sie die `Name`-Eigenschaft von der `Department`-Entität, die sich in der `Course.Department`-Navigations Eigenschaft befindet, erhalten.

Erstellen Sie für den `Course`-Entitätstyp einen Controller mit dem Namen `CourseController` (nicht coursescontroller), und verwenden Sie die gleichen Optionen für den **MVC 5-Controller mit Ansichten. verwenden Sie Entity Framework** Gerüst, das Sie zuvor für den `Student` Controller verwendet haben:

| Einstellung | Wert |
| ------- | ----- |
| Modell Klasse | Wählen Sie **Kurs (Conto souniversity. Models)** aus. |
| Datenkontext Klasse | Wählen Sie **schoolContext (condesouniversity. DAL)** aus. |
| Controller Name | Geben Sie *coursecontroller*ein. Auch hier, nicht auf *coursescontroller* mit *s*. Wenn Sie **Kurs (conysouniversity. Models)** ausgewählt haben, wurde der Wert des **Controller namens** automatisch aufgefüllt. Sie müssen den Wert ändern. |

Überlassen Sie die anderen Standardwerte, und fügen Sie den Controller hinzu.

Öffnen Sie " *controllers\coursecontroller.cs* ", und sehen Sie sich die `Index`-Methode an:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Der automatische Gerüstbau hat mithilfe der `Include`-Methode ein Eager Loading für die `Department`-Navigationseigenschaft angegeben.

Öffnen Sie *views\course\index.cshtml* , und ersetzen Sie den Vorlagen Code durch den folgenden Code. Die Änderungen werden hervorgehoben:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Sie haben die folgenden Änderungen am eingerüsteten Code vorgenommen:

- Die Überschrift von **Index** in **Kurse**wurde geändert.
- Die Spalte **Anzahl** wurde hinzugefügt. Sie zeigt den `CourseID`-Eigenschaftswert an. Standardmäßig werden Primärschlüssel nicht mit einem Gerüst gefüllt, da Sie normalerweise für Endbenutzer bedeutungslos sind. Allerdings ist in diesem Fall der Primärschlüssel sinnvoll, und Sie möchten ihn anzeigen.
- Die **Abteilungs** Spalte wurde auf die Rechte Seite verschoben und die Überschrift geändert. Das Gerüst hat ordnungsgemäß ausgewählt, dass die `Name`-Eigenschaft aus der `Department`-Entität angezeigt wird, aber hier auf der Kursseite sollte die Spaltenüberschrift " **Department** " und nicht " **Name**" lauten

Beachten Sie, dass der Gerüst Code für die Department-Spalte die `Name`-Eigenschaft der `Department` Entität anzeigt, die in die `Department`-Navigations Eigenschaft geladen wird:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Führen Sie die Seite aus (Wählen Sie auf der Startseite der Configuration Manager-Homepage die Registerkarte **Kurse** aus), um die Liste mit den Abteilungsnamen anzuzeigen.

## <a name="create-an-instructors-page"></a>Erstellen Sie eine Seite „Instructors“

In diesem Abschnitt erstellen Sie einen Controller und eine Ansicht für die `Instructor` Entität, um die Seite Dozenten anzuzeigen. Auf dieser Seite werden verwandte Daten auf folgende Weise gelesen und angezeigt:

- Die Liste der Dozenten zeigt verwandte Daten aus der `OfficeAssignment` Entität an. Die `Instructor`- und `OfficeAssignment`-Entitäten stehen in einer 1:0..1-Beziehung zueinander. Sie verwenden Eager Loading für die `OfficeAssignment` Entitäten. Wie zuvor erläutert, ist Eager Loading in der Regel effizienter, wenn Sie die verwandten Daten für alle abgerufenen Zeilen der primären Tabelle benötigen. In diesem Fall sollten Sie die Office-Anweisungen für alle angezeigten Dozenten anzeigen.
- Wenn der Benutzer einen Dozenten auswählt, werden zugehörige `Course`-Entitäten angezeigt. Die `Instructor`- und `Course`-Entitäten stehen in einer m:n-Beziehung zueinander. Sie verwenden Eager Loading für die `Course` Entitäten und ihre zugehörigen `Department` Entitäten. In diesem Fall sind Lazy Loading möglicherweise effizienter, da Sie nur Kurse für den ausgewählten Dozenten benötigen. Dieses Beispiel zeigt jedoch, wie Eager Loading für Navigationseigenschaften in Entitäten in den Navigationseigenschaften verwendet wird.
- Wenn der Benutzer einen Kurs auswählt, werden verwandte Daten aus der `Enrollments` Entitätenmenge angezeigt. Die `Course`- und `Enrollment`-Entitäten stehen in einer 1:n-Beziehung zueinander. Sie fügen Explizites Laden für `Enrollment` Entitäten und ihre zugehörigen `Student` Entitäten hinzu. (Explizites Laden ist nicht erforderlich, da Lazy Loading aktiviert ist, aber dies zeigt, wie Explizites Laden durchzuführen ist.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Erstellen eines Ansichts Modells für die Ansicht "Dozenten Index"

Auf der Seite Dozenten werden drei verschiedene Tabellen angezeigt. Aus diesem Grund erstellen Sie ein Ansichtsmodell, das drei Eigenschaften enthält. Jede enthält Daten für eine der Tabellen.

Erstellen Sie im *ViewModels* -Ordner *InstructorIndexData.cs* , und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Erstellen des Dozenten Controllers und der Ansichten

Erstellen Sie mit der EF-Lese-/Schreibaktion einen `InstructorController`-Controller (kein Instructor-Controller):

| Einstellung | Wert |
| ------- | ----- |
| Modell Klasse | Wählen Sie **Instructor (Conto souniversity. Models)** aus. |
| Datenkontext Klasse | Wählen Sie **schoolContext (condesouniversity. DAL)** aus. |
| Controller Name | Geben Sie *Instructor Controller*ein. Auch hier *gilt: nicht*für " *instrutoriscontroller* " mit. Wenn Sie **Kurs (conysouniversity. Models)** ausgewählt haben, wurde der Wert des **Controller namens** automatisch aufgefüllt. Sie müssen den Wert ändern. |

Überlassen Sie die anderen Standardwerte, und fügen Sie den Controller hinzu.

Öffnen Sie " *controllers\instructor Controller.cs* ", und fügen Sie eine `using`-Anweisung für den `ViewModels`-Namespace hinzu:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Der Gerüst Code in der `Index`-Methode gibt Eager Loading nur für die Navigations Eigenschaft `OfficeAssignment` an:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Ersetzen Sie die `Index`-Methode durch den folgenden Code, um zusätzliche verwandte Daten zu laden und in das Ansichts Modell einzufügen:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Die-Methode akzeptiert optionale Routendaten (`id`) und einen Abfrage Zeichen folgen Parameter (`courseID`), die die ID-Werte des ausgewählten Dozenten und des ausgewählten Kurses bereitstellen, und übergibt alle erforderlichen Daten an die Ansicht. Die Parameter werden durch die **Auswählen**-Links auf der Seite bereitgestellt.

Der Code erstellt zuerst eine Instanz des Ansichtsmodells und fügt die Dozentenliste ein. Der Code gibt Eager Loading für die `Instructor.OfficeAssignment` und die `Instructor.Courses` Navigations Eigenschaft an.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

Mit dem zweiten `Include`-Methode werden Kurse geladen, und für jeden Kurs, der geladen wird, wird für die Navigations Eigenschaft `Course.Department` Eager Loading.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Wie bereits erwähnt, ist Eager Loading nicht erforderlich, wird jedoch zur Verbesserung der Leistung ausgeführt. Da die Sicht immer die `OfficeAssignment` Entität erfordert, ist es effizienter, Sie in derselben Abfrage abzurufen. `Course` Entitäten sind erforderlich, wenn ein Dozent auf der Webseite ausgewählt wird. Eager Loading ist daher besser als Lazy Loading, wenn die Seite häufiger angezeigt wird, wenn ein Kurs ausgewählt ist als ohne.

Wenn eine Dozenten-ID ausgewählt wurde, wird der ausgewählte Dozenten aus der Liste der Dozenten im Ansichts Modell abgerufen. Die `Courses`-Eigenschaft des Ansichts Modells wird dann mit den `Course` Entitäten aus der `Courses` Navigations Eigenschaft dieses Dozenten geladen.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Die `Where`-Methode gibt eine Auflistung zurück, aber in diesem Fall führen die an diese Methode über gebenden Kriterien dazu, dass nur eine einzige `Instructor` Entität zurückgegeben wird. Die `Single`-Methode konvertiert die Auflistung in eine einzelne `Instructor` Entität, die Ihnen den Zugriff auf die `Courses`-Eigenschaft dieser Entität ermöglicht.

Sie verwenden die [einzige](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) Methode für eine Auflistung, wenn Sie wissen, dass die Sammlung nur ein Element enthält. Die `Single`-Methode löst eine Ausnahme aus, wenn die an Sie weiter gegebene Auflistung leer ist oder wenn mehr als ein Element vorhanden ist. Eine Alternative ist " [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx)", die einen Standardwert zurückgibt (in diesem Fall`null`), wenn die Auflistung leer ist. In diesem Fall führt dies jedoch immer noch zu einer Ausnahme (von der Suche nach einer `Courses`-Eigenschaft für einen `null` Verweis), und die Ausnahme Meldung würde die Ursache des Problems weniger eindeutig angeben. Wenn Sie die `Single`-Methode aufrufen, können Sie auch die `Where` Bedingung übergeben, anstatt die `Where`-Methode separat aufzurufen:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

anstelle von:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Wenn ein Kurs ausgewählt wurde, wird der ausgewählte Kurs aus der Kursliste im Ansichtsmodell abgerufen. Anschließend wird die `Enrollments`-Eigenschaft des Ansichts Modells mit den `Enrollment` Entitäten aus der `Enrollments` Navigations Eigenschaft dieses Kurses geladen.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Ändern der Ansicht "Dozenten Index"

Ersetzen Sie in *views\instructor \index.cshtml*den Vorlagen Code durch den folgenden Code. Die Änderungen werden hervorgehoben:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Sie haben die folgenden Änderungen am bestehenden Code vorgenommen:

- Die Modellklasse wurde zu `InstructorIndexData` geändert.
- Der Seitenname wurde von **Index** in **Dozenten** geändert.
- Es wurde eine **Office** -Spalte hinzugefügt, die nur `item.OfficeAssignment.Location` anzeigt, wenn `item.OfficeAssignment` nicht NULL ist. (Da dies eine 1:0-oder 1-Beziehung ist, gibt es möglicherweise keine Verwandte `OfficeAssignment` Entität.)

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Es wurde Code hinzugefügt, mit dem `class="success"` dem `tr`-Element des ausgewählten Dozenten dynamisch hinzugefügt wird. Dadurch wird eine Hintergrundfarbe für die ausgewählte Zeile mithilfe einer [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) -Klasse festgelegt.

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Es wurde eine neue `ActionLink` mit der Bezeichnung **Select** direkt vor den anderen Links in den einzelnen Zeilen hinzugefügt, die bewirkt, dass die ausgewählte Dozenten-ID an die `Index`-Methode gesendet wird.

Führen Sie die Anwendung aus, und wählen Sie die Registerkarte **Dozenten** . Die Seite zeigt die `Location`-Eigenschaft verwandter `OfficeAssignment` Entitäten und eine leere Tabellenzelle an, wenn keine Verwandte `OfficeAssignment` Entität vorhanden ist.

Fügen Sie in der Datei " *views\instructor \index.cshtml* " nach dem schließenden `table` Element (am Ende der Datei) den folgenden Code hinzu. Dieser Code zeigt eine Liste der Kurse an, die im Zusammenhang mit einem Dozenten stehen, wenn ein Dozent ausgewählt wird.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Dieser Code liest die `Courses`-Eigenschaft des Ansichtsmodells, um eine Kursliste anzuzeigen. Außerdem wird ein `Select` Hyperlink bereitstellt, der die ID des ausgewählten Kurses an die `Index` Aktionsmethode sendet.

Führen Sie die Seite aus, und wählen Sie einen Dozenten. Jetzt sehen Sie ein Raster, das die dem Dozenten zugewiesenen Kurse anzeigt. Sie sehen auch den Namen der zugewiesenen Abteilung für jeden Kurs.

Fügen Sie den folgenden Code hinzu, nachdem Sie den Codeblock hinzugefügt haben. Dies zeigt eine Liste der Studenten an, die im Kurs registriert sind, wenn dieser Kurs ausgewählt ist.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Mit diesem Code wird die `Enrollments`-Eigenschaft des Ansichts Modells gelesen, um eine Liste der im Kurs registrierten Studenten anzuzeigen.

Führen Sie die Seite aus, und wählen Sie einen Dozenten. Wählen Sie dann einen Kurs aus, um die Liste der registrierten Studenten und deren Noten einzusehen.

### <a name="adding-explicit-loading"></a>Explizites Laden

Öffnen Sie *InstructorController.cs* , und sehen Sie sich an, wie die `Index`-Methode die Liste der Registrierungen für einen ausgewählten Kurs abruft:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Beim Abrufen der Liste der Dozenten haben Sie Eager Loading für die `Courses` Navigations Eigenschaft und für die Eigenschaft `Department` der einzelnen Kurse angegeben. Dann fügen Sie die `Courses`-Auflistung in das Ansichts Modell ein, und nun greifen Sie auf die `Enrollments`-Navigations Eigenschaft aus einer Entität in dieser Auflistung zu. Da Sie Eager Loading für die `Course.Enrollments`-Navigations Eigenschaft nicht angegeben haben, werden die Daten aus dieser Eigenschaft als Ergebnis von Lazy Loading auf der Seite angezeigt.

Wenn Sie Lazy Loading deaktiviert haben, ohne den Code auf andere Weise zu ändern, wäre die `Enrollments`-Eigenschaft unabhängig von der Anzahl der Registrierungen, die der Kurs tatsächlich hatte, NULL. Wenn Sie die `Enrollments`-Eigenschaft laden möchten, müssten Sie entweder Eager Loading oder explizites Laden angeben. Sie haben bereits gesehen, wie Sie Eager Loading. Um ein Beispiel für explizites laden anzuzeigen, ersetzen Sie die `Index`-Methode durch den folgenden Code, der die `Enrollments`-Eigenschaft explizit lädt. Der geänderte Code wird hervorgehoben.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Nachdem Sie die ausgewählte `Course` Entität erhalten haben, lädt der neue Code die `Enrollments` Navigations Eigenschaft dieses Kurses explizit:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Anschließend werden die verknüpften `Student` Entitäten jeder `Enrollment` Entität explizit geladen:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Beachten Sie, dass Sie die `Collection`-Methode verwenden, um eine Auflistungs Eigenschaft zu laden, aber für eine Eigenschaft, die nur eine Entität enthält, verwenden Sie die `Reference`-Methode.

Führen Sie die Seite "Instructor Index" jetzt aus, und Sie sehen keinen Unterschied in der Anzeige auf der Seite, obwohl Sie geändert haben, wie die Daten abgerufen werden.

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Links zu anderen Entity Framework Ressourcen finden Sie unter [ASP.NET Data Access-Empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial:

> [!div class="checklist"]
> * Haben Sie gelernt, wie verwandte Daten geladen werden
> * Wurde eine Seite „Kurse“ erstellt
> * Wurde eine Seite „Instructors“ erstellt

Wechseln Sie zum nächsten Artikel, um mehr über das Aktualisieren zugehöriger Daten zu erfahren.

> [!div class="nextstepaction"]
> [Aktualisieren relevanter Daten](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
