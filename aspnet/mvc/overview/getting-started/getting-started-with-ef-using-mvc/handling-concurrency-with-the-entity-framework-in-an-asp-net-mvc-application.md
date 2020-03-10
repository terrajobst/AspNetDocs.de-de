---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: Behandeln von Parallelität mit EF in einer ASP.NET MVC 5-App'
description: In diesem Tutorial wird gezeigt, wie Sie mit optimistischer Parallelität Konflikte behandeln, wenn mehrere Benutzer gleichzeitig dieselbe Entität aktualisieren.
author: tdykstra
ms.author: riande
ms.topic: tutorial
ms.date: 01/15/2019
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 43c5fdff5601c9bff32300d3460de0079a498d28
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499389"
---
# <a name="tutorial-handle-concurrency-with-ef-in-an-aspnet-mvc-5-app"></a>Tutorial: Behandeln von Parallelität mit EF in einer ASP.NET MVC 5-App

In den vorherigen Tutorials haben Sie gelernt, wie Sie Daten aktualisieren. In diesem Tutorial wird gezeigt, wie Sie mit optimistischer Parallelität Konflikte behandeln, wenn mehrere Benutzer gleichzeitig dieselbe Entität aktualisieren. Sie ändern die Webseiten, die mit der `Department` Entität funktionieren, damit Sie Parallelitäts Fehler behandeln. In der nachfolgenden Abbildung sehen Sie die Seiten „Bearbeiten“ und „Löschen“, einschließlich einiger Meldungen, die angezeigt werden, wenn ein Parallelitätskonflikt auftritt.

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erhalten Sie Informationen über Parallelitätskonflikte
> * Optimistische Parallelität hinzufügen
> * Ändern des Abteilungs Controllers
> * Testen der Parallelitäts Behandlung
> * Aktualisieren der Seite „Delete“ (Löschen)

## <a name="prerequisites"></a>Voraussetzungen

