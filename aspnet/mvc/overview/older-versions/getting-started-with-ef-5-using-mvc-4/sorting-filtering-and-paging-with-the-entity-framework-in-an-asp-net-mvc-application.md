---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Sortieren, Filtern und Paging mit dem Entity Framework in einer ASP.NET MVC-Anwendung (3 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio erstellt werden...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b1ddb70805dcb07fb60eea895ff572c054bde5c6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595223"
---
# <a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Sortieren, Filtern und Paging mit dem Entity Framework in einer ASP.NET MVC-Anwendung (3 von 10)

von [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio 2012 erstellt werden. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe von Anfang an starten oder [ein Starter-Projekt für dieses Kapitel herunterladen](building-the-ef5-mvc4-chapter-downloads.md) und hier beginnen.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem stoßen, das Sie nicht beheben können, [Laden Sie das abgeschlossene Kapitel herunter](building-the-ef5-mvc4-chapter-downloads.md) , und versuchen Sie, das Problem zu reproduzieren. Im Allgemeinen können Sie die Lösung für das Problem finden, indem Sie Ihren Code mit dem abgeschlossenen Code vergleichen. Informationen zu häufigen Fehlern und deren Lösung finden Sie unter [Fehler und](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors) Problem Umgehungen.

Im vorherigen Tutorial haben Sie eine Reihe von Webseiten für grundlegende CRUD-Vorgänge für `Student` Entitäten implementiert. In diesem Tutorial fügen Sie die Funktion zum Sortieren, Filtern und Paging zur Index Seite " **Studenten** " hinzu. Sie werden auch eine Seite erstellen, auf der einfache Gruppierungsvorgänge ausgeführt werden.

Die folgende Abbildung zeigt, wie die Seite am Ende aussehen wird. Die Spaltenüberschriften sind Links, auf die der Benutzer klicken kann, um die Spalte zu sortieren. Wiederholtes Klicken auf eine Spaltenüberschrift schaltet zwischen aufsteigender und absteigender Sortierreihenfolge um.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Hinzufügen von Spaltensortierungslinks zur Studentenindexseite

Um der Indexseite "Student" eine Sortierung hinzuzufügen, ändern Sie die `Index`-Methode des `Student` Controllers, und fügen Sie der `Student` Index Sicht Code hinzu.

### <a name="add-sorting-functionality-to-the-index-method"></a>Hinzufügen von Sortierungs Funktionen zur Index Methode

Ersetzen Sie in *controllers\studentcontroller.cs*die `Index`-Methode durch den folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Dieser Code empfängt einen `sortOrder`-Parameter aus der Abfragezeichenfolge in der URL. Der Wert der Abfrage Zeichenfolge wird von ASP.NET MVC als Parameter für die Aktionsmethode bereitgestellt. Der Parameter ist eine Zeichenfolge, entweder „Name“ oder „Date“, optional gefolgt von einem Unterstrich und der Zeichenfolge „desc“, die die absteigende Reihenfolge angibt. Standardmäßig wird eine aufsteigende Sortierreihenfolge verwendet.

Bei der ersten Anforderung der Indexseite gibt es keine Abfragezeichenfolge. Die Studenten werden in aufsteigender Reihenfolge nach `LastName`angezeigt. Dies ist die Standardeinstellung, die durch den Fall-Through-Fall in der `switch`-Anweisung festgelegt wird. Wenn der Benutzer auf den Link einer Spaltenüberschrift klickt, wird der entsprechende `sortOrder`-Wert in der Abfragezeichenfolge bereitgestellt.

Die beiden `ViewBag` Variablen werden verwendet, damit die Sicht die Spaltenüberschriften Hyperlinks mit den entsprechenden Abfrage Zeichen folgen Werten konfigurieren kann:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Hierbei handelt es sich um ternäre Anweisungen. Der erste Wert gibt an, dass `ViewBag.NameSortParm` auf "Name\_" festgelegt werden muss, wenn der `sortOrder` Parameter NULL oder leer ist. Andernfalls sollte Sie auf eine leere Zeichenfolge festgelegt werden. Durch diese beiden Anweisungen können in der Ansicht die Hyperlinks in den Spaltenüberschriften wie folgt festgelegt werden:

| Aktuelle Sortierreihenfolge | Hyperlink „Nachname“ | Hyperlink „Datum“ |
| --- | --- | --- |
| Nachname (aufsteigend) | descending | ascending |
| Nachname (absteigend) | ascending | ascending |
| Datum (aufsteigend) | ascending | descending |
| Datum (absteigend) | ascending | ascending |

Die-Methode verwendet [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) , um die Spalte anzugeben, nach der sortiert werden soll. Der Code erstellt vor der `switch` Anweisung eine [iquerable](https://msdn.microsoft.com/library/bb351562.aspx) -Variable, ändert Sie in der `switch`-Anweisung und ruft die `ToList`-Methode nach der `switch`-Anweisung auf. Es wir keine Abfrage an die Datenbank gesendet, wenn Sie die `IQueryable`-Variablen erstellen und ändern. Die Abfrage wird erst ausgeführt, wenn Sie das `IQueryable` Objekt in eine Auflistung konvertieren, indem Sie eine Methode aufrufen, z. b. `ToList`. Daher führt dieser Code zu einer einzelnen Abfrage, die bis zur `return View`-Anweisung nicht ausgeführt wird.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Hinzufügen von Links zu Spaltenüberschriften zur Index Ansicht "Student"

Ersetzen Sie in *views\student\index.cshtml*die Elemente `<tr>` und `<th>` für die Kopfzeile durch den hervorgehobenen Code:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

In diesem Code werden die Informationen in den `ViewBag` Eigenschaften verwendet, um Hyperlinks mit den entsprechenden Abfrage Zeichen folgen Werten einzurichten.

Führen Sie die Seite aus **, und klicken** Sie auf die Spaltenüberschriften **Nachname** und Registrierung, um zu überprüfen, ob die Sortierung funktioniert.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Nachdem Sie auf die Überschrift " **Nachname** " klicken, werden die Studenten in absteigender Reihenfolge nach Namen angezeigt.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Hinzufügen eines Suchfelds zur Index Seite "Studenten"

Wenn Sie eine Filterfunktion zur Studentenindexseite hinzufügen möchten, dann fügen Sie ein Textfeld und die Schaltfläche „Senden“ zur Ansicht hinzu, und führen Sie die entsprechenden Änderungen in der `Index`-Methode aus. Sie können eine Zeichenfolge in das Textfeld für Vor- und Nachnamen eingeben, um eine Suche zu starten.

### <a name="add-filtering-functionality-to-the-index-method"></a>Hinzufügen der Filter Funktionalität zur Index Methode

Ersetzen Sie in *controllers\studentcontroller.cs*die `Index`-Methode durch den folgenden Code (die Änderungen werden hervorgehoben):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Sie haben einen `searchString`-Parameter zur `Index`-Methode hinzugefügt. Sie haben auch der LINQ-Anweisung eine `where`-Klausel hinzugefügt, die nur Studenten auswählt, deren Vorname oder Nachname die Such Zeichenfolge enthält. Der Wert der Such Zeichenfolge wird von einem Textfeld empfangen, das Sie der Index Ansicht hinzufügen. Die Anweisung, die die [Where](https://msdn.microsoft.com/library/bb535040.aspx) -Klausel hinzufügt, wird nur ausgeführt, wenn ein Wert vorhanden ist, nach dem gesucht werden soll.

> [!NOTE]
> In vielen Fällen können Sie dieselbe Methode entweder für eine Entity Framework Entitätenmenge oder als Erweiterungsmethode für eine Auflistung im Arbeitsspeicher aufzurufen. Die Ergebnisse sind in der Regel identisch, aber in manchen Fällen können Sie sich unterscheiden. Beispielsweise gibt die .NET Framework Implementierung der `Contains`-Methode alle Zeilen zurück, wenn Sie eine leere Zeichenfolge an Sie übergeben, aber der Entity Framework Provider für SQL Server Compact 4,0 gibt null Zeilen für leere Zeichen folgen zurück. Daher stellt der Code im Beispiel (mit der `Where`-Anweisung in einer `if`-Anweisung) sicher, dass Sie für alle Versionen von SQL Server dieselben Ergebnisse erhalten. Außerdem führt die .NET Framework Implementierung der `Contains`-Methode standardmäßig einen Vergleich mit Berücksichtigung der Groß-/Kleinschreibung durch, Entity Framework SQL Server Anbieter jedoch standardmäßig Vergleiche ohne Berücksichtigung der Groß-/Kleinschreibung ausführen. Wenn Sie die `ToUpper`-Methode aufrufen, um den Test explizit ohne Berücksichtigung der Groß-/Kleinschreibung zu machen, wird dadurch sichergestellt, dass sich die Ergebnisse nicht ändern, wenn Sie den Code später ändern, um ein Repository zu verwenden, das eine `IEnumerable` Auflistung anstelle eines `IQueryable` Objekts (Beim Aufrufen der `Contains`-Methode einer `IEnumerable`-Sammlung erhalten Sie die .NET Framework-Implementierung. Wenn Sie sie auf einem `IQueryable`-Objekt aufrufen, erhalten Sie die Implementierung des Datenanbieters.)

### <a name="add-a-search-box-to-the-student-index-view"></a>Hinzufügen eines Suchfelds zur Studentenindexansicht

Fügen Sie in *views\student\index.cshtml*den markierten Code direkt vor dem öffnenden `table`-Tag hinzu, um eine Beschriftung, ein Textfeld und eine **Such** Schaltfläche zu erstellen.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Führen Sie die Seite aus, geben Sie eine Such Zeichenfolge ein, und klicken Sie auf **Suchen** , um zu überprüfen, ob

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Beachten Sie, dass die URL nicht die Such Zeichenfolge "a" enthält, was bedeutet, dass Sie bei Verwendung des Lesezeichens nicht die gefilterte Liste erhalten, wenn Sie das Lesezeichen für diese Seite verwenden. Sie ändern die **Such** Schaltfläche, um später in diesem Tutorial Abfrage Zeichenfolgen für Filterkriterien zu verwenden.

## <a name="add-paging-to-the-students-index-page"></a>Hinzufügen von Paging zur Index Seite "Studenten"

Um Paging zur Index Seite "Studenten" hinzuzufügen, installieren Sie zunächst das nuget-Paket " **pagedlist. MVC** ". Anschließend nehmen Sie zusätzliche Änderungen in der `Index`-Methode vor und fügen paginglinks zur `Index` Ansicht hinzu. **Pagedlist. MVC** ist eines von vielen guten Paging-und Sortierungs Paketen für ASP.NET MVC, und die Verwendung hier ist nur als Beispiel gedacht, nicht als Empfehlung für andere Optionen. In der folgenden Abbildung sind die Paging-Links dargestellt.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installieren Sie das nuget-Paket "pgedlist. MVC".

Das nuget-Paket " **pgedlist. MVC** " installiert automatisch das Paket " **pgedlist** " als Abhängigkeit. Das Paket " **pgedlist** " installiert einen `PagedList` Sammlungstyp und Erweiterungs Methoden für `IQueryable` und `IEnumerable` Sammlungen. Die Erweiterungs Methoden erstellen eine einzelne Datenseite in einer `PagedList` Auflistung aus Ihrem `IQueryable` oder `IEnumerable`, und die `PagedList` Auflistung bietet mehrere Eigenschaften und Methoden, die das Paging vereinfachen. Das Paket " **pagedlist. MVC** " installiert ein Paging-Hilfsprogramm, das die Pagingschaltflächen anzeigt.

Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager** und dann **auf nuget-Pakete für**Projekt Mappe verwalten.

Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf die Registerkarte **Online** , und geben Sie dann im Suchfeld "Auslagerungsseiten" ein. Wenn das Paket " **pgedlist. MVC** " angezeigt wird, klicken Sie auf **Installieren**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Klicken Sie im Feld **Projekte auswählen** auf **OK**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Paging-Funktionalität zur Index Methode hinzufügen

Fügen Sie in *controllers\studentcontroller.cs*eine `using`-Anweisung für den `PagedList`-Namespace hinzu:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Ersetzen Sie die `Index`-Methode durch folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Dieser Code fügt der Methoden Signatur einen `page` Parameter, einen aktuellen Sortierreihenfolge-Parameter und einen aktuellen Filter Parameter hinzu, wie hier gezeigt:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Wenn die Seite zum ersten Mal angezeigt wird oder wenn der Benutzer nicht auf einen Paging- oder Sortierlink geklickt hat, werden alle Parameter gleich 0 (null) sein. Wenn auf einen paginglink geklickt wird, enthält die `page` Variable die anzuzeigende Seitenzahl.

`A ViewBag`-Eigenschaft stellt die Ansicht mit der aktuellen Sortierreihenfolge bereit, da diese in den paginglinks enthalten sein muss, damit die Sortierreihenfolge beim Paging unverändert bleibt:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Eine andere Eigenschaft, `ViewBag.CurrentFilter`, stellt die Ansicht mit der aktuellen Filter Zeichenfolge bereit. Dieser Wert muss in den Paginglinks enthalten sein, damit die Filtereinstellungen während des Pagingvorgangs beibehalten werden, und er muss im Textfeld wiederhergestellt werden, wenn die Seite erneut angezeigt wird. Wenn die Suchzeichenfolge während des Pagingvorgangs geändert wird, muss die Seite auf 1 zurückgesetzt werden, da der neue Filter andere Daten anzeigen kann. Die Such Zeichenfolge wird geändert, wenn ein Wert in das Textfeld eingegeben wird und die Schaltfläche "Senden" gedrückt wird. In diesem Fall ist der `searchString`-Parameter nicht NULL.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Am Ende der-Methode konvertiert die `ToPagedList`-Erweiterungsmethode für das Students `IQueryable`-Objekt die Student-Abfrage in eine einzelne Seite von Studenten in einem Sammlungstyp, der Paging unterstützt. Diese einzelne Seite der Schüler/Studenten wird dann an die Ansicht weitergeleitet:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Die `ToPagedList`-Methode nimmt eine Seitenanzahl. Die zwei Fragezeichen stellen den [null-Sammel Operator](https://msdn.microsoft.com/library/ms173224.aspx)dar. Der Nullzusammensetzungsoperator definiert einen Standardwert für einen Nullable-Typ. Der `(page ?? 1)`-Ausdruck bedeutet, dass `page` zurückgegeben wird, wenn dies über einen Wert verfügt, oder 1, wenn `page` gleich 0 (null) ist.

### <a name="add-paging-links-to-the-student-index-view"></a>Paging-Links zur Index Ansicht "Student" hinzufügen

Ersetzen Sie in *views\student\index.cshtml*den vorhandenen Code durch den folgenden Code:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

Die `@model`-Anweisung am oberen Rand der Seite gibt an, dass die Ansicht nun ein `PagedList`-Objekt anstelle eines `List`-Objekts aufruft.

Die `using`-Anweisung für `PagedList.Mvc` ermöglicht den Zugriff auf das MVC-Hilfsprogramm für die Paging-Schaltflächen.

Im Code wird eine Überladung von [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) verwendet, mit der [FormMethod. Get angegeben werden](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css)kann.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Der Standard [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) übermittelt Formulardaten mit einem Post. Dies bedeutet, dass Parameter im HTTP-Nachrichtentext und nicht in der URL als Abfrage Zeichenfolgen übergeben werden. Bei der Angabe von HTTP GET werden die Formulardaten als Abfragezeichenfolgen an die URL übergeben. Dadurch können Benutzer ein Lesezeichen für die URL erstellen. Die [W3C-Richtlinien für die Verwendung von HTTP Get](http://www.w3.org/2001/tag/doc/whenToUseGet.html) geben an, dass Sie Get verwenden sollten, wenn die Aktion nicht zu einem Update führt.

Das Textfeld wird mit der aktuellen Such Zeichenfolge initialisiert, sodass beim Klicken auf eine neue Seite die aktuelle Such Zeichenfolge angezeigt wird.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Die Spaltenüberschriftenlinks verwenden die Abfragezeichenfolge, um die aktuelle Suchzeichenfolge an den Controller zu übergeben, damit Benutzer Filterergebnisse sortieren können:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Die aktuelle Seite und die Gesamtzahl der Seiten werden angezeigt.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Wenn keine Seiten zum Anzeigen vorhanden sind, wird "Seite 0 von 0" angezeigt. (In diesem Fall ist die Seitenzahl größer als die Seitenanzahl, da `Model.PageNumber` 1 und `Model.PageCount` 0 ist.)

Die Paging-Schaltflächen werden von der `PagedListPager`-Hilfsprogramm angezeigt:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Das `PagedListPager`-Hilfsprogramm bietet eine Reihe von Optionen, die Sie anpassen können, einschließlich URLs und Formatierungen. Weitere Informationen finden Sie auf der GitHub-Website unter [troygoode/pgedlist](https://github.com/TroyGoode/PagedList) .

Führen Sie die Seite aus.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Klicken Sie auf die Paginglinks in verschiedenen Sortierreihenfolgen, um sicherzustellen, dass die Paging funktioniert. Geben Sie dann eine Suchzeichenfolge ein. Probieren Sie Paging erneut aus, um sicherzustellen, dass sie auch mit Sortier- und Filtervorgängen ordnungsgemäß funktioniert.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Erstellen einer Info-Seite mit Studenten Statistiken

Die Seite "Info" der Website der Website der Website von "Website" zeigt an, wie viele Studenten sich für jedes Registrierungsdatum registriert haben. Das erfordert Gruppieren und einfache Berechnungen dieser Gruppen. Um dies zu erreichen, ist Folgendes erforderlich:

- Erstellen Sie eine Ansichtsmodellklasse für die Daten, die Sie an die Ansicht übergeben müssen.
- Ändern Sie die `About`-Methode im `Home` Controller.
- Ändern Sie die Ansicht `About`.

### <a name="create-the-view-model"></a>Erstellen des Ansichts Modells

Erstellen Sie einen *ViewModels* -Ordner. Fügen Sie in diesem Ordner eine Klassendatei *EnrollmentDateGroup.cs* hinzu, und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Ändern des Home-Controllers

Fügen Sie in *HomeController.cs*die folgenden `using`-Anweisungen am Anfang der Datei hinzu:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Fügen Sie eine Klassen Variable für den Daten Bank Kontext unmittelbar nach der öffnenden geschweiften Klammer für die-Klasse hinzu:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Ersetzen Sie die `About`-Methode durch folgenden Code:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

Die LINQ-Anweisung gruppiert die Studentenentitäten nach Anmeldedatum, berechnet die Anzahl der Entitäten in jeder Gruppe und speichert die Ergebnisse in einer Sammlung von `EnrollmentDateGroup`-Ansichtsmodellobjekten.

Fügen Sie eine `Dispose`-Methode hinzu:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Ändern der Infoansicht

Ersetzen Sie den Code in der Datei " *views\home\about.cshtml* " durch den folgenden Code:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Führen Sie die APP aus, und klicken Sie **auf den Link** Info. Die Anzahl der Studenten für die jeweiligen Anmeldedatumswerte wird in einer Tabelle angezeigt.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Optional: Bereitstellen der app in Windows Azure

Bisher wurde Ihre Anwendung lokal in IIS Express auf dem Entwicklungs Computer ausgeführt. Um es anderen Benutzern die Verwendung über das Internet zur Verfügung zu stellen, müssen Sie Sie für einen Webhostinganbieter bereitstellen. In diesem optionalen Abschnitt des Tutorials stellen Sie die Anwendung auf einer Windows Azure-Website bereit.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Verwenden von Code First-Migrationen zum Bereitstellen der Datenbank

Zum Bereitstellen der Datenbank verwenden Sie Code First-Migrationen. Wenn Sie das Veröffentlichungs Profil erstellen, das Sie zum Konfigurieren der Einstellungen für die Bereitstellung von Visual Studio verwenden, wählen Sie ein Kontrollkästchen mit der Bezeichnung **Execute Code First-Migrationen (wird beim Anwendungsstart ausgeführt)** aus. Diese Einstellung bewirkt, dass der Bereitstellungs Prozess die *Web. config* -Datei der Anwendung auf dem Zielserver automatisch konfiguriert, sodass Code First die `MigrateDatabaseToLatestVersion` Initialisierer-Klasse verwendet.

Visual Studio führt während des Bereitstellungs Prozesses keine Aktion mit der Datenbank durch. Wenn die bereitgestellte Anwendung zum ersten Mal nach der Bereitstellung auf die Datenbank zugreift, erstellt Code First die Datenbank automatisch oder aktualisiert das Datenbankschema auf die neueste Version. Wenn die Anwendung eine Migrations `Seed`-Methode implementiert, wird die-Methode ausgeführt, nachdem die Datenbank erstellt oder das Schema aktualisiert wurde.

Die Migrations `Seed` Methode fügt Testdaten ein. Wenn Sie in einer Produktionsumgebung bereitgestellt haben, müssten Sie die `Seed`-Methode so ändern, dass nur Daten eingefügt werden, die in die Produktionsdatenbank eingefügt werden sollen. Beispielsweise können Sie in Ihrem aktuellen Datenmodell echte Kurse, aber fiktive Studenten in der Entwicklungs Datenbank haben. Sie können eine `Seed` Methode schreiben, um beide in der Entwicklung zu laden und dann die fiktiven Studenten vor der Bereitstellung in der Produktion auszukommentieren. Sie können auch eine `Seed` Methode schreiben, um nur Kurse zu laden, und die fiktiven Studenten in der Testdatenbank manuell über die Benutzeroberfläche der Anwendung eingeben.

### <a name="get-a-windows-azure-account"></a>Windows Azure-Konto erhalten

Sie benötigen ein Windows Azure-Konto. Wenn Sie noch nicht über eins verfügen, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [Kostenlose Windows Azure-Testversion](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Erstellen einer Website und einer SQL-Datenbank in Windows Azure

Ihre Windows Azure-Website wird in einer freigegebenen Hostingumgebung ausgeführt, was bedeutet, dass Sie auf virtuellen Computern (VMS) ausgeführt wird, die gemeinsam mit anderen Windows Azure-Clients verwendet werden. Eine freigegebene Hostingumgebung ist eine kostengünstige Möglichkeit für den Einstieg in die Cloud. Wenn der Webdatenverkehr später zunimmt, kann die Anwendung skaliert werden, um die Anforderungen zu erfüllen, indem Sie auf dedizierten VMS ausgeführt wird. Wenn Sie eine komplexere Architektur benötigen, können Sie zu einem Windows Azure-clouddienst migrieren. Clouddienste werden auf dedizierten VMS ausgeführt, die Sie entsprechend Ihren Anforderungen konfigurieren können.

Windows Azure SQL-Datenbank ist ein cloudbasierter relationaler Datenbankdienst, der auf SQL Server-Technologien aufbaut. Tools und Anwendungen, die mit SQL Server arbeiten, funktionieren auch mit der SQL-Datenbank.

1. Klicken Sie im [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/)auf der linken Registerkarte auf **Websites** , und klicken Sie dann auf **neu**.

    ![Schaltfläche "neu" in Verwaltungsportal](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Klicken Sie auf **Benutzer definiert erstellen**.

    ![Link "mit Datenbank erstellen" in Verwaltungsportal](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   Der Assistent zum **Erstellen einer neuen Website** wird geöffnet.
3. Geben Sie im Schritt **neue Website** des Assistenten eine Zeichenfolge in das Feld **URL** ein, um Sie als eindeutige URL für Ihre Anwendung zu verwenden. Die vollständige URL besteht aus den hier eingegebenen Informationen sowie dem Suffix, das neben dem Textfeld angezeigt wird. Die Abbildung zeigt "", aber diese URL wird wahrscheinlich übernommen, sodass Sie eine andere auswählen müssen.

    ![Link "mit Datenbank erstellen" in Verwaltungsportal](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. Wählen Sie in der Dropdown Liste **Region** eine Region in der Nähe aus. Mit dieser Einstellung wird festgelegt, in welchem Rechenzentrum die Website ausgeführt wird.
5. Wählen Sie in der Dropdown Liste **Datenbank** die Option **eine kostenlose 20-MB-SQL-Datenbank erstellen**aus.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. Geben Sie im **Namen der DB-Verbindungs Zeichenfolge** *schoolContext*ein.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Klicken Sie auf den Pfeil rechts unten im Feld. Der Assistent wechselt zum Schritt **Datenbankeinstellungen** .
8. Geben Sie im Feld **Name den Namen** *Conto souniversitydb*ein.
9. Wählen Sie im Feld **Server** die Option **neuer SQL-Datenbankserver**aus. Wenn Sie zuvor einen Server erstellt haben, können Sie alternativ diesen Server aus der Dropdown Liste auswählen.
10. Geben Sie einen Administrator **Anmelde Namen** und ein **Kennwort**ein. Wenn Sie **neuer SQL-Datenbankserver** ausgewählt haben, geben Sie hier keinen vorhandenen Namen und kein Kennwort ein. Sie geben einen neuen Namen und ein Kennwort ein, die Sie jetzt definieren, um Sie später beim Zugriff auf die Datenbank zu verwenden. Wenn Sie einen Server ausgewählt haben, den Sie zuvor erstellt haben, geben Sie die Anmelde Informationen für diesen Server ein. Für dieses Tutorial aktivieren Sie das Kontrollkästchen ***erweitert*** nicht. Mit den ***erweiterten*** Optionen können Sie die Daten Bank [Sortierung](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx)festlegen.
11. Wählen Sie die **Region** aus, die Sie für die Website ausgewählt haben.
12. Klicken Sie auf das Häkchen unten rechts im Feld, um anzugeben, dass Sie fertig sind.   
  
    ![Schritt der Datenbankeinstellungen neuer Website-Assistent zum Erstellen mit Datenbanken](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    In der folgenden Abbildung wird die Verwendung eines vorhandenen SQL Server und eines Anmelde namens gezeigt.   
  
    ![Schritt der Datenbankeinstellungen neuer Website-Assistent zum Erstellen mit Datenbanken](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    Der Verwaltungsportal wird zur Seite Websites zurückgegeben, und in der Spalte **Status** wird angezeigt, dass die Website erstellt wird. Nach einer Weile (in der Regel weniger als einer Minute) wird in der Spalte **Status** angezeigt, dass die Website erfolgreich erstellt wurde. In der Navigationsleiste auf der linken Seite wird die Anzahl der Websites, die Sie in Ihrem Konto haben, **neben dem Symbol "Websites"** angezeigt, und die Anzahl der Datenbanken wird neben dem Symbol " **SQL-Datenbanken** " angezeigt.

## <a name="deploy-the-application-to-windows-azure"></a>Bereitstellen der Anwendung in Windows Azure

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt in **Projektmappen-Explorer** , und wählen Sie im Kontextmenü **veröffentlichen** aus.  
  
    ![Im Projektkontext Menü veröffentlichen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. Klicken Sie im Assistenten **Web veröffentlichen** auf der Registerkarte **Profil** auf **importieren**.  
  
    ![Veröffentlichungseinstellungen importieren](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Wenn Sie Ihr Windows Azure-Abonnement nicht bereits in Visual Studio hinzugefügt haben, führen Sie die folgenden Schritte aus. In diesen Schritten fügen Sie Ihr Abonnement hinzu, sodass die Dropdown Liste, die auf **einer Windows Azure-Website importiert** wird, Ihre Website enthält.

    a. Klicken Sie im Dialogfeld **Veröffentlichungs Profil importieren** auf **aus einer Windows Azure-Website importieren**, und klicken Sie dann auf **Windows Azure-Abonnement hinzufügen**.

    ![Windows Azure-Abonnement hinzufügen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. Klicken Sie im Dialogfeld **Windows Azure-Abonnements importieren** auf **Abonnement Datei herunterladen**.

    ![Abonnement Datei herunterladen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. Speichern Sie die *publishsettings* -Datei in Ihrem Browserfenster.

    ![Datei ". publishsettings" herunterladen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Sicherheit: die *publishsettings* -Datei enthält Ihre (unverschlüsselten) Anmelde Informationen, die zum Verwalten Ihrer Windows Azure-Abonnements und-Dienste verwendet werden. Die bewährte Sicherheitsmaßnahme für diese Datei besteht darin, diese temporär außerhalb der Quellverzeichnisse (z. b. im Ordner *libraries\documents* ) zu speichern und Sie nach Abschluss des Imports zu löschen. Ein böswilliger Benutzer, der Zugriff auf die `.publishsettings`-Datei erlangt, kann Ihre Windows Azure-Dienste bearbeiten, erstellen und löschen.

    d. Klicken Sie im Dialogfeld **Windows Azure-Abonnements importieren** auf **Durchsuchen** , und navigieren Sie zur Datei *publishsettings* .

    ![Sub herunterladen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Klicken Sie auf **importieren**.

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. Wählen Sie im Dialogfeld **Veröffentlichungs Profil importieren** die Option **aus einer Windows Azure-Website importieren aus**, wählen Sie in der Dropdown Liste die Website aus, und klicken Sie dann auf **OK**.  
  
    ![Veröffentlichungs Profil importieren](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. Klicken Sie auf der Registerkarte **Verbindung** auf **Verbindung** überprüfen, um sicherzustellen, dass die Einstellungen richtig sind.  
  
    ![Verbindung überprüfen](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Wenn die Verbindung überprüft wurde, wird neben der Schaltfläche **Verbindung** überprüfen ein grünes Häkchen angezeigt. Klicken Sie auf **Weiter**.  
  
    ![Verbindung erfolgreich überprüft](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Öffnen Sie die Dropdown Liste **Remote Connection String** unter **schoolContext** , und wählen Sie die Verbindungs Zeichenfolge für die Datenbank aus, die Sie erstellt haben.
8. Wählen Sie **Code First-Migrationen ausführen (wird beim Anwendungsstart ausgeführt) aus**.
9. Deaktivieren Sie die Option **Diese Verbindungs Zeichenfolge zur Laufzeit** für den **userContext verwenden (DefaultConnection)** , da diese Anwendung nicht die Mitgliedschafts Datenbank verwendet.   
  
    ![Registerkarte „Einstellungen“](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Klicken Sie auf **Weiter**.
11. Klicken Sie auf der Registerkarte **Vorschau** auf **Vorschau starten**.  
  
    ![StartPreview-Schaltfläche auf der Registerkarte "Vorschau"](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    Auf der Registerkarte wird eine Liste der Dateien angezeigt, die auf den Server kopiert werden. Das Anzeigen der Vorschau ist nicht erforderlich, um die Anwendung zu veröffentlichen, aber Sie ist eine nützliche Funktion, die Sie kennen sollten. In diesem Fall müssen Sie nichts mit der Liste der angezeigten Dateien durchführen. Wenn Sie diese Anwendung das nächste Mal bereitstellen, werden nur die geänderten Dateien in dieser Liste angezeigt.  
  
    ![StartPreview-Dateiausgabe](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Klicken Sie auf **Veröffentlichen**.  
    Visual Studio beginnt mit dem Kopieren der Dateien auf den Windows Azure-Server.
13. Das Fenster **Ausgabe** zeigt, welche Bereitstellungs Aktionen ausgeführt wurden, und meldet einen erfolgreichen Abschluss der Bereitstellung.  
  
    ![Ausgabefenster für erfolgreiche Bereitstellung](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Nach erfolgreicher Bereitstellung wird der Standardbrowser automatisch mit der URL der bereitgestellten Website geöffnet.  
    Die Anwendung, die Sie erstellt haben, wird jetzt in der Cloud ausgeführt. Klicken Sie auf die Registerkarte Studenten.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

An diesem Punkt wurde die *School Context* -Datenbank in der Windows Azure SQL-Datenbank erstellt, weil Sie die Option **Code First-Migrationen ausführen ausgewählt haben (wird beim APP-Start ausgeführt)** . Die Datei " *Web. config* " auf der bereitgestellten Website wurde geändert, sodass der [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) -Initialisierer beim ersten Lesen oder Schreiben von Daten in der Datenbank ausgeführt wird (was geschah, als Sie die Registerkarte " **Students** " ausgewählt haben):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Beim Bereitstellungs Prozess wurde außerdem eine neue Verbindungs Zeichenfolge *(schoolContext\_databasepublish*) erstellt, um Code First-Migrationen zum Aktualisieren des Datenbankschemas und zum Seeding der Datenbank zu verwenden.

![Verbindungs Zeichenfolge Database_Publish](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

Die *DefaultConnection* -Verbindungs Zeichenfolge ist für die Mitgliedschafts Datenbank (die in diesem Tutorial nicht verwendet wird). Die Verbindungs Zeichenfolge für den *schoolContext* ist für die Datenbank condesouniversity vorgesehen.

Die bereitgestellte Version der Datei "Web. config" finden Sie auf Ihrem eigenen Computer unter *condesouniversity\obj\release\package\packagetmp\web.config*. Sie können auf die bereitgestellte *Web. config* -Datei selbst über FTP zugreifen. Anweisungen finden Sie unter [ASP.net Web Deployment using Visual Studio: Deployment a Code Update](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Befolgen Sie die Anweisungen, die mit "So verwenden Sie ein FTP-Tool" beginnen: die FTP-URL, der Benutzername und das Kennwort. "

> [!NOTE]
> Die Web-App implementiert keine Sicherheit, sodass jeder, der die URL findet, die Daten ändern kann. Anweisungen zum Sichern der Website finden Sie unter Bereitstellen [einer Secure ASP.NET MVC-App mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Windows Azure-](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)Website. Sie können verhindern, dass andere Personen die Website verwenden, indem Sie die Windows Azure-Verwaltungsportal oder **Server-Explorer** in Visual Studio verwenden, um die Website zu unterbinden.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Code First-Initialisierer

Im Abschnitt Bereitstellung sehen Sie, dass der [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) -Initialisierer verwendet wird. Code First bietet auch andere Initialisierer, die Sie verwenden können, einschließlich " [kreatedatabaseifnotexists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (Standard)", " [dropkreatedatabaseifmodelchanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) " und " [dropkreatedatabasealways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx)". Der `DropCreateAlways` Initialisierer kann beim Einrichten von Bedingungen für Komponententests hilfreich sein. Sie können auch eigene Initialisierer schreiben, und Sie können einen Initialisierer explizit aufzurufen, wenn Sie nicht warten möchten, bis die Anwendung aus der Datenbank liest oder in diese schreibt. Eine umfassende Erläuterung der Initialisierer finden Sie in Kapitel 6 der Buch [Programmierung Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) von Julie Lerman und Rowan Miller.

## <a name="summary"></a>Summary

In diesem Tutorial haben Sie erfahren, wie Sie ein Datenmodell erstellen und grundlegende CRUD-, Sortier-, Filter-, Paging-und Gruppierungsfunktionen implementieren. Im nächsten Tutorial sehen Sie sich die erweiterten Themen an, indem Sie das Datenmodell erweitern.

Links zu anderen Entity Framework Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Weiter](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
