---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Maximieren der Leistung mit dem Entity Framework 4,0 in einer ASP.NET 4-Webanwendung | Microsoft-Dokumentation
author: tdykstra
description: Diese tutorialreihe basiert auf der Webanwendung der Website von "Web", die in der tutorialreihe für die ersten Schritte mit Entity Framework 4,0 erstellt wurde. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 5630200a1ad1d30f6d89b38e15179f15b699fa9f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78439263"
---
# <a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Maximieren der Leistung mit dem Entity Framework 4,0 in einer ASP.NET 4-Webanwendung

von [Tom Dykstra](https://github.com/tdykstra)

> Diese tutorialreihe basiert auf der Webanwendung der Website von "Web", die in der tutorialreihe für die ersten Schritte [mit Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) erstellt wurde. Wenn Sie die vorherigen Tutorials nicht durchgearbeitet haben, können Sie die Anwendung, die Sie erstellt haben, als Ausgangspunkt für dieses Tutorial [herunterladen](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) . Sie können auch [die Anwendung herunterladen](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , die von der kompletten tutorialreihe erstellt wird. Wenn Sie Fragen zu den Tutorials haben, können Sie Sie im ASP.net- [Entity Framework Forum](https://forums.asp.net/1227.aspx)Posten.

Im vorherigen Tutorial haben Sie erfahren, wie Parallelitäts Konflikte behandelt werden. Dieses Tutorial zeigt Optionen zur Verbesserung der Leistung einer ASP.NET-Webanwendung, die die Entity Framework verwendet. Sie lernen verschiedene Methoden zum Maximieren der Leistung oder zur Diagnose von Leistungsproblemen kennen.

Die in den folgenden Abschnitten dargestellten Informationen sind wahrscheinlich in einer Vielzahl von Szenarios nützlich:

- Effizientes Laden verwandter Daten.
- Verwalten des Ansichts Zustands.

Die in den folgenden Abschnitten dargestellten Informationen können nützlich sein, wenn Sie über einzelne Abfragen verfügen, die Leistungsprobleme darstellen:

- Verwenden Sie die Option `NoTracking` Merge.
- LINQ-Abfragen vor der Kompilierung.
- Überprüfen Sie die an die Datenbank gesendeten Abfrage Befehle.

Die im folgenden Abschnitt dargestellten Informationen sind möglicherweise nützlich für Anwendungen, die über extrem große Datenmodelle verfügen:

- Vorab Generieren von Sichten.

> [!NOTE]
> Die Leistung der Webanwendung ist von vielen Faktoren betroffen, z. b. der Größe von Anforderungs-und Antwortdaten, der Geschwindigkeit von Datenbankabfragen, der Anzahl von Anforderungen, die der Server in die Warteschlange aufnehmen kann, und der Art und Weise, wie schnell der Dienst Sie bedienen kann Client-Skript Bibliotheken, die Sie möglicherweise verwenden. Wenn die Leistung in Ihrer Anwendung kritisch ist, oder wenn Tests oder Benutzeroberflächen anzeigen, dass die Anwendungsleistung nicht zufriedenstellend ist, sollten Sie das normale Protokoll zur Leistungsoptimierung befolgen. Legen Sie fest, wo Leistungsengpässe auftreten, und beheben Sie dann die Bereiche, die die größten Auswirkungen auf die Gesamtleistung der Anwendung haben.
> 
> Dieses Thema konzentriert sich hauptsächlich auf Möglichkeiten, mit denen Sie die Leistung der Entity Framework in ASP.net möglicherweise verbessern können. Die hier aufgeführten Vorschläge sind hilfreich, wenn Sie feststellen, dass der Datenzugriff einer der Leistungsengpässe in der Anwendung ist. Sofern nicht erwähnt, sollten die hier erläuterten Methoden nicht &quot;bewährten Methoden&quot; im Allgemeinen nicht berücksichtigt werden – viele von Ihnen sind nur in Ausnahmesituationen oder sehr spezifischen Arten von Leistungs Engpässen geeignet.

Um das Lernprogramm zu starten, starten Sie Visual Studio, und öffnen Sie die Webanwendung der Website "Web-so University", die Sie im vorherigen Tutorial gearbeitet haben.

## <a name="efficiently-loading-related-data"></a>Effizientes Laden verwandter Daten

Es gibt mehrere Möglichkeiten, wie die Entity Framework verknüpfte Daten in die Navigations Eigenschaften einer Entität laden kann:

- *Lazy Loading (verzögertes Laden)* . Wenn die Entität zuerst gelesen wird, werden verwandte Daten nicht abgerufen. Wenn Sie jedoch zum ersten Mal versuchen, auf eine Navigationseigenschaft zuzugreifen, werden die für diese Navigationseigenschaft erforderlichen Daten automatisch abgerufen. Dies führt dazu, dass mehrere Abfragen an die Datenbank gesendet werden – eine für die Entität selbst und eine, wenn verknüpfte Daten für die Entität abgerufen werden müssen. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Eager Loading (vorzeitiges Laden)* . Wenn die Entität gelesen wird, werden ihre verwandten Daten mit ihr abgerufen. Dies führt normalerweise zu einer einzelnen Joinabfrage, die alle Daten abruft, die erforderlich sind. Sie können Eager Loading mit der `Include`-Methode angeben, wie Sie bereits in diesen Tutorials gesehen haben.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Explizites Laden*. Dies ähnelt Lazy Loading, mit dem Unterschied, dass Sie die zugehörigen Daten explizit im Code abrufen. Dies geschieht nicht automatisch, wenn Sie auf eine Navigations Eigenschaft zugreifen. Mit der `Load`-Methode der Navigations Eigenschaft für Auflistungen können Sie verknüpfte Daten manuell laden, oder Sie verwenden die `Load`-Methode der Reference-Eigenschaft für Eigenschaften, die ein einzelnes Objekt enthalten. (Beispielsweise wird die `PersonReference.Load`-Methode aufgerufen, um die `Person` Navigations Eigenschaft einer `Department` Entität zu laden.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Da Sie die Eigenschaftswerte nicht sofort abrufen, werden Lazy Loading und explizites laden auch als *Verzögertes Laden*bezeichnet.

Lazy Load ist das Standardverhalten für einen Objekt Kontext, der vom Designer generiert wurde. Wenn Sie die *SchoolModel.Designer.cs* -Datei öffnen, die die Objekt Kontext Klasse definiert, finden Sie drei Konstruktormethoden, die jeweils die folgende Anweisung enthalten:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Wenn Sie wissen, dass Sie verwandte Daten für jede abgerufene Entität benötigen, bietet Eager Loading im Allgemeinen die beste Leistung, da eine einzelne Abfrage, die an die Datenbank gesendet wird, in der Regel effizienter als separate Abfragen für jede abgerufene Entität ist. Wenn Sie andererseits nur selten oder nur für eine kleine Gruppe von Entitäten auf die Navigations Eigenschaften einer Entität zugreifen müssen, ist Lazy Loading oder explizites laden möglicherweise effizienter, da Eager Loading mehr Daten abrufen, als Sie benötigen.

In einer Webanwendung ist Lazy Loading möglicherweise relativ wenig Wert, da Benutzeraktionen, die sich auf die Notwendigkeit von verknüpften Daten auswirken, im Browser stattfinden, der keine Verbindung mit dem Objekt Kontext hat, der die Seite gerendert hat. Wenn Sie jedoch ein Steuerelement DataBind, wissen Sie in der Regel, welche Daten Sie benötigen, und daher ist es am besten, Eager Loading oder verzögertes Laden basierend auf den in den einzelnen Szenarios geeigneten Anforderungen auszuwählen.

Außerdem kann ein Daten gebundene Steuerelement ein Entitäts Objekt verwenden, nachdem der Objekt Kontext verworfen wurde. In diesem Fall würde der Versuch, eine Navigations Eigenschaft verzögert zu laden, fehlschlagen. Die Fehlermeldung, die Sie erhalten, ist eindeutig: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

Das `EntityDataSource`-Steuerelement deaktiviert Lazy Loading standardmäßig. Für das `ObjectDataSource` Steuerelement, das Sie für das aktuelle Tutorial verwenden (oder wenn Sie über den Seitencode auf den Objekt Kontext zugreifen), gibt es mehrere Möglichkeiten, Lazy Loading standardmäßig deaktiviert zu machen. Sie können Sie deaktivieren, wenn Sie einen Objekt Kontext instanziieren. Beispielsweise können Sie der Konstruktormethode der `SchoolRepository`-Klasse die folgende Zeile hinzufügen:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Für die Anwendung "" der Anwendung "" der Anwendung "" wird der Objekt Kontext automatisch deaktiviert Lazy Loading, sodass diese Eigenschaft nicht festgelegt werden muss, wenn ein Kontext instanziiert wird.

Öffnen Sie das Datenmodell *School Model. edmx* , klicken Sie auf die Entwurfs Oberfläche, und legen Sie dann im Bereich Eigenschaften die Eigenschaft **Lazy Load aktiviert** auf `False`fest. Speichern und schließen Sie das Datenmodell.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Verwalten des Ansichts Zustands

Um die Aktualisierungs Funktionalität bereitzustellen, muss eine ASP.NET-Webseite die ursprünglichen Eigenschaftswerte einer Entität speichern, wenn eine Seite gerendert wird. Während des Postbacks kann das Steuerelement den ursprünglichen Zustand der Entität neu erstellen und die `Attach` Methode der Entität aufrufen, bevor Sie Änderungen anwenden und die `SaveChanges`-Methode aufrufen. Standardmäßig verwenden ASP.net-Web Forms Daten Steuerelemente den Ansichts Zustand, um die ursprünglichen Werte zu speichern. Der Ansichts Zustand kann sich jedoch auf die Leistung auswirken, da er in ausgeblendeten Feldern gespeichert ist, die die Größe der Seite, die an den Browser gesendet wird, erheblich vergrößern kann.

Techniken zum Verwalten des Ansichts Zustands oder Alternativen, wie z. b. der Sitzungs Status, sind nicht für die Entity Framework eindeutig, sodass dieses Tutorial nicht ausführlich behandelt wird. Weitere Informationen finden Sie unter den Links am Ende des Tutorials.

Version 4 von ASP.net bietet jedoch eine neue Möglichkeit, mit dem Ansichts Zustand zu arbeiten, den jeder ASP.NET-Entwickler von Web Forms Anwendungen beachten sollte: die `ViewStateMode`-Eigenschaft. Diese neue Eigenschaft kann auf der Seiten-oder Steuerelement Ebene festgelegt werden. Sie ermöglicht es Ihnen, den Ansichts Zustand für eine Seite standardmäßig zu deaktivieren und nur für Steuerelemente zu aktivieren, die Sie benötigen.

Für Anwendungen, bei denen die Leistung kritisch ist, empfiehlt es sich, den Ansichts Zustand auf Seitenebene immer zu deaktivieren und nur für Steuerelemente zu aktivieren, die dies erfordern. Die Größe des Ansichts Zustands auf den Seiten der "Seite" der Seite "Seite" wird von dieser Methode nicht wesentlich gesenkt, aber um die Funktionsweise zu überprüfen, machen Sie dies für die Seite " *Dozenten. aspx* ". Diese Seite enthält viele Steuerelemente, einschließlich eines `Label` Steuer Elements, für das der Ansichts Zustand deaktiviert ist. Für keines der Steuerelemente auf dieser Seite muss der Ansichts Zustand aktiviert sein. (Die `DataKeyNames`-Eigenschaft des `GridView` Steuer Elements gibt den Zustand an, der zwischen Postbacks beibehalten werden muss. diese Werte werden jedoch im Steuerelement Zustand beibehalten, was von der `ViewStateMode`-Eigenschaft nicht beeinträchtigt wird.)

Die `Page`-Direktive und `Label`-Steuerelement Markup ähneln derzeit dem folgenden Beispiel:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Nehmen Sie die folgenden Änderungen vor:

- Fügen Sie `ViewStateMode="Disabled"` der `Page` Direktive hinzu.
- Entfernen Sie `ViewStateMode="Disabled"` aus dem `Label`-Steuerelement.

Das Markup ähnelt nun dem folgenden Beispiel:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Der Ansichts Zustand ist jetzt für alle Steuerelemente deaktiviert. Wenn Sie später ein Steuerelement hinzufügen, das den Ansichts Zustand verwenden muss, müssen Sie lediglich das `ViewStateMode="Enabled"`-Attribut für dieses Steuerelement einschließen.

## <a name="using-the-notracking-merge-option"></a>Verwenden der NoTracking-MergeOption

Wenn ein Objekt Kontext Daten Bank Zeilen abruft und Entitäts Objekte erstellt, die diese darstellen, werden diese Entitäts Objekte standardmäßig auch mithilfe des Objekt Zustands-Managers nachverfolgt. Diese Überwachungsdaten fungieren als Cache und werden beim Aktualisieren einer Entität verwendet. Da eine Webanwendung in der Regel über kurzlebige Objekt Kontext Instanzen verfügt, geben Abfragen häufig Daten zurück, die nicht nachverfolgt werden müssen, da der Objekt Kontext, der Sie liest, verworfen wird, bevor eine der gelesenen Entitäten wieder verwendet oder aktualisiert wird.

Im Entity Framework können Sie angeben, ob der Objekt Kontext Entitäts Objekte nachverfolgt, indem Sie eine *MergeOption*festlegen. Sie können die MergeOption für einzelne Abfragen oder für Entitätenmengen festlegen. Wenn Sie diese Einstellung für eine Entitätenmenge festlegen, bedeutet dies, dass Sie die standardmäßige MergeOption für alle Abfragen festlegen, die für diese Entitätenmenge erstellt werden.

Für die Anwendung der Anwendung "die Anwendung" wird die Überwachung für keine Entitätenmengen benötigt, auf die Sie aus dem Repository zugreifen, sodass Sie die Merge-Option auf `NoTracking` für diese Entitätenmengen festlegen können, wenn Sie den Objekt Kontext in der Repository-Klasse instanziieren. (Beachten Sie, dass das Festlegen der Merge-Option in diesem Tutorial keine merkliche Auswirkung auf die Leistung der Anwendung hat. Die Option `NoTracking` wird wahrscheinlich nur in bestimmten Szenarios mit hohem Datenvolumen zu einer wahrnehmbaren Leistungsverbesserung führen.)

Öffnen Sie im Ordner dal die Datei *SchoolRepository.cs* , und fügen Sie eine Konstruktormethode hinzu, die die MergeOption für die Entitätenmengen festlegt, auf die das Repository zugreift:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Vorkompilieren von LINQ-Abfragen

Wenn das Entity Framework zum ersten Mal eine Entity SQL Abfrage innerhalb der Lebensdauer einer bestimmten `ObjectContext` Instanz ausführt, dauert die Kompilierung der Abfrage einige Zeit. Das Ergebnis der Kompilierung wird zwischengespeichert, was bedeutet, dass nachfolgende Ausführungen der Abfrage viel schneller sind. LINQ-Abfragen folgen einem ähnlichen Muster, mit dem Unterschied, dass ein Teil der Arbeit, die zum Kompilieren der Abfrage erforderlich ist, jedes Mal erfolgt, wenn die Abfrage ausgeführt wird. Anders ausgedrückt: für LINQ-Abfragen werden standardmäßig nicht alle Ergebnisse der Kompilierung zwischengespeichert.

Wenn Sie eine LINQ-Abfrage haben, die im Lebenszyklus eines Objekt Kontexts wiederholt ausgeführt werden soll, können Sie Code schreiben, der bewirkt, dass alle Ergebnisse der Kompilierung beim ersten Ausführen der LINQ-Abfrage zwischengespeichert werden.

Als Abbildung wird dies für zwei `Get` Methoden in der `SchoolRepository`-Klasse ausgeführt, von denen eine keine Parameter übernimmt (die `GetInstructorNames`-Methode), und eine, die einen-Parameter benötigt (die `GetDepartmentsByAdministrator`-Methode). Diese Methoden sind jetzt nicht mehr kompiliert, da Sie keine LINQ-Abfragen sind:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Wenn Sie jedoch kompilierte Abfragen ausprobieren möchten, können Sie so vorgehen, als wären diese als die folgenden LINQ-Abfragen geschrieben worden:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Sie können den Code in diesen Methoden in den obigen Code ändern und die Anwendung ausführen, um zu überprüfen, ob Sie funktioniert, bevor Sie fortfahren. Die folgenden Anweisungen werden jedoch direkt in das Erstellen vorkompilierter Versionen einfließen.

Erstellen Sie eine Klassendatei im Ordner *dal* , benennen Sie Sie *SchoolEntities.cs*, und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Dieser Code erstellt eine partielle Klasse, die die automatisch generierte Objekt Kontext Klasse erweitert. Die partielle Klasse enthält zwei kompilierte LINQ-Abfragen mit der `Compile`-Methode der `CompiledQuery`-Klasse. Außerdem werden Methoden erstellt, die Sie verwenden können, um die Abfragen aufzurufen. Speichern und schließen Sie diese Datei.

Ändern Sie anschließend in *SchoolRepository.cs*die vorhandenen `GetInstructorNames`-und `GetDepartmentsByAdministrator` Methoden in der Repository-Klasse, sodass Sie die kompilierten Abfragen aufzurufen:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Führen Sie die Seite *Departments. aspx* aus, um zu überprüfen, ob Sie wie zuvor funktioniert hat. Die `GetInstructorNames`-Methode wird aufgerufen, um die Dropdown Liste "Administrator" aufzufüllen, und die `GetDepartmentsByAdministrator`-Methode wird aufgerufen, wenn Sie auf " **Aktualisieren** " klicken, um zu überprüfen, ob kein Dozenten Administrator mehrerer Abteilungen ist.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Sie haben Abfragen in der Anwendung "" der Anwendung "" der Anwendung "" von "" in der Anwendung "" der Anwendung "" der Anwendung "" bereits kompiliert, nicht, weil dadurch die Leistung Durch das Vorkompilieren von LINQ-Abfragen wird dem Code ein Komplexitäts Grad hinzugefügt. Stellen Sie daher sicher, dass Sie ihn nur für Abfragen ausführen, die tatsächlich Leistungsengpässe in der Anwendung darstellen.

## <a name="examining-queries-sent-to-the-database"></a>Untersuchen der an die Datenbank gesendeten Abfragen

Wenn Sie Leistungsprobleme untersuchen, ist es manchmal hilfreich, die exakten SQL-Befehle zu kennen, die vom Entity Framework an die Datenbank gesendet werden. Wenn Sie mit einem `IQueryable` Objekt arbeiten, besteht eine Möglichkeit darin, die `ToTraceString`-Methode zu verwenden.

Ändern Sie in *SchoolRepository.cs*den Code in der `GetDepartmentsByName`-Methode so, dass er dem folgenden Beispiel entspricht:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

Die `departments` Variable muss nur in einen `ObjectQuery`-Typ umgewandelt werden, da die `Where`-Methode am Ende der vorangehenden Zeile ein `IQueryable` Objekt erstellt. ohne die `Where`-Methode ist die Umwandlung nicht erforderlich.

Legen Sie einen Haltepunkt in der `return` Zeile fest, und führen Sie dann die Seite *Departments. aspx* im Debugger aus. Wenn Sie den Breakpoint erreichen, überprüfen Sie die `commandText` Variable **im Fenster "** lokal", und verwenden Sie die Text Schnellansicht (die Lupe in der Spalte **Wert** ), um den Wert im Fenster **Text** Schnellansicht anzuzeigen. Der gesamte SQL-Befehl, der sich aus diesem Code ergibt, wird angezeigt:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Als Alternative bietet das IntelliTrace-Feature in Visual Studio Ultimate eine Möglichkeit zum Anzeigen von SQL-Befehlen, die von der Entity Framework generiert werden, ohne dass Sie Ihren Code ändern oder sogar einen Haltepunkt festlegen müssen.

> [!NOTE]
> Sie können die folgenden Prozeduren nur ausführen, wenn Sie Visual Studio Ultimate haben.

Stellen Sie den ursprünglichen Code in der `GetDepartmentsByName`-Methode wieder her, und führen Sie dann die Seite *Departments. aspx* im Debugger aus.

Wählen Sie in Visual Studio das Menü **Debuggen** , dann **IntelliTrace**und dann **IntelliTrace-Ereignisse**aus.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Klicken Sie im **IntelliTrace** -Fenster auf **Alle unterbrechen**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Das **IntelliTrace** -Fenster zeigt eine Liste aktueller Ereignisse an:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Klicken Sie auf die Zeile **ADO.net** . Es wird erweitert, um Ihnen den Befehls Text anzuzeigen:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Sie können die gesamte Befehls Text Zeichenfolge aus dem **Lokal Fenster in** die Zwischenablage kopieren.

Angenommen, Sie arbeiten mit einer Datenbank, die mehr Tabellen, Beziehungen und Spalten als die einfache `School` Datenbank hat. Möglicherweise werden Sie feststellen, dass eine Abfrage, die alle Informationen sammelt, die Sie in einer einzigen `Select` Anweisung mit mehreren `Join` Klauseln benötigen, zu komplex wird, um effizient zu arbeiten. In diesem Fall können Sie von Eager Loading zum expliziten Laden wechseln, um die Abfrage zu vereinfachen.

Versuchen Sie z. b., den Code in der `GetDepartmentsByName`-Methode in *SchoolRepository.cs*zu ändern. Derzeit verfügen Sie in dieser Methode über eine Objekt Abfrage, die über `Include` Methoden für die Navigations Eigenschaften `Person` und `Courses` verfügt. Ersetzen Sie die `return`-Anweisung durch Code, der Explizites Laden ausführt, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Führen Sie im Debugger die Seite " *Departments. aspx* " aus, und überprüfen Sie das **IntelliTrace** -Fenster wie zuvor. Nun, wo eine einzelne Abfrage vorhanden war, wird eine lange Sequenz von Ihnen angezeigt.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Klicken Sie auf die erste Zeile **ADO.net** , um zu sehen, was mit der komplexen Abfrage geschehen ist, die Sie zuvor gesehen haben

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

Die Abfrage aus Abteilungen ist eine einfache `Select` Abfrage ohne `Join`-Klausel, gefolgt von separaten Abfragen, die Verwandte Kurse und einen Administrator abrufen, wobei eine Reihe von zwei Abfragen für jede Abteilung verwendet werden, die von der ursprünglichen Abfrage zurückgegeben wird.

> [!NOTE]
> Wenn Sie Lazy Loading aktiviert lassen, kann das Muster, das hier angezeigt wird, mit derselben Abfrage mehrmals wiederholt werden, von Lazy Loading resultieren. Ein Muster, das Sie in der Regel vermeiden möchten, ist das verzögerte Laden verwandter Daten für jede Zeile der primären Tabelle. Wenn Sie nicht überprüft haben, dass eine einzelne joinabfrage zu komplex ist, um effizient zu sein, können Sie in solchen Fällen die Leistung verbessern, indem Sie die primäre Abfrage so ändern, dass Sie Eager Loading verwendet.

## <a name="pre-generating-views"></a>Vorab generierende Sichten

Wenn ein `ObjectContext` Objekt erstmalig in einer neuen Anwendungsdomäne erstellt wird, generiert die Entity Framework eine Reihe von Klassen, die für den Zugriff auf die Datenbank verwendet werden. Diese Klassen werden als *Sichten*bezeichnet. Wenn Sie über ein sehr großes Datenmodell verfügen, können Sie durch das Erstellen dieser Sichten die Website Antwort auf die erste Anforderung einer Seite verzögern, nachdem eine neue Anwendungsdomäne initialisiert wurde. Sie können diese Verzögerung der ersten Anforderung verringern, indem Sie die Sichten zur Kompilierzeit statt zur Laufzeit erstellen.

> [!NOTE]
> Wenn Ihre Anwendung nicht über ein extrem großes Datenmodell verfügt oder wenn Sie über ein großes Datenmodell verfügt, Sie sich aber nicht um ein Leistungsproblem kümmern, das sich nur auf die Anforderung der ersten Seite auswirkt, nachdem IIS wieder verwendet wurde, können Sie diesen Abschnitt überspringen. Die Ansichts Erstellung findet nicht jedes Mal statt, wenn Sie ein `ObjectContext` Objekt instanziieren, da die Ansichten in der Anwendungsdomäne zwischengespeichert werden. Wenn Sie Ihre Anwendung nicht häufig in IIS wieder verwenden, profitieren nur wenige Seiten Anforderungen von vordefinierten Ansichten.

Sie können Sichten mit dem Befehlszeilen Tool " *EdmGen. exe* " oder mithilfe einer T4-Vorlage ( *Text Template Transformation Toolkit* ) vorab generieren. In diesem Tutorial verwenden Sie eine T4-Vorlage.

Fügen Sie im Ordner *dal* eine Datei mithilfe der Vorlage **Text Vorlage** hinzu (Sie befindet sich unter dem Knoten **Allgemein** in der Liste **installierte Vorlagen** ), und nennen Sie Sie *SchoolModel.views.tt*. Ersetzen Sie den vorhandenen Code in der Datei durch den folgenden Code:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Dieser Code generiert Sichten für eine *edmx* -Datei, die sich im selben Ordner wie die Vorlage befindet und denselben Namen wie die Vorlagen Datei hat. Wenn die Vorlagen Datei z. b. den Namen " *SchoolModel.views.tt*" hat, sucht Sie nach einer Datenmodell Datei mit dem Namen " *SchoolModel. edmx*".

Speichern Sie die Datei, klicken Sie dann mit der rechten Maustaste auf **Projektmappen-Explorer** , und wählen Sie **benutzerdefiniertes Tool ausführen**aus.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio generiert eine Codedatei, mit der die Ansichten erstellt werden, die basierend auf der Vorlage den Namen *SchoolModel.views.cs* haben. (Möglicherweise haben Sie bemerkt, dass die Codedatei generiert wird, bevor Sie **benutzerdefiniertes Tool ausführen**auswählen, sobald Sie die Vorlagen Datei speichern.)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Sie können die Anwendung jetzt ausführen und überprüfen, ob Sie wie zuvor funktioniert.

Weitere Informationen zu vorab generierten Sichten finden Sie in den folgenden Ressourcen:

- Gewusst [wie: Vorgenerieren von Ansichten zur Verbesserung der Abfrageleistung](https://msdn.microsoft.com/library/bb896240.aspx) auf der MSDN-Website. Erläutert die Verwendung des Befehlszeilen Tools `EdmGen.exe`, um Sichten vorab zu generieren.
- [Isolieren der Leistung mit vorkompilierten/vordefinierten Sichten in der Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) im Windows Server AppFabric Customer Advisory Team Blog.

Dies schließt die Einführung zur Verbesserung der Leistung in einer ASP.NET-Webanwendung ab, die die Entity Framework verwendet. Weitere Informationen finden Sie in den folgenden Ressourcen:

- Über [Legungen zur Leistung (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) auf der MSDN-Website.
- [Leistungsbezogene Beiträge im Entity Framework Teamblog](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [EF-mergeoptionen und kompilierte Abfragen](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Blog Beitrag, der unerwartetes Verhalten von kompilierten Abfragen und mergeoptionen wie `NoTracking`erläutert. Wenn Sie beabsichtigen, kompilierte Abfragen zu verwenden oder die Einstellungen für zusammenzustellungsoptionen in Ihrer Anwendung zu bearbeiten, lesen Sie dies zuerst
- [Entity Framework Beiträge im Blog zum Daten-und Modellierungs Team Blog](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Enthält Beiträge zu kompilierten Abfragen und verwendet den Visual Studio 2010 Profiler zum Ermitteln von Leistungsproblemen.
- [Entity Framework Forums Thread mit Ratschläge zum Verbessern der Leistung von sehr komplexen Abfragen](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Empfehlungen zur ASP.net State Management](https://msdn.microsoft.com/library/z1hkazw7.aspx)
- [Verwenden des Entity Framework und der ObjectDataSource: Custom Paging](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Blog Beitrag, der auf der in diesen Tutorials erstellten Anwendung condesouniversity aufbaut und erläutert, wie Paging auf der Seite " *Departments. aspx* " implementiert wird.

Im nächsten Tutorial werden einige der wichtigen Verbesserungen der in Version 4 neuen Entity Framework überprüft.

> [!div class="step-by-step"]
> [Zurück](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [Weiter](what-s-new-in-the-entity-framework-4.md)
