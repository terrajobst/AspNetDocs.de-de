---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: 'Tutorial: Weitere Informationen zu erweiterten EF-Szenarios für eine MVC 5-Web-App'
description: In diesem Tutorial werden verschiedene Themen vorgestellt, die Sie beim Entwickeln der Grundlagen der Entwicklung von ASP.NET-Webanwendungen, die Entity Framework Code First verwenden, berücksichtigen sollten.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: d7cc83a5b78a60f575f5c3065079679189296a0c
ms.sourcegitcommit: fe5c7512383a9b0a05d321ff10d3cca1611556f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "58425274"
---
# <a name="tutorial-learn-about-advanced-ef-scenarios-for-an-mvc-5-web-app"></a>Tutorial: Weitere Informationen zu erweiterten EF-Szenarios für eine MVC 5-Web-App

Im vorherigen Tutorial haben Sie die "Tabelle pro Hierarchie"-Vererbung implementiert. In diesem Tutorial werden verschiedene Themen vorgestellt, die Sie beim Entwickeln der Grundlagen der Entwicklung von ASP.NET-Webanwendungen, die Entity Framework Code First verwenden, berücksichtigen sollten. Die ersten Abschnitte enthalten Schritt-für-Schritt-Anweisungen für den Code und die Verwendung von Visual Studio zum Ausführen von Aufgaben in den folgenden Abschnitten werden verschiedene Themen mit kurzen Einführungen, gefolgt von Links zu Ressourcen, um weitere Informationen zu erhalten.

Für die meisten dieser Themen arbeiten Sie mit Seiten, die Sie bereits erstellt haben. Wenn Sie für Massen Aktualisierungen Rohdaten verwenden möchten, erstellen Sie eine neue Seite, auf der die Anzahl der Gutschriften aller Kurse in der Datenbank aktualisiert wird:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

In diesem Tutorial:

> [!div class="checklist"]
> * Durchführen unformatierter SQL-Abfragen
> * Ausführen von Abfragen ohne Nachverfolgung
> * SQL-Abfragen, die an die Datenbank gesendet werden

Außerdem erfahren Sie mehr über:

> [!div class="checklist"]
> * Erstellen einer Abstraktions Ebene
> * Proxy Klassen
> * Automatische Änderungserkennung
> * Automatische Validierung
> * Entity Framework Power Tools
> * Entity Framework Quellcode

## <a name="prerequisite"></a>Vorbereitungsmaßnahme

