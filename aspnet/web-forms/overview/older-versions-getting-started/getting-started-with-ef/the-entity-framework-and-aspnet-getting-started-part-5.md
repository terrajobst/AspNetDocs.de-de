---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Einführung in Entity Framework 4,0 Database First und ASP.NET 4 Web Forms-Part 5 | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie mithilfe der Entity Framework Web Forms Anwendungen erstellen. Die Beispielanwendung ist...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 75328e67abb4295b619cac5423a9eb970942fff7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78423867"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Ersten Schritte mit Entity Framework 4,0 Database First und ASP.NET 4 Web Forms-Teil 5

von [Tom Dykstra](https://github.com/tdykstra)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie ASP.net-Web Forms Anwendungen mithilfe von Entity Framework 4,0 und Visual Studio 2010 erstellen. Weitere Informationen zur tutorialreihe finden Sie [im ersten Tutorial der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md) .

## <a name="working-with-related-data-continued"></a>Arbeiten mit verwandten Daten, Fortsetzung

Im vorherigen Tutorial haben Sie begonnen, das `EntityDataSource`-Steuerelement für die Arbeit mit verknüpften Daten zu verwenden. In den Navigations Eigenschaften haben Sie mehrere Ebenen der Hierarchie und bearbeiteten Daten angezeigt. In diesem Tutorial arbeiten Sie weiterhin mit verknüpften Daten durch Hinzufügen und Löschen von Beziehungen und durch Hinzufügen einer neuen Entität, die eine Beziehung zu einer vorhandenen Entität aufweist.

Sie erstellen eine Seite, die Kurse hinzufügt, die Abteilungen zugewiesen sind. Die Abteilungen sind bereits vorhanden, und wenn Sie einen neuen Kurs erstellen, erstellen Sie gleichzeitig eine Beziehung zwischen der IT-Abteilung und einer vorhandenen Abteilung.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Außerdem erstellen Sie eine Seite, die mit einer m:n-Beziehung funktioniert, indem Sie einen Dozenten einem Kurs zuweisen (indem Sie eine Beziehung zwischen zwei Entitäten hinzufügen, die Sie auswählen) oder einen Dozenten aus einem Kurs entfernen (Entfernen einer Beziehung zwischen zwei Entitäten, die Sie Wählen Sie aus). Wenn in der Datenbank eine Beziehung zwischen einem Dozenten und einem Kurs hinzugefügt wird, wird der `CourseInstructor` Association-Tabelle eine neue Zeile hinzugefügt. das Entfernen einer Beziehung umfasst das Löschen einer Zeile aus der `CourseInstructor` Zuordnungs Tabelle. Dies geschieht jedoch im Entity Framework, indem Navigations Eigenschaften festgelegt werden, ohne dass explizit auf die `CourseInstructor` Tabelle verwiesen wird.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Hinzufügen einer Entität mit einer Beziehung zu einer vorhandenen Entität

Erstellen Sie eine neue Webseite namens " *coursesadd. aspx* ", die die Master Seite " *Site. Master* " verwendet, und fügen Sie das folgende Markup zum `Content`-Steuerelement mit dem Namen `Content2`hinzu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Dieses Markup erstellt ein `EntityDataSource` Steuerelement, das Kurse auswählt, das Einfügen ermöglicht und einen Handler für das `Inserting` Ereignis angibt. Sie verwenden den-Handler, um die `Department`-Navigations Eigenschaft zu aktualisieren, wenn eine neue `Course` Entität erstellt wird.

Das Markup erstellt außerdem ein `DetailsView`-Steuerelement, das zum Hinzufügen neuer `Course` Entitäten verwendet werden soll. Das Markup verwendet gebundene Felder für `Course` Entitäts Eigenschaften. Sie müssen den `CourseID` Wert eingeben, da es sich hierbei nicht um ein vom System generiertes ID-Feld handelt. Stattdessen ist es eine Kursnummer, die manuell angegeben werden muss, wenn der Kurs erstellt wird.

Sie verwenden ein Vorlagen Feld für die `Department`-Navigations Eigenschaft, da Navigations Eigenschaften nicht mit `BoundField` Steuerelementen verwendet werden können. Das Feld Vorlage enthält eine Dropdown Liste, in der die Abteilung ausgewählt werden soll. Die Dropdown Liste wird mit `Eval` anstelle von `Bind`an die `Departments` Entitätenmenge gebunden, da Navigations Eigenschaften nicht direkt gebunden werden können, um Sie zu aktualisieren. Sie geben einen Handler für das `Init` Ereignis des `DropDownList`-Steuer Elements an, sodass Sie einen Verweis auf das Steuerelement speichern können, um den Code zu verwenden, der den `DepartmentID` Fremdschlüssel aktualisiert.

Fügen Sie in *CoursesAdd.aspx.cs* unmittelbar nach der Deklaration der partiellen Klasse ein Klassenfeld hinzu, um einen Verweis auf das `DepartmentsDropDownList` Steuerelement zu enthalten:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Fügen Sie einen Handler für das `Init` Ereignis des `DepartmentsDropDownList`-Steuer Elements hinzu, damit Sie einen Verweis auf das Steuerelement speichern können. Auf diese Weise können Sie den vom Benutzer eingegebenen Wert erhalten und ihn verwenden, um den `DepartmentID` Wert der `Course` Entität zu aktualisieren.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Fügen Sie einen Handler für das `Inserting` Ereignis des `DetailsView`-Steuer Elements hinzu:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Wenn der Benutzer auf `Insert`klickt, wird das `Inserting`-Ereignis ausgelöst, bevor der neue Datensatz eingefügt wird. Der Code im Handler Ruft die `DepartmentID` aus dem `DropDownList`-Steuerelement ab und verwendet Sie, um den Wert festzulegen, der für die `DepartmentID`-Eigenschaft der `Course` Entität verwendet wird.

Die Entity Framework übernimmt das Hinzufügen dieses Kurses zur `Courses` Navigations Eigenschaft der zugeordneten `Department` Entität. Außerdem wird die Abteilung der Navigations Eigenschaft `Department` der `Course` Entität hinzugefügt.

Führen Sie die Seite aus.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Geben Sie eine ID, einen Titel, eine Anzahl von Gutschriften ein, wählen Sie eine Abteilung aus, und klicken Sie dann auf **Einfügen**.

Führen Sie die Seite " *Courses. aspx* " aus, und wählen Sie dieselbe Abteilung aus, um den neuen Kurs anzuzeigen.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Arbeiten mit m:n-Beziehungen

Die Beziehung zwischen der `Courses` Entitätenmenge und der `People` Entitätenmenge ist eine m:n-Beziehung. Eine `Course` Entität verfügt über eine Navigations Eigenschaft mit dem Namen `People`, die 0 (null), eine oder mehrere verwandte `Person` Entitäten enthalten kann (die Dozenten darstellen, die diesem Kurs zugewiesen sind). Und eine `Person` Entität verfügt über eine Navigations Eigenschaft mit dem Namen `Courses`, die 0 (null), eine oder mehrere verwandte `Course` Entitäten enthalten kann (d. h. die Kurse, die Dozenten zugewiesen werden). Ein Dozent könnte mehrere Kurse unterrichten, und ein Kurs kann von mehreren Dozenten unterrichtet werden. In diesem Abschnitt der exemplarischen Vorgehensweise fügen Sie Beziehungen zwischen `Person` und `Course` Entitäten hinzu und entfernen Sie durch Aktualisieren der Navigations Eigenschaften der verknüpften Entitäten.

Erstellen Sie eine neue Webseite namens " *Instructor scourses. aspx* ", die die Master Seite " *Site. Master* " verwendet, und fügen Sie das folgende Markup zum `Content`-Steuerelement mit dem Namen `Content2`hinzu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Dieses Markup erstellt ein `EntityDataSource` Steuerelement, das den Namen und `PersonID` der `Person` Entitäten für Dozenten abruft. Ein `DropDrownList`-Steuerelement ist an das `EntityDataSource` Steuerelement gebunden. Das `DropDownList`-Steuerelement gibt einen Handler für das `DataBound` Ereignis an. Sie verwenden diesen Handler, um die beiden Dropdown Listen zu übernehmen, in denen Kurse angezeigt werden.

Das Markup erstellt außerdem die folgende Gruppe von Steuerelementen, die verwendet werden, um dem ausgewählten Dozenten einen Kurs zuzuweisen:

- Ein `DropDownList` Steuerelement zum Auswählen eines Kurses, der zugewiesen werden soll. Dieses Steuerelement wird mit Kursen aufgefüllt, die dem ausgewählten Dozenten derzeit nicht zugewiesen sind.
- Ein `Button`-Steuerelement, um die Zuweisung zu initiieren.
- Ein `Label` Steuerelement, um eine Fehlermeldung anzuzeigen, wenn die Zuweisung fehlschlägt.

Schließlich erstellt das Markup auch eine Gruppe von Steuerelementen, die zum Entfernen eines Kurses aus dem ausgewählten Dozenten verwendet werden.

Fügen Sie in *InstructorsCourses.aspx.cs*eine using-Anweisung hinzu:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Fügen Sie eine Methode zum Auffüllen der beiden Dropdown Listen hinzu, die Kurse anzeigen:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Dieser Code Ruft alle Kurse aus der `Courses` Entitätenmenge ab und ruft die Kurse von der `Courses` Navigations Eigenschaft der `Person` Entität für den ausgewählten Dozenten ab. Anschließend bestimmt er, welche Kurse diesem Dozenten zugewiesen sind, und füllt die Dropdown Listen entsprechend auf.

Fügen Sie einen Handler für das `Click` Ereignis der `Assign` Schaltfläche hinzu:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Mit diesem Code wird die `Person` Entität für den ausgewählten Dozenten abgerufen, die `Course` Entität für den ausgewählten Kurs abgerufen und der ausgewählte Kurs der Navigations Eigenschaft `Courses` der `Person` Entität des Dozenten hinzugefügt. Anschließend werden die Änderungen in der Datenbank gespeichert, und die Dropdown Listen werden erneut aufgefüllt, damit die Ergebnisse sofort angezeigt werden.

Fügen Sie einen Handler für das `Click` Ereignis der `Remove` Schaltfläche hinzu:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Mit diesem Code wird die `Person` Entität für den ausgewählten Dozenten abgerufen, die `Course` Entität für den ausgewählten Kurs abgerufen und der ausgewählte Kurs aus der `Courses` Navigations Eigenschaft der `Person` Entität entfernt. Anschließend werden die Änderungen in der Datenbank gespeichert, und die Dropdown Listen werden erneut aufgefüllt, damit die Ergebnisse sofort angezeigt werden.

Fügen Sie der `Page_Load`-Methode Code hinzu, mit dem sichergestellt wird, dass die Fehlermeldungen nicht sichtbar sind, wenn kein Fehler gemeldet werden kann, und fügen Sie Handler für die `DataBound` und `SelectedIndexChanged` Ereignisse der Dozenten-Dropdown Liste hinzu, um die Dropdown Listen für Kurse aufzufüllen:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Führen Sie die Seite aus.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Wählen Sie einen Dozenten aus. In der Dropdown Liste <strong>Kurs zuweisen</strong> werden die Kurse angezeigt, die der Dozenten nicht vermittelt, und in der Dropdown Liste <strong>Kurs entfernen</strong> werden die Kurse angezeigt, denen der Dozenten bereits zugewiesen ist. Wählen Sie im Abschnitt <strong>Kurs zuweisen</strong> einen Kurs aus, und klicken Sie dann auf <strong>zuweisen</strong>. Der Kurs wechselt zur Dropdown Liste " <strong>Kurs entfernen</strong> ". Wählen Sie im Abschnitt <strong>Kurs entfernen</strong> einen Kurs aus, und klicken Sie auf <strong>Entfernen</strong><em>.</em> Der Kurs wechselt zur Dropdown Liste <strong>Kurs zuweisen</strong> .

Sie haben jetzt weitere Möglichkeiten gesehen, mit verwandten Daten zu arbeiten. Im folgenden Tutorial erfahren Sie, wie Sie im Datenmodell Vererbung verwenden, um die Wartbarkeit Ihrer Anwendung zu verbessern.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-6.md)
