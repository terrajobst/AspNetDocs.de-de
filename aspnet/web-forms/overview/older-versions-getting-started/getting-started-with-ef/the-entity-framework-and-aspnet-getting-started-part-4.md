---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Einführung in Entity Framework 4,0 Database First und ASP.NET 4 Web Forms-Part 4 | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie mithilfe der Entity Framework Web Forms Anwendungen erstellen. Die Beispielanwendung ist...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: eb75a76038466bf30738387ed4739687de1df944
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518283"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Ersten Schritte mit Entity Framework 4,0 Database First und ASP.NET 4 Web Forms-Teil 4

von [Tom Dykstra](https://github.com/tdykstra)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie ASP.net-Web Forms Anwendungen mithilfe von Entity Framework 4,0 und Visual Studio 2010 erstellen. Weitere Informationen zur tutorialreihe finden Sie [im ersten Tutorial der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md) .

## <a name="working-with-related-data"></a>Arbeiten mit verwandten Daten

Im vorherigen Tutorial haben Sie das `EntityDataSource`-Steuerelement verwendet, um Daten zu filtern, zu sortieren und zu gruppieren. In diesem Tutorial werden verwandte Daten angezeigt und aktualisiert.

Sie erstellen die Seite Dozenten, auf der eine Liste der Dozenten angezeigt wird. Wenn Sie einen Dozenten auswählen, wird eine Liste der Kurse angezeigt, die von diesem Dozenten gelehrt werden. Wenn Sie einen Kurs auswählen, werden die Details für den Kurs und eine Liste der im Kurs registrierten Studenten angezeigt. Sie können den Namen des Dozenten, das Einstellungs Datum und die Büro Zuweisung bearbeiten. Die Office-Zuweisung ist eine separate Entitätenmenge, auf die Sie über eine Navigations Eigenschaft zugreifen.

Sie können Master Daten mit Detaildaten in Markup oder im Code verknüpfen. In diesem Teil des Tutorials verwenden Sie beide Methoden.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Anzeigen und aktualisieren verwandter Entitäten in einem GridView-Steuerelement

Erstellen Sie eine neue Webseite namens " *Dozenten. aspx* ", die die Master Seite " *Site. Master* " verwendet, und fügen Sie das folgende Markup zum `Content`-Steuerelement mit dem Namen `Content2`hinzu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Dieses Markup erstellt ein `EntityDataSource` Steuerelement, das Dozenten auswählt und Updates aktiviert. Das `div`-Element konfiguriert Markup so, dass es auf der linken Seite dargestellt wird, sodass Sie später eine-Spalte hinzufügen können.

Fügen Sie zwischen dem `EntityDataSource` Markup und dem schließenden `</div>`-Tag das folgende Markup hinzu, das ein `GridView`-Steuerelement und ein `Label` Steuerelement erstellt, das Sie für Fehlermeldungen verwenden:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Dieses `GridView` Steuerelement aktiviert die Zeilenauswahl, hebt die ausgewählte Zeile mit einer hellgrauen Hintergrundfarbe hervor und gibt Handler (die Sie später erstellen) für die `SelectedIndexChanged`-und `Updating`-Ereignisse an. Außerdem gibt Sie `PersonID` für die `DataKeyNames`-Eigenschaft an, sodass der Schlüsselwert der ausgewählten Zeile an ein anderes Steuerelement weitergegeben werden kann, das Sie später hinzufügen.

Die letzte Spalte enthält die Büro Zuweisung des Dozenten, die in einer Navigations Eigenschaft der `Person` Entität gespeichert wird, da Sie von einer zugeordneten Entität stammt. Beachten Sie, dass das `EditItemTemplate` Element `Eval` anstelle von `Bind`angibt, da das `GridView` Steuerelement nicht direkt an Navigations Eigenschaften gebunden werden kann, um Sie zu aktualisieren. Die Office-Zuweisung wird im Code aktualisiert. Zu diesem Zweck benötigen Sie einen Verweis auf das `TextBox`-Steuerelement, das Sie im `Init` Ereignis des `TextBox`-Steuer Elements speichern und speichern können.

Nach dem `GridView`-Steuerelement handelt es sich um ein `Label` Steuerelement, das für Fehlermeldungen verwendet wird. Die `Visible`-Eigenschaft des Steuer Elements ist `false`, und der Ansichts Zustand ist deaktiviert, sodass die Bezeichnung nur dann angezeigt wird, wenn der Code Sie als Reaktion auf einen Fehler sichtbar macht.

Öffnen Sie die Datei *Instructors.aspx.cs* , und fügen Sie die folgende `using`-Anweisung hinzu:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Fügen Sie direkt nach der Deklaration der partiellen Klassennamen ein privates Klassenfeld hinzu, um einen Verweis auf das Textfeld für die Office-Zuweisung zu erhalten.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Fügen Sie einen Stub für den `SelectedIndexChanged` Ereignishandler hinzu, für den Sie später Code hinzufügen. Fügen Sie außerdem einen Handler für das `Init` Ereignis des Office-Zuweisungs `TextBox`-Steuer Elements hinzu, damit Sie einen Verweis auf das `TextBox`-Steuerelement speichern können. Sie verwenden diese Referenz, um den Wert zu erhalten, den der Benutzer eingegeben hat, um die mit der Navigations Eigenschaft verknüpfte Entität zu aktualisieren.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Sie verwenden das `Updating`-Ereignis des `GridView`-Steuer Elements, um die `Location`-Eigenschaft der zugeordneten `OfficeAssignment` Entität zu aktualisieren. Fügen Sie den folgenden Handler für das `Updating`-Ereignis hinzu:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Dieser Code wird ausgeführt, wenn der Benutzer in einer `GridView` Zeile auf **Aktualisieren** klickt. Der Code verwendet LINQ to Entities, um die `OfficeAssignment` Entität abzurufen, die der aktuellen `Person` Entität zugeordnet ist. dabei wird die `PersonID` der ausgewählten Zeile aus dem Ereignis Argument verwendet.

Der Code führt dann abhängig von dem Wert im `InstructorOfficeTextBox` Steuerelement eine der folgenden Aktionen aus:

- Wenn das Textfeld einen Wert hat und keine zu Aktualisier `OfficeAssignment` Entität vorhanden ist, wird eines erstellt.
- Wenn das Textfeld einen Wert enthält und eine `OfficeAssignment` Entität vorhanden ist, aktualisiert es den Wert der `Location`-Eigenschaft.
- Wenn das Textfeld leer ist und eine `OfficeAssignment` Entität vorhanden ist, wird die Entität gelöscht.

Anschließend werden die Änderungen in der Datenbank gespeichert. Wenn eine Ausnahme auftritt, wird eine Fehlermeldung angezeigt.

Führen Sie die Seite aus.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Klicken Sie auf **Bearbeiten** , und ändern Sie alle Felder in Textfelder.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Ändern Sie die Werte, einschließlich der **Office-Zuweisung**. Klicken Sie auf **Aktualisieren** , und die Änderungen werden in der Liste angezeigt.

## <a name="displaying-related-entities-in-a-separate-control"></a>Anzeigen verwandter Entitäten in einem separaten Steuerelement

Jeder Dozenten kann einen oder mehrere Kurse unterrichten. daher fügen Sie ein `EntityDataSource`-Steuerelement und ein `GridView`-Steuerelement hinzu, um die Kurse aufzulisten, die mit dem in den Dozenten `GridView`-Steuerelement verknüpften Dozenten verknüpft sind. Fügen Sie zum Erstellen einer Überschrift und der `EntityDataSource`-Steuerelement für Kurs Entitäten das folgende Markup zwischen der Fehlermeldung `Label`-Steuerelement und dem schließenden `</div>`-Tag hinzu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

Der `Where`-Parameter enthält den Wert des `PersonID` des Dozenten, dessen Zeile im `InstructorsGridView`-Steuerelement ausgewählt ist. Die `Where`-Eigenschaft enthält einen untergeordneten SELECT-Befehl, der alle zugeordneten `Person` Entitäten aus der `People` Navigations Eigenschaft einer `Course` Entität abruft und die `Course` Entität nur dann auswählt, wenn eine der zugeordneten `Person` Entitäten den ausgewählten `PersonID` Wert enthält.

Um das `GridView` Steuerelement zu erstellen, fügen Sie das folgende Markup direkt nach dem Steuerelement `CoursesEntityDataSource` (vor dem schließenden `</div>`-Tag) hinzu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Da keine Kurse angezeigt werden, wenn kein Dozenten ausgewählt ist, wird ein `EmptyDataTemplate`-Element eingeschlossen.

Führen Sie die Seite aus.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Wählen Sie einen Dozenten aus, dem ein oder mehrere Kurse zugewiesen sind, und der Kurs oder die Kurse werden in der Liste angezeigt. (Hinweis: Obwohl das Datenbankschema mehrere Kurse zulässt, verfügt der Dozenten in den Testdaten, die in der Datenbank bereitgestellt werden, über mehr als einen Kurs. Sie können der Datenbank selbst Kurse mithilfe des **Server-Explorer** Fensters oder der Seite " *coursesadd. aspx* " hinzufügen, die Sie in einem späteren Tutorial hinzufügen.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

Das `CoursesGridView`-Steuerelement zeigt nur einige Kurs Felder an. Wenn Sie alle Details für einen Kurs anzeigen möchten, verwenden Sie ein `DetailsView`-Steuerelement für den Kurs, den der Benutzer auswählt. Fügen Sie in " *Dozenten. aspx*" nach dem schließenden `</div>` Tag das folgende Markup hinzu (stellen Sie sicher, dass Sie **Dieses Markup hinter** dem schließenden div-Tag platzieren, nicht davor):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Mit diesem Markup wird ein `EntityDataSource` Steuerelement erstellt, das an die `Courses` Entitätenmenge gebunden ist. Die `Where`-Eigenschaft wählt einen Kurs mithilfe des `CourseID` Werts der ausgewählten Zeile in den Kursen `GridView`-Steuerelement aus. Das Markup gibt einen Handler für das `Selected`-Ereignis an, das Sie später zum Anzeigen von Studenten Qualitäten verwenden. Dies ist eine weitere Ebene, die in der Hierarchie weiter unten ist.

Erstellen Sie in *Instructors.aspx.cs*den folgenden Stub für die `CourseDetailsEntityDataSource_Selected`-Methode. (Sie füllen diesen Stub später in diesem Tutorial aus. vorerst benötigen Sie den Stub, damit die Seite kompiliert und ausgeführt wird.)

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Führen Sie die Seite aus.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Anfänglich sind keine Kursdetails vorhanden, da kein Kurs ausgewählt ist. Wählen Sie einen Dozenten aus, dem ein Kurs zugewiesen ist, und wählen Sie dann einen Kurs aus, um die Details anzuzeigen.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Verwenden des "Selected"-Ereignisses von EntityDataSource zum Anzeigen verwandter Daten

Schließlich möchten Sie alle registrierten Studenten und deren Noten für den ausgewählten Kurs anzeigen. Zu diesem Zweck verwenden Sie das `Selected`-Ereignis des `EntityDataSource` Steuer Elements, das an den Kurs `DetailsView`gebunden ist.

Fügen Sie in *Dozenten. aspx*das folgende Markup nach dem `DetailsView`-Steuerelement hinzu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Dieses Markup erstellt ein `ListView` Steuerelement, das eine Liste von Schülern und deren Noten für den ausgewählten Kurs anzeigt. Es wurde keine Datenquelle angegeben, da Sie das Steuerelement im Code übernehmen. Das `EmptyDataTemplate`-Element enthält eine Meldung, die angezeigt wird, wenn kein Kurs ausgewählt ist – in diesem Fall sind keine anzuzeigenden Studenten vorhanden. Das `LayoutTemplate` Element erstellt eine HTML-Tabelle zum Anzeigen der Liste, und die `ItemTemplate` gibt die anzuzeigenden Spalten an. Die Student-ID und die Student-Klasse stammen aus der `StudentGrade`-Entität, und der Student Name wird von der `Person` Entität abgeleitet, die der Entity Framework in der `Person` Navigations Eigenschaft der `StudentGrade`-Entität verfügbar macht.

Ersetzen Sie in *Instructors.aspx.cs*die `CourseDetailsEntityDataSource_Selected`-Methode stubout durch den folgenden Code:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

Das Ereignis Argument für dieses Ereignis stellt die ausgewählten Daten in Form einer Auflistung bereit, die 0 (null) Elemente enthält, wenn nichts ausgewählt ist, oder ein Element, wenn eine `Course` Entität ausgewählt ist. Wenn eine `Course` Entität ausgewählt ist, verwendet der Code die `First`-Methode, um die Auflistung in ein einzelnes Objekt zu konvertieren. Anschließend werden `StudentGrade` Entitäten aus der Navigations Eigenschaft abgerufen, in eine Auflistung konvertiert und das `GradesListView` Steuerelement an die Auflistung gebunden.

Dies ist ausreichend, um die Qualität anzuzeigen, aber Sie möchten sicherstellen, dass die Meldung in der leeren Daten Vorlage angezeigt wird, wenn die Seite zum ersten Mal angezeigt wird, und wenn kein Kurs ausgewählt ist. Erstellen Sie hierzu die folgende Methode, die Sie von zwei Stellen aus abrufen:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Aufrufen Sie diese neue Methode aus der `Page_Load`-Methode, um die leere Daten Vorlage anzuzeigen, wenn die Seite zum ersten Mal angezeigt wird. Und über die `InstructorsGridView_SelectedIndexChanged`-Methode aufrufen, da dieses Ereignis ausgelöst wird, wenn ein Dozent ausgewählt wird. das bedeutet, dass neue Kurse in die Kurse `GridView` Steuer Elements geladen werden und keine noch ausgewählt ist. Dies sind die beiden Aufrufe:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Führen Sie die Seite aus.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Wählen Sie einen Dozenten aus, dem ein Kurs zugewiesen ist, und wählen Sie dann den Kurs aus.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Sie haben nun einige Möglichkeiten gesehen, mit verwandten Daten zu arbeiten. Im folgenden Tutorial erfahren Sie, wie Sie Beziehungen zwischen vorhandenen Entitäten hinzufügen, Beziehungen entfernen und eine neue Entität hinzufügen, die über eine Beziehung zu einer vorhandenen Entität verfügt.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-5.md)
