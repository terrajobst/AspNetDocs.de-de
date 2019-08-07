---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: Hinzufügen von sortieren, Filtern und Paging mit dem Entity Framework in einer ASP.NET MVC-Anwendung | Microsoft-Dokumentation'
author: tdykstra
description: In diesem Tutorial fügen Sie die Funktion zum Sortieren, Filtern und Paging zur Index Seite " **Studenten** " hinzu. Außerdem erstellen Sie eine einfache Gruppierungs Seite.
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: d9fadc12aa83a8095f364cf39e5376243a7d0670
ms.sourcegitcommit: f774732a3960fca079438a88a5472c37cf7be08a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68810753"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Tutorial: Hinzufügen von sortieren, Filtern und Paging mit dem Entity Framework in einer ASP.NET MVC-Anwendung

Im [vorherigen Tutorial](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)haben Sie eine Reihe von Webseiten für grundlegende CRUD-Vorgänge für `Student` Entitäten implementiert. In diesem Tutorial fügen Sie die Funktion zum Sortieren, Filtern und Paging zur Index Seite " **Studenten** " hinzu. Außerdem erstellen Sie eine einfache Gruppierungs Seite.

In der folgenden Abbildung wird gezeigt, wie die Seite aussieht, wenn Sie fertig sind. Die Spaltenüberschriften sind Links, auf die der Benutzer klicken kann, um die Spalte zu sortieren. Wiederholtes Klicken auf eine Spaltenüberschrift schaltet zwischen aufsteigender und absteigender Sortierreihenfolge um.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

In diesem Tutorial:

> [!div class="checklist"]
> * Hinzufügen von Spaltensortierungslinks
> * Hinzufügen eines Suchfelds
> * Paging hinzufügen
> * Erstellen einer Infoseite

## <a name="prerequisites"></a>Vorraussetzungen