* [Asynchrone und gespeicherte Prozeduren](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="concurrency-conflicts"></a>Nebenläufigkeitskonflikte

Ein Parallelitätskonflikt tritt auf, wenn ein Benutzer die Daten einer Entität anzeigt, um diese zu bearbeiten, und ein anderer Benutzer eben diese Entitätsdaten aktualisiert, bevor die Änderungen des ersten Benutzers in die Datenbank geschrieben wurden. Wenn Sie die Erkennung solcher Konflikte nicht aktivieren, überschreibt das letzte Update der Datenbank die Änderungen des anderen Benutzers. In vielen Anwendungen ist dieses Risiko akzeptabel: Wenn es nur wenige Benutzer bzw. wenige Updates gibt, oder wenn es nicht schlimm ist, dass Änderungen überschrieben werden können, ist es den Aufwand, für die Parallelität zu programmieren, möglicherweise nicht wert. In diesem Fall müssen Sie für die Anwendung keine Behandlung von Nebenläufigkeitskonflikten konfigurieren.

### <a name="pessimistic-concurrency-locking"></a>Pessimistische Parallelität (Sperren)

Wenn Ihre Anwendung versehentliche Datenverluste in Parallelitätsszenarios verhindern muss, ist die Verwendung von Datenbanksperren eine Möglichkeit. Dies wird als *pessimistische*Parallelität bezeichnet. Bevor Sie zum Beispiel eine Zeile aus einer Datenbank lesen, fordern Sie eine Sperre für den schreibgeschützten Zugriff oder den Aktualisierungszugriff an. Wenn Sie eine Zeile für den Aktualisierungszugriff sperren, kann kein anderer Benutzer diese Zeile für den schreibgeschützten Zugriff oder den Aktualisierungszugriff sperren, da er eine Kopie der Daten erhalten würde, die gerade geändert werden. Wenn Sie eine Zeile für den schreibgeschützten Zugriff sperren, können andere diese Zeile ebenfalls für den schreibgeschützten Zugriff sperren, aber nicht für den Aktualisierungszugriff.

Das Verwalten von Sperren hat Nachteile. Es kann komplex sein, sie zu programmieren. Dies erfordert erhebliche Datenbankverwaltungsressourcen und kann mit steigender Anzahl der Benutzer einer Anwendung zu Leistungsproblemen führen. Aus diesen Gründen unterstützen nicht alle Datenbankverwaltungssysteme die pessimistische Parallelität. Die Entity Framework bietet keine integrierte Unterstützung für diese, und dieses Tutorial zeigt Ihnen nicht, wie Sie Sie implementieren können.

### <a name="optimistic-concurrency"></a>Optimistische Nebenläufigkeit

Die Alternative zur pessimistischen Parallelität ist die *optimistische*Parallelität. Die Verwendung der optimistischen Parallelität bedeutet, Nebenläufigkeitskonflikte zu erlauben und entsprechend zu reagieren, wenn diese auftreten. John führt z. b. die Bearbeitungsseite für Abteilungen aus, ändert den **Budget** Betrag für die englische Abteilung von $350.000,00 in $0,00.

Bevor John auf **Save**klickt, führt Jane dieselbe Seite aus und ändert das Feld **Start Datum** von 9/1/2007 in 8/8/2013.

John klickt zuerst auf **Speichern** und sieht seine Änderung, wenn der Browser zur Index Seite zurückkehrt. Andrea klickt dann auf **Speichern**. Was daraufhin geschieht, ist abhängig davon, wie Sie Nebenläufigkeitskonflikte behandeln. Einige der Optionen schließen Folgendes ein:

- Sie können nachverfolgen, welche Eigenschaft ein Benutzer geändert hat und nur die entsprechenden Spalten in der Datenbank aktualisieren. Im Beispielszenario würden keine Daten verloren gehen, da verschiedene Eigenschaften von zwei Benutzern aktualisiert wurden. Beim nächsten Mal, wenn jemand die englische Abteilung durchsucht, werden sowohl die Änderungen von John es als auch Jane angezeigt – ein Startdatum von 8/8/2013 und ein Budget von NULL Dollar.

    Diese Methode der Aktualisierung kann die Anzahl von Konflikten reduzieren, die zu Datenverlusten führen können. Sie kann Datenverluste jedoch nicht verhindern, wenn konkurrierende Änderungen an der gleichen Eigenschaft einer Entität vorgenommen werden. Ob Entity Framework auf diese Weise funktioniert, hängt davon ab, wie Sie Ihren Aktualisierungscode implementieren. Oft ist dies in Webanwendungen nicht praktikabel, da es erforderlich sein kann, viele Zustände zu verwalten, um alle ursprünglichen Eigenschaftswerte einer Entität und die neuen Werte im Auge zu behalten. Das Verwalten vieler Zuständen kann sich auf die Leistung der Anwendung auswirken, da es entweder Serverressourcen beansprucht oder in der Webseite selbst (z.B. in ausgeblendeten Feldern) oder in einem Cookie enthalten sein muss.
- Sie können die Änderung von John überschreiben. Beim nächsten Mal, wenn jemand die englische Abteilung durchsucht, werden 8/8/2013 und der wiederhergestellte $350.000,00-Wert angezeigt. Das ist entweder ein *Client gewinnt*- oder ein *Letzter Schreiber gewinnt*-Szenario. (Alle Werte vom Client haben Vorrang vor dem, was sich im Datenspeicher befindet.) Wie in der Einführung zu diesem Abschnitt erwähnt, geschieht dies automatisch, wenn Sie keine Codierung für die Parallelitäts Behandlung durchführen.
- Sie können verhindern, dass die Änderungen von Jane in der Datenbank aktualisiert werden. In der Regel würden Sie eine Fehlermeldung anzeigen, den aktuellen Zustand der Daten anzeigen und zulassen, dass Ihre Änderungen erneut angewendet werden, wenn Sie Sie trotzdem vornehmen möchten. Dieses Szenario wird *Store Wins* (Speicher gewinnt) genannt. (Die Datenspeicher Werte haben Vorrang vor den Werten, die vom Client gesendet wurden.) Sie implementieren das Store WINS-Szenario in diesem Tutorial. Diese Methode stellt sicher, dass keine Änderung überschrieben wird, ohne dass ein Benutzer darüber benachrichtigt wird.

### <a name="detecting-concurrency-conflicts"></a>Erkennen von Parallelitäts Konflikten

Sie können Konflikte lösen, indem Sie die vom Entity Framework ausgelösten [Optimierungen von optimistic-](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Parallelitäts Ausnahmen verarbeiten. Entity Framework muss dazu in der Lage sein, Konflikte zu erkennen, damit es weiß, wann diese Ausnahmen ausgelöst werden sollen. Aus diesem Grund müssen Sie die Datenbank und das Datenmodell entsprechend konfigurieren. Einige der Optionen für das Aktivieren der Konflikterkennung schließen Folgendes ein:

- Fügen Sie eine Änderungsverfolgungsspalte in die Datenbanktabelle ein, die verwendet werden kann, um zu bestimmen, wenn eine Änderung an einer Zeile vorgenommen wurde. Anschließend können Sie die Entity Framework so konfigurieren, dass Sie diese Spalte in die `Where`-Klausel von SQL `Update` oder `Delete`-Befehlen einschließt.

    Der Datentyp der nach Verfolgungs Spalte ist in der Regel " [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)". Der [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) -Wert ist eine sequenzielle Zahl, die jedes Mal, wenn die Zeile aktualisiert wird, inkrementiert wird. In einem `Update`-oder `Delete`-Befehl enthält die `Where`-Klausel den ursprünglichen Wert der nach Verfolgungs Spalte (die Original Zeilen Version). Wenn die zu Aktualisier Ende Zeile von einem anderen Benutzer geändert wurde, unterscheidet sich der Wert in der Spalte `rowversion` vom ursprünglichen Wert, sodass die `Update` oder `Delete` Anweisung die zu Aktualisier Ende Zeile aufgrund der `Where` Klausel nicht finden kann. Wenn das Entity Framework feststellt, dass keine Zeilen mit dem `Update` oder `Delete` Befehl aktualisiert wurden (d. h., wenn die Anzahl der betroffenen Zeilen 0 (null) ist), wird dies als Parallelitäts Konflikt interpretiert.
- Konfigurieren Sie die Entity Framework, um die ursprünglichen Werte jeder Spalte in der Tabelle in der `Where`-Klausel der Befehle `Update` und `Delete` einzuschließen.

    Wie bei der ersten Option gibt die `Where`-Klausel keine zu Aktualisier Ende Zeile zurück, wenn in der Zeile nach dem ersten Lesen der Zeile Änderungen vorgenommen wurden, die der Entity Framework als Parallelitäts Konflikt interpretiert. Bei Datenbanktabellen, die viele Spalten enthalten, kann dieser Ansatz zu sehr großen `Where` Klauseln führen, und es kann erforderlich sein, dass Sie große Mengen an Zuständen aufbewahren. Wie bereits erwähnt, kann das Verwalten großer Mengen von Zuständen die Anwendungsleistung beeinträchtigen. Deshalb wird dieser Ansatz in der Regel nicht empfohlen, und ist nicht die Methode, die in diesem Tutorial verwendet wird.

    Wenn Sie diesen Ansatz für Parallelität implementieren möchten, müssen Sie alle nicht-Primärschlüssel-Eigenschaften in der Entität, [für die Sie die](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) Parallelität nachverfolgen möchten, markieren, indem Sie das Attribut "-Attribut hinzufügen" hinzufügen. Diese Änderung ermöglicht es dem Entity Framework, alle Spalten in der SQL `WHERE`-Klausel von `UPDATE`-Anweisungen einzubeziehen.

Im restlichen Teil dieses Tutorials fügen Sie der Entität `Department` eine [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) -nach Verfolgungs Eigenschaft hinzu, erstellen einen Controller und Sichten und testen, ob alles ordnungsgemäß funktioniert.

## <a name="add-optimistic-concurrency"></a>Optimistische Parallelität hinzufügen

Fügen Sie in *models\department.cs*eine Überwachungs Eigenschaft mit dem Namen `RowVersion`hinzu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

Das [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) -Attribut gibt an, dass diese Spalte in der `Where`-Klausel von `Update` und `Delete` Befehle enthalten ist, die an die Datenbank gesendet werden. Das-Attribut wird als [Zeitstempel](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) bezeichnet, da frühere Versionen von SQL Server einen SQL- [Zeitstempel](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) -Datentyp verwendet haben, bevor die SQL- [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) ihn ersetzt hat. Der .NET-Typ für [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) ist ein Bytearray.

Wenn Sie die fließende API bevorzugen, können Sie die [iskonaccesscytoken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) -Methode verwenden, um die nach Verfolgungs Eigenschaft anzugeben, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Durch das Hinzufügen einer Eigenschaft ändern Sie das Datenbankmodell, daher müssen Sie eine weitere Migration durchführen. Geben Sie die folgenden Befehle in die Paket-Manager-Konsole ein:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-department-controller"></a>Ändern des Abteilungs Controllers

Fügen Sie in *controllers\departmentcontroller.cs*eine `using`-Anweisung hinzu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Ändern Sie in der Datei *DepartmentController.cs* alle vier Vorkommen von "LastName" in "FullName", sodass die Dropdown Listen der Abteilungs Administratoren den vollständigen Namen des Dozenten und nicht nur den Nachnamen enthalten.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Ersetzen Sie den vorhandenen Code für die `HttpPost` `Edit`-Methode durch den folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Wenn die Methode `FindAsync` NULL zurückgibt, wurde die Abteilung von einem anderen Benutzer gelöscht. Der gezeigte Code verwendet die bereitgestellte Formular Werte, um eine Abteilungs Entität zu erstellen, sodass die Bearbeitungsseite mit einer Fehlermeldung erneut angezeigt werden kann. Alternativ müssen Sie die Abteilungsentität nicht erneut erstellen, wenn Sie nur eine Fehlermeldung anzeigen, ohne die Abteilungsfelder erneut anzuzeigen.

In der Ansicht wird der ursprüngliche `RowVersion` Wert in einem ausgeblendeten Feld gespeichert, und die-Methode empfängt Sie im `rowVersion`-Parameter. Bevor Sie `SaveChanges` aufrufen, müssen Sie diesen ursprünglichen Eigenschaftswert von `RowVersion` in die Auflistung `OriginalValues` für die Entität einfügen. Wenn die Entity Framework dann einen SQL `UPDATE`-Befehl erstellt, enthält dieser Befehl eine `WHERE`-Klausel, die nach einer Zeile mit dem ursprünglichen `RowVersion` Wert sucht.

Wenn der `UPDATE`-Befehl keine Zeilen beeinflusst (keine Zeilen haben den ursprünglichen `RowVersion` Wert), löst der Entity Framework eine `DbUpdateConcurrencyException` Ausnahme aus, und der Code im `catch`-Block Ruft die betroffene `Department` Entität aus dem Ausnahme Objekt ab.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Dieses Objekt verfügt über die neuen Werte, die vom Benutzer in der `Entity`-Eigenschaft eingegeben werden, und Sie können die aus der Datenbank gelesenen Werte abrufen, indem Sie die `GetDatabaseValues`-Methode aufrufen.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Die `GetDatabaseValues`-Methode gibt NULL zurück, wenn die Zeile aus der Datenbank gelöscht wurde. Andernfalls müssen Sie das zurückgegebene Objekt in die `Department` Klasse umwandeln, um auf die `Department` Eigenschaften zugreifen zu können. (Da Sie bereits auf das Löschen geprüft haben, ist `databaseEntry` nur dann NULL, wenn die Abteilung nach der Ausführung `FindAsync` und vor der `SaveChanges` Ausführung gelöscht wurde.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Als Nächstes fügt der Code eine benutzerdefinierte Fehlermeldung für jede Spalte hinzu, die Daten Bankwerte aufweist, die sich von dem Benutzer auf der Bearbeitungsseite unterscheiden:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Eine längere Fehlermeldung erläutert, was passiert ist und was damit zu tun ist:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Schließlich legt der Code den `RowVersion` Wert des `Department`-Objekts auf den neuen Wert fest, der aus der Datenbank abgerufen wird. Dieser neue `RowVersion`-Wert wird in dem ausgeblendeten Feld gespeichert, wenn die Seite „Bearbeiten“ erneut angezeigt wird. Das nächste Mal, wenn der Benutzer auf **Speichern** klickt, werden nur Parallelitätsfehler abgefangen, die nach dem erneuten Anzeigen der Seite „Bearbeiten“ aufgetreten sind.

Fügen Sie in *views\department\edit.cshtml*ein ausgeblendetes Feld hinzu, um den `RowVersion`-Eigenschafts Wert direkt nach dem ausgeblendeten Feld für die `DepartmentID`-Eigenschaft zu speichern:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="test-concurrency-handling"></a>Testen der Parallelitäts Behandlung

Führen Sie den Standort aus, und klicken Sie auf **Abteilungen**

Klicken Sie mit der rechten Maustaste auf den Link **Bearbeiten** für die englische Abteilung, und wählen Sie **auf der Registerkarte neu öffnen** und dann auf den Link **Bearbeiten** für die englische Abteilung. Auf den beiden Registerkarten werden dieselben Informationen angezeigt.

Ändern Sie ein Feld in der ersten Registerkarte, und klicken Sie auf **Speichern**.

Der Browser zeigt die Indexseite mit dem geänderten Wert an.

Ändern Sie ein Feld auf der zweiten Registerkarte des Browsers, und klicken Sie auf **Speichern**. Folgende Fehlermeldung wird angezeigt:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Klicken Sie erneut auf **Speichern**. Der Wert, den Sie in der zweiten Registerkarte des Browsers eingegeben haben, wird zusammen mit dem ursprünglichen Wert der Daten, die Sie im ersten Browser geändert haben, gespeichert. Die gespeicherten Werte werden Ihnen auf der Indexseite angezeigt.

## <a name="update-the-delete-page"></a>Aktualisieren der Seite „Delete“ (Löschen)

Bei der Seite „Löschen“ entdeckt Entity Framework Nebenläufigkeitskonflikte, die durch die Bearbeitung einer Abteilung ausgelöst wurden, auf ähnliche Weise. Wenn die `HttpGet` `Delete`-Methode die Bestätigungs Ansicht anzeigt, enthält die Sicht den ursprünglichen `RowVersion` Wert in ein ausgeblendetes Feld. Dieser Wert ist für die `HttpPost` `Delete` Methode verfügbar, die aufgerufen wird, wenn der Benutzer den Löschvorgang bestätigt. Wenn der Entity Framework den SQL `DELETE`-Befehl erstellt, enthält er eine `WHERE`-Klausel mit dem ursprünglichen `RowVersion` Wert. Wenn für den Befehl null Zeilen betroffen sind (d. h. die Zeile wurde nach dem Anzeigen der Lösch Bestätigungsseite geändert), wird eine Parallelitäts Ausnahme ausgelöst, und die `HttpGet Delete`-Methode wird mit einem Fehlerflag aufgerufen, das auf `true` festgelegt ist, um die Bestätigungsseite mit einer Fehlermeldung erneut anzuzeigen. Es ist auch möglich, dass 0 Zeilen betroffen sind, weil die Zeile von einem anderen Benutzer gelöscht wurde. in diesem Fall wird eine andere Fehlermeldung angezeigt.

Ersetzen Sie in *DepartmentController.cs*die `HttpGet` `Delete`-Methode durch den folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Die Methode akzeptiert einen optionalen Parameter, der angibt, ob die Seite nach einem Parallelitätsfehler erneut angezeigt wird. Wenn dieses Flag `true`ist, wird eine Fehlermeldung mithilfe einer `ViewBag`-Eigenschaft an die Sicht gesendet.

Ersetzen Sie den Code in der `HttpPost` `Delete`-Methode (mit dem Namen `DeleteConfirmed`) durch den folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

In dem eingerüsteten Code, den Sie soeben ersetzt haben, akzeptiert diese Methode nur eine Datensatz-ID:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Sie haben diesen Parameter in eine `Department` Entitäts Instanz geändert, die von der Modell Bindung erstellt wurde. Dadurch erhalten Sie zusätzlich zum Daten Satz Schlüssel Zugriff auf den `RowVersion`-Eigenschafts Wert.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Ebenfalls haben Sie den Namen der Aktionsmethode `DeleteConfirmed` auf `Delete` geändert. Der Gerüst Code hat den `HttpPost` `Delete`-Methode genannt `DeleteConfirmed`, um der `HttpPost`-Methode eine eindeutige Signatur zu übergeben. (Die CLR erfordert überladene Methoden, um verschiedene Methoden Parameter zu haben.) Nachdem die Signaturen eindeutig sind, können Sie die MVC-Konvention verwenden und den gleichen Namen für die `HttpPost`-und `HttpGet` Delete-Methode verwenden.

Wenn ein Parallelitätsfehler abgefangen wird, zeigt der Code erneut die Bestätigungsseite „Löschen“ an, und stellt ein Flag bereit, das angibt, dass eine Fehlermeldung für die Parallelität angezeigt werden soll.

Ersetzen Sie in *views\department\delete.cshtml*den Gerüst Code durch den folgenden Code, der ein Fehlermeldungs Feld und ausgeblendete Felder für die Eigenschaften "DepartmentID" und "rowversion" hinzufügt. Die Änderungen werden hervorgehoben.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

Dieser Code fügt eine Fehlermeldung zwischen den Überschriften `h2` und `h3` hinzu:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

`LastName` wird durch `FullName` im Feld `Administrator` ersetzt:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Schließlich werden nach der `Html.BeginForm`-Anweisung ausgeblendete Felder für die Eigenschaften `DepartmentID` und `RowVersion` hinzugefügt:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

Führen Sie die Index Seite für Abteilungen aus. Klicken Sie mit der rechten Maustaste auf den Link **Löschen** für die englische Abteilung, und wählen Sie **in neuer Registerkarte öffnen aus,** und klicken Sie dann auf der ersten Registerkarte auf den Link **Bearbeiten** für die

Ändern Sie im ersten Fenster einen der Werte, und klicken Sie auf **Speichern**.

Die Seite Index bestätigt die Änderung.

Klicken Sie in der zweiten Registerkarte auf **Löschen**.

Ihnen wird eine Fehlermeldung zur Parallelität angezeigt, und die Abteilungswerte werden mit den aktuellen Werten der Datenbank aktualisiert.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Wenn Sie erneut auf **Löschen** klicken, werden Sie auf die Indexseite weitergeleitet, die anzeigt, dass die Abteilung gelöscht wurde.

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Links zu anderen Entity Framework Ressourcen finden Sie unter [ASP.NET Data Access-Empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

Informationen zu anderen Möglichkeiten, verschiedene Parallelitäts Szenarios zu behandeln, finden Sie unter [optimistische](https://msdn.microsoft.com/data/jj592904) Parallelitäts Muster und [Arbeiten mit Eigenschafts Werten](https://msdn.microsoft.com/data/jj592677) auf MSDN. Das nächste Tutorial zeigt, wie die "Tabelle pro Hierarchie"-Vererbung für die `Instructor`-und `Student` Entitäten implementiert wird.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Haben Sie Informationen über Parallelitätskonflikte erhalten
> * Optimistische Parallelität hinzugefügt
> * Geänderte Abteilungs Controller
> * Getestete Parallelitäts Behandlung
> * Die Seite „Löschen“ aktualisiert

Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie die Vererbung im Datenmodell implementiert wird.
> [!div class="nextstepaction"]
> [Implementieren der Vererbung im Datenmodell](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
