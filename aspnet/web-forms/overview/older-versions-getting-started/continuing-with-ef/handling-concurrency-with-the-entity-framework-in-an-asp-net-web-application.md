---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Behandeln von Parallelität mit dem Entity Framework 4,0 in einer ASP.NET 4-Webanwendung | Microsoft-Dokumentation
author: tdykstra
description: Diese tutorialreihe basiert auf der Webanwendung der Website von "Web", die in der tutorialreihe für die ersten Schritte mit Entity Framework 4,0 erstellt wurde. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 3df5f7d9c8fb22e1ea34fe16560bdb9a1309bb56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78513501"
---
# <a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Behandeln von Parallelität mit dem Entity Framework 4,0 in einer ASP.NET 4-Webanwendung

von [Tom Dykstra](https://github.com/tdykstra)

> Diese tutorialreihe basiert auf der Webanwendung der Website von "Web", die in der tutorialreihe für die ersten Schritte [mit Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) erstellt wurde. Wenn Sie die vorherigen Tutorials nicht durchgearbeitet haben, können Sie die Anwendung, die Sie erstellt haben, als Ausgangspunkt für dieses Tutorial [herunterladen](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) . Sie können auch [die Anwendung herunterladen](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , die von der kompletten tutorialreihe erstellt wird. Wenn Sie Fragen zu den Tutorials haben, können Sie Sie im ASP.net- [Entity Framework Forum](https://forums.asp.net/1227.aspx)Posten.

Im vorherigen Tutorial haben Sie gelernt, wie Sie Daten mithilfe des `ObjectDataSource`-Steuer Elements und des Entity Framework sortieren und filtern. Dieses Tutorial zeigt Optionen für die Handhabung von Parallelität in einer ASP.NET-Webanwendung, die die Entity Framework verwendet. Sie erstellen eine neue Webseite, die für das Aktualisieren von Dozenten-Office-Zuweisungen reserviert ist. Sie behandeln Parallelitäts Probleme auf dieser Seite und auf der Seite "Abteilungen", die Sie zuvor erstellt haben.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Nebenläufigkeitskonflikte

Ein Parallelitäts Konflikt tritt auf, wenn ein Benutzer einen Datensatz bearbeitet und ein anderer Benutzer denselben Datensatz bearbeitet, bevor die Änderung des ersten Benutzers in die Datenbank geschrieben wird. Wenn Sie die Entity Framework nicht so einrichten, dass solche Konflikte erkannt werden, überschreibt der Benutzer, der die Datenbank zuletzt aktualisiert hat, die Änderungen des anderen Benutzers. In vielen Anwendungen ist dieses Risiko akzeptabel, und Sie müssen die Anwendung nicht so konfigurieren, dass mögliche Parallelitäts Konflikte behandelt werden. (Wenn es nur wenige Benutzer oder wenige Updates gibt, oder wenn nicht wirklich wichtig ist, wenn einige Änderungen überschrieben werden, können die Kosten für die Programmierung für Parallelität den Vorteil überwiegen.) Wenn Sie sich keine Gedanken über Parallelitäts Konflikte machen müssen, können Sie dieses Tutorial überspringen. die verbleibenden beiden Tutorials in der Reihe sind nicht von etwas abhängig, das Sie in diesem erstellen.

### <a name="pessimistic-concurrency-locking"></a>Pessimistische Parallelität (Sperren)

Wenn Ihre Anwendung versehentliche Datenverluste in Parallelitätsszenarios verhindern muss, ist die Verwendung von Datenbanksperren eine Möglichkeit. Dies wird als *pessimistische*Parallelität bezeichnet. Bevor Sie zum Beispiel eine Zeile aus einer Datenbank lesen, fordern Sie eine Sperre für den schreibgeschützten Zugriff oder den Aktualisierungszugriff an. Wenn Sie eine Zeile für den Aktualisierungszugriff sperren, kann kein anderer Benutzer diese Zeile für den schreibgeschützten Zugriff oder den Aktualisierungszugriff sperren, da er eine Kopie der Daten erhalten würde, die gerade geändert werden. Wenn Sie eine Zeile für den schreibgeschützten Zugriff sperren, können andere diese Zeile ebenfalls für den schreibgeschützten Zugriff sperren, aber nicht für den Aktualisierungszugriff.

Das Verwalten von Sperren hat einige Nachteile. Es kann komplex sein, sie zu programmieren. Hierfür sind beträchtliche Ressourcen für die Datenbankverwaltung erforderlich, und es kann zu Leistungsproblemen führen, wenn die Anzahl der Benutzer einer Anwendung zunimmt (das heißt, es ist nicht gut skalierbar). Aus diesen Gründen unterstützen nicht alle Datenbankverwaltungssysteme die pessimistische Parallelität. Die Entity Framework bietet keine integrierte Unterstützung für diese, und dieses Tutorial zeigt Ihnen nicht, wie Sie Sie implementieren können.

### <a name="optimistic-concurrency"></a>Optimistische Nebenläufigkeit

Die Alternative zur pessimistischen Parallelität ist die *optimistische*Parallelität. Die Verwendung der optimistischen Parallelität bedeutet, Nebenläufigkeitskonflikte zu erlauben und entsprechend zu reagieren, wenn diese auftreten. John führt z. b. die Seite *Department. aspx* aus, klickt auf den Link **Bearbeiten** für die Verlaufs Abteilung und reduziert den **Budget** Betrag von $1.000.000,00 auf $125.000,00. (John verwaltet eine konkurrierende Abteilung und möchte Geld für seine eigene Abteilung freigeben.)

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Bevor John auf **Update**klickt, führt Jane dieselbe Seite aus, klickt auf den Link **Bearbeiten** für die Verlaufs Abteilung und ändert dann das Feld **Start Datum** von 1/10/2011 in 1/1/1999. (Jane verwaltet die Verlaufs Abteilung und möchte ihr eine höhere Rangfolge einräumen.)

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John klickt zuerst auf **Aktualisieren** und dann auf **Aktualisieren**. Der Browser von Jane listet jetzt den **Budget** Betrag als $1.000.000,00 auf, aber dies ist falsch, da der Betrag von John in $125.000,00 geändert wurde.

In diesem Szenario können Sie beispielsweise folgende Aktionen ausführen:

- Sie können nachverfolgen, welche Eigenschaft ein Benutzer geändert hat und nur die entsprechenden Spalten in der Datenbank aktualisieren. Im Beispielszenario würden keine Daten verloren gehen, da verschiedene Eigenschaften von zwei Benutzern aktualisiert wurden. Wenn das nächste Mal die Verlaufs Abteilung durchsucht, werden 1/1/1999 und $125.000,00 angezeigt. 

    Dies ist das Standardverhalten im Entity Framework, das die Anzahl der Konflikte erheblich reduzieren kann, die zu Datenverlusten führen können. Dieses Verhalten vermeidet jedoch keinen Datenverlust, wenn an derselben Eigenschaft einer Entität konkurrierende Änderungen vorgenommen werden. Außerdem ist dieses Verhalten nicht immer möglich. Wenn Sie gespeicherte Prozeduren einem Entitätstyp zuordnen, werden alle Eigenschaften einer Entität aktualisiert, wenn Änderungen an der Entität in der Datenbank vorgenommen werden.
- Sie können die Änderung von John überschreiben. Nachdem Andrea auf " **Aktualisieren**" geklickt hat, wird der **Budget** Betrag auf $1.000.000,00 zurückgeleitet. Das ist entweder ein *Client gewinnt*- oder ein *Letzter Schreiber gewinnt*-Szenario. (Die Client Werte haben Vorrang vor dem, was im Datenspeicher liegt.)
- Sie können verhindern, dass die Änderungen von Jane in der Datenbank aktualisiert werden. Normalerweise würden Sie eine Fehlermeldung anzeigen, den aktuellen Zustand der Daten anzeigen und zulassen, dass die Änderungen erneut eingegeben werden, wenn Sie Sie noch erstellen möchten. Sie können den Prozess weiter automatisieren, indem Sie die Eingabe speichern und Ihnen die Möglichkeit geben, Sie erneut anzuwenden, ohne Sie erneut eingeben zu müssen. Dieses Szenario wird *Store Wins* (Speicher gewinnt) genannt. (Die Werte des Datenspeichers haben Vorrang vor den Werten, die vom Client gesendet werden.)

### <a name="detecting-concurrency-conflicts"></a>Erkennen von Parallelitäts Konflikten

Im Entity Framework können Sie Konflikte lösen, indem Sie `OptimisticConcurrencyException` Ausnahmen behandeln, die vom Entity Framework ausgelöst werden. Entity Framework muss dazu in der Lage sein, Konflikte zu erkennen, damit es weiß, wann diese Ausnahmen ausgelöst werden sollen. Aus diesem Grund müssen Sie die Datenbank und das Datenmodell entsprechend konfigurieren. Einige der Optionen für das Aktivieren der Konflikterkennung schließen Folgendes ein:

- Fügen Sie in der-Datenbank eine Tabellenspalte ein, die verwendet werden kann, um zu bestimmen, wann eine Zeile geändert wurde. Anschließend können Sie die Entity Framework so konfigurieren, dass Sie diese Spalte in die `Where`-Klausel von SQL `Update` oder `Delete`-Befehlen einschließt.

    Dies ist der Zweck der `Timestamp` Spalte in der `OfficeAssignment` Tabelle.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Der Datentyp der Spalte `Timestamp` wird auch als `Timestamp`bezeichnet. Die Spalte enthält jedoch keinen Datums-oder Uhrzeitwert. Stattdessen ist der Wert eine sequenzielle Zahl, die jedes Mal erhöht wird, wenn die Zeile aktualisiert wird. In einem `Update`-oder `Delete`-Befehl enthält die `Where`-Klausel den ursprünglichen `Timestamp` Wert. Wenn die zu Aktualisier Ende Zeile von einem anderen Benutzer geändert wurde, unterscheidet sich der Wert in `Timestamp` vom ursprünglichen Wert, sodass die `Where`-Klausel keine zu Aktualisier Ende Zeile zurückgibt. Wenn das Entity Framework feststellt, dass keine Zeilen mit dem aktuellen `Update` oder `Delete` Befehl aktualisiert wurden (d. h., wenn die Anzahl der betroffenen Zeilen 0 (null) ist), wird dies als Parallelitäts Konflikt interpretiert.
- Konfigurieren Sie die Entity Framework, um die ursprünglichen Werte jeder Spalte in der Tabelle in der `Where`-Klausel der Befehle `Update` und `Delete` einzuschließen.

    Wie bei der ersten Option gibt die `Where`-Klausel keine zu Aktualisier Ende Zeile zurück, wenn in der Zeile nach dem ersten Lesen der Zeile Änderungen vorgenommen wurden, die der Entity Framework als Parallelitäts Konflikt interpretiert. Diese Methode ist so effektiv wie die Verwendung eines `Timestamp` Felds, kann jedoch ineffizient sein. Bei Datenbanktabellen, die viele Spalten enthalten, kann dies zu sehr großen `Where` Klauseln führen, und in einer Webanwendung kann es erforderlich sein, dass Sie große Mengen an Zuständen aufbewahren. Die Verwaltung großer Zustands Mengen kann die Leistung der Anwendung beeinträchtigen, da Sie entweder Server Ressourcen (z. b. den Sitzungszustand) erfordert oder in der Webseite selbst enthalten sein muss (z. b. Ansichts Zustand).

In diesem Tutorial fügen Sie eine Fehlerbehandlung für Konflikte mit optimistischen Parallelität für eine Entität hinzu, die keine nach Verfolgungs Eigenschaft (die `Department` Entität) und eine Entität mit einer nach Verfolgungs Eigenschaft (der `OfficeAssignment` Entität) besitzt.

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Verarbeiten von optimistischer Parallelität ohne Überwachungs Eigenschaft

Zum Implementieren der vollständigen Parallelität für die `Department`-Entität, die über keine Überwachungs Eigenschaft (`Timestamp`) verfügt, führen Sie die folgenden Aufgaben aus:

- Ändern Sie das Datenmodell, um die Parallelitäts Verfolgung für `Department` Entitäten zu aktivieren.
- Behandeln Sie in der `SchoolRepository`-Klasse Parallelitäts Ausnahmen in der `SaveChanges`-Methode.
- Behandeln Sie Parallelitäts Ausnahmen auf der Seite " *Departments. aspx* ", indem Sie eine Meldung an die Benutzerwarnung anzeigen, dass die versuchten Änderungen nicht erfolgreich waren. Der Benutzer kann dann die aktuellen Werte sehen und die Änderungen wiederholen, wenn Sie noch benötigt werden.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Aktivieren der Parallelitäts Verfolgung im Datenmodell

Öffnen Sie in Visual Studio die Webanwendung "Web-App für die Internet-app", mit der Sie im vorherigen Tutorial dieser Reihe gearbeitet haben.

Öffnen Sie *School Model. edmx*, und klicken Sie im Datenmodell-Designer mit der rechten Maustaste auf die `Name`-Eigenschaft in der `Department` Entität, und klicken Sie dann auf **Eigenschaften**. Ändern Sie im **Eigenschaften** Fenster die `ConcurrencyMode`-Eigenschaft in `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Führen Sie die gleichen Schritte für die anderen skalaren Eigenschaften aus, die keine Primärschlüssel sind (`Budget`, `StartDate`und `Administrator`.) (Dies ist für Navigations Eigenschaften nicht möglich.) Dies gibt an, dass die Spalten (mit ursprünglichen Werten) in der `Where`-Klausel enthalten sein müssen, wenn die Entity Framework eine `Update` oder `Delete` SQL-Befehl generiert, um die `Department` Entität in der Datenbank zu aktualisieren. Wenn bei der Ausführung des `Update`-oder `Delete` Befehls keine Zeile gefunden wird, löst der Entity Framework eine Ausnahme der vollständigen Parallelität aus.

Speichern und schließen Sie das Datenmodell.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Behandeln von Parallelitäts Ausnahmen in der Dal

Öffnen Sie *SchoolRepository.cs* , und fügen Sie die folgende `using`-Anweisung für den `System.Data`-Namespace hinzu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Fügen Sie die folgende neue `SaveChanges`-Methode hinzu, die Ausnahmen bei der vollständigen Parallelität behandelt:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Wenn beim Aufrufen dieser Methode ein Parallelitäts Fehler auftritt, werden die Eigenschaftswerte der Entität im Arbeitsspeicher durch die aktuell in der Datenbank vorhandenen Werte ersetzt. Die Parallelitäts Ausnahme wird erneut ausgelöst, sodass Sie von der Webseite behandelt werden kann.

Ersetzen Sie in den Methoden `DeleteDepartment` und `UpdateDepartment` den vorhandenen Aufruf von `context.SaveChanges()` durch einen Aufruf von `SaveChanges()`, um die neue Methode aufzurufen.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Behandeln von Parallelitäts Ausnahmen in der Darstellungs Schicht

Öffnen Sie " *Departments. aspx* ", und fügen Sie dem `DepartmentsObjectDataSource` Steuerelement ein `OnDeleted="DepartmentsObjectDataSource_Deleted"` Attribut hinzu. Das öffnende Tag für das-Steuerelement ähnelt nun dem folgenden Beispiel.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Geben Sie im `DepartmentsGridView`-Steuerelement alle Tabellen Spalten im `DataKeyNames`-Attribut an, wie im folgenden Beispiel gezeigt. Beachten Sie, dass dadurch sehr große Ansichts Zustands Felder erstellt werden. Dies ist ein Grund, warum die Verwendung eines nach Verfolgungs Felds im Allgemeinen die bevorzugte Methode zum Nachverfolgen von Parallelitäts Konflikten ist.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Öffnen Sie *Departments.aspx.cs* , und fügen Sie die folgende `using`-Anweisung für den `System.Data`-Namespace hinzu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Fügen Sie die folgende neue Methode hinzu, die Sie aus dem `Updated` des Datenquellen-Steuer Elements und `Deleted` Ereignis Handlern für die Behandlung von Parallelitäts Ausnahmen aufzurufen:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Dieser Code überprüft den Ausnahmetyp. wenn es sich um eine Parallelitäts Ausnahme handelt, erstellt der Code dynamisch ein `CustomValidator` Steuerelement, das wiederum eine Meldung im `ValidationSummary` Steuerelement anzeigt.

Nennen Sie die neue Methode aus dem `Updated` Ereignishandler, den Sie zuvor hinzugefügt haben. Erstellen Sie außerdem einen neuen `Deleted` Ereignishandler, der dieselbe Methode aufruft (aber nichts anderes tut):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Testen optimistischer Parallelität auf der Seite "Abteilungen"

Führen Sie die Seite " *Departments. aspx* " aus.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Klicken Sie in einer Zeile auf **Bearbeiten** , und ändern Sie den Wert in der Spalte **Budget** . (Denken Sie daran, dass Sie nur die Datensätze bearbeiten können, die Sie für dieses Tutorial erstellt haben, da die vorhandenen `School` Datenbankdaten Sätze ungültige Daten enthalten. Der Datensatz für die Wirtschaftsabteilung ist ein sicherer, mit dem Sie experimentieren können.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Öffnen Sie ein neues Browserfenster, und führen Sie die Seite erneut aus (Kopieren Sie die URL aus dem Adressfeld des ersten Browserfensters in das zweite Browserfenster).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Klicken Sie in derselben Zeile, die Sie zuvor bearbeitet haben, auf **Bearbeiten** , und ändern Sie den Wert für **Budget** in etwas anderes

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Klicken Sie im zweiten Browserfenster auf **Aktualisieren**. Der **Budget** Betrag wurde erfolgreich auf diesen neuen Wert geändert.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Klicken Sie im ersten Browserfenster auf **Aktualisieren**. Die Aktualisierung schlägt fehl. Der **Budget** Betrag wird mit dem Wert, den Sie im zweiten Browserfenster festgelegt haben, erneut angezeigt, und es wird eine Fehlermeldung angezeigt.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Verarbeiten von optimistischer Parallelität mithilfe einer nach Verfolgungs Eigenschaft

Zum Verarbeiten der vollständigen Parallelität für eine Entität, die über eine Überwachungs Eigenschaft verfügt, führen Sie die folgenden Aufgaben aus:

- Fügen Sie dem Datenmodell gespeicherte Prozeduren hinzu, um `OfficeAssignment` Entitäten zu verwalten. (Überwachungs Eigenschaften und gespeicherte Prozeduren müssen nicht zusammen verwendet werden. Sie werden hier nur zur Veranschaulichung zusammengefasst.)
- Fügen Sie die CRUD-Methoden der dal und der BLL für `OfficeAssignment` Entitäten hinzu, einschließlich Code zum Behandeln von Ausnahmen in der vollständigen Parallelität in der DAL.
- Erstellen einer Webseite für Office-Zuweisungen.
- Testen Sie die optimistische Parallelität auf der neuen Webseite.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Hinzufügen von officezuweisung-gespeicherten Prozeduren zum Datenmodell

Öffnen Sie die Datei " *SchoolModel. edmx* " im Modell-Designer, klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und klicken Sie auf **Modell aus Datenbank aktualisieren**. Erweitern Sie im Dialogfeld **Wählen Sie Ihre Datenbankobjekte** auf der Registerkarte **Hinzufügen** die Option **gespeicherte Prozeduren** , und wählen Sie die drei gespeicherten Prozeduren `OfficeAssignment` aus (siehe den folgenden Screenshot **).** (Diese gespeicherten Prozeduren waren bereits in der Datenbank vorhanden, als Sie Sie mithilfe eines Skripts heruntergeladen oder erstellt haben.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Klicken Sie mit der rechten Maustaste auf die `OfficeAssignment` Entität, und wählen Sie **Zuordnung der gespeicherten**

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Legen Sie die Funktionen **Insert**, **Update**und **Delete** so fest, dass Sie die entsprechenden gespeicherten Prozeduren verwenden. Legen Sie für den `OrigTimestamp`-Parameter der `Update`-Funktion die- **Eigenschaft** auf `Timestamp` fest, und wählen Sie die Option **ursprünglichen Wert verwenden** aus.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Wenn das Entity Framework die gespeicherte Prozedur `UpdateOfficeAssignment` aufruft, übergibt es den ursprünglichen Wert der Spalte `Timestamp` im `OrigTimestamp`-Parameter. Die gespeicherte Prozedur verwendet diesen Parameter in der `Where`-Klausel:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

Die gespeicherte Prozedur wählt auch den neuen Wert der `Timestamp` Spalte nach dem Update aus, damit die Entity Framework die `OfficeAssignment` Entität, die sich im Arbeitsspeicher befindet, mit der entsprechenden Datenbankzeile synchron halten können.

(Beachten Sie, dass die gespeicherte Prozedur zum Löschen einer Office-Zuweisung nicht über einen `OrigTimestamp`-Parameter verfügt. Aus diesem Grund kann der Entity Framework nicht überprüfen, ob eine Entität vor dem Löschen unverändert ist.)

Speichern und schließen Sie das Datenmodell.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Hinzufügen von officezuweisungs Methoden zur Dal

Öffnen Sie *ISchoolRepository.cs* , und fügen Sie die folgenden CRUD-Methoden für die `OfficeAssignment` Entitätenmenge hinzu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Fügen Sie *SchoolRepository.cs*die folgenden neuen Methoden hinzu. In der `UpdateOfficeAssignment`-Methode wird die lokale `SaveChanges`-Methode anstelle von `context.SaveChanges`aufgerufen.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Öffnen Sie *MockSchoolRepository.cs* im Testprojekt, und fügen Sie ihm die folgenden `OfficeAssignment` Collection-und CRUD-Methoden hinzu. (Das mockrepository muss die Repository-Schnittstelle implementieren, oder die Lösung wird nicht kompiliert.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Hinzufügen von officezuweisungs Methoden zur BLL

Öffnen Sie im Hauptprojekt *SchoolBL.cs* , und fügen Sie die folgenden CRUD-Methoden für die `OfficeAssignment` Entitätenmenge hinzu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Erstellen einer officezuweisungen-Webseite

Erstellen Sie eine neue Webseite, die die Master Seite *Site. Master* verwendet, und nennen Sie Sie *officezuweisungen. aspx*. Fügen Sie das folgende Markup dem `Content`-Steuerelement mit dem Namen `Content2`hinzu:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Beachten Sie, dass im `DataKeyNames`-Attribut das Markup die `Timestamp`-Eigenschaft und den Daten Satz Schlüssel (`InstructorID`) angibt. Das Angeben von Eigenschaften im `DataKeyNames`-Attribut bewirkt, dass das Steuerelement diese im Steuerelement Zustand (ähnlich dem Ansichts Zustand) speichert, sodass die ursprünglichen Werte während der Post Back Verarbeitung verfügbar sind.

Wenn Sie den `Timestamp` Wert nicht gespeichert haben, hat der Entity Framework ihn nicht für die `Where`-Klausel des SQL-`Update` Befehls. Folglich würde nichts zu aktualisieren gefunden werden. Folglich würde der Entity Framework jedes Mal, wenn eine `OfficeAssignment` Entität aktualisiert wird, eine vollständige Parallelitäts Ausnahme auslösen.

Öffnen Sie *OfficeAssignments.aspx.cs* , und fügen Sie die folgende `using`-Anweisung für die Datenzugriffs Ebene hinzu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Fügen Sie die folgende `Page_Init`-Methode hinzu, die dynamische Daten Funktionalität ermöglicht. Fügen Sie außerdem den folgenden Handler für das `Updated` Ereignis des `ObjectDataSource`-Steuer Elements hinzu, um nach Parallelitäts Fehlern zu suchen:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Testen der optimistischen Parallelität auf der Seite "officezuweisungen"

Führen Sie die Seite *officezuweisungen. aspx* aus.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Klicken Sie in einer Zeile auf **Bearbeiten** , und ändern Sie den Wert in der Spalte **Speicherort** .

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Öffnen Sie ein neues Browserfenster, und führen Sie die Seite erneut aus (Kopieren Sie die URL aus dem ersten Browserfenster in das zweite Browserfenster).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Klicken Sie in derselben Zeile, die Sie zuvor bearbeitet haben, auf **Bearbeiten** , und ändern Sie den **Speicherort** Wert in einen anderen

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

Klicken Sie im zweiten Browserfenster auf **Aktualisieren**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Wechseln Sie zum ersten Browserfenster, und klicken Sie auf **Aktualisieren**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Es wird eine Fehlermeldung angezeigt, und der **Location** -Wert wurde aktualisiert, um den Wert anzuzeigen, den Sie im zweiten Browserfenster geändert haben.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Behandeln von Parallelität mit dem EntityDataSource-Steuerelement

Das `EntityDataSource` Steuerelement enthält integrierte Logik, die die Parallelitäts Einstellungen im Datenmodell erkennt und Aktualisierungs-und Löschvorgänge entsprechend behandelt. Wie bei allen Ausnahmen müssen Sie jedoch `OptimisticConcurrencyException` Ausnahmen selbst behandeln, um eine benutzerfreundliche Fehlermeldung bereitzustellen.

Als Nächstes konfigurieren Sie die Seite " *Courses. aspx* " (die ein `EntityDataSource`-Steuerelement verwendet), um Aktualisierungs-und Löschvorgänge zuzulassen und eine Fehlermeldung anzuzeigen, wenn ein Parallelitäts Konflikt auftritt. Die `Course`-Entität verfügt nicht über eine Spalte für die Parallelitäts Nachverfolgung, daher verwenden Sie dieselbe Methode wie für die `Department`-Entität: Nachverfolgen der Werte aller nicht Schlüsseleigenschaften.

Öffnen Sie die Datei " *SchoolModel. edmx* ". Legen Sie für die nicht Schlüsseleigenschaften der `Course` Entität (`Title`, `Credits`und `DepartmentID`) die Eigenschaft Parallelitäts **Modus** auf `Fixed`fest. Speichern und schließen Sie dann das Datenmodell.

Öffnen Sie die Seite " *Courses. aspx* ", und nehmen Sie die folgenden Änderungen vor:

- Fügen Sie im `CoursesEntityDataSource`-Steuerelement die Attribute `EnableUpdate="true"` und `EnableDelete="true"` hinzu. Das öffnende Tag für dieses Steuerelement ähnelt nun dem folgenden Beispiel:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- Ändern Sie im `CoursesGridView` Steuerelement den Wert des `DataKeyNames` Attributs in `"CourseID,Title,Credits,DepartmentID"`. Fügen Sie dem `Columns` Element, das die Schaltflächen **Bearbeiten** und **Löschen** anzeigt (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`), ein `CommandField` Element hinzu. Das `GridView`-Steuerelement ähnelt nun dem folgenden Beispiel:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Führen Sie die Seite aus, und erstellen Sie wie zuvor auf der Seite "Abteilungen" eine Konfliktsituation. Führen Sie die Seite in zwei Browserfenstern aus, klicken Sie in jedem Fenster in derselben Zeile auf **Bearbeiten** , und nehmen Sie jeweils eine andere Änderung vor. Klicken Sie in einem Fenster auf **Aktualisieren** , und klicken Sie dann im anderen Fenster auf **Aktualisieren** . Wenn Sie zum zweiten Mal auf **Aktualisieren** klicken, wird die Fehlerseite angezeigt, die sich aus einer nicht behandelten Parallelitäts Ausnahme ergibt.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Dieser Fehler wird so behandelt, wie Sie ihn für das `ObjectDataSource` Steuerelement behandelt haben. Öffnen Sie die Seite " *Courses. aspx* ", und geben Sie im `CoursesEntityDataSource`-Steuerelement Handler für die `Deleted`-und `Updated`-Ereignisse an. Das öffnende Tag des-Steuer Elements ähnelt nun dem folgenden Beispiel:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Fügen Sie vor dem `CoursesGridView`-Steuerelement das folgende `ValidationSummary` Steuerelement hinzu:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

Fügen Sie in *Courses.aspx.cs*eine `using`-Anweisung für den `System.Data`-Namespace hinzu, fügen Sie eine Methode hinzu, die auf neben läufigkeits Ausnahmen prüft, und fügen Sie Handler für die `Updated`-und `Deleted` Handler des `EntityDataSource`-Steuer Elements hinzu. Der Code sieht wie folgt aus:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

Der einzige Unterschied zwischen diesem Code und dem `ObjectDataSource` Steuerelement besteht darin, dass in diesem Fall die Parallelitäts Ausnahme in der `Exception`-Eigenschaft des Ereignis arguments-Objekts statt in der `InnerException`-Eigenschaft dieser Ausnahme liegt.

Führen Sie die Seite aus, und erstellen Sie einen Parallelitäts Konflikt erneut. Dieses Mal wird eine Fehlermeldung angezeigt:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Damit ist die Einführung in die Behandlung von Nebenläufigkeitskonflikten abgeschlossen. Im nächsten Tutorial finden Sie Anleitungen zur Verbesserung der Leistung in einer Webanwendung, die die Entity Framework verwendet.

> [!div class="step-by-step"]
> [Zurück](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [Weiter](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