* [Implementieren von Vererbung](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="perform-raw-sql-queries"></a>Durchführen unformatierter SQL-Abfragen

Die Entity Framework Code First-API umfasst Methoden, die es Ihnen ermöglichen, SQL-Befehle direkt an die Datenbank zu übergeben. Sie haben die folgenden Optionen:

- Verwenden Sie die [dbset. sqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) -Methode für Abfragen, die Entitäts Typen zurückgeben. Die zurückgegebenen Objekte müssen vom Typ sein, der vom `DbSet` -Objekt erwartet wird, und Sie werden automatisch vom Daten Bank Kontext nachverfolgt, es sei denn, Sie deaktivieren die Nachverfolgung. (Informationen zur [asnotracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) -Methode finden Sie im folgenden Abschnitt.)
- Verwenden Sie die [Database. sqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) -Methode für Abfragen, die Typen zurückgeben, die keine Entitäten sind. Die zurückgegebenen Daten werden nicht vom Datenbankkontext nachverfolgt, auch wenn Sie diese Methode zum Abrufen von Entitätstypen verwenden.
- Verwenden Sie " [Database. ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) " für nicht-Abfrage Befehle.

Einer der Vorteile von Entity Framework ist die Tatsache, dass vermieden wird, den Code zu eng an eine bestimmte Methode zum Speichern von Daten zu binden. Dies geschieht, indem SQL-Abfragen und -Befehle für Sie generiert werden. Somit müssen Sie sie nicht selbst schreiben. Es gibt jedoch außergewöhnliche Szenarios, in denen Sie bestimmte SQL-Abfragen ausführen müssen, die Sie manuell erstellt haben. diese Methoden ermöglichen es Ihnen, solche Ausnahmen zu behandeln.

Dies trifft immer zu, wenn Sie SQL-Befehle in einer Webanwendung ausführen. Treffen Sie deshalb Vorsichtsmaßnahmen, um Ihre Website vor Angriffen durch Einschleusung von SQL-Befehlen zu schützen. Verwenden Sie dazu parametrisierte Abfragen, um sicherzustellen, dass Zeichenfolgen, die von einer Webseite übermittelt werden, nicht als SQL-Befehle interpretiert werden können. In diesem Tutorial verwenden Sie parametrisierte Abfragen beim Integrieren von Benutzereingaben in eine Abfrage.

### <a name="calling-a-query-that-returns-entities"></a>Aufrufen einer Abfrage, die Entitäten zurückgibt

Die [dbset&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) -Klasse stellt eine Methode bereit, mit der Sie eine Abfrage ausführen können, die eine Entität vom Typ `TEntity`zurückgibt. Um zu sehen, wie dies funktioniert, ändern Sie den Code `Details` in der- `Department` Methode des Controllers.

Ersetzen Sie in *DepartmentController.cs*in `Details` der-Methode den `db.Departments.FindAsync` -Methoden Befehl durch `db.Departments.SqlQuery` einen-Methoden Befehl, wie im folgenden hervorgehobenen Code gezeigt:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Wählen Sie die Registerkarte **Departments** (Abteilungen) und dann **Details** für eine der Abteilungen aus. So können Sie überprüfen, ob der neue Code korrekt funktioniert. Stellen Sie sicher, dass alle Daten erwartungsgemäß angezeigt werden.

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Aufrufen einer Abfrage, die andere Objekttypen zurückgibt

Sie haben zuvor ein Statistikraster für Studenten für die Infoseite erstellt, das die Anzahl der Studenten für jedes Anmeldedatum zeigt. Der Code, der dies in *HomeController.cs* durchführt, verwendet LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Angenommen, Sie möchten den Code schreiben, der diese Daten direkt in SQL abruft, statt LINQ zu verwenden. Hierzu müssen Sie eine Abfrage ausführen, die etwas anderes als Entitäts Objekte zurückgibt. Dies bedeutet, dass Sie die [Database. sqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) -Methode verwenden müssen.

Ersetzen Sie in *HomeController.cs*die LINQ-Anweisung in `About` der-Methode durch eine SQL-Anweisung, wie im folgenden hervorgehobenen Code gezeigt:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Führen Sie die Seite Info aus. Vergewissern Sie sich, dass die gleichen Daten wie zuvor angezeigt werden.

### <a name="calling-an-update-query"></a>Aufrufen einer Update Abfrage

Nehmen wir an, dass die Administratoren von Administratoren in der Lage sein sollen, Massen Änderungen in der Datenbank auszuführen, z. b. die Anzahl der Gutschriften für jeden Kurs zu ändern. Wenn die Universität über eine große Anzahl an Kursen verfügt, wäre es ineffizient, sie alle als Entitäten abzurufen und separat zu ändern. In diesem Abschnitt implementieren Sie eine Webseite, die es dem Benutzer ermöglicht, einen Faktor anzugeben, nach dem die Anzahl der Gutschriften für alle Kurse geändert werden soll, und Sie nehmen die Änderung vor, `UPDATE` indem Sie eine SQL-Anweisung ausführen. 

FügenSie in CourseController.cs `UpdateCourseCredits` Methoden für `HttpGet` und `HttpPost`hinzu:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Wenn der Controller eine `HttpGet` Anforderung verarbeitet, wird in der `ViewBag.RowsAffected` Variablen nichts zurückgegeben, und in der Ansicht werden ein leeres Textfeld und eine Schaltfläche zum Senden angezeigt.

Wenn Sie auf die Schaltfläche **Aktualisieren** klicken `HttpPost` , wird die-Methode `multiplier` aufgerufen, und der Wert wird in das Textfeld eingegeben. Der Code führt dann den SQL-Code aus, der die Kurse aktualisiert, und gibt die Anzahl der betroffenen `ViewBag.RowsAffected` Zeilen an die Sicht in der Variablen zurück. Wenn die Ansicht einen Wert in dieser Variablen erhält, wird die Anzahl der aktualisierten Zeilen anstelle des Textfelds und der Schaltfläche "Senden" angezeigt.

Klicken Sie in *CourseController.cs*mit der `UpdateCourseCredits` rechten Maustaste auf eine der Methoden, und klicken Sie dann auf **Ansicht hinzufügen**. Das Dialog **Feld Ansicht hinzufügen** wird angezeigt. Überlassen Sie die Standardeinstellungen, und wählen Sie **Hinzufügen**

Ersetzen Sie in *views\course\updatecoursecrediz.cshtml*den Vorlagen Code durch den folgenden Code:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Führen Sie die `UpdateCourseCredits`-Methode aus, indem Sie die Registerkarte **Courses** (Kurse) auswählen, und dann „/UpdateCourseCredits“ am Ende der URL in die Adressleiste des Browsers einfügen (z.B.: `http://localhost:50205/Course/UpdateCourseCredits`). Geben Sie eine Zahl in das Textfeld ein:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Klicken Sie auf **Aktualisieren**. Die Anzahl der betroffenen Zeilen wird angezeigt.

Klicken Sie auf **Zurück zur Liste**, um die Kursliste mit der geänderte Anzahl von Credits anzuzeigen.

Weitere Informationen zu unformatierten SQL-Abfragen finden Sie unter unformatierte [SQL-Abfragen](https://msdn.microsoft.com/data/jj592907) auf MSDN.

## <a name="no-tracking-queries"></a>Abfragen ohne Nachverfolgung

Wenn ein Datenbankkontext Tabellenzeilen abruft und Entitätsobjekte erstellt, die diese darstellen, verfolgen sie standardgemäß, ob die Entitäten im Arbeitsspeicher mit dem Inhalt der Datenbank synchronisiert sind. Die Daten im Arbeitsspeicher fungieren als Cache und werden verwendet, wenn Sie eine Entität aktualisieren. Diese Zwischenspeicherung ist in Webanwendungen oft überflüssig, da Kontextinstanzen in der Regel kurzlebig sind (eine neue wird bei jeder Anforderung erstellt und gelöscht), und der Kontext, der eine Entität liest, wird in der Regel gelöscht, bevor diese Entität erneut verwendet wird.

Sie können die Nachverfolgung von Entitäts Objekten im Arbeitsspeicher mithilfe der [asnotracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) -Methode deaktivieren. Typische Szenarios, in denen Sie das möglicherweise tun wollen, umfassen Folgendes:

- Eine Abfrage ruft eine solche große Menge von Daten ab, die durch das Ausschalten der Nachverfolgung die Leistung deutlich verbessern können.
- Sie möchten eine Entität anfügen, um Sie zu aktualisieren, aber Sie haben zuvor dieselbe Entität für einen anderen Zweck abgerufen. Da diese Entität bereits vom Datenbankkontext nachverfolgt wird, können Sie die zu ändernde Entität nicht anfügen. Eine Möglichkeit, diese Situation zu behandeln, besteht darin `AsNoTracking` , die-Option mit der vorherigen Abfrage zu verwenden.

Ein Beispiel für die Verwendung der [asnotracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) -Methode finden Sie in [der früheren Version dieses Tutorials](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). In dieser Version des Tutorials wird das geänderte Flag für eine Modell Binder erstellte Entität nicht in der Edit-Methode festgelegt, daher ist `AsNoTracking`es nicht erforderlich.

## <a name="examine-sql-sent-to-database"></a>SQL-Daten Bank Übermittlung überprüfen

Manchmal ist es hilfreich, die tatsächlichen SQL-Abfragen anzuzeigen, die an die Datenbank gesendet werden. In einem früheren Tutorial haben Sie erfahren, wie Sie dies in Interceptor Code durchführen können. Nun sehen Sie einige Möglichkeiten, ohne Interceptor Code zu schreiben. Um dies auszuprobieren, sehen Sie sich eine einfache Abfrage an und sehen sich an, was passiert, wenn Sie Optionen wie Eager Loading, Filterung und Sortierung hinzufügen.

Ersetzen Sie in *Controllers/coursecontroller*die `Index` -Methode durch den folgenden Code, um Eager Loading vorübergehend anzuhalten:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Legen Sie nun einen Haltepunkt für `return` die-Anweisung fest (F9 mit dem Cursor in dieser Zeile). Drücken Sie **F5** , um das Projekt im Debugmodus auszuführen, und wählen Sie die Seite Index Index aus. Wenn der Code den Breakpoint erreicht, untersuchen `sql` Sie die Variable. Die Abfrage, die an SQL Server gesendet wird, wird angezeigt. Es handelt sich um `Select` eine einfache Anweisung.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Klicken Sie auf das Vergrößerungsglas, um die Abfrage in der **Text**Schnellansicht anzuzeigen.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Nun fügen Sie der Index Seite des Kurses eine Dropdown Liste hinzu, damit Benutzer nach einer bestimmten Abteilung filtern können. Sie sortieren die Kurse nach Titel und geben Eager Loading für die `Department` Navigations Eigenschaft an.

Ersetzen Sie in *CourseController.cs*die `Index` -Methode durch den folgenden Code:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Stellen Sie den Breakpoint in `return` der Anweisung wieder her.

Die-Methode empfängt den ausgewählten Wert der Dropdown Liste im `SelectedDepartment` -Parameter. Wenn nichts ausgewählt ist, wird dieser Parameter NULL sein.

Eine `SelectList` Sammlung, die alle Abteilungen enthält, wird an die Ansicht für die Dropdown Liste übermittelt. Die Parameter, die an `SelectList` den Konstruktor übergeben werden, geben den Wert Feldnamen, den Text Feldnamen und das ausgewählte Element an.

Bei der `Get` -Methode `Course` des Repository gibt der Code einen Filter Ausdruck, eine Sortierreihenfolge und Eager Loading für die `Department` Navigations Eigenschaft an. Der Filter Ausdruck gibt immer `true` zurück, `SelectedDepartment` wenn in der Dropdown Liste nichts ausgewählt ist (d. h. ist NULL).

Fügen Sie in " *views\course\index.cshtml*" direkt `table` vor dem öffnenden Tag den folgenden Code hinzu, um die Dropdown Liste und die Schaltfläche "Senden" zu erstellen:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Wenn der Breakpoint noch festgelegt ist, führen Sie die Index Seite Course aus. Fahren Sie mit dem ersten Mal fort, wenn der Code auf einen Haltepunkt trifft, sodass die Seite im Browser angezeigt wird. Wählen Sie in der Dropdown Liste eine Abteilung aus, und klicken Sie auf **Filtern**.

Dieses Mal wird der erste Breakpoint für die Abfrage der Abteilungen für die Dropdown Liste angezeigt. Überspringen Sie, und `query` zeigen Sie die Variable an, wenn der Code das nächste Mal den Breakpoint erreicht `Course` , um zu sehen, wie die Abfrage nun aussieht. Folgendes wird angezeigt:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Sie können sehen, dass es sich bei der `JOIN` Abfrage um eine `Department` Abfrage handelt, die `Course` Daten zusammen mit den Daten lädt und `WHERE` eine-Klausel enthält.

Entfernen Sie `var sql = courses.ToString()` die Zeile.

## <a name="create-an-abstraction-layer"></a>Erstellen einer Abstraktionsschicht

Viele Entwickler schreiben Code, um das Repository- und Arbeitseinheitsmuster als Wrapper um den Code zu implementieren, der mit dem Entity Framework arbeitet. Diese Muster sollen eine Abstraktionsebene zwischen der Datenzugriffsebene und den Geschäftslogikebene einer Anwendung erstellen. Die Implementierung dieser Muster unterstützt die Isolation Ihrer Anwendung vor Änderungen im Datenspeicher und kann automatisierte Komponententests oder eine testgesteuerte Entwicklung (Test-Driven Development, TDD) erleichtern. Das Schreiben von zusätzlichem Code zum Implementieren dieser Muster ist jedoch nicht immer die beste Wahl für Anwendungen, die EF verwenden, aus verschiedenen Gründen:

- Die EF-Kontextklasse isoliert Ihren Code selbst vor datenspeicherspezifischem Code.
- Die EF-Kontextklasse kann als Arbeitseinheitsklasse für Updates der Datenbank fungieren, die Sie mithilfe von EF ausführen.
- Die in Entity Framework 6 eingeführten Features vereinfachen die Implementierung von TDD, ohne den Repository-Code schreiben zu müssen.

Weitere Informationen zum Implementieren des Repository und der Arbeits Einheits Muster finden Sie [in der Entity Framework 5-Version dieser tutorialreihe](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Informationen zu Methoden zum Implementieren von TDD in Entity Framework 6 finden Sie in den folgenden Ressourcen:

- [Die einfache Aktivierung von dbsets durch EF6](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Testen mit einem Frameworks](https://msdn.microsoft.com/data/dn314429)
- [Testen mit ihren eigenen Test Doubles](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>

## <a name="proxy-classes"></a>Proxy Klassen

Wenn das Entity Framework Entitäts Instanzen erstellt (z. b. beim Ausführen einer Abfrage), werden diese häufig als Instanzen eines dynamisch generierten abgeleiteten Typs erstellt, der als Proxy für die Entität fungiert. Sehen Sie sich z. b. die folgenden zwei Debugger-Images an. Im ersten Bild sehen Sie, dass die `student` Variable der erwartete `Student` Typ ist, unmittelbar nachdem Sie die Entität instanziieren. Nachdem EF in der zweiten Abbildung verwendet wurde, um eine Entität "Student" aus der Datenbank zu lesen, sehen Sie die Proxy Klasse.

![Vor Proxy Klasse](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Nach Proxy Klasse](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Diese Proxy Klasse überschreibt einige virtuelle Eigenschaften der Entität, um beim Zugriff auf die Eigenschaft automatisch Hooks zum Ausführen von Aktionen einzufügen. Eine Funktion, für die dieser Mechanismus verwendet wird, ist Lazy Loading.

In den meisten Fällen müssen Sie sich diese Verwendung von Proxys nicht bewusst sein, aber es gibt Ausnahmen:

- In einigen Szenarien möchten Sie möglicherweise verhindern, dass die Entity Framework Proxy Instanzen erstellen. Wenn Sie z. b. Entitäten serialisieren, benötigen Sie in der Regel die poco-Klassen, nicht die Proxy Klassen. Eine Möglichkeit, um Serialisierungsprobleme zu vermeiden, ist das Serialisieren von DTOs (Data Transfer Objects) anstelle von Entitäts Objekten, wie im Tutorial [using Web API with Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) gezeigt. Eine andere Möglichkeit besteht darin, die [Proxy Erstellung zu deaktivieren](https://msdn.microsoft.com/data/jj592886.aspx).
- Wenn Sie eine Entitäts Klasse mit dem `new` -Operator instanziieren, erhalten Sie keine Proxy Instanz. Dies bedeutet, dass Sie keine Funktionen wie Lazy Loading und die automatische Änderungs Nachverfolgung erhalten. Dies ist in der Regel in Ordnung. im Allgemeinen benötigen Sie Lazy Loading nicht, da Sie eine neue Entität erstellen, die sich nicht in der Datenbank befindet, und Sie benötigen im Allgemeinen keine Änderungs Nachverfolgung, wenn `Added`Sie die Entität explizit als markieren. Wenn Sie jedoch Lazy Loading benötigen und die Änderungs Nachverfolgung benötigen, können Sie mithilfe der [Create](https://msdn.microsoft.com/library/gg679504.aspx) -Methode `DbSet` der-Klasse neue Entitäts Instanzen mit Proxys erstellen.
- Möglicherweise möchten Sie einen tatsächlichen Entitätstyp aus einem Proxytyp erhalten. Sie können die [getObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) -Methode der `ObjectContext` -Klasse verwenden, um den tatsächlichen Entitätstyp einer Proxytyp Instanz zu erhalten.

Weitere Informationen finden Sie unter [Arbeiten mit](https://msdn.microsoft.com/data/JJ592886.aspx) Proxys auf MSDN.

## <a name="automatic-change-detection"></a>Automatische Änderungserkennung

Entity Framework bestimmt wie eine Entität geändert wurde (und welche Updates an die Datenbank gesendet werden müssen), indem die aktuellen Werte einer Entität mit den ursprünglichen Werten verglichen werden. Die ursprünglichen Werte werden gespeichert, wenn die Entität abgefragt oder angefügt wird. Einige der Methoden, die automatisch eine Änderungserkennung durchführen, sind die folgenden:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Wenn Sie eine große Anzahl von Entitäten nachverfolgen und eine dieser Methoden mehrmals in einer Schleife aufzurufen, können Sie erhebliche Leistungsverbesserungen erzielen, indem Sie die automatische Änderungs Erkennung mithilfe der [autodetectchangesenabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) -Eigenschaft vorübergehend ausschalten. Weitere Informationen finden Sie unter [Automatisches Erkennen von Änderungen](https://msdn.microsoft.com/data/jj556205) auf MSDN.

## <a name="automatic-validation"></a>Automatische Validierung

Wenn Sie die `SaveChanges` -Methode aufzurufen, überprüft das Entity Framework standardmäßig die Daten in allen Eigenschaften aller geänderten Entitäten, bevor die Datenbank aktualisiert wird. Wenn Sie eine große Anzahl von Entitäten aktualisiert haben und die Daten bereits überprüft haben, ist diese Arbeit unnötig, und Sie können das Speichern der Änderungen weniger Zeit in Anspruch nehmen, indem Sie die Validierung vorübergehend deaktivieren. Dazu können Sie die [validateonsaveaktivierte](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) -Eigenschaft verwenden. Weitere Informationen finden Sie unter über [Prüfung](https://msdn.microsoft.com/data/gg193959) auf MSDN.

## <a name="entity-framework-power-tools"></a>Entity Framework Power Tools

[Entity Framework Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) ist ein Visual Studio-Add-in, das zum Erstellen der in diesen Tutorials gezeigten Datenmodell Diagramme verwendet wurde. Die Tools können auch andere Funktionen ausführen, wie z. b. das Generieren von Entitäts Klassen basierend auf den Tabellen in einer vorhandenen Datenbank, sodass Sie die Datenbank mit Code First verwenden können. Nachdem Sie die Tools installiert haben, werden einige zusätzliche Optionen in den Kontextmenüs angezeigt. Wenn Sie z. b. in **Projektmappen-Explorer**mit der rechten Maustaste auf die Kontext Klasse klicken, sehen Sie die Option und **Entity Framework** . Dadurch haben Sie die Möglichkeit, ein Diagramm zu generieren. Wenn Sie Code First verwenden, können Sie das Datenmodell im Diagramm nicht ändern, aber Sie können die Dinge verschieben, um es verständlicher zu machen.

![EF-Diagramm](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

## <a name="entity-framework-source-code"></a>Entity Framework Quellcode

Der Quellcode für Entity Framework 6 steht auf [GitHub](https://github.com/aspnet/EntityFramework6)zur Verfügung. Sie können Fehler melden, und Sie können Ihre eigenen Verbesserungen am EF-Quellcode einbringen.

Obwohl der Quellcode offen ist, wird Entity Framework als Microsoft-Produkt vollständig unterstützt. Das Microsoft Entity Framework-Team überprüft, welche Beiträge akzeptiert werden. Es testet alle Codeänderungen, um die Qualität jedes Release zu garantieren.

## <a name="acknowledgments"></a>Danksagungen

- Tom Dykstra hat die Originalversion dieses Tutorials geschrieben, das EF 5-Update mitverfasst und das EF 6-Update verfasst. Tom ist Senior Programming Writer im Inhalts Team von Microsoft-Webplattform und-Tools.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT)) hat den größten Teil des Tutorials für EF 5 und MVC 4 aktualisiert und das EF 6-Update gemeinsam verfasst. Rick ist Senior Programming Writer für Microsoft, der sich auf Azure und MVC konzentriert.
- [Rowan Miller](http://www.romiller.com) und andere Mitglieder des Entity Framework Teams unterstützten Code Reviews und halfen dabei, viele Probleme mit Migrationen zu debuggen, die während des Aktualisierens des Tutorials für EF 5 und EF 6 entstanden sind.

## <a name="troubleshoot-common-errors"></a>Problembehandlung bei häufigen Fehlern

### <a name="cannot-createshadow-copy"></a>Erstellen/Schatten Kopie nicht möglich.

Fehlermeldung:

> "&lt;Dateiname&gt;" kann nicht erstellt/kopiert werden, wenn diese Datei bereits vorhanden ist.

Lösung

Warten Sie einige Sekunden, und aktualisieren Sie die Seite.

### <a name="update-database-not-recognized"></a>Update-Datenbank wurde nicht erkannt.

Fehlermeldung (über den `Update-Database` Befehl in der PMC):

> Der Begriff "Update-Database" wird nicht als Name eines Cmdlets, einer Funktion, einer Skriptdatei oder eines ausführbaren Programms erkannt. Überprüfen Sie die Schreibweise des Namens, oder überprüfen Sie, ob der Pfad korrekt ist, und versuchen Sie es noch mal.

Lösung

Beenden Sie Visual Studio. Öffnen Sie das Projekt erneut, und versuchen Sie es

### <a name="validation-failed"></a>Überprüfung fehlgeschlagen

Fehlermeldung (über den `Update-Database` Befehl in der PMC):

> Fehler bei der Überprüfung für mindestens eine Entität. Weitere Informationen finden Sie unter der Eigenschaft "entityvalidationerrors".

Lösung

Eine Ursache für dieses Problem sind Validierungs Fehler, wenn `Seed` die-Methode ausgeführt wird. Tipps zum Debuggen der `Seed` -Methode finden Sie unter [Seeding und Debuggen Entity Framework (EF)-DSB](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) .

### <a name="http-50019-error"></a>HTTP 500,19-Fehler

Fehlermeldung:

> HTTP-Fehler 500,19-interner Server Fehler: auf die angeforderte Seite kann nicht zugegriffen werden, da die zugehörigen Konfigurationsdaten für die Seite ungültig sind.

Lösung

Eine Möglichkeit, diesen Fehler zu erhalten, besteht darin, mehrere Kopien der Lösung zu verwenden, von denen jeder dieselbe Portnummer verwendet. Sie können dieses Problem in der Regel beheben, indem Sie alle Instanzen von Visual Studio beenden und dann das Projekt neu starten, an dem Sie arbeiten. Wenn dies nicht funktioniert, versuchen Sie, die Portnummer zu ändern. Klicken Sie mit der rechten Maustaste auf die Projektdatei und dann auf Eigenschaften. Wählen Sie die Registerkarte **Web** aus, und ändern Sie dann die Portnummer im Textfeld **Projekt-URL** .

### <a name="error-locating-sql-server-instance"></a>Fehler beim Bestimmen der SQL Server-Instanz

Fehlermeldung:

> Ein netzwerkbezogener oder instanzspezifischer Fehler beim Herstellen einer Verbindung mit SQL Server. Der Server wurde nicht gefunden oder es konnte nicht auf ihn zugegriffen werden. Stellen Sie sicher, dass der Instanzname richtig und SQL Server so konfiguriert ist, das Remoteverbindungen zulässig sind. (Anbieter: SQL-Netzwerkschnittstellen, Fehler: 26: Fehler beim Suchen des angegebenen Servers/der angegebenen Instanz)

Lösung

Überprüfen Sie die Verbindungszeichenfolge. Wenn Sie die Datenbank manuell gelöscht haben, ändern Sie den Namen der Datenbank in der Konstruktions Zeichenfolge.

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

 Weitere Informationen zum Arbeiten mit Daten mithilfe der Entity Framework finden Sie auf der EF- [Dokumentationsseite auf MSDN](https://msdn.microsoft.com/data/ee712907) und [ASP.NET Data Access-Empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

Weitere Informationen zum Bereitstellen der Webanwendung nach der Erstellung finden Sie unter [ASP.net Web Deployment-Empfohlene Ressourcen](../../../../whitepapers/aspnet-web-deployment-content-map.md) in der MSDN Library.

Informationen zu anderen Themen im Zusammenhang mit MVC, z. b. Authentifizierung und Autorisierung, finden Sie in den [ASP.NET MVC-empfohlenen Ressourcen](../recommended-resources-for-mvc.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial:

> [!div class="checklist"]
> * Haben Sie unformatierte SQL-Abfragen durchgeführt
> * Ausgeführte Abfragen ohne Nachverfolgung
> * Untersuchte SQL-Abfragen, die an die Datenbank gesendet wurden

Außerdem haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer Abstraktions Ebene
> * Proxy Klassen
> * Automatische Änderungserkennung
> * Automatische Validierung
> * Entity Framework Power Tools
> * Entity Framework Quellcode

Dies schließt diese Reihe von Tutorials zur Verwendung des Entity Framework in einer ASP.NET MVC-Anwendung ab. Weitere Informationen zu EF-Database First finden Sie in der DB First-tutorialreihe.
> [!div class="nextstepaction"]
> [Entity Framework Database First](../database-first-development/setting-up-database.md)