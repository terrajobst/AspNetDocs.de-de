---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: 'Tutorial: Implementieren von CRUD-Funktionen mit dem Entity Framework in ASP.NET MVC | Microsoft-Dokumentation'
description: Überprüfen und Anpassen Sie den Code zum Erstellen, lesen, aktualisieren und löschen (CRUD), den der MVC-Gerüstbau automatisch in Controllern und Ansichten erstellt.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9ed388543dd54d209ff2a0b92df4f7659962582c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471147"
---
# <a name="tutorial-implement-crud-functionality-with-the-entity-framework-in-aspnet-mvc"></a>Tutorial: Implementieren von CRUD-Funktionen mit dem Entity Framework in ASP.NET MVC

Im [vorherigen Tutorial](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)haben Sie eine MVC-Anwendung erstellt, die Daten mit dem Entity Framework (EF) 6 und SQL Server localdb speichert und anzeigt. In diesem Tutorial überprüfen und passen Sie den Code zum Erstellen, lesen, aktualisieren und löschen (CRUD) an, den der MVC-Gerüstbau automatisch für Controller und Ansichten erstellt.

> [!NOTE]
> Es ist üblich, dass das Repositorymuster implementiert wird, um eine Abstraktionsebene zwischen Ihrem Controller und der Datenzugriffsebene zu erstellen. Um diese Lernprogramme auf dem neuesten Stand zu halten und die Verwendung von EF 6 selbst zu unterhalten, verwenden Sie keine Depots. Informationen zum Implementieren von Repository finden Sie unter [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

Hier finden Sie Beispiele für die von Ihnen erstellten Webseiten:

![Screenshot der Seite "Student Details".](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Screenshot der Seite "Student erstellen".](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Screenshot der Seite "Student Delete"](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Seite "Details erstellen"
> * Aktualisieren der Seite „Erstellen“
> * Aktualisieren der HttpPost-Bearbeitungsmethode
> * Aktualisieren der Seite „Delete“ (Löschen)
> * Schließen von Datenbankverbindungen
> * Behandeln von Transaktionen

## <a name="prerequisites"></a>Voraussetzungen

* [Erstellen des Entity Framework Datenmodells](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

## <a name="create-a-details-page"></a>Seite "Details erstellen"

Der Gerüst Code für die `Index` Seite "Students" hat die `Enrollments`-Eigenschaft ausgelassen, da diese Eigenschaft eine Auflistung enthält. Auf der Seite `Details` zeigen Sie den Inhalt der Auflistung in einer HTML-Tabelle an.

In *controllers\studentcontroller.cs*verwendet die Aktionsmethode für die `Details` Ansicht die [Find](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) -Methode, um eine einzelne `Student` Entität abzurufen.

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Der Schlüsselwert wird als `id`-Parameter an die-Methode übergeben und stammt aus den *Routendaten* im Link **Details** auf der Index Seite.

### <a name="tip-route-data"></a>Tipp: **Routendaten**

Bei den Routendaten handelt es sich um Daten, die der Modell Binder in einem in der Routing Tabelle angegebenen URL-Segment gefunden hat. Die Standardroute gibt beispielsweise `controller`, `action`und `id` Segmente an:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

In der folgenden URL wird die Standardroute `Instructor` als `controller``Index` als `action` und 1 als `id`zugeordnet. Dabei handelt es sich um Routendaten Werte.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

`?courseID=2021` ist ein Abfrage Zeichen folgen Wert. Der Modell Binder funktioniert auch, wenn Sie die `id` als Abfrage Zeichenfolgen-Wert übergeben:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Die URLs werden von `ActionLink`-Anweisungen in der Razor-Ansicht erstellt. Im folgenden Code entspricht der `id`-Parameter der Standardroute, sodass `id` den Routendaten hinzugefügt wird.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

Im folgenden Code entspricht `courseID` nicht einem Parameter in der Standardroute, daher wird er als Abfrage Zeichenfolge hinzugefügt.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]

### <a name="to-create-the-details-page"></a>So erstellen Sie die Detailseite

1. Öffnen Sie *views\student\details.cshtml*.

   Jedes Feld wird mit einem `DisplayFor`-Hilfsprogramm angezeigt, wie im folgenden Beispiel gezeigt:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]

2. Fügen Sie nach dem `EnrollmentDate`-Feld und unmittelbar vor dem schließenden `</dl>`-Tag den hervorgehobenen Code hinzu, um eine Liste der Registrierungen anzuzeigen, wie im folgenden Beispiel gezeigt:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Wenn der Code Einzug nach dem Einfügen des Codes falsch ist, drücken Sie **STRG**+**K**, **STRG**+**D** , um ihn zu formatieren.

    Dieser Code durchläuft die Entitäten in der Navigationseigenschaft `Enrollments`. Für jede `Enrollment` Entität in der Eigenschaft werden der Kurs Titel und die Qualität angezeigt. Der Kurstitel wird aus der `Course` Entität abgerufen, die in der `Course`-Navigations Eigenschaft der `Enrollments`-Entität gespeichert ist. Alle diese Daten werden automatisch aus der Datenbank abgerufen, wenn Sie benötigt werden. Anders ausgedrückt: Sie verwenden hier Lazy Loading. Sie haben keine *Eager Loading* für die `Courses` Navigations Eigenschaft angegeben, sodass die Registrierungen nicht in derselben Abfrage abgerufen wurden, die die Schüler/Studenten erhalten hat. Wenn Sie zum ersten Mal versuchen, auf die `Enrollments` Navigations Eigenschaft zuzugreifen, wird stattdessen eine neue Abfrage an die Datenbank gesendet, um die Daten abzurufen. Weitere Informationen zu Lazy Loading und Eager Loading finden Sie im Tutorial [Lesen verwandter Daten](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) weiter unten in dieser Reihe.

3. Öffnen Sie die Seite "Details", indem Sie das Programm starten (**STRG**+**F5**), die Registerkarte " **Students** " auswählen und dann auf den Link **Details** für Alexander Carson klicken. (Wenn Sie **STRG**+**F5** drücken, während die Datei " *Details. cshtml* " geöffnet ist, erhalten Sie einen HTTP 400-Fehler. Dies liegt daran, dass Visual Studio versucht, die Detailseite auszuführen, aber nicht über einen Link erreicht wurde, der den anzuzeigenden Studenten angibt. Wenn dies der Fall ist, entfernen Sie "Student/Details" aus der URL, und versuchen Sie es erneut, oder schließen Sie den Browser, klicken Sie mit der rechten Maustaste auf das Projekt, und klicken Sie auf > **Ansicht im Browser** **anzeigen** .)

    Die Liste der Kurse und Noten für den ausgewählten Studenten wird angezeigt.

4. Schließen Sie den Browser.

## <a name="update-the-create-page"></a>Aktualisieren der Seite „Erstellen“

1. Ersetzen Sie in *controllers\studentcontroller.cs*die <xref:System.Web.Mvc.HttpPostAttribute> `Create` Action-Methode durch den folgenden Code. Mit diesem Code wird ein `try-catch`-Block hinzugefügt und `ID` aus dem <xref:System.Web.Mvc.BindAttribute>-Attribut für die Gerüst Methode entfernt:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Mit diesem Code wird dem `Students` Entitätenmenge die `Student` Entität hinzugefügt, die vom ASP.NET MVC-Modell Binder erstellt wurde, und die Änderungen werden dann in der Datenbank gespeichert. *Modell Binder* bezieht sich auf die ASP.NET MVC-Funktionalität, die Ihnen das Arbeiten mit Daten erleichtert, die von einem Formular übermittelt werden. ein Modell Binder konvertiert die übermittelten Formular Werte in CLR-Typen und übergibt sie in den Parametern an die Aktionsmethode. In diesem Fall instanziiert der Modell Binder eine `Student` Entität für Sie mithilfe von Eigenschafts Werten aus der `Form` Auflistung.

    Sie haben `ID` aus dem Attribut bind entfernt, da `ID` der Primärschlüssel Wert ist, der SQL Server beim Einfügen der Zeile automatisch festgelegt wird. Die Eingabe des Benutzers legt den `ID` Wert nicht fest.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgery-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Sicherheitswarnung: das `ValidateAntiForgeryToken`-Attribut trägt dazu bei, [Website übergreifende Anforderungs Fälschungs](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) Angriffe zu verhindern. Hierfür ist eine entsprechende `Html.AntiForgeryToken()`-Anweisung in der Sicht erforderlich, die Sie später sehen werden.

    Das `Bind`-Attribut ist eine Möglichkeit zum Schutz vor *overposting* in Erstellungs Szenarien. Nehmen Sie beispielsweise an, dass die `Student`-Entität eine `Secret` Eigenschaft enthält, die von dieser Webseite nicht festgelegt werden soll.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Auch wenn Sie nicht über ein `Secret` Feld auf der Webseite verfügen, könnte ein Hacker ein Tool wie z. b. " [fddler](http://fiddler2.com/home)" verwenden oder JavaScript schreiben, um einen `Secret` Formular Wert zu veröffentlichen. Ohne das <xref:System.Web.Mvc.BindAttribute>-Attribut, das die Felder einschränkt, die die Modell Bindung verwendet, wenn Sie eine `Student` Instanz erstellt<em>,</em> übernimmt der Modell Binder diesen `Secret` Formular Wert und verwendet ihn zum Erstellen der `Student` Entitäts Instanz. Dann würde jeder beliebige Wert in Ihre Datenbank aktualisiert werden, den der Hacker für das Formularfeld `Secret` festlegt. In der folgenden Abbildung wird das Tool "tddler" gezeigt, das das `Secret` Feld (mit dem Wert "overpost") den bereit gemittelten Formular Werten hinzufügt.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

    Der Wert „OverPost“ würde dann erfolgreich der Eigenschaft `Secret` der eingefügten Zeile hinzugefügt werden, obwohl Sie nie beabsichtigt haben, dass die Webseite diese Eigenschaft festlegen kann.

    Es empfiehlt sich, den `Include`-Parameter mit dem `Bind`-Attribut zu verwenden, um Felder in die *Whitelist* aufzunehmen. Es ist auch möglich, den `Exclude`-Parameter für die *Blacklist* von Feldern zu verwenden, die Sie ausschließen möchten. Der Grund `Include` sicherer ist, wenn Sie der Entität eine neue Eigenschaft hinzufügen, wird das neue Feld nicht automatisch durch eine `Exclude` Liste geschützt.

    Sie können das Überschreiben in Bearbeitungs Szenarios verhindern, indem Sie zuerst die Entität aus der Datenbank lesen und dann `TryUpdateModel`aufrufen und dabei eine explizit zulässige Eigenschaften Liste übergeben. Dies ist die in diesen Tutorials verwendete Methode.

    Eine alternative Möglichkeit zum Verhindern von über schreibungen, die von vielen Entwicklern bevorzugt wird, ist die Verwendung von Ansichts Modellen anstelle von Entitäts Klassen mit Modell Bindung. Schließen Sie nur die Eigenschaften in dem Ansichtsmodell ein, die Sie aktualisieren möchten. Nachdem die MVC-Modell Bindung abgeschlossen ist, kopieren Sie die Ansichts Modell Eigenschaften in die Entitäts Instanz, und verwenden Sie optional ein Tool wie [AutoMapper](http://automapper.org/). Verwenden Sie die Datenbank. Eintrag für die Entitäts Instanz, um den Status unverändert festzulegen, und legen Sie dann die Eigenschaft ("PropertyName") fest. IsModified ist für jede Entitäts Eigenschaft, die im Ansichts Modell enthalten ist, auf true fest. Diese Methode funktioniert sowohl im Bearbeitungsszenario als auch im Erstellungsszenario.

    Außer dem `Bind`-Attribut ist der `try-catch`-Block die einzige Änderung, die Sie an dem mit dem Gerüst erstellten Code vorgenommen haben. Wenn eine Ausnahme abgefangen wird, die von <xref:System.Data.DataException> abgeleitet wird, während die Änderungen gespeichert werden, wird eine generische Fehlermeldung angezeigt. <xref:System.Data.DataException>-Ausnahmen werden manchmal durch etwas außerhalb der Anwendung ausgelöst, und nicht durch einen Programmierfehler. Es wird empfohlen, dass der Benutzer es erneut versucht. Zwar wird es in diesem Beispiel nicht implementiert, aber eine qualitätsorientierte Produktionsanwendung würde die Ausnahme protokollieren. Weitere Informationen finden Sie im Abschnitt **Log for insight (Einblicke durch Protokollierung)** im Artikel [Monitoring and Telemetry (Building Real-World Cloud Apps with Azure) (Überwachung und Telemetrie (Erstellen von realitätsnahen Cloud-Apps mit Azure))](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Der Code in " *views\student\kreate.cshtml* " ähnelt dem, was Sie in der Datei " *Details. cshtml*" gesehen haben, mit dem Unterschied, dass für jedes Feld anstelle von `DisplayFor``EditorFor`-und `ValidationMessageFor` Hilfsprogramme verwendet werden. Hier ist der entsprechende Code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create. cshtml* enthält auch `@Html.AntiForgeryToken()`, das mit dem `ValidateAntiForgeryToken`-Attribut im Controller zusammenarbeitet, um [Website übergreifende Anforderungs Fälschungs](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) Angriffe zu verhindern.

    In *Create. cshtml*sind keine Änderungen erforderlich.

2. Führen Sie die Seite Durchstarten des Programms aus, wählen Sie die Registerkarte **Studenten** aus, und klicken Sie anschließend auf **neu erstellen**.

3. Geben Sie Namen und ein ungültiges Datum ein, und klicken Sie auf **Erstellen** , um die Fehlermeldung anzuzeigen.

    Dabei handelt es sich um eine serverseitige Validierung, die Sie standardmäßig erhalten. In einem späteren Tutorial erfahren Sie, wie Sie Attribute hinzufügen, die Code für die Client seitige Validierung generieren. Der folgende hervorgehobene Code zeigt die Überprüfung der Modell Validierung in der **Create** -Methode.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]

4. Ändern Sie das Datum in einen gültigen Wert und klicken auf **Erstellen**, damit der neue Student auf der Seite **Index** angezeigt wird.

5. Schließen Sie den Browser.

## <a name="update-httppost-edit-method"></a>HttpPost-Bearbeitungsmethode aktualisieren

1. Ersetzen Sie die <xref:System.Web.Mvc.HttpPostAttribute> `Edit` Aktionsmethode durch den folgenden Code:

   [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

   > [!NOTE]
   > In *controllers\studentcontroller.cs*verwendet die `HttpGet Edit`-Methode (die-Methode ohne das `HttpPost`-Attribut) die `Find`-Methode, um die ausgewählte `Student` Entität abzurufen, wie Sie in der `Details`-Methode gesehen haben. Sie müssen diese Methode nicht ändern.

   Diese Änderungen implementieren eine bewährte Sicherheitsmaßnahme, um [Überschreitungen](#overpost)zu verhindern. das Gerüst hat ein `Bind` Attribut generiert und die Entität, die vom Modell Binder erstellt wurde, der Entitätenmenge mit einem geänderten Flag hinzugefügt. Der Code wird nicht mehr empfohlen, da das `Bind`-Attribut alle bereits vorhandenen Daten in Feldern löscht, die nicht im `Include`-Parameter aufgeführt sind. In Zukunft wird das MVC-Controller Gerüst so aktualisiert, dass keine `Bind` Attribute für Bearbeitungsmethoden generiert werden.

   Der neue Code liest die vorhandene Entität und ruft <xref:System.Web.Mvc.Controller.TryUpdateModel%2A> auf, um Felder aus Benutzereingaben in den veröffentlichten Formulardaten zu aktualisieren. Die automatische Änderungs Nachverfolgung des Entity Framework legt das Flag " [EntityState. modified](<xref:System.Data.EntityState.Modified>) " für die Entität fest. Wenn die [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) -Methode aufgerufen wird, bewirkt das <xref:System.Data.EntityState.Modified>-Flag, dass der Entity Framework SQL-Anweisungen erstellt, um die Datenbankzeile zu aktualisieren. Parallelitäts [Konflikte](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) werden ignoriert, und alle Spalten der Datenbankzeile werden aktualisiert, einschließlich derjenigen, die der Benutzer nicht geändert hat. (In einem späteren Tutorial wird gezeigt, wie Parallelitäts Konflikte behandelt werden, und wenn Sie nur einzelne Felder in der Datenbank aktualisieren möchten, können Sie die Entität auf [EntityState. unverändert](<xref:System.Data.EntityState.Unchanged>) festlegen und einzelne Felder auf [EntityState. modified](<xref:System.Data.EntityState.Modified>)festlegen.)

   Um das Überschreiben zu verhindern, werden die Felder, die Sie auf der Bearbeitungsseite aktualisieren können, in die Whitelist der `TryUpdateModel` Parameter eingefügt. Derzeit sind keine zusätzlichen von Ihnen geschützten Felder vorhanden. Wenn Sie jedoch die Felder auflisten, die die Modellbindung binden soll, stellen Sie sicher, dass zukünftig hinzugefügte Felder automatisch geschützt sind, bis Sie sie explizit hier hinzufügen.

   Infolge dieser Änderungen ist die Methoden Signatur der HttpPost-Bearbeitungsmethode mit der HttpGet-Bearbeitungsmethode identisch. Daher haben Sie die Methode "EditPost" umbenannt.

   > [!TIP]
   >
   > **Entitäts Zustände und die Attach-Methode und die SaveChanges-Methode**
   >
   > Der Datenbankkontext verfolgt, ob die Entitäten im Arbeitsspeicher mit ihren entsprechenden Zeilen in der Datenbank synchronisiert sind. Diese Information bestimmt, was passiert, wenn Sie die Methode `SaveChanges` aufrufen. Wenn Sie z. b. eine neue Entität an die [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) -Methode übergeben, wird der Zustand dieser Entität auf `Added`festgelegt. Wenn Sie dann die [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) -Methode aufzurufen, gibt der Daten Bank Kontext einen SQL `INSERT`-Befehl aus.
   >
   > Eine Entität kann einen der folgenden [Zustände](xref:System.Data.EntityState)aufweisen:
   >
   > - [https://login.microsoftonline.com/consumers/](`Added`). Die Entität ist noch nicht in der Datenbank vorhanden. Die `SaveChanges`-Methode muss eine `INSERT`-Anweisung ausgeben.
   > - [https://login.microsoftonline.com/consumers/](`Unchanged`). Die Methode `SaveChanges` muss nichts mit dieser Entität tun. Wenn Sie eine Entität aus der Datenbank lesen, beginnt die Entität mit diesem Status.
   > - [https://login.microsoftonline.com/consumers/](`Modified`). Einige oder alle Eigenschaftswerte der Entität wurden geändert. Die `SaveChanges`-Methode muss eine `UPDATE`-Anweisung ausgeben.
   > - [https://login.microsoftonline.com/consumers/](`Deleted`). Die Entität wurde zum Löschen markiert. Die `SaveChanges`-Methode muss eine `DELETE`-Anweisung ausgeben.
   > - [https://login.microsoftonline.com/consumers/](`Detached`). Die Entität wird nicht vom Datenbankkontext nachverfolgt.
   >
   > Statusänderungen werden in einer Desktop-App in der Regel automatisch festgelegt. In einem desktoptyp der Anwendung lesen Sie eine Entität und nehmen Änderungen an einigen Eigenschafts Werten vor. Dadurch wird der Entitätsstatus automatisch auf `Modified` festgelegt. Wenn Sie `SaveChanges`aufgerufen haben, generiert der Entity Framework eine SQL `UPDATE`-Anweisung, die nur die tatsächlich geänderten Eigenschaften aktualisiert.
   >
   > Die getrennte Art von Web-Apps lässt diese kontinuierliche Sequenz nicht zu. Der [dbcontext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , der eine Entität liest, wird verworfen, nachdem eine Seite gerendert wurde. Wenn die `HttpPost` `Edit` Aktionsmethode aufgerufen wird, wird eine neue Anforderung erstellt, und Sie verfügen über eine neue Instanz von [dbcontext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx). Daher müssen Sie den Entitäts Zustand manuell auf `Modified.` festlegen, wenn Sie `SaveChanges`aufrufen, aktualisiert der Entity Framework alle Spalten der Datenbankzeile, da der Kontext nicht wissen kann, welche Eigenschaften Sie geändert haben.
   >
   > Wenn Sie möchten, dass die SQL `Update`-Anweisung nur die Felder aktualisiert, die der Benutzer tatsächlich geändert hat, können Sie die ursprünglichen Werte auf irgendeine Weise speichern (z. b. ausgeblendete Felder), damit Sie verfügbar sind, wenn die `HttpPost` `Edit`-Methode aufgerufen wird. Anschließend können Sie eine `Student` Entität mit den ursprünglichen Werten erstellen, die `Attach`-Methode mit der ursprünglichen Version der Entität aufzurufen, die Werte der Entität auf die neuen Werte aktualisieren und dann `SaveChanges.` abrufen, um weitere Informationen zu erhalten. Weitere Informationen finden Sie unter [Entitäts Zustände und SaveChanges](/ef/ef6/saving/change-tracking/entity-state) und [lokale Daten](/ef/ef6/querying/local-data).

   Der HTML-und Razor-Code in *views\student\edit.cshtml* ähnelt dem, was Sie in *Create. cshtml*gesehen haben, und es sind keine Änderungen erforderlich.

2. Führen Sie die Seite Durchstarten des Programms aus, wählen Sie die Registerkarte **Studenten** aus, und klicken Sie dann auf einen Link **Bearbeiten** .

3. Ändern Sie einige der Daten, und klicken Sie auf **Speichern**. Die geänderten Daten werden auf der Index Seite angezeigt.

4. Schließen Sie den Browser.

## <a name="update-the-delete-page"></a>Aktualisieren der Seite „Delete“ (Löschen)

In *controllers\studentcontroller.cs*verwendet der Vorlagen Code für die <xref:System.Web.Mvc.HttpGetAttribute> `Delete`-Methode die `Find`-Methode, um die ausgewählte `Student` Entität abzurufen, wie Sie in den `Details`-und `Edit`-Methoden gesehen haben. Allerdings müssen Sie dieser Methode und der dazugehörigen Ansicht einige Funktionen hinzufügen, um eine benutzerdefinierte Fehlermeldung zu implementieren, wenn der Aufruf von `SaveChanges` fehlschlägt.

Wie Sie bereits bei den Vorgängen zum Aktualisieren und Erstellen gesehen haben, benötigen Löschvorgänge zwei Aktionsmethoden. Die Methode, die als Reaktion auf eine GET-Anforderung aufgerufen wird, zeigt eine Ansicht an, die dem Benutzer die Möglichkeit gibt, den Löschvorgang zu genehmigen oder abzubrechen. Wenn der Benutzer diesen Löschvorgang genehmigt, wird eine POST-Anforderung erstellt. Wenn dies der Fall ist, wird die `HttpPost` `Delete`-Methode aufgerufen, und dann führt diese Methode den Löschvorgang aus.

Fügen Sie der <xref:System.Web.Mvc.HttpPostAttribute> `Delete`-Methode einen `try-catch`-Block hinzu, um alle Fehler zu behandeln, die beim Aktualisieren der Datenbank auftreten können. Wenn ein Fehler auftritt, ruft die <xref:System.Web.Mvc.HttpPostAttribute> `Delete`-Methode die <xref:System.Web.Mvc.HttpGetAttribute> `Delete`-Methode auf und übergibt ihr einen Parameter, der angibt, dass ein Fehler aufgetreten ist. Die <xref:System.Web.Mvc.HttpGetAttribute> `Delete`-Methode zeigt dann die Bestätigungsseite und die Fehlermeldung erneut an, um dem Benutzer die Möglichkeit zu geben, ihn abzubrechen oder erneut zu versuchen.

1. Ersetzen Sie die <xref:System.Web.Mvc.HttpGetAttribute> `Delete` Aktionsmethode durch den folgenden Code, der die Fehlerberichterstattung verwaltet:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Dieser Code akzeptiert einen [optionalen Parameter](https://msdn.microsoft.com/library/dd264739.aspx) , der angibt, ob die Methode nach einem Fehler beim Speichern der Änderungen aufgerufen wurde. Dieser Parameter ist `false`, wenn die `HttpGet` `Delete`-Methode ohne vorherige Fehler aufgerufen wird. Wenn Sie von der `HttpPost` `Delete`-Methode als Reaktion auf einen Fehler bei der Datenbankaktualisierung aufgerufen wird, wird der-Parameter `true`, und eine Fehlermeldung wird an die Ansicht übergeben.

2. Ersetzen Sie die <xref:System.Web.Mvc.HttpPostAttribute> `Delete` Aktionsmethode (mit dem Namen `DeleteConfirmed`) durch den folgenden Code, der den eigentlichen Löschvorgang ausführt und alle Daten Bank Aktualisierungs Fehler abfängt.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Dieser Code Ruft die ausgewählte Entität ab und ruft dann die [Remove](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) -Methode auf, um den Status der Entität auf `Deleted`festzulegen. Wenn `SaveChanges` aufgerufen wird, wird ein SQL `DELETE`-Befehl generiert. Ebenfalls haben Sie den Namen der Aktionsmethode `DeleteConfirmed` auf `Delete` geändert. Der Gerüst Code hat den `HttpPost` `Delete`-Methode genannt `DeleteConfirmed`, um der `HttpPost`-Methode eine eindeutige Signatur zu übergeben. (Die CLR erfordert überladene Methoden, um verschiedene Methoden Parameter zu haben.) Nachdem die Signaturen eindeutig sind, können Sie die MVC-Konvention verwenden und den gleichen Namen für die `HttpPost`-und `HttpGet` Delete-Methode verwenden.

    Wenn die Leistungsverbesserung in einer Anwendung mit hohem Volumen eine Priorität ist, können Sie eine unnötige SQL-Abfrage zum Abrufen der Zeile vermeiden, indem Sie die Codezeilen ersetzen, die die `Find`-und `Remove` Methoden durch folgenden Code aufrufen:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Dieser Code instanziiert eine `Student` Entität nur mit dem Primärschlüssel Wert und legt dann den Entitäts Status auf `Deleted`fest. Das ist alles, was Entity Framework benötigt, um die Entität löschen zu können.

    Wie bereits erwähnt, werden die Daten von der `HttpGet` `Delete`-Methode nicht gelöscht. Das Ausführen eines Löschvorgangs als Reaktion auf eine GET-Anforderung (oder für die Ausführung eines Bearbeitungsvorgangs, eines Erstellungs Vorgangs oder eines anderen Vorgangs zum Ändern von Daten) führt zu einem Sicherheitsrisiko. Weitere Informationen finden Sie unter [ASP.NET MVC Tip #46 – verwenden Sie keine Lösch Links, da Sie Sicherheitslücken](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) im Blog von Stephen Walther erstellen.

3. Fügen Sie in *views\student\delete.cshtml*eine Fehlermeldung zwischen der `h2` Überschrift und der `h3` Überschrift hinzu, wie im folgenden Beispiel gezeigt:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

4. Führen Sie die Seite Durchstarten des Programms aus, wählen Sie die Registerkarte **Studenten** aus, und klicken Sie dann auf den Link **Löschen** .

5. Wählen Sie auf der Seite **Löschen** aus, die besagt, dass **Sie diesen Vorgang wirklich löschen möchten**.

    Die Index Seite wird ohne den gelöschten Studenten angezeigt. (Im [Tutorial](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)zur Parallelität wird ein Beispiel für den Fehler Behandlungs Code in Aktion angezeigt.)

## <a name="close-database-connections"></a>Schließen von Datenbankverbindungen

Um Datenbankverbindungen zu schließen und die Ressourcen freizugeben, die Sie so bald wie möglich halten, löschen Sie die Kontext Instanz, wenn Sie damit abgeschlossen sind. Aus diesem Grund stellt der Gerüst Code eine verwerfen- [Methode am](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) Ende der `StudentController`-Klasse in *StudentController.cs*bereit, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

Die Basis `Controller` Klasse implementiert bereits die `IDisposable`-Schnittstelle, sodass dieser Code der `Dispose(bool)`-Methode einfach eine Überschreibung hinzufügt, um die Kontext Instanz explizit zu verwerfen.

## <a name="handle-transactions"></a>Behandeln von Transaktionen

Standardgemäß implementiert Entity Framework implizit Transaktionen. In Szenarien, in denen Sie Änderungen an mehreren Zeilen oder Tabellen vornehmen und dann `SaveChanges`aufzurufen, stellt der Entity Framework automatisch sicher, dass entweder alle Änderungen erfolgreich verlaufen oder alle fehlschlagen. Wenn ein Fehler auftritt, nachdem einige der Änderungen durchgeführt wurden, werden diese Änderungen automatisch zurückgesetzt. Szenarios, in denen Sie mehr Kontrolle benötigen&mdash;z. b., wenn Sie Vorgänge, die außerhalb der Entity Framework in einer Transaktion ausgeführt werden, in eine Transaktion einschließen möchten&mdash;siehe [Arbeiten mit Transaktionen](/ef/ef6/saving/transactions).

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Sie verfügen jetzt über einen vollständigen Satz von Seiten, die einfache CRUD-Vorgänge für `Student` Entitäten ausführen. Sie haben MVC-Hilfsprogramme verwendet, um Benutzeroberflächen Elemente für Datenfelder zu generieren. Weitere Informationen zu MVC-Hilfsprogrammen finden [Sie unter Rendern eines Formulars mithilfe von HTML](/previous-versions/aspnet/dd410596(v=vs.98)) -Hilfsprogrammen (der Artikel ist für MVC 3, aber noch für MVC 5 relevant).

Links zu anderen EF 6-Ressourcen finden Sie unter [ASP.NET Data Access-Empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Seite "Details" wurde erstellt
> * Die Seite „Erstellen“ aktualisiert
> * Die HttpPost-Bearbeitungsmethode wurde aktualisiert.
> * Die Seite „Löschen“ aktualisiert
> * Datenbankverbindungen geschlossen
> * Behandelte Transaktionen

Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie das Sortieren, Filtern und Paging zum Projekt hinzufügen.
> [!div class="nextstepaction"]
> [Sortieren, Filtern und Auslagern](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
