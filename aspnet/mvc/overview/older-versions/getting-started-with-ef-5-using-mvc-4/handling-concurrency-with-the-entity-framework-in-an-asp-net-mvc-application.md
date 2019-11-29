---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Behandeln von Parallelität mit dem Entity Framework in einer ASP.NET MVC-Anwendung (7 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio erstellt werden...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 0383974baa16bb0d5fc588f9303290bdb0fd979c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595348"
---
# <a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Behandeln von Parallelität mit dem Entity Framework in einer ASP.NET MVC-Anwendung (7 von 10)

von [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio 2012 erstellt werden. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe von Anfang an starten oder [ein Starter-Projekt für dieses Kapitel herunterladen](building-the-ef5-mvc4-chapter-downloads.md) und hier beginnen.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem stoßen, das Sie nicht beheben können, [Laden Sie das abgeschlossene Kapitel herunter](building-the-ef5-mvc4-chapter-downloads.md) , und versuchen Sie, das Problem zu reproduzieren. Im Allgemeinen können Sie die Lösung für das Problem finden, indem Sie Ihren Code mit dem abgeschlossenen Code vergleichen. Informationen zu häufigen Fehlern und deren Lösung finden Sie unter [Fehler und](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors) Problem Umgehungen.

In den beiden vorherigen Tutorials haben Sie mit verwandten Daten gearbeitet. In diesem Tutorial wird gezeigt, wie Sie Parallelität behandeln. Sie erstellen Webseiten, die mit der `Department`-Entität arbeiten, und die Seiten, die `Department` Entitäten bearbeiten und löschen, behandeln Parallelitäts Fehler. Die folgenden Abbildungen zeigen die Index-und Lösch Seiten, einschließlich einiger Meldungen, die angezeigt werden, wenn ein Parallelitäts Konflikt auftritt.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Nebenläufigkeitskonflikte

Ein Parallelitätskonflikt tritt auf, wenn ein Benutzer die Daten einer Entität anzeigt, um diese zu bearbeiten, und ein anderer Benutzer eben diese Entitätsdaten aktualisiert, bevor die Änderungen des ersten Benutzers in die Datenbank geschrieben wurden. Wenn Sie die Erkennung solcher Konflikte nicht aktivieren, überschreibt das letzte Update der Datenbank die Änderungen des anderen Benutzers. In vielen Anwendungen ist dieses Risiko akzeptabel: Wenn es nur wenige Benutzer bzw. wenige Updates gibt, oder wenn es nicht schlimm ist, dass Änderungen überschrieben werden können, ist es den Aufwand, für die Parallelität zu programmieren, möglicherweise nicht wert. In diesem Fall müssen Sie für die Anwendung keine Behandlung von Nebenläufigkeitskonflikten konfigurieren.

### <a name="pessimistic-concurrency-locking"></a>Pessimistische Parallelität (Sperren)

Wenn Ihre Anwendung versehentliche Datenverluste in Parallelitätsszenarios verhindern muss, ist die Verwendung von Datenbanksperren eine Möglichkeit. Dies wird als *pessimistische*Parallelität bezeichnet. Bevor Sie zum Beispiel eine Zeile aus einer Datenbank lesen, fordern Sie eine Sperre für den schreibgeschützten Zugriff oder den Aktualisierungszugriff an. Wenn Sie eine Zeile für den Aktualisierungszugriff sperren, kann kein anderer Benutzer diese Zeile für den schreibgeschützten Zugriff oder den Aktualisierungszugriff sperren, da er eine Kopie der Daten erhalten würde, die gerade geändert werden. Wenn Sie eine Zeile für den schreibgeschützten Zugriff sperren, können andere diese Zeile ebenfalls für den schreibgeschützten Zugriff sperren, aber nicht für den Aktualisierungszugriff.

Das Verwalten von Sperren hat Nachteile. Es kann komplex sein, sie zu programmieren. Hierfür sind beträchtliche Ressourcen für die Datenbankverwaltung erforderlich, und es kann zu Leistungsproblemen führen, wenn die Anzahl der Benutzer einer Anwendung zunimmt (das heißt, es ist nicht gut skalierbar). Aus diesen Gründen unterstützen nicht alle Datenbankverwaltungssysteme die pessimistische Parallelität. Die Entity Framework bietet keine integrierte Unterstützung für diese, und dieses Tutorial zeigt Ihnen nicht, wie Sie Sie implementieren können.

### <a name="optimistic-concurrency"></a>Optimistische Nebenläufigkeit

Die Alternative zur pessimistischen Parallelität ist die *optimistische*Parallelität. Die Verwendung der optimistischen Parallelität bedeutet, Nebenläufigkeitskonflikte zu erlauben und entsprechend zu reagieren, wenn diese auftreten. John führt z. b. die Bearbeitungsseite für Abteilungen aus, ändert den **Budget** Betrag für die englische Abteilung von $350.000,00 in $0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Bevor John auf **Save**klickt, führt Jane dieselbe Seite aus und ändert das Feld **Start Datum** von 9/1/2007 in 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John klickt zuerst auf **Speichern** und sieht seine Änderung, wenn der Browser zur Index Seite zurückkehrt. Andrea klickt dann auf **Speichern**. Was daraufhin geschieht ist abhängig davon, wie Sie Nebenläufigkeitskonflikte handhaben. Einige der Optionen schließen Folgendes ein:

- Sie können nachverfolgen, welche Eigenschaft ein Benutzer geändert hat und nur die entsprechenden Spalten in der Datenbank aktualisieren. Im Beispielszenario würden keine Daten verloren gehen, da verschiedene Eigenschaften von zwei Benutzern aktualisiert wurden. Beim nächsten Mal, wenn jemand die englische Abteilung durchsucht, werden sowohl die Änderungen von John es als auch Jane angezeigt – ein Startdatum von 8/8/2013 und ein Budget von NULL Dollar.

    Diese Methode der Aktualisierung kann die Anzahl von Konflikten reduzieren, die zu Datenverlusten führen können. Sie kann Datenverluste jedoch nicht verhindern, wenn konkurrierende Änderungen an der gleichen Eigenschaft einer Entität vorgenommen werden. Ob Entity Framework auf diese Weise funktioniert, hängt davon ab, wie Sie Ihren Aktualisierungscode implementieren. Oft ist dies in Webanwendungen nicht praktikabel, da es erforderlich sein kann, viele Zustände zu verwalten, um alle ursprünglichen Eigenschaftswerte einer Entität und die neuen Werte im Auge zu behalten. Die Verwaltung großer Zustands Mengen kann die Leistung der Anwendung beeinträchtigen, da Sie entweder Server Ressourcen erfordert oder in der Webseite selbst enthalten sein muss (z. b. in ausgeblendeten Feldern).
- Sie können die Änderung von John überschreiben. Beim nächsten Mal, wenn jemand die englische Abteilung durchsucht, werden 8/8/2013 und der wiederhergestellte $350.000,00-Wert angezeigt. Das ist entweder ein *Client gewinnt*- oder ein *Letzter Schreiber gewinnt*-Szenario. (Die Client Werte haben Vorrang vor dem, was im Datenspeicher liegt.) Wie in der Einführung zu diesem Abschnitt erwähnt, geschieht dies automatisch, wenn Sie keine Codierung für die Parallelitäts Behandlung durchführen.
- Sie können verhindern, dass die Änderungen von Jane in der Datenbank aktualisiert werden. In der Regel würden Sie eine Fehlermeldung anzeigen, den aktuellen Zustand der Daten anzeigen und zulassen, dass Ihre Änderungen erneut angewendet werden, wenn Sie Sie trotzdem vornehmen möchten. Dieses Szenario wird *Store Wins* (Speicher gewinnt) genannt. (Die Datenspeicher Werte haben Vorrang vor den Werten, die vom Client gesendet wurden.) Sie implementieren das Store WINS-Szenario in diesem Tutorial. Diese Methode stellt sicher, dass keine Änderung überschrieben wird, ohne dass ein Benutzer darüber benachrichtigt wird.

### <a name="detecting-concurrency-conflicts"></a>Erkennen von Parallelitäts Konflikten

Sie können Konflikte lösen, indem Sie die vom Entity Framework ausgelösten [Optimierungen von optimistic-](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Parallelitäts Ausnahmen verarbeiten. Entity Framework muss dazu in der Lage sein, Konflikte zu erkennen, damit es weiß, wann diese Ausnahmen ausgelöst werden sollen. Aus diesem Grund müssen Sie die Datenbank und das Datenmodell entsprechend konfigurieren. Einige der Optionen für das Aktivieren der Konflikterkennung schließen Folgendes ein:

- Fügen Sie eine Änderungsverfolgungsspalte in die Datenbanktabelle ein, die verwendet werden kann, um zu bestimmen, wenn eine Änderung an einer Zeile vorgenommen wurde. Anschließend können Sie die Entity Framework so konfigurieren, dass Sie diese Spalte in die `Where`-Klausel von SQL `Update` oder `Delete`-Befehlen einschließt.

    Der Datentyp der nach Verfolgungs Spalte ist in der Regel " [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)". Der [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) -Wert ist eine sequenzielle Zahl, die jedes Mal, wenn die Zeile aktualisiert wird, inkrementiert wird. In einem `Update`-oder `Delete`-Befehl enthält die `Where`-Klausel den ursprünglichen Wert der nach Verfolgungs Spalte (die Original Zeilen Version). Wenn die zu Aktualisier Ende Zeile von einem anderen Benutzer geändert wurde, unterscheidet sich der Wert in der Spalte `rowversion` vom ursprünglichen Wert, sodass die `Update` oder `Delete` Anweisung die zu Aktualisier Ende Zeile aufgrund der `Where` Klausel nicht finden kann. Wenn das Entity Framework feststellt, dass keine Zeilen mit dem `Update` oder `Delete` Befehl aktualisiert wurden (d. h., wenn die Anzahl der betroffenen Zeilen 0 (null) ist), wird dies als Parallelitäts Konflikt interpretiert.
- Konfigurieren Sie die Entity Framework, um die ursprünglichen Werte jeder Spalte in der Tabelle in der `Where`-Klausel der Befehle `Update` und `Delete` einzuschließen.

    Wie bei der ersten Option gibt die `Where`-Klausel keine zu Aktualisier Ende Zeile zurück, wenn in der Zeile nach dem ersten Lesen der Zeile Änderungen vorgenommen wurden, die der Entity Framework als Parallelitäts Konflikt interpretiert. Bei Datenbanktabellen, die viele Spalten enthalten, kann dieser Ansatz zu sehr großen `Where` Klauseln führen, und es kann erforderlich sein, dass Sie große Mengen an Zuständen aufbewahren. Wie bereits erwähnt, kann die Verwaltung großer Zustands Mengen die Leistung der Anwendung beeinträchtigen, da Sie entweder Server Ressourcen erfordert oder auf der Webseite selbst enthalten sein muss. Daher wird dieser Ansatz in der Regel nicht empfohlen und ist nicht die in diesem Tutorial verwendete Methode.

    Wenn Sie diesen Ansatz für Parallelität implementieren möchten, müssen Sie alle nicht-Primärschlüssel-Eigenschaften in der Entität, [für die Sie die](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) Parallelität nachverfolgen möchten, markieren, indem Sie das Attribut "-Attribut hinzufügen" hinzufügen. Diese Änderung ermöglicht es dem Entity Framework, alle Spalten in der SQL `WHERE`-Klausel von `UPDATE`-Anweisungen einzubeziehen.

Im restlichen Teil dieses Tutorials fügen Sie der Entität `Department` eine [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) -nach Verfolgungs Eigenschaft hinzu, erstellen einen Controller und Sichten und testen, ob alles ordnungsgemäß funktioniert.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Hinzufügen einer Eigenschaft der vollständigen Parallelität zur Abteilungs Entität

Fügen Sie in *models\department.cs*eine Überwachungs Eigenschaft mit dem Namen `RowVersion`hinzu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

Das [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) -Attribut gibt an, dass diese Spalte in der `Where`-Klausel von `Update` und `Delete` Befehle enthalten ist, die an die Datenbank gesendet werden. Das-Attribut wird als [Zeitstempel](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) bezeichnet, da frühere Versionen von SQL Server einen SQL- [Zeitstempel](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) -Datentyp verwendet haben, bevor die SQL- [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) ihn ersetzt hat. Der .NET-Typ für `rowversion` ist ein Bytearray. Wenn Sie die fließende API bevorzugen, können Sie die [iskonaccesscytoken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) -Methode verwenden, um die nach Verfolgungs Eigenschaft anzugeben, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Durch das Hinzufügen einer Eigenschaft ändern Sie das Datenbankmodell, daher müssen Sie eine weitere Migration durchführen. Geben Sie die folgenden Befehle in die Paket-Manager-Konsole ein:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Erstellen eines Abteilungs Controllers

Erstellen Sie einen `Department` Controller und Sichten wie die anderen Controller, und verwenden Sie dabei die folgenden Einstellungen:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

Fügen Sie in *controllers\departmentcontroller.cs*eine `using`-Anweisung hinzu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Ändern Sie "LastName" in "FullName" überall in dieser Datei (vier Vorkommen), damit die Dropdown Listen der Abteilungs Administratoren den vollständigen Namen des Dozenten und nicht nur den Nachnamen enthalten.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Ersetzen Sie den vorhandenen Code für die `HttpPost` `Edit`-Methode durch den folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

In der Ansicht wird der ursprüngliche `RowVersion` Wert in einem ausgeblendeten Feld gespeichert. Wenn der Modell Binder die `department` Instanz erstellt, hat dieses Objekt den ursprünglichen `RowVersion`-Eigenschafts Wert und die neuen Werte für die anderen Eigenschaften, die vom Benutzer auf der Bearbeitungsseite eingegeben werden. Wenn die Entity Framework dann einen SQL `UPDATE`-Befehl erstellt, enthält dieser Befehl eine `WHERE`-Klausel, die nach einer Zeile mit dem ursprünglichen `RowVersion` Wert sucht.

Wenn der `UPDATE`-Befehl keine Zeilen beeinflusst (keine Zeilen haben den ursprünglichen `RowVersion` Wert), löst der Entity Framework eine `DbUpdateConcurrencyException` Ausnahme aus, und der Code im `catch`-Block Ruft die betroffene `Department` Entität aus dem Ausnahme Objekt ab. Diese Entität verfügt sowohl über die aus der Datenbank gelesenen Werte als auch über die neuen Werte, die vom Benutzer eingegeben werden:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Als Nächstes fügt der Code eine benutzerdefinierte Fehlermeldung für jede Spalte hinzu, die Daten Bankwerte aufweist, die sich von dem Benutzer auf der Bearbeitungsseite unterscheiden:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Eine längere Fehlermeldung erläutert, was passiert ist und was damit zu tun ist:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Schließlich legt der Code den `RowVersion` Wert des `Department`-Objekts auf den neuen Wert fest, der aus der Datenbank abgerufen wird. Dieser neue `RowVersion`-Wert wird in dem ausgeblendeten Feld gespeichert, wenn die Seite „Bearbeiten“ erneut angezeigt wird. Das nächste Mal, wenn der Benutzer auf **Speichern** klickt, werden nur Parallelitätsfehler abgefangen, die nach dem erneuten Anzeigen der Seite „Bearbeiten“ aufgetreten sind.

Fügen Sie in *views\department\edit.cshtml*ein ausgeblendetes Feld hinzu, um den `RowVersion`-Eigenschafts Wert direkt nach dem ausgeblendeten Feld für die `DepartmentID`-Eigenschaft zu speichern:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

Ersetzen Sie in *views\department\index.cshtml*den vorhandenen Code durch den folgenden Code, um Zeilen links nach Links zu verschieben und den Seitentitel und die Spaltenüberschriften so zu ändern, dass Sie `FullName` anstatt `LastName` in der Spalte **Administrator** angezeigt werden:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>Testen der Verarbeitung optimistischer Parallelität

Führen Sie den Standort aus, und klicken Sie auf **Abteilungen**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Klicken Sie mit der rechten Maustaste auf den Link **Bearbeiten** für Kim Abercrombie, wählen Sie **in neuer Registerkarte öffnen aus,** und klicken Sie dann auf den Link **Bearbeiten** für Kim Abercrombie In den beiden Fenstern werden dieselben Informationen angezeigt.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Ändern Sie ein Feld im ersten Browserfenster, und klicken Sie auf **Speichern**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Der Browser zeigt die Indexseite mit dem geänderten Wert an.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Ändern Sie das Feld beliebiges Feld im zweiten Browserfenster, und klicken Sie auf **Speichern**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Klicken Sie im zweiten Browserfenster auf **Speichern** . Folgende Fehlermeldung wird angezeigt:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Klicken Sie erneut auf **Speichern**. Der Wert, den Sie im zweiten Browser eingegeben haben, wird zusammen mit dem ursprünglichen Wert der Daten gespeichert, die Sie im ersten Browser ändern. Die gespeicherten Werte werden Ihnen auf der Indexseite angezeigt.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualisieren der Seite "Löschen"

Bei der Seite „Löschen“ entdeckt Entity Framework Nebenläufigkeitskonflikte, die durch die Bearbeitung einer Abteilung ausgelöst wurden, auf ähnliche Weise. Wenn die `HttpGet` `Delete`-Methode die Bestätigungs Ansicht anzeigt, enthält die Sicht den ursprünglichen `RowVersion` Wert in ein ausgeblendetes Feld. Dieser Wert ist für die `HttpPost` `Delete` Methode verfügbar, die aufgerufen wird, wenn der Benutzer den Löschvorgang bestätigt. Wenn der Entity Framework den SQL `DELETE`-Befehl erstellt, enthält er eine `WHERE`-Klausel mit dem ursprünglichen `RowVersion` Wert. Wenn für den Befehl null Zeilen betroffen sind (d. h. die Zeile wurde nach dem Anzeigen der Lösch Bestätigungsseite geändert), wird eine Parallelitäts Ausnahme ausgelöst, und die `HttpGet Delete`-Methode wird mit einem Fehlerflag aufgerufen, das auf `true` festgelegt ist, um die Bestätigungsseite mit einer Fehlermeldung erneut anzuzeigen. Es ist auch möglich, dass 0 Zeilen betroffen sind, weil die Zeile von einem anderen Benutzer gelöscht wurde. in diesem Fall wird eine andere Fehlermeldung angezeigt.

Ersetzen Sie in *DepartmentController.cs*die `HttpGet` `Delete`-Methode durch den folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Die Methode akzeptiert einen optionalen Parameter, der angibt, ob die Seite nach einem Parallelitätsfehler erneut angezeigt wird. Wenn dieses Flag `true`ist, wird eine Fehlermeldung mithilfe einer `ViewBag`-Eigenschaft an die Sicht gesendet.

Ersetzen Sie den Code in der `HttpPost` `Delete`-Methode (mit dem Namen `DeleteConfirmed`) durch den folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

In dem eingerüsteten Code, den Sie soeben ersetzt haben, akzeptiert diese Methode nur eine Datensatz-ID:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Sie haben diesen Parameter in eine `Department` Entitäts Instanz geändert, die von der Modell Bindung erstellt wurde. Dadurch erhalten Sie zusätzlich zum Daten Satz Schlüssel Zugriff auf den `RowVersion`-Eigenschafts Wert.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Ebenfalls haben Sie den Namen der Aktionsmethode `DeleteConfirmed` auf `Delete` geändert. Der Gerüst Code hat den `HttpPost` `Delete`-Methode genannt `DeleteConfirmed`, um der `HttpPost`-Methode eine eindeutige Signatur zu übergeben. (Die CLR erfordert überladene Methoden, um verschiedene Methoden Parameter zu haben.) Nachdem die Signaturen eindeutig sind, können Sie die MVC-Konvention verwenden und den gleichen Namen für die `HttpPost`-und `HttpGet` Delete-Methode verwenden.

Wenn ein Parallelitätsfehler abgefangen wird, zeigt der Code erneut die Bestätigungsseite „Löschen“ an, und stellt ein Flag bereit, das angibt, dass eine Fehlermeldung für die Parallelität angezeigt werden soll.

Ersetzen Sie in *views\department\delete.cshtml*den erstellten Code durch den folgenden Code, durch den einige Formatierungs Änderungen vorgenommen und ein Fehlermeldungs Feld hinzugefügt wird. Die Änderungen werden hervorgehoben.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Dieser Code fügt eine Fehlermeldung zwischen den Überschriften `h2` und `h3` hinzu:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`LastName` wird durch `FullName` im Feld `Administrator` ersetzt:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Schließlich werden nach der `Html.BeginForm`-Anweisung ausgeblendete Felder für die Eigenschaften `DepartmentID` und `RowVersion` hinzugefügt:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Führen Sie die Index Seite für Abteilungen aus. Klicken Sie mit der rechten Maustaste auf den Link **Löschen** für die englische Abteilung, und wählen Sie **in neuem Fenster öffnen aus,** und klicken Sie dann im ersten Fenster auf den Link **Bearbeiten** für die englische Abteilung.

Ändern Sie im ersten Fenster einen der Werte, und klicken Sie auf **Speichern** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

Die Seite Index bestätigt die Änderung.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Klicken Sie im zweiten Fenster auf **Löschen**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Ihnen wird eine Fehlermeldung zur Parallelität angezeigt, und die Abteilungswerte werden mit den aktuellen Werten der Datenbank aktualisiert.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Wenn Sie erneut auf **Löschen** klicken, werden Sie auf die Indexseite weitergeleitet, die anzeigt, dass die Abteilung gelöscht wurde.

## <a name="summary"></a>Summary

Damit ist die Einführung in die Behandlung von Nebenläufigkeitskonflikten abgeschlossen. Informationen zu anderen Möglichkeiten, verschiedene Parallelitäts Szenarios zu behandeln, finden Sie unter [optimistische](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) Parallelitäts Muster und [Arbeiten mit Eigenschafts Werten](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) im Entity Framework Teamblog. Das nächste Tutorial zeigt, wie die "Tabelle pro Hierarchie"-Vererbung für die `Instructor`-und `Student` Entitäten implementiert wird.

Links zu anderen Entity Framework Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
