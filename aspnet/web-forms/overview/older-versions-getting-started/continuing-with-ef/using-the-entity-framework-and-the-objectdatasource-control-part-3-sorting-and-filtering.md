---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Verwenden des Entity Framework 4,0 und des ObjectDataSource-Steuer Elements, Teil 3: Sortieren und Filtern | Microsoft-Dokumentation'
author: tdykstra
description: Diese tutorialreihe basiert auf der Webanwendung der Website von "Web", die in der tutorialreihe für die ersten Schritte mit Entity Framework 4,0 erstellt wurde. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 603120864528b9a5ff81214270eb9a7f1b68b347
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78512715"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Verwenden des Entity Framework 4,0 und des ObjectDataSource-Steuer Elements, Teil 3: Sortieren und Filtern

von [Tom Dykstra](https://github.com/tdykstra)

> Diese tutorialreihe basiert auf der Webanwendung der Website von "Web", die in der tutorialreihe für die ersten Schritte [mit Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) erstellt wurde. Wenn Sie die vorherigen Tutorials nicht durchgearbeitet haben, können Sie die Anwendung, die Sie erstellt haben, als Ausgangspunkt für dieses Tutorial [herunterladen](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) . Sie können auch [die Anwendung herunterladen](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , die von der kompletten tutorialreihe erstellt wird. Wenn Sie Fragen zu den Tutorials haben, können Sie Sie im ASP.net- [Entity Framework Forum](https://forums.asp.net/1227.aspx)Posten.

Im vorherigen Tutorial haben Sie das Repository-Muster in einer n-Tier-Webanwendung implementiert, die die Entity Framework und das `ObjectDataSource`-Steuerelement verwendet. In diesem Tutorial wird gezeigt, wie Sie das Sortieren und Filtern durchführen und Master/Detail-Szenarien verarbeiten. Die folgenden Erweiterungen werden der Seite " *Departments. aspx* " hinzugefügt:

- Ein Textfeld, in dem Benutzer Abteilungen nach Namen auswählen können.
- Eine Liste der Kurse für jede Abteilung, die im Raster angezeigt wird.
- Die Sortier Fähigkeit durch Klicken auf Spaltenüberschriften.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Hinzufügen der Möglichkeit zum Sortieren von GridView-Spalten

Öffnen Sie die Seite " *Departments. aspx* ", und fügen Sie dem `ObjectDataSource` Steuerelement mit dem Namen `DepartmentsObjectDataSource`ein `SortParameterName="sortExpression"` Attribut hinzu. (Später erstellen Sie eine `GetDepartments`-Methode, die einen Parameter mit dem Namen "`sortExpression`" annimmt.) Das Markup für das öffnende Tag des-Steuer Elements ähnelt nun dem folgenden Beispiel.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Fügen Sie das `AllowSorting="true"`-Attribut zum öffnenden Tag des `GridView` Steuer Elements hinzu. Das Markup für das öffnende Tag des-Steuer Elements ähnelt nun dem folgenden Beispiel.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

Legen Sie in *Departments.aspx.cs*die Standard Sortierreihenfolge fest, indem Sie die `Sort` Methode des `GridView`-Steuer Elements von der `Page_Load`-Methode aufrufen:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Sie können Code hinzufügen, der die Geschäftslogik Klasse oder die Repository-Klasse sortiert oder filtert. Wenn Sie in der Geschäftslogik Klasse Vorgehen, wird die Sortierung oder Filterung durchgeführt, nachdem die Daten aus der Datenbank abgerufen wurden, da die Geschäftslogik Klasse mit einem vom Repository zurückgegebenen `IEnumerable` Objekt arbeitet. Wenn Sie Sortier-und Filterungs Code in der Repository-Klasse hinzufügen und diesen Vorgang vor dem Konvertieren eines LINQ-Ausdrucks oder einer Objekt Abfrage in ein `IEnumerable` Objekt ausführen, werden die Befehle zur Verarbeitung an die Datenbank weitergegeben, was in der Regel effizienter ist. In diesem Tutorial implementieren Sie das Sortieren und Filtern auf eine Weise, die bewirkt, dass die Verarbeitung von der Datenbank ausgeführt wird, d. –. im Repository.

Zum Hinzufügen der Sortierfunktion müssen Sie der Repository-Schnittstelle und den Repository-Klassen sowie der Geschäftslogik Klasse eine neue Methode hinzufügen. Fügen Sie in der Datei *ISchoolRepository.cs* eine neue `GetDepartments`-Methode hinzu, die einen `sortExpression` Parameter annimmt, der zum Sortieren der Liste der zurückgegebenen Abteilungen verwendet wird:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

Der `sortExpression`-Parameter gibt die Spalte an, nach der sortiert werden soll, und die Sortierrichtung.

Fügen Sie der Datei *SchoolRepository.cs* Code für die neue Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Ändern Sie die vorhandene Parameter lose `GetDepartments`-Methode, um die neue Methode aufzurufen:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

Fügen Sie im Testprojekt die folgende neue Methode zu *MockSchoolRepository.cs*hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Wenn Sie Komponententests erstellen, die von dieser Methode abhängen, die eine sortierte Liste zurückgibt, müssen Sie die Liste vor der Rückgabe sortieren. In diesem Tutorial erstellen Sie keine Tests, sodass die Methode nur die unsortierte Liste der Abteilungen zurückgeben kann.

Fügen Sie in der Datei *SchoolBL.cs* der Geschäftslogik Klasse die folgende neue Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Dieser Code übergibt den Sort-Parameter an die Repository-Methode.

Führen Sie die Seite " *Departments. aspx* " aus.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Sie können jetzt auf eine beliebige Spaltenüberschrift klicken, um nach dieser Spalte zu sortieren. Wenn die Spalte bereits sortiert ist, wird durch Klicken auf die Überschrift die Sortierreihenfolge umgekehrt.

## <a name="adding-a-search-box"></a>Hinzufügen eines Suchfelds

In diesem Abschnitt fügen Sie ein Such Textfeld hinzu, verknüpfen es mit dem `ObjectDataSource`-Steuerelement mithilfe eines Control-Parameters und fügen der Geschäftslogik Klasse eine Methode hinzu, um das Filtern zu unterstützen.

Öffnen Sie die Seite " *Departments. aspx* ", und fügen Sie das folgende Markup zwischen der Überschrift und dem ersten `ObjectDataSource` Steuerelement hinzu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

Gehen Sie im `ObjectDataSource` Steuerelement mit dem Namen `DepartmentsObjectDataSource`folgendermaßen vor:

- Fügen Sie ein `SelectParameters`-Element für einen Parameter mit dem Namen `nameSearchString` hinzu, der den im `SearchTextBox`-Steuerelement eingegebenen Wert abruft.
- Ändern Sie den Wert des `SelectMethod` Attributs in `GetDepartmentsByName`. (Sie erstellen diese Methode später.)

Das Markup für das `ObjectDataSource`-Steuerelement ähnelt nun dem folgenden Beispiel:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

Fügen Sie in *ISchoolRepository.cs*eine `GetDepartmentsByName` Methode hinzu, die sowohl `sortExpression` als auch `nameSearchString` Parameter annimmt:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

Fügen Sie in *SchoolRepository.cs*die folgende neue Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

In diesem Code wird eine `Where`-Methode verwendet, um Elemente auszuwählen, die die Such Zeichenfolge enthalten. Wenn die Such Zeichenfolge leer ist, werden alle Datensätze ausgewählt. Beachten Sie Folgendes: Wenn Sie Methodenaufrufe in einer Anweisung wie diesem angeben (`Include`und dann `OrderBy`, dann `Where`), muss die `Where` Methode immer "Last" lauten.

Ändern Sie die vorhandene `GetDepartments`-Methode, die einen `sortExpression`-Parameter annimmt, um die neue Methode aufzurufen:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

Fügen Sie in *MockSchoolRepository.cs* im Testprojekt die folgende neue Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

Fügen Sie in *SchoolBL.cs*die folgende neue Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Führen Sie die Seite *Departments. aspx* aus, und geben Sie eine Such Zeichenfolge ein, um sicherzustellen, dass die Auswahl Logik funktioniert Lassen Sie das Textfeld leer, und versuchen Sie eine Suche, um sicherzustellen, dass alle Datensätze zurückgegeben werden.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Hinzufügen einer Detail Spalte für jede Raster Zeile

Als nächstes möchten Sie alle Kurse der einzelnen Abteilungen sehen, die in der rechten Zelle des Rasters angezeigt werden. Zu diesem Zweck verwenden Sie ein geändertes `GridView`-Steuerelement und verbinden es mit Daten aus der `Courses`-Navigations Eigenschaft der `Department`-Entität.

Öffnen Sie " *Departments. aspx* ", und geben Sie im Markup für das `GridView` Steuerelement einen Handler für das `RowDataBound` Ereignis an. Das Markup für das öffnende Tag des-Steuer Elements ähnelt nun dem folgenden Beispiel.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Fügen Sie ein neues `TemplateField`-Element nach dem `Administrator` Vorlagen Feld hinzu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Dieses Markup erstellt ein geschsted `GridView` Steuerelement, das die Kursnummer und den Titel einer Liste von Kursen anzeigt. Es wird keine Datenquelle angegeben, da Sie Sie im Code in den `RowDataBound`-Handler übernehmen.

Öffnen Sie *Departments.aspx.cs* , und fügen Sie den folgenden Handler für das `RowDataBound`-Ereignis hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Dieser Code Ruft die `Department`-Entität aus den Ereignis Argumenten ab, konvertiert die `Courses`-Navigations Eigenschaft in eine `List`-Auflistung und stellt der der-Auflistung hinzugefügten `GridView`.

Öffnen Sie die Datei *SchoolRepository.cs* , und geben Sie Eager Loading für die `Courses` Navigations Eigenschaft an, indem Sie die `Include`-Methode in der Objekt Abfrage aufrufen, die Sie in der `GetDepartmentsByName`-Methode erstellen. Die `return`-Anweisung in der `GetDepartmentsByName`-Methode ähnelt nun dem folgenden Beispiel.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Führen Sie die Seite aus. Zusätzlich zu den Sortier-und Filterfunktionen, die Sie zuvor hinzugefügt haben, zeigt das GridView-Steuerelement jetzt geschamelte Kursdetails für jede Abteilung an.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Dadurch wird die Einführung in Sortier-, Filter-und Master/Detail-Szenarien vervollständigt. Im nächsten Tutorial erfahren Sie, wie Sie Parallelität verarbeiten.

> [!div class="step-by-step"]
> [Zurück](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Weiter](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
