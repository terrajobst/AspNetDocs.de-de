---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Lesen verwandter Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung (5 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio erstellt werden...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e1752022b66952783039fbea0c2880e2f9be6ef8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595211"
---
# <a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Lesen verwandter Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung (5 von 10)

von [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio 2012 erstellt werden. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe von Anfang an starten oder [ein Starter-Projekt für dieses Kapitel herunterladen](building-the-ef5-mvc4-chapter-downloads.md) und hier beginnen.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem stoßen, das Sie nicht beheben können, [Laden Sie das abgeschlossene Kapitel herunter](building-the-ef5-mvc4-chapter-downloads.md) , und versuchen Sie, das Problem zu reproduzieren. Im Allgemeinen können Sie die Lösung für das Problem finden, indem Sie Ihren Code mit dem abgeschlossenen Code vergleichen. Informationen zu häufigen Fehlern und deren Lösung finden Sie unter [Fehler und](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors) Problem Umgehungen.

Im vorherigen Tutorial haben Sie das Datenmodell "School" abgeschlossen. In diesem Tutorial werden verwandte Daten gelesen und angezeigt – d. h. Daten, die vom Entity Framework in Navigations Eigenschaften geladen werden.

Die folgenden Abbildungen zeigen die Seiten, mit den Sie arbeiten werden.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Lazy, eifrig und Explizites Laden verwandter Daten

Es gibt mehrere Möglichkeiten, wie die Entity Framework verknüpfte Daten in die Navigations Eigenschaften einer Entität laden kann:

- *Lazy Loading (verzögertes Laden)* . Wenn die Entität zuerst gelesen wird, werden verwandte Daten nicht abgerufen. Wenn Sie jedoch zum ersten Mal versuchen, auf eine Navigationseigenschaft zuzugreifen, werden die für diese Navigationseigenschaft erforderlichen Daten automatisch abgerufen. Dies führt dazu, dass mehrere Abfragen an die Datenbank gesendet werden – eine für die Entität selbst und eine, wenn verknüpfte Daten für die Entität abgerufen werden müssen. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Eager Loading (vorzeitiges Laden)* . Wenn die Entität gelesen wird, werden ihre verwandten Daten mit ihr abgerufen. Dies führt normalerweise zu einer einzelnen Joinabfrage, die alle Daten abruft, die erforderlich sind. Sie geben Eager Loading mithilfe der `Include`-Methode an.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Explizites Laden*. Dies ähnelt Lazy Loading, mit dem Unterschied, dass Sie die zugehörigen Daten explizit im Code abrufen. Dies geschieht nicht automatisch, wenn Sie auf eine Navigations Eigenschaft zugreifen. Sie laden verknüpfte Daten manuell, indem Sie den Eintrag Objekt Zustands-Manager für eine Entität abrufen und die `Collection.Load`-Methode für Auflistungen oder die `Reference.Load`-Methode für Eigenschaften aufrufen, die eine einzelne Entität enthalten. (Im folgenden Beispiel würden Sie, wenn Sie die Administrator Navigations Eigenschaft laden möchten, `Collection(x => x.Courses)` durch `Reference(x => x.Administrator)`ersetzen.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Da Sie die Eigenschaftswerte nicht sofort abrufen, werden Lazy Loading und explizites laden auch als *Verzögertes Laden*bezeichnet.

Wenn Sie wissen, dass Sie verwandte Daten für jede abgerufene Entität benötigen, bietet Eager Loading im Allgemeinen die beste Leistung, da eine einzelne Abfrage, die an die Datenbank gesendet wird, in der Regel effizienter als separate Abfragen für jede abgerufene Entität ist. Nehmen Sie beispielsweise in den obigen Beispielen an, dass jede Abteilung zehn Verwandte Kurse hat. Das Eager Loading Beispiel würde zu einer einzelnen (Join-) Abfrage und einem einzelnen Roundtrip zur Datenbank führen. Die Lazy Loading-und expliziten Lade Beispiele führen zu elf Abfragen und elf Roundtrips zur Datenbank. Die zusätzlichen Roundtrips zur Datenbank beeinträchtigen die Leistung besonders bei hoher Latenz.

In einigen Szenarios ist Lazy Loading hingegen effizienter. Das unverzügliches Laden kann dazu führen, dass ein sehr komplexer Join generiert wird, der SQL Server nicht effizient verarbeiten kann. Wenn Sie nur für eine Teilmenge einer Gruppe von Entitäten, die Sie verarbeiten, auf die Navigations Eigenschaften einer Entität zugreifen müssen, Lazy Loading möglicherweise eine bessere Leistung, da Eager Loading mehr Daten abrufen, als Sie benötigen. Wenn die Leistung wichtig ist, empfiehlt es sich, die Leistung mit beiden Möglichkeiten zu testen, um die beste Wahl treffen zu können.

In der Regel verwenden Sie Explizites Laden nur dann, wenn Sie Lazy Loading deaktiviert haben. Ein Szenario, in dem Sie Lazy Loading deaktivieren sollten, ist die Serialisierung. Lazy Load und Serialisierung sind nicht gut gemischt, und wenn Sie nicht vorsichtig sind, können Sie am Ende mehr Daten Abfragen, als wenn Lazy Loading aktiviert ist. Die Serialisierung funktioniert in der Regel durch den Zugriff auf jede Eigenschaft einer Instanz eines Typs. Eigenschafts Zugriffs Trigger Lazy Loading, und diese Lazy Loaded-Entitäten werden serialisiert. Der Serialisierungsprozess greift dann auf jede Eigenschaft der verzögert geladenen Entitäten zu, was möglicherweise noch mehr Lazy Loading und die Serialisierung verursacht. Um dies zu verhindern, sollten Sie Lazy Loading deaktivieren, bevor Sie eine Entität serialisieren.

Die Daten Bank Kontext Klasse führt Lazy Loading standardmäßig aus. Es gibt zwei Möglichkeiten, Lazy Loading zu deaktivieren:

- Beim Deklarieren der Eigenschaft können Sie für bestimmte Navigations Eigenschaften das `virtual` Schlüsselwort weglassen.
- Legen Sie für alle Navigations Eigenschaften `LazyLoadingEnabled` auf `false`fest. Beispielsweise können Sie den folgenden Code in den Konstruktor der Kontext Klasse einfügen: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Lazy Load kann Code maskieren, der Leistungsprobleme verursacht. Beispielsweise kann es vorkommen, dass Code, der nicht das sorgfältige oder explizite laden angibt, aber eine große Menge an Entitäten verarbeitet und in jeder Iterationen mehrere Navigations Eigenschaften verwendet, möglicherweise sehr ineffizient ist (aufgrund vieler Roundtrips zur Datenbank). Eine Anwendung, die eine gute Leistung bei der Entwicklung mit einer lokalen SQL Server-Datenbank erzielt, kann bei der Umstellung auf die Azure SQL-Datenbank aufgrund erhöhter Latenz und Lazy Loading Leistungsprobleme verursachen. Durch die Profilerstellung für die Datenbankabfragen mit einem realistischen Test Ladevorgang können Sie ermitteln, ob Lazy Loading geeignet ist. Weitere Informationen finden Sie unter [Demystifying Entity Framework Strategien: Laden](https://msdn.microsoft.com/magazine/hh205756.aspx) von verknüpften Daten und [Verwenden des Entity Framework, um die Netzwerk Latenz zu SQL Azure zu verringern](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Erstellen einer Kurs Index Seite, auf der der Abteilungs Name angezeigt wird

Die `Course`-Entität enthält eine Navigations Eigenschaft, die die `Department` Entität der Abteilung enthält, der der Kurs zugewiesen ist. Um den Namen der zugewiesenen Abteilung in einer Liste von Kursen anzuzeigen, müssen Sie die `Name`-Eigenschaft von der `Department`-Entität, die sich in der `Course.Department`-Navigations Eigenschaft befindet, erhalten.

Erstellen Sie für den `Course` Entitätstyp einen Controller mit dem Namen `CourseController`, und verwenden Sie die gleichen Optionen wie zuvor für den `Student` Controller, wie in der folgenden Abbildung dargestellt (mit Ausnahme des Bilds befindet sich Ihre Kontext Klasse im dal-Namespace und nicht im Namespace "Models"):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Öffnen Sie " *controllers\coursecontroller.cs* ", und sehen Sie sich die `Index`-Methode an:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Der automatische Gerüstbau hat mithilfe der `Include`-Methode ein Eager Loading für die `Department`-Navigationseigenschaft angegeben.

Öffnen Sie *views\course\index.cshtml* , und ersetzen Sie den vorhandenen Code durch den folgenden Code. Die Änderungen werden hervorgehoben:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Sie haben die folgenden Änderungen am eingerüsteten Code vorgenommen:

- Die Überschrift von **Index** in **Kurse**wurde geändert.
- Die Zeilen Links wurden nach links verschoben.
- Unter der Überschrift **Nummer** wurde eine Spalte hinzugefügt, die den `CourseID` Eigenschafts Wert anzeigt. (Standardmäßig werden Primärschlüssel nicht mit einem Gerüst gefüllt, da Sie normalerweise für Endbenutzer bedeutungslos sind. In diesem Fall ist der Primärschlüssel jedoch sinnvoll, und Sie möchten ihn anzeigen.)
- Die letzte Spaltenüberschrift von " **DepartmentID** " (der Name des fremd Schlüssels in die `Department` Entität) wurde in " **Department**" geändert.

Beachten Sie, dass in der letzten Spalte der Gerüst Code die `Name`-Eigenschaft der `Department` Entität anzeigt, die in die `Department`-Navigations Eigenschaft geladen wird:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Führen Sie die Seite aus (Wählen Sie auf der Startseite der Configuration Manager-Homepage die Registerkarte **Kurse** aus), um die Liste mit den Abteilungsnamen anzuzeigen.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Erstellen einer Dozenten Index Seite, die Kurse und Registrierungen anzeigt

In diesem Abschnitt erstellen Sie einen Controller und eine Ansicht für die `Instructor` Entität, um die Index Seite "Dozenten" anzuzeigen:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Auf dieser Seite werden verwandte Daten auf folgende Weise gelesen und angezeigt:

- Die Liste der Dozenten zeigt verwandte Daten aus der `OfficeAssignment` Entität an. Die `Instructor`- und `OfficeAssignment`-Entitäten stehen in einer 1:0..1-Beziehung zueinander. Sie verwenden Eager Loading für die `OfficeAssignment` Entitäten. Wie zuvor erläutert, ist Eager Loading in der Regel effizienter, wenn Sie die verwandten Daten für alle abgerufenen Zeilen der primären Tabelle benötigen. In diesem Fall sollten Sie die Office-Anweisungen für alle angezeigten Dozenten anzeigen.
- Wenn der Benutzer einen Dozenten auswählt, werden zugehörige `Course`-Entitäten angezeigt. Die `Instructor`- und `Course`-Entitäten stehen in einer m:n-Beziehung zueinander. Sie verwenden Eager Loading für die `Course` Entitäten und ihre zugehörigen `Department` Entitäten. In diesem Fall sind Lazy Loading möglicherweise effizienter, da Sie nur Kurse für den ausgewählten Dozenten benötigen. Dieses Beispiel zeigt jedoch, wie Eager Loading für Navigationseigenschaften in Entitäten in den Navigationseigenschaften verwendet wird.
- Wenn der Benutzer einen Kurs auswählt, werden verwandte Daten aus der `Enrollments` Entitätenmenge angezeigt. Die `Course`- und `Enrollment`-Entitäten stehen in einer 1:n-Beziehung zueinander. Sie fügen Explizites Laden für `Enrollment` Entitäten und ihre zugehörigen `Student` Entitäten hinzu. (Explizites Laden ist nicht erforderlich, da Lazy Loading aktiviert ist, aber dies zeigt, wie Explizites Laden durchzuführen ist.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Erstellen eines Ansichts Modells für die Ansicht "Dozenten Index"

Die Seite "Instructor Index" zeigt drei verschiedene Tabellen an. Aus diesem Grund erstellen Sie ein Ansichtsmodell, das drei Eigenschaften enthält. Jede enthält Daten für eine der Tabellen.

Erstellen Sie im *ViewModels* -Ordner *InstructorIndexData.cs* , und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Hinzufügen eines Stils für ausgewählte Zeilen

Zum Markieren ausgewählter Zeilen benötigen Sie eine andere Hintergrundfarbe. Fügen Sie den folgenden hervorgehobenen Code zum Abschnitt `/* info and errors */` in " *content\site.CSS*" hinzu, um einen Stil für diese Benutzeroberfläche bereitzustellen:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Erstellen des Dozenten Controllers und der Ansichten

Erstellen Sie einen `InstructorController` Controller, wie in der folgenden Abbildung dargestellt:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Öffnen Sie " *controllers\instructor Controller.cs* ", und fügen Sie eine `using`-Anweisung für den `ViewModels`-Namespace hinzu:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Der Gerüst Code in der `Index`-Methode gibt Eager Loading nur für die Navigations Eigenschaft `OfficeAssignment` an:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Ersetzen Sie die `Index`-Methode durch den folgenden Code, um zusätzliche verwandte Daten zu laden und in das Ansichts Modell einzufügen:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Die-Methode akzeptiert optionale Routendaten (`id`) und einen Abfrage Zeichen folgen Parameter (`courseID`), die die ID-Werte des ausgewählten Dozenten und des ausgewählten Kurses bereitstellen, und übergibt alle erforderlichen Daten an die Ansicht. Die Parameter werden durch die **Auswählen**-Links auf der Seite bereitgestellt.

> [!TIP]
> 
> **Weiterleiten von Daten**
> 
> Bei den Routendaten handelt es sich um Daten, die der Modell Binder in einem in der Routing Tabelle angegebenen URL-Segment gefunden hat. Die Standardroute gibt beispielsweise `controller`, `action`und `id` Segmente an:
> 
> ```csharp
> routes.MapRoute(  
>  name: "Default",  
>  url: "{controller}/{action}/{id}",  
>  defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }  
> );
> ```
> 
> In der folgenden URL wird die Standardroute `Instructor` als `controller``Index` als `action` und 1 als `id`zugeordnet. Dabei handelt es sich um Routendaten Werte.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? CourseID = 2021" ist ein Abfrage Zeichen folgen Wert. Der Modell Binder funktioniert auch, wenn Sie die `id` als Abfrage Zeichenfolgen-Wert übergeben:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Die URLs werden von `ActionLink`-Anweisungen in der Razor-Ansicht erstellt. Im folgenden Code entspricht der `id`-Parameter der Standardroute, sodass `id` den Routendaten hinzugefügt wird.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> Im folgenden Code entspricht `courseID` nicht einem Parameter in der Standardroute, daher wird er als Abfrage Zeichenfolge hinzugefügt.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

Der Code erstellt zuerst eine Instanz des Ansichtsmodells und fügt die Dozentenliste ein. Der Code gibt Eager Loading für die `Instructor.OfficeAssignment` und die `Instructor.Courses` Navigations Eigenschaft an.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

Mit dem zweiten `Include`-Methode werden Kurse geladen, und für jeden Kurs, der geladen wird, wird für die Navigations Eigenschaft `Course.Department` Eager Loading.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Wie bereits erwähnt, ist Eager Loading nicht erforderlich, wird jedoch zur Verbesserung der Leistung ausgeführt. Da die Sicht immer die `OfficeAssignment` Entität erfordert, ist es effizienter, Sie in derselben Abfrage abzurufen. `Course` Entitäten sind erforderlich, wenn ein Dozent auf der Webseite ausgewählt wird. Eager Loading ist daher besser als Lazy Loading, wenn die Seite häufiger angezeigt wird, wenn ein Kurs ausgewählt ist als ohne.

Wenn eine Dozenten-ID ausgewählt wurde, wird der ausgewählte Dozenten aus der Liste der Dozenten im Ansichts Modell abgerufen. Die `Courses`-Eigenschaft des Ansichts Modells wird dann mit den `Course` Entitäten aus der `Courses` Navigations Eigenschaft dieses Dozenten geladen.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Die `Where`-Methode gibt eine Auflistung zurück, aber in diesem Fall führen die an diese Methode über gebenden Kriterien dazu, dass nur eine einzige `Instructor` Entität zurückgegeben wird. Die `Single`-Methode konvertiert die Auflistung in eine einzelne `Instructor` Entität, die Ihnen den Zugriff auf die `Courses`-Eigenschaft dieser Entität ermöglicht.

Sie verwenden die [einzige](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) Methode für eine Auflistung, wenn Sie wissen, dass die Sammlung nur ein Element enthält. Die `Single`-Methode löst eine Ausnahme aus, wenn die an Sie weiter gegebene Auflistung leer ist oder wenn mehr als ein Element vorhanden ist. Eine Alternative ist " [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx)", die einen Standardwert zurückgibt (in diesem Fall`null`), wenn die Auflistung leer ist. In diesem Fall führt dies jedoch immer noch zu einer Ausnahme (von der Suche nach einer `Courses`-Eigenschaft für einen `null` Verweis), und die Ausnahme Meldung würde die Ursache des Problems weniger eindeutig angeben. Wenn Sie die `Single`-Methode aufrufen, können Sie auch die `Where` Bedingung übergeben, anstatt die `Where`-Methode separat aufzurufen:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

anstelle von:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Wenn ein Kurs ausgewählt wurde, wird der ausgewählte Kurs aus der Kursliste im Ansichtsmodell abgerufen. Anschließend wird die `Enrollments`-Eigenschaft des Ansichts Modells mit den `Enrollment` Entitäten aus der `Enrollments` Navigations Eigenschaft dieses Kurses geladen.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Ändern der Ansicht "Dozenten Index"

Ersetzen Sie den vorhandenen Code in *views\instructor \index.cshtml*durch den folgenden Code. Die Änderungen werden hervorgehoben:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Sie haben die folgenden Änderungen am bestehenden Code vorgenommen:

- Die Modellklasse wurde zu `InstructorIndexData` geändert.
- Der Seitenname wurde von **Index** in **Dozenten** geändert.
- Die Zeilen Link Spalten werden nach links verschoben.
- Die **FullName** -Spalte wurde entfernt.
- Es wurde eine **Office** -Spalte hinzugefügt, die nur `item.OfficeAssignment.Location` anzeigt, wenn `item.OfficeAssignment` nicht NULL ist. (Da dies eine 1:0-oder 1-Beziehung ist, gibt es möglicherweise keine Verwandte `OfficeAssignment` Entität.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Es wurde Code hinzugefügt, mit dem `class="selectedrow"` dem `tr`-Element des ausgewählten Dozenten dynamisch hinzugefügt wird. Dadurch wird eine Hintergrundfarbe für die ausgewählte Zeile mithilfe der CSS-Klasse festgelegt, die Sie zuvor erstellt haben. (Das `valign`-Attribut wird im folgenden Tutorial nützlich sein, wenn Sie der Tabelle eine Spalte mit mehreren Zeilen hinzufügen.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Es wurde eine neue `ActionLink` mit der Bezeichnung **Select** direkt vor den anderen Links in den einzelnen Zeilen hinzugefügt, die bewirkt, dass die ausgewählte Dozenten-ID an die `Index`-Methode gesendet wird.

Führen Sie die Anwendung aus, und wählen Sie die Registerkarte **Dozenten** . Die Seite zeigt die `Location`-Eigenschaft verwandter `OfficeAssignment` Entitäten und eine leere Tabellenzelle an, wenn keine Verwandte `OfficeAssignment` Entität vorhanden ist.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Fügen Sie in der Datei " *views\instructor \index.cshtml* " nach dem schließenden `table` Element (am Ende der Datei) den folgenden markierten Code hinzu. Dadurch wird eine Liste von Kursen angezeigt, die mit einem Dozenten verknüpft sind, wenn ein Dozent ausgewählt wird.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Dieser Code liest die `Courses`-Eigenschaft des Ansichtsmodells, um eine Kursliste anzuzeigen. Außerdem wird ein `Select` Hyperlink bereitstellt, der die ID des ausgewählten Kurses an die `Index` Aktionsmethode sendet.

> [!NOTE]
> Die *CSS* -Datei wird von Browsern zwischengespeichert. Wenn die Änderungen beim Ausführen der Anwendung nicht angezeigt werden, führen Sie eine harte Aktualisierung aus (halten Sie die STRG-Taste gedrückt, während Sie auf die Schaltfläche **Aktualisieren** klicken, oder drücken Sie STRG + F5).

Führen Sie die Seite aus, und wählen Sie einen Dozenten. Jetzt sehen Sie ein Raster, das die dem Dozenten zugewiesenen Kurse anzeigt. Sie sehen auch den Namen der zugewiesenen Abteilung für jeden Kurs.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Fügen Sie den folgenden Code hinzu, nachdem Sie den Codeblock hinzugefügt haben. Dies zeigt eine Liste der Studenten an, die im Kurs registriert sind, wenn dieser Kurs ausgewählt ist.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Mit diesem Code wird die `Enrollments`-Eigenschaft des Ansichts Modells gelesen, um eine Liste der im Kurs registrierten Studenten anzuzeigen.

Führen Sie die Seite aus, und wählen Sie einen Dozenten. Wählen Sie dann einen Kurs aus, um die Liste der registrierten Studenten und deren Noten einzusehen.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Explizites Laden

Öffnen Sie *InstructorController.cs* , und sehen Sie sich an, wie die `Index`-Methode die Liste der Registrierungen für einen ausgewählten Kurs abruft:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Beim Abrufen der Liste der Dozenten haben Sie Eager Loading für die `Courses` Navigations Eigenschaft und für die Eigenschaft `Department` der einzelnen Kurse angegeben. Dann fügen Sie die `Courses`-Auflistung in das Ansichts Modell ein, und nun greifen Sie auf die `Enrollments`-Navigations Eigenschaft aus einer Entität in dieser Auflistung zu. Da Sie Eager Loading für die `Course.Enrollments`-Navigations Eigenschaft nicht angegeben haben, werden die Daten aus dieser Eigenschaft als Ergebnis von Lazy Loading auf der Seite angezeigt.

Wenn Sie Lazy Loading deaktiviert haben, ohne den Code auf andere Weise zu ändern, wäre die `Enrollments`-Eigenschaft unabhängig von der Anzahl der Registrierungen, die der Kurs tatsächlich hatte, NULL. Wenn Sie die `Enrollments`-Eigenschaft laden möchten, müssten Sie entweder Eager Loading oder explizites Laden angeben. Sie haben bereits gesehen, wie Sie Eager Loading. Um ein Beispiel für explizites laden anzuzeigen, ersetzen Sie die `Index`-Methode durch den folgenden Code, der die `Enrollments`-Eigenschaft explizit lädt. Der geänderte Code wird hervorgehoben.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Nachdem Sie die ausgewählte `Course` Entität erhalten haben, lädt der neue Code die `Enrollments` Navigations Eigenschaft dieses Kurses explizit:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Anschließend werden die verknüpften `Student` Entitäten jeder `Enrollment` Entität explizit geladen:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Beachten Sie, dass Sie die `Collection`-Methode verwenden, um eine Auflistungs Eigenschaft zu laden, aber für eine Eigenschaft, die nur eine Entität enthält, verwenden Sie die `Reference`-Methode. Sie können die Seite "Instructor Index" jetzt ausführen, und es wird kein Unterschied in der Anzeige auf der Seite angezeigt, obwohl Sie geändert haben, wie die Daten abgerufen werden.

## <a name="summary"></a>Summary

Sie haben jetzt alle drei Methoden (Lazy, eifrig und explizit) verwendet, um verwandte Daten in Navigations Eigenschaften zu laden. Das nächste Tutorial zeigt die Aktualisierung verwandter Daten.

Links zu anderen Entity Framework Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Weiter](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
