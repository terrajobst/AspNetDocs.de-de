---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implementieren von Repository-und Arbeitseinheiten Mustern in einer ASP.NET MVC-Anwendung (9 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio erstellt werden...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 18de9b125ee5d10795b9ce1a366918dadf4fc4e3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78434379"
---
# <a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implementieren von Repository-und Arbeitseinheiten Mustern in einer ASP.NET MVC-Anwendung (9 von 10)

von [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio 2012 erstellt werden. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe von Anfang an starten oder [ein Starter-Projekt für dieses Kapitel herunterladen](building-the-ef5-mvc4-chapter-downloads.md) und hier beginnen.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem stoßen, das Sie nicht beheben können, [Laden Sie das abgeschlossene Kapitel herunter](building-the-ef5-mvc4-chapter-downloads.md) , und versuchen Sie, das Problem zu reproduzieren. Im Allgemeinen können Sie die Lösung für das Problem finden, indem Sie Ihren Code mit dem abgeschlossenen Code vergleichen. Informationen zu häufigen Fehlern und deren Lösung finden Sie unter [Fehler und](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors) Problem Umgehungen.

Im vorherigen Tutorial haben Sie Vererbung verwendet, um redundanten Code in den `Student`-und `Instructor` Entitäts Klassen zu verringern. In diesem Tutorial sehen Sie einige Möglichkeiten, das Repository und Arbeitseinheiten Muster für CRUD-Vorgänge zu verwenden. Wie im vorherigen Tutorial ändern Sie in diesem Tutorial die Funktionsweise Ihres Codes mit bereits erstellten Seiten, anstatt neue Seiten zu erstellen.

## <a name="the-repository-and-unit-of-work-patterns"></a>Das Repository und Arbeitseinheits Muster

Die Muster "Repository" und "Arbeitseinheit" dienen dazu, eine Abstraktions Ebene zwischen der Datenzugriffs Schicht und der Geschäftslogik Ebene einer Anwendung zu erstellen. Die Implementierung dieser Muster unterstützt die Isolation Ihrer Anwendung vor Änderungen im Datenspeicher und kann automatisierte Komponententests oder eine testgesteuerte Entwicklung (Test-Driven Development, TDD) erleichtern.

In diesem Tutorial implementieren Sie eine Repository-Klasse für jeden Entitätstyp. Für den `Student` Entitätstyp erstellen Sie eine Repository-Schnittstelle und eine Repository-Klasse. Wenn Sie das Repository in Ihrem Controller instanziieren, verwenden Sie die-Schnittstelle, damit der Controller einen Verweis auf jedes Objekt akzeptiert, das die Repository-Schnittstelle implementiert. Wenn der Controller unter einem Webserver ausgeführt wird, empfängt er ein Repository, das mit dem Entity Framework funktioniert. Wenn der Controller unter einer Komponenten Testklasse ausgeführt wird, empfängt er ein Repository, das mit Daten funktioniert, die auf eine Weise gespeichert werden, die Sie problemlos für Tests bearbeiten können, z. b. eine Auflistung im Arbeitsspeicher.

Später in diesem Tutorial verwenden Sie mehrere Depots und eine Arbeitseinheits-Klasse für die `Course`-und `Department` Entitäts Typen im `Course` Controller. Die Unit of Work-Klasse koordiniert die Arbeit mehrerer Depots, indem eine einzelne Daten Bank Kontext Klasse erstellt wird, die von Ihnen gemeinsam genutzt wird. Wenn Sie in der Lage sein möchten, Automatisierte Komponententests durchzuführen, erstellen und verwenden Sie Schnittstellen für diese Klassen auf die gleiche Weise wie für das `Student` Repository. Um das Tutorial einfach zu halten, erstellen und verwenden Sie diese Klassen ohne Schnittstellen.

Die folgende Abbildung zeigt eine Möglichkeit, die Beziehungen zwischen dem Controller und den Kontext Klassen zu konzipieren, verglichen mit der Verwendung des-Repository oder der Arbeitseinheits Muster.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

In dieser tutorialreihe werden keine Komponententests erstellt. Eine Einführung in TDD mit einer MVC-Anwendung, die das Repository-Muster verwendet, finden Sie unter Exemplarische Vorgehensweise [: Verwenden von TDD mit ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Weitere Informationen zum Repository-Muster finden Sie in den folgenden Ressourcen:

- [Das Repository-Muster](https://msdn.microsoft.com/library/ff649690.aspx) auf MSDN.
- [Verwenden von Repository und Arbeitseinheiten Mustern mit Entity Framework 4,0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) im Entity Framework Team Blog.
- [Agile-Entity Framework 4-Repository](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) -Reihe von Beiträgen im Blog von Julie Lerman.
- [Das Konto wird auf einen Blick auf eine HTML5/jQuery-Anwendung](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) im Blog von Dan Wahlin integriert.

> [!NOTE]
> Es gibt zahlreiche Möglichkeiten, das Repository und Arbeits Einheits Muster zu implementieren. Sie können Repository-Klassen mit oder ohne Arbeitseinheits Klasse verwenden. Sie können ein einzelnes Repository für alle Entitäts Typen oder einen für jeden Typ implementieren. Wenn Sie einen für jeden Typ implementieren, können Sie separate Klassen, eine generische Basisklasse und abgeleitete Klassen oder eine abstrakte Basisklasse und abgeleitete Klassen verwenden. Sie können Geschäftslogik in Ihr Repository einschließen oder auf Datenzugriffs Logik beschränken. Sie können auch eine Abstraktions Ebene in der Daten Bank Kontext Klasse erstellen, indem Sie anstelle von [dbset](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) -Typen für ihre Entitätenmengen [idbset](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) -Schnittstellen verwenden. Der Ansatz für die Implementierung einer in diesem Tutorial gezeigten Abstraktions Ebene ist eine Option, die Sie berücksichtigen sollten, nicht für alle Szenarien und Umgebungen.

## <a name="creating-the-student-repository-class"></a>Erstellen der Student Repository-Klasse

Erstellen Sie im Ordner *dal* eine Klassendatei mit dem Namen *IStudentRepository.cs* , und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Dieser Code deklariert einen typischen Satz von CRUD-Methoden, einschließlich zwei Read-Methoden – eine, die alle `Student` Entitäten zurückgibt, und eine, die eine einzelne `Student` Entität nach ID findet.

Erstellen Sie im Ordner *dal* eine Klassendatei mit dem Namen *StudentRepository.cs* file. Ersetzen Sie den vorhandenen Code durch den folgenden Code, der die `IStudentRepository`-Schnittstelle implementiert:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Der Daten Bank Kontext wird in einer Klassen Variablen definiert, und der Konstruktor erwartet, dass das aufrufende Objekt eine Instanz des Kontexts übergibt:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Sie könnten einen neuen Kontext im Repository instanziieren, aber wenn Sie mehrere Repository in einem Controller verwendet haben, würde jeder einen separaten Kontext haben. Später verwenden Sie mehrere Repository im `Course` Controller, und Sie sehen, wie eine Arbeitseinheits Klasse sicherstellen kann, dass alle Depots denselben Kontext verwenden.

Das Repository implementiert [iverwerfund](https://msdn.microsoft.com/library/system.idisposable.aspx) gibt den Daten Bank kontextfrei, wie zuvor im Controller gesehen, und die CRUD-Methoden führen Aufrufe an den Daten Bank Kontext auf die gleiche Weise aus wie zuvor.

## <a name="change-the-student-controller-to-use-the-repository"></a>Ändern Sie den Student Controller, um das Repository zu verwenden.

Ersetzen Sie in *StudentController.cs*den aktuell in der-Klasse aufgeführten Code durch den folgenden Code. Die Änderungen werden hervorgehoben.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Der Controller deklariert nun eine Klassen Variable für ein Objekt, das die `IStudentRepository`-Schnittstelle anstelle der Kontext Klasse implementiert:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Der standardmäßige (parameterlose) Konstruktor erstellt eine neue Kontext Instanz, und ein optionaler Konstruktor ermöglicht es dem Aufrufer, eine Kontext Instanz zu übergeben.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Wenn Sie die *Abhängigkeitsinjektion*oder di verwendet haben, benötigen Sie den Standardkonstruktor nicht, da die di-Software sicherstellt, dass immer das korrekte Repository-Objekt bereitgestellt wird.)

In den CRUD-Methoden wird das Repository nun anstelle des Kontexts aufgerufen:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

Die `Dispose`-Methode verwirft nun das Repository anstelle des Kontexts:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Führen Sie die Website aus, und klicken Sie auf die Registerkarte **Studenten**

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

Die Seite sieht ordnungsgemäß aus und funktioniert genauso wie vor dem Ändern des Codes, um das Repository zu verwenden, und die anderen Seiten des Studenten funktionieren ebenfalls identisch. Es gibt jedoch einen wichtigen Unterschied in der Art und Weise, in der die `Index`-Methode des Controllers filtert und anordnet. Die ursprüngliche Version dieser Methode enthielt den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

Die aktualisierte `Index`-Methode enthält den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Nur der hervorgehobene Code wurde geändert.

In der ursprünglichen Version des Codes ist `students` als `IQueryable`-Objekt typisiert. Die Abfrage wird erst dann an die Datenbank gesendet, wenn Sie mithilfe einer Methode wie `ToList`in eine Auflistung konvertiert wird. Dies geschieht erst, wenn die Index Sicht auf das Student-Modell zugreift. Die `Where`-Methode im obigen Code wird zu einer `WHERE`-Klausel in der SQL-Abfrage, die an die Datenbank gesendet wird. Dies bedeutet wiederum, dass nur die ausgewählten Entitäten von der Datenbank zurückgegeben werden. Aufgrund der Änderung von `context.Students` in `studentRepository.GetStudents()`ist die `students` Variable nach dieser Anweisung jedoch eine `IEnumerable` Auflistung, die alle Studenten in der Datenbank enthält. Das Endergebnis der Anwendung der `Where` Methode ist identisch, aber jetzt wird die Arbeit im Arbeitsspeicher auf dem Webserver und nicht in der Datenbank ausgeführt. Bei Abfragen, die große Datenmengen zurückgeben, kann dies ineffizient sein.

> [!TIP]
> 
> **Iquerable im Vergleich zu IEnumerable**
> 
> Nachdem Sie das Repository wie hier gezeigt implementiert haben, gibt die an SQL Server gesendete Abfrage alle Studenten Zeilen zurück, da **Sie die Such** Kriterien nicht enthält:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Diese Abfrage gibt alle Studenten Daten zurück, weil das Repository die Abfrage ausgeführt hat, ohne dass die Suchkriterien bekannt sind. Das Sortieren, Anwenden von Suchkriterien und Auswählen einer Teilmenge der Daten für das Paging (in diesem Fall nur 3 Zeilen) erfolgt später, wenn die `ToPagedList`-Methode für die `IEnumerable` Auflistung aufgerufen wird.
> 
> In der vorherigen Version des Codes (vor der Implementierung des Repository) wird die Abfrage erst dann an die Datenbank gesendet, wenn Sie die Suchkriterien angewendet haben, wenn `ToPagedList` für das `IQueryable` Objekt aufgerufen wird.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Wenn topagedlist für ein `IQueryable` Objekt aufgerufen wird, gibt die an SQL Server gesendete Abfrage die Such Zeichenfolge an, und als Ergebnis werden nur Zeilen zurückgegeben, die die Suchkriterien erfüllen, und im Arbeitsspeicher muss keine Filterung durchgeführt werden.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (Im folgenden Tutorial wird erläutert, wie Sie Abfragen untersuchen, die an SQL Server gesendet werden.)

Der folgende Abschnitt zeigt, wie Sie Repository-Methoden implementieren können, mit denen Sie angeben können, dass diese Arbeit von der Datenbank ausgeführt werden soll.

Sie haben jetzt eine Abstraktions Ebene zwischen dem Controller und dem Entity Framework Daten Bank Kontext erstellt. Wenn Sie automatisierte Komponententests mit dieser Anwendung ausführen, können Sie eine alternative Repository-Klasse in einem Komponenten Testprojekt erstellen, das `IStudentRepository`implementiert *.* Anstatt den Kontext zum Lesen und Schreiben von Daten aufzurufende, könnte diese mockrepository-Klasse in-Memory-Auflistungen manipulieren, um Controller Funktionen zu testen.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementieren Sie ein generisches Repository und eine Arbeitseinheits-Klasse.

Das Erstellen einer Repository-Klasse für jeden Entitätstyp kann zu viel redundantem Code führen, was zu Teil Updates führen könnte. Nehmen Sie beispielsweise an, dass Sie zwei verschiedene Entitäts Typen als Teil derselben Transaktion aktualisieren müssen. Wenn jeweils eine separate Daten Bank Kontext Instanz verwendet wird, kann ein Erfolg auftreten, während die andere fehlschlägt. Eine Möglichkeit, redundanten Code zu minimieren, ist die Verwendung eines generischen Repository, und eine Möglichkeit, um sicherzustellen, dass alle Depots denselben Daten Bank Kontext verwenden (und somit alle Updates koordiniert), ist die Verwendung einer Arbeitseinheit.

In diesem Abschnitt des Tutorials erstellen Sie eine `GenericRepository`-Klasse und eine `UnitOfWork`-Klasse und verwenden Sie im `Course` Controller, um sowohl auf die `Department`-als auch auf die `Course`-Entitätenmengen zuzugreifen. Wie bereits erläutert, erstellen Sie keine Schnittstellen für diese Klassen, um diesen Teil des Tutorials einfach zu halten. Wenn Sie Sie jedoch zum Vereinfachen von TDD verwenden, implementieren Sie Sie in der Regel mit Schnittstellen auf die gleiche Weise wie das `Student` Repository.

### <a name="create-a-generic-repository"></a>Erstellen eines generischen Repository

Erstellen Sie im Ordner *dal* *GenericRepository.cs* , und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Klassen Variablen werden für den Daten Bank Kontext und für die Entitätenmenge deklariert, für die das Repository instanziiert wird:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Der Konstruktor akzeptiert eine Daten Bank Kontext Instanz und initialisiert die entitätenmengenvariable:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

Die `Get`-Methode verwendet Lambda-Ausdrücke, damit der aufrufende Code eine Filterbedingung und eine Spalte angeben kann, nach der die Ergebnisse sortiert werden, und mit einem Zeichen folgen Parameter kann der Aufrufer eine durch Trennzeichen getrennte Liste von Navigations Eigenschaften für Eager Loading bereitstellen:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Der Code `Expression<Func<TEntity, bool>> filter` bedeutet, dass der Aufrufer einen Lambda-Ausdruck auf der Grundlage des `TEntity` Typs bereitstellt. dieser Ausdruck gibt einen booleschen Wert zurück. Wenn das Repository z. b. für den `Student` Entitätstyp instanziiert wird, kann der Code in der aufrufenden Methode `student => student.LastName == "Smith`&quot; für den `filter`-Parameter angeben.

Der Code `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` auch bedeutet, dass der Aufrufer einen Lambda Ausdruck bereitstellt. In diesem Fall ist die Eingabe für den Ausdruck jedoch ein `IQueryable` Objekt für den `TEntity`-Typ. Der Ausdruck gibt eine geordnete Version dieses `IQueryable`-Objekts zurück. Wenn das Repository z. b. für den `Student` Entitätstyp instanziiert wird, kann der Code in der aufrufenden Methode `q => q.OrderBy(s => s.LastName)` für den `orderBy`-Parameter angeben.

Der Code in der `Get`-Methode erstellt ein `IQueryable` Objekt und wendet dann den Filter Ausdruck an, falls vorhanden:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Als nächstes wendet Sie die Ausdrücke mit unverzüglichem laden nach dem Durchführen der durch Trennzeichen getrennten Liste an:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Schließlich wendet Sie den `orderBy` Ausdruck an, wenn ein solcher vorhanden ist, und gibt die Ergebnisse zurück. Andernfalls werden die Ergebnisse der ungeordneten Abfrage zurückgegeben:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Wenn Sie die `Get`-Methode aufzurufen, können Sie die von der-Methode zurückgegebenen `IEnumerable` Auflistung filtern und sortieren, anstatt Parameter für diese Funktionen bereitzustellen. Die Sortier-und Filteraufgaben werden dann im Arbeitsspeicher auf dem Webserver ausgeführt. Wenn Sie diese Parameter verwenden, stellen Sie sicher, dass die Arbeit von der Datenbank statt vom Webserver ausgeführt wird. Eine Alternative besteht darin, abgeleitete Klassen für bestimmte Entitäts Typen zu erstellen und spezialisierte `Get` Methoden hinzuzufügen, z. b. `GetStudentsInNameOrder` oder `GetStudentsByName`. In einer komplexen Anwendung kann dies jedoch zu einer großen Anzahl von abgeleiteten Klassen und spezialisierten Methoden führen, bei denen es möglicherweise mehr Arbeit gibt.

Der Code in den `GetByID`-, `Insert`-und `Update`-Methoden ähnelt dem, was Sie im nicht generischen Repository gesehen haben. (Sie geben keinen Eager Loading Parameter in der `GetByID` Signatur an, da Sie Eager Loading nicht mit der `Find`-Methode ausführen können.)

Für die `Delete`-Methode werden zwei über Ladungen bereitgestellt:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Mit einer dieser Informationen können Sie nur die ID der zu löschenden Entität übergeben und eine Entitäts Instanz. Wie Sie im Tutorial zur [Handhabung](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) von Parallelität gesehen haben, benötigen Sie für die Parallelitäts Behandlung eine `Delete` Methode, die eine Entitäts Instanz annimmt, die den ursprünglichen Wert einer nach Verfolgungs Eigenschaft enthält.

In diesem generischen Repository werden typische CRUD-Anforderungen behandelt. Wenn für einen bestimmten Entitätstyp besondere Anforderungen gelten (z. b. komplexere Filter oder Reihenfolge), können Sie eine abgeleitete Klasse erstellen, die über zusätzliche Methoden für diesen Typ verfügt.

## <a name="creating-the-unit-of-work-class"></a>Erstellen der Arbeitseinheits Klasse

Die Unit of Work-Klasse dient einem Zweck: um sicherzustellen, dass Sie bei Verwendung mehrerer Depots einen einzelnen Daten Bank Kontext gemeinsam verwenden. Wenn eine Arbeitseinheit vollständig ist, können Sie auf diese Weise die `SaveChanges`-Methode für diese Instanz des Kontexts aufzurufen und sicher sein, dass alle zugehörigen Änderungen koordiniert werden. Die Klasse benötigt lediglich eine `Save`-Methode und eine-Eigenschaft für jedes Repository. Jede Repository-Eigenschaft gibt eine Repository-Instanz zurück, die mit derselben Daten Bank Kontext Instanz wie die anderen Repository-Instanzen instanziiert wurde.

Erstellen Sie im Ordner *dal* eine Klassendatei mit dem Namen *UnitOfWork.cs* , und ersetzen Sie den Vorlagen Code durch den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Der Code erstellt Klassen Variablen für den Daten Bank Kontext und jedes Repository. Für die `context` Variable wird ein neuer Kontext instanziiert:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Jede Repository-Eigenschaft überprüft, ob das Repository bereits vorhanden ist. Wenn dies nicht der Fall ist, wird das Repository instanziiert, und die Kontext Instanz wird übergeben. Daher verwenden alle Depots die gleiche Kontext Instanz.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

Die `Save`-Methode ruft `SaveChanges` im Daten Bank Kontext auf.

Wie jede Klasse, die einen Daten Bank Kontext in einer Klassen Variablen instanziiert, implementiert die `UnitOfWork` Klasse `IDisposable` und gibt den kontextfrei.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Ändern des Kurs Controllers für die Verwendung der Klasse "Unito fwork" und der Repository

Ersetzen Sie den derzeit in *CourseController.cs* aufgeführten Code durch den folgenden Code:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Mit diesem Code wird eine Klassen Variable für die `UnitOfWork`-Klasse hinzugefügt. (Wenn Sie hier Schnittstellen verwenden, würden Sie die Variable hier nicht initialisieren. stattdessen würden Sie ein Muster von zwei Konstruktoren genauso wie für das `Student` Repository implementieren.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

Im Rest der Klasse werden alle Verweise auf den Daten Bank Kontext durch Verweise auf das entsprechende Repository ersetzt, wobei `UnitOfWork` Eigenschaften für den Zugriff auf das Repository verwendet werden. Die `Dispose`-Methode verwirft die `UnitOfWork` Instanz.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Führen Sie die Website aus, und klicken Sie auf die Registerkarte **Kurse**

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

Die Seite wird genauso aussehen und funktioniert wie vor Ihren Änderungen, und die anderen Kursseiten funktionieren ebenfalls identisch.

## <a name="summary"></a>Zusammenfassung

Sie haben nun sowohl das Repository als auch das Arbeits Einheits Muster implementiert. Sie haben Lambda-Ausdrücke als Methoden Parameter im generischen Repository verwendet. Weitere Informationen zum Verwenden dieser Ausdrücke mit einem `IQueryable`-Objekt finden Sie unter [iquervable (t) Interface (t) Interface (System. Linq)](https://msdn.microsoft.com/library/bb351562.aspx) in der MSDN Library. Im nächsten Tutorial erfahren Sie, wie einige erweiterte Szenarien behandelt werden.

Links zu anderen Entity Framework Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