* [Implementieren von grundlegenden CRUD-Funktionen](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a>Hinzufügen von Spaltensortierungslinks

Um der Indexseite "Student" eine Sortierung hinzuzufügen, ändern `Index` Sie die- `Student` Methode des Controllers, und fügen `Student` Sie der Index Ansicht Code hinzu.

### <a name="add-sorting-functionality-to-the-index-method"></a>Hinzufügen von Sortierungs Funktionen zur Index Methode

- Ersetzen Sie in *controllers\studentcontroller.cs*die `Index` -Methode durch den folgenden Code:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Dieser Code empfängt einen `sortOrder`-Parameter aus der Abfragezeichenfolge in der URL. Der Wert der Abfrage Zeichenfolge wird von ASP.NET MVC als Parameter für die Aktionsmethode bereitgestellt. Der-Parameter ist eine Zeichenfolge, die entweder "Name" oder "Date" ist, optional gefolgt von einem Unterstrich und der Zeichenfolge "DESC", um die absteigende Reihenfolge anzugeben. Standardmäßig wird eine aufsteigende Sortierreihenfolge verwendet.

Bei der ersten Anforderung der Indexseite gibt es keine Abfragezeichenfolge. Die Studenten werden in aufsteigender Reihenfolge `LastName`nach angezeigt. Dies ist der Standardwert, der durch den Fall-Through `switch` -Fall in der-Anweisung festgelegt wird. Wenn der Benutzer auf den Link einer Spaltenüberschrift klickt, wird der entsprechende `sortOrder`-Wert in der Abfragezeichenfolge bereitgestellt.

Die beiden `ViewBag` Variablen werden verwendet, damit die Sicht die Spaltenüberschriften Hyperlinks mit den entsprechenden Abfrage Zeichen folgen Werten konfigurieren kann:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Hierbei handelt es sich um ternäre Anweisungen. Der erste Wert gibt an, dass `sortOrder` für den Fall, dass der `ViewBag.NameSortParm` Parameter NULL oder leer ist,\_auf "Name UNSC" festgelegt werden muss. andernfalls sollte er auf eine leere Zeichenfolge festgelegt werden. Durch diese beiden Anweisungen können in der Ansicht die Hyperlinks in den Spaltenüberschriften wie folgt festgelegt werden:

| Aktuelle Sortierreihenfolge | Hyperlink „Nachname“ | Hyperlink „Datum“ |
| --- | --- | --- |
| Nachname (aufsteigend) | descending | ascending |
| Nachname (absteigend) | ascending | ascending |
| Datum (aufsteigend) | ascending | descending |
| Datum (absteigend) | ascending | ascending |

Die-Methode verwendet [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) , um die Spalte anzugeben, nach der sortiert werden soll. Der Code erstellt eine <xref:System.Linq.IQueryable%601> -Variable vor `switch` der-Anweisung, ändert Sie in `switch` der-Anweisung und ruft `ToList` die-Methode `switch` nach der-Anweisung auf. Es wir keine Abfrage an die Datenbank gesendet, wenn Sie die `IQueryable`-Variablen erstellen und ändern. Die Abfrage wird erst ausgeführt, wenn Sie das `IQueryable` Objekt in eine-Auflistung konvertieren, indem Sie eine `ToList`Methode aufrufen, z. b. Daher führt dieser Code zu einer einzelnen Abfrage, die bis zur `return View` -Anweisung nicht ausgeführt wird.

Als Alternative zum Schreiben verschiedener LINQ-Anweisungen für jede Sortierreihenfolge können Sie eine LINQ-Anweisung dynamisch erstellen. Weitere Informationen zu Dynamic LINQ finden Sie unter [Dynamic LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Hinzufügen von Links zu Spaltenüberschriften zur Index Ansicht "Student"

1. Ersetzen Sie in *views\student\index.cshtml*das `<tr>` - `<th>` Element und das-Element für die Überschriften Zeile durch den hervorgehobenen Code:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   Dieser Code verwendet die Informationen in den `ViewBag` -Eigenschaften, um Hyperlinks mit den entsprechenden Abfrage Zeichen folgen Werten einzurichten.

2. Führen Sie die Seite aus, und klicken Sie auf die Spaltenüberschriften **Nachname** und Registrierung, um zu überprüfen, ob die Sortierung funktioniert.

   Nachdem Sie auf die Überschrift " **Nachname** " klicken, werden die Studenten in absteigender Reihenfolge nach Namen angezeigt.

## <a name="add-a-search-box"></a>Hinzufügen eines Suchfelds

Um der Indexseite "Studenten" Filter hinzuzufügen, fügen Sie der Ansicht ein Textfeld und eine Schaltfläche "Senden" hinzu, und `Index` nehmen Sie entsprechende Änderungen in der-Methode vor. Mit dem Textfeld können Sie in den Feldern Vorname und Nachname eine Zeichenfolge eingeben, nach der gesucht werden soll.

### <a name="add-filtering-functionality-to-the-index-method"></a>Hinzufügen der Filterfunktion zur Indexmethode

- Ersetzen Sie in *controllers\studentcontroller.cs*die `Index` -Methode durch den folgenden Code (die Änderungen werden hervorgehoben):

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Der Code fügt der `searchString` `Index` Methode einen Parameter hinzu. Der Zeichenfolgenwert für die Suche wird aus einem Textfeld empfangen, das Sie zur Indexansicht hinzufügen. Außerdem wird der LINQ-Anweisung eine `where` -Klausel hinzugefügt, die nur Studenten auswählt, deren Vorname oder Nachname die Such Zeichenfolge enthält. Die Anweisung, die die <xref:System.Linq.Queryable.Where%2A> -Klausel hinzufügt, wird nur ausgeführt, wenn ein Wert vorhanden ist, der gesucht werden soll.

> [!NOTE]
> In vielen Fällen können Sie dieselbe Methode entweder für eine Entity Framework Entitätenmenge oder als Erweiterungsmethode für eine Auflistung im Arbeitsspeicher aufzurufen. Die Ergebnisse sind in der Regel identisch, aber in manchen Fällen können Sie sich unterscheiden.
>
> Beispielsweise gibt die .NET Framework Implementierung `Contains` der-Methode alle Zeilen zurück, wenn Sie eine leere Zeichenfolge an Sie übergeben, aber der Entity Framework Provider für SQL Server Compact 4,0 gibt 0 Zeilen für leere Zeichen folgen zurück. Daher stellt der Code im Beispiel (mit der `Where` -Anweisung in `if` einer-Anweisung) sicher, dass Sie für alle Versionen von SQL Server dieselben Ergebnisse erhalten. Außerdem führt die .NET Framework Implementierung `Contains` der-Methode standardmäßig einen Vergleich mit Berücksichtigung der Groß-/Kleinschreibung durch, Entity Framework SQL Server Anbieter jedoch standardmäßig Vergleiche ohne Berücksichtigung der Groß-/Kleinschreibung ausführen. Wenn Sie die- `ToUpper` Methode aufrufen, um den Test explizit ohne Berücksichtigung der Groß-/Kleinschreibung zu machen, wird daher sichergestellt, dass sich die Ergebnisse nicht ändern, `IEnumerable` Wenn Sie den Code `IQueryable` später ändern, um ein Repository zu verwenden (Beim Aufrufen der `Contains`-Methode einer `IEnumerable`-Sammlung erhalten Sie die .NET Framework-Implementierung. Wenn Sie sie auf einem `IQueryable`-Objekt aufrufen, erhalten Sie die Implementierung des Datenanbieters.)
>
> Die NULL-Behandlung kann sich auch bei unterschiedlichen Datenbankanbietern oder bei `IQueryable` Verwendung eines-Objekts im Vergleich zu `IEnumerable` unterscheiden, wenn Sie eine-Auflistung verwenden. In einigen Szenarien gibt eine `Where` Bedingung `table.Column != 0` wie z. b. möglicherweise keine Spalten zurück `null` , die als Wert aufweisen. Standardmäßig generiert EF zusätzliche SQL-Operatoren, um Gleichheit zwischen NULL-Werten in der Datenbank zu gewährleisten, wie es im Arbeitsspeicher funktioniert, aber Sie können das [usedatabasenullsemantics](https://docs.microsoft.com/dotnet/api/system.data.entity.infrastructure.dbcontextconfiguration.usedatabasenullsemantics) -Flag in EF6 festlegen oder die [userelationalnulls](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.infrastructure.relationaldbcontextoptionsbuilder-2.userelationalnulls) -Methode in EF Core auf Konfigurieren Sie dieses Verhalten.

### <a name="add-a-search-box-to-the-student-index-view"></a>Hinzufügen eines Suchfelds zur Index Ansicht "Student"

1. Fügen Sie in *views\student\index.cshtml*den markierten Code direkt vor dem öffnenden `table` Tag hinzu, um eine Beschriftung, ein Textfeld und eine **Such** Schaltfläche zu erstellen.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. Führen Sie die Seite aus, geben Sie eine Such Zeichenfolge ein, und klicken Sie auf **Suchen** , um zu überprüfen, ob

   Beachten Sie, dass die URL nicht die Such Zeichenfolge "a" enthält, was bedeutet, dass Sie bei Verwendung des Lesezeichens nicht die gefilterte Liste erhalten, wenn Sie das Lesezeichen für diese Seite verwenden. Dies gilt auch für die Spalten Sortier Links, da die gesamte Liste sortiert wird. Sie ändern die **Such** Schaltfläche, um später in diesem Tutorial Abfrage Zeichenfolgen für Filterkriterien zu verwenden.

## <a name="add-paging"></a>Paging hinzufügen

Um Paging zur Indexseite "Studenten" hinzuzufügen, installieren Sie zunächst das nuget-Paket " **pagedlist. MVC** ". Anschließend nehmen Sie zusätzliche Änderungen an der `Index` -Methode vor und fügen paginglinks `Index` zur Ansicht hinzu. **Pagedlist. MVC** ist eines von vielen guten Paging-und Sortierungs Paketen für ASP.NET MVC, und die Verwendung hier ist nur als Beispiel gedacht, nicht als Empfehlung für andere Optionen.

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installieren Sie das nuget-Paket "pgedlist. MVC".

Das nuget-Paket " **pgedlist. MVC** " installiert automatisch das Paket " **pgedlist** " als Abhängigkeit. Mit dem Paket "pgedlist" werden ein `PagedList` Sammlungstyp `IQueryable` und `IEnumerable` Erweiterungs Methoden für-und-Sammlungen installiert. Die Erweiterungs Methoden erstellen eine einzelne `PagedList` Datenseite in einer Auflistung aus Ihrem `IQueryable` oder `IEnumerable`, und die `PagedList` -Auflistung stellt mehrere Eigenschaften und Methoden bereit, die das Paging vereinfachen. Das Paket " **pagedlist. MVC** " installiert ein Paging-Hilfsprogramm, das die Pagingschaltflächen anzeigt.

1. Klicken Sie im Menü Extras auf **nuget-Paket-Manager** und dann auf Paket- **Manager-Konsole**.

2. Stellen Sie im Fenster **Paket-Manager-Konsole** sicher, dass die **Paketquelle** **nuget.org** und das **Standard Projekt** den Wert **condesouniversity**hat, und geben Sie dann den folgenden Befehl ein:

   ```text
   Install-Package PagedList.Mvc
   ```

3. Erstellen Sie das Projekt.

### <a name="add-paging-functionality-to-the-index-method"></a>Fügen Sie Pagingfunktionen zur Indexmethode hinzu

1. Fügen Sie in *controllers\studentcontroller.cs*eine `using` -Anweisung für `PagedList` den-Namespace hinzu:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. Ersetzen Sie die `Index`-Methode durch folgenden Code:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   Dieser Code fügt der `page` Methoden Signatur einen Parameter, einen aktuellen Sortierreihenfolge-Parameter und einen aktuellen Filter Parameter hinzu:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   Wenn die Seite zum ersten Mal angezeigt wird oder wenn der Benutzer nicht auf einen Paging-oder Sortier Link geklickt hat, sind alle Parameter NULL. Wenn auf einen paginglink geklickt wird, `page` enthält die Variable die anzuzeigende Seitenzahl.

   Eine `ViewBag` Eigenschaft stellt die Ansicht mit der aktuellen Sortierreihenfolge bereit, da diese in den paginglinks enthalten sein muss, damit die Sortierreihenfolge beim Paging unverändert bleibt:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   Eine andere Eigenschaft `ViewBag.CurrentFilter`,, stellt die Ansicht mit der aktuellen Filter Zeichenfolge bereit. Dieser Wert muss in den Paginglinks enthalten sein, damit die Filtereinstellungen während des Pagingvorgangs beibehalten werden, und er muss im Textfeld wiederhergestellt werden, wenn die Seite erneut angezeigt wird. Wenn die Suchzeichenfolge während des Pagingvorgangs geändert wird, muss die Seite auf 1 zurückgesetzt werden, da der neue Filter andere Daten anzeigen kann. Die Such Zeichenfolge wird geändert, wenn ein Wert in das Textfeld eingegeben wird und die Schaltfläche "Senden" gedrückt wird. In diesem Fall ist der `searchString` -Parameter nicht NULL.

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   Am Ende der-Methode konvertiert die `ToPagedList` -Erweiterungsmethode für das Students `IQueryable` -Objekt die Student-Abfrage in eine einzelne Seite von Studenten in einem Sammlungstyp, der Paging unterstützt. Diese einzelne Seite der Schüler/Studenten wird dann an die Ansicht weitergeleitet:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   Die `ToPagedList`-Methode nimmt eine Seitenanzahl. Die zwei Fragezeichen stellen den [null-Sammel Operator](/dotnet/csharp/language-reference/operators/null-coalescing-operator)dar. Der Nullzusammensetzungsoperator definiert einen Standardwert für einen Nullable-Typ. Der `(page ?? 1)`-Ausdruck bedeutet, dass `page` zurückgegeben wird, wenn dies über einen Wert verfügt, oder 1, wenn `page` gleich 0 (null) ist.

### <a name="add-paging-links-to-the-student-index-view"></a>Paging-Links zur Index Ansicht "Student" hinzufügen

1. Ersetzen Sie in *views\student\index.cshtml*den vorhandenen Code durch den folgenden Code. Die Änderungen werden hervorgehoben.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   Die `@model`-Anweisung am oberen Rand der Seite gibt an, dass die Ansicht nun ein `PagedList`-Objekt anstelle eines `List`-Objekts aufruft.

   Die `using` -Anweisung `PagedList.Mvc` für ermöglicht den Zugriff auf das MVC-Hilfsprogramm für die Paging-Schaltflächen.

   Im Code wird eine Überladung von [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) verwendet, mit der [FormMethod. Get angegeben werden](/previous-versions/aspnet/dd460179(v=vs.100))kann.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   Der Standard [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) übermittelt Formulardaten mit einem Post. Dies bedeutet, dass Parameter im HTTP-Nachrichtentext und nicht in der URL als Abfrage Zeichenfolgen übergeben werden. Bei der Angabe von HTTP GET werden die Formulardaten als Abfragezeichenfolgen an die URL übergeben. Dadurch können Benutzer ein Lesezeichen für die URL erstellen. Die [W3C-Richtlinien für die Verwendung von HTTP Get](http://www.w3.org/2001/tag/doc/whenToUseGet.html) empfehlen die Verwendung von Get, wenn die Aktion nicht zu einem Update führt.

   Das Textfeld wird mit der aktuellen Such Zeichenfolge initialisiert, sodass beim Klicken auf eine neue Seite die aktuelle Such Zeichenfolge angezeigt wird.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   Die Spaltenüberschriftenlinks verwenden die Abfragezeichenfolge, um die aktuelle Suchzeichenfolge an den Controller zu übergeben, damit Benutzer Filterergebnisse sortieren können:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   Die aktuelle Seite und die Gesamtzahl der Seiten werden angezeigt.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   Wenn keine Seiten zum Anzeigen vorhanden sind, wird "Seite 0 von 0" angezeigt. (In diesem Fall ist die Seitenzahl größer als die Seitenanzahl, `Model.PageNumber` da 1 und `Model.PageCount` 0 ist.)

   Die Paging-Schaltflächen werden vom `PagedListPager` Hilfsprogramm angezeigt:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   Das `PagedListPager` Hilfsprogramm bietet eine Reihe von Optionen, die Sie anpassen können, einschließlich URLs und Formatierungen. Weitere Informationen finden Sie auf der GitHub-Website unter [troygoode/pgedlist](https://github.com/TroyGoode/PagedList) .

2. Führen Sie die Seite aus.

   Klicken Sie auf die Paginglinks in verschiedenen Sortierreihenfolgen, um sicherzustellen, dass die Paging funktioniert. Geben Sie dann eine Suchzeichenfolge ein. Probieren Sie Paging erneut aus, um sicherzustellen, dass sie auch mit Sortier- und Filtervorgängen ordnungsgemäß funktioniert.

## <a name="create-an-about-page"></a>Erstellen einer Infoseite

Die Seite "Info" der Website der Website der Website von "Website" zeigt an, wie viele Studenten sich für jedes Registrierungsdatum registriert haben. Das erfordert Gruppieren und einfache Berechnungen dieser Gruppen. Um dies zu erreichen, ist Folgendes erforderlich:

- Erstellen Sie eine Ansichtsmodellklasse für die Daten, die Sie an die Ansicht übergeben müssen.
- Ändern Sie `About` die-Methode `Home` im Controller.
- Ändern Sie `About` die Ansicht.

### <a name="create-the-view-model"></a>Erstellen des Ansichts Modells

Erstellen Sie einen *ViewModels* -Ordner im Projektordner. Fügen Sie in diesem Ordner eine Klassendatei *EnrollmentDateGroup.cs* hinzu, und ersetzen Sie den Vorlagen Code durch den folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Ändern des Home-Controllers

1. Fügen Sie in *HomeController.cs*am Anfang `using` der Datei die folgenden-Anweisungen hinzu:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. Fügen Sie eine Klassen Variable für den Daten Bank Kontext unmittelbar nach der öffnenden geschweiften Klammer für die-Klasse hinzu:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. Ersetzen Sie die `About`-Methode durch folgenden Code:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   Die LINQ-Anweisung gruppiert die Studentenentitäten nach Anmeldedatum, berechnet die Anzahl der Entitäten in jeder Gruppe und speichert die Ergebnisse in einer Sammlung von `EnrollmentDateGroup`-Ansichtsmodellobjekten.

4. Fügen Sie `Dispose` eine Methode hinzu:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Ändern der Infoansicht

1. Ersetzen Sie den Code in der Datei " *views\home\about.cshtml* " durch den folgenden Code:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. Führen Sie die APP aus, und klicken Sie auf den Link info.

   Die Anzahl der Schüler/Studenten für die einzelnen Registrierungs Datumsangaben werden in einer Tabelle angezeigt.

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Links zu anderen Entity Framework Ressourcen finden Sie unter [ASP.NET Data Access-Empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial:

> [!div class="checklist"]
> * Hinzufügen von Spaltensortierungslinks
> * Hinzufügen eines Suchfelds
> * Paging hinzufügen
> * Erstellen einer Infoseite

Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie Verbindungs Resilienz und Befehls Abfang Funktion verwenden.
> [!div class="nextstepaction"]
> [Verbindungsresilienz und Befehls Abfang Funktion](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
