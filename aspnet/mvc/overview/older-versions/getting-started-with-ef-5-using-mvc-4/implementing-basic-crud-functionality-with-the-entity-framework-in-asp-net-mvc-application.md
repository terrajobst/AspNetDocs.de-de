---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementieren grundlegender CRUD-Funktionen mit dem Entity Framework in ASP.NET MVC-Anwendung (2 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio erstellt werden...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: eba146d2975e0dcf243facf7c205c4acfe40b6f6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595330"
---
# <a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementieren grundlegender CRUD-Funktionen mit dem Entity Framework in ASP.NET MVC-Anwendung (2 von 10)

von [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe der Entity Framework 5 Code First und Visual Studio 2012 erstellt werden. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe von Anfang an starten oder [ein Starter-Projekt für dieses Kapitel herunterladen](building-the-ef5-mvc4-chapter-downloads.md) und hier beginnen.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem stoßen, das Sie nicht beheben können, [Laden Sie das abgeschlossene Kapitel herunter](building-the-ef5-mvc4-chapter-downloads.md) , und versuchen Sie, das Problem zu reproduzieren. Im Allgemeinen können Sie die Lösung für das Problem finden, indem Sie Ihren Code mit dem abgeschlossenen Code vergleichen. Informationen zu häufigen Fehlern und deren Lösung finden Sie unter [Fehler und](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors) Problem Umgehungen.

Im vorherigen Tutorial haben Sie eine MVC-Anwendung erstellt, die Daten mithilfe der Entity Framework und SQL Server localdb speichert und anzeigt. In diesem Tutorial überprüfen und Anpassen Sie den CRUD-Code (Create, Read, Update, DELETE), den der MVC-Gerüstbau automatisch für Controller und Ansichten erstellt.

> [!NOTE]
> Es ist üblich, dass das Repositorymuster implementiert wird, um eine Abstraktionsebene zwischen Ihrem Controller und der Datenzugriffsebene zu erstellen. Um diese Lernprogramme so einfach wie möglich zu halten, wird ein Repository erst nach einem späteren Tutorial in dieser Reihe implementiert.

In diesem Tutorial erstellen Sie die folgenden Webseiten:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Erstellen einer Detailseite

Der Gerüst Code für die `Index` Seite "Students" hat die `Enrollments`-Eigenschaft ausgelassen, da diese Eigenschaft eine Auflistung enthält. Auf der Seite `Details` zeigen Sie den Inhalt der Auflistung in einer HTML-Tabelle an.

 In *controllers\studentcontroller.cs*verwendet die Aktionsmethode für die `Details` Ansicht die `Find`-Methode, um eine einzelne `Student` Entität abzurufen. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Der Schlüsselwert wird als `id`-Parameter an die-Methode übergeben und stammt aus den Routendaten im Link **Details** auf der Index Seite. 

1. Öffnen Sie *views\student\details.cshtml*. Jedes Feld wird mit einem `DisplayFor`-Hilfsprogramm angezeigt, wie im folgenden Beispiel gezeigt: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Fügen Sie nach dem `EnrollmentDate`-Feld und unmittelbar vor dem schließenden `fieldset`-Tag Code hinzu, um eine Liste der Registrierungen anzuzeigen, wie im folgenden Beispiel gezeigt:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Dieser Code durchläuft die Entitäten in der Navigationseigenschaft `Enrollments`. Für jede `Enrollment` Entität in der Eigenschaft werden der Kurs Titel und die Qualität angezeigt. Der Kurstitel wird aus der `Course` Entität abgerufen, die in der `Course`-Navigations Eigenschaft der `Enrollments`-Entität gespeichert ist. Alle diese Daten werden automatisch aus der Datenbank abgerufen, wenn Sie benötigt werden. (Anders ausgedrückt: Sie verwenden hier Lazy Loading. Sie haben keine *Eager Loading* für die `Courses` Navigations Eigenschaft angegeben. Wenn Sie also zum ersten Mal versuchen, auf diese Eigenschaft zuzugreifen, wird eine Abfrage an die Datenbank gesendet, um die Daten abzurufen. Weitere Informationen zu Lazy Loading und Eager Loading finden Sie im Tutorial [Lesen verwandter Daten](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) weiter unten in dieser Reihe.)
3. Führen Sie die Seite aus, indem Sie die Registerkarte **Studenten** auswählen und auf einen **Detail** Link für Alexander Carson klicken. Die Liste der Kurse und Klassen für den ausgewählten Studenten wird angezeigt:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Aktualisieren der Seite "erstellen"

1. Ersetzen Sie in *controllers\studentcontroller.cs*die `HttpPost``Create` Action-Methode durch den folgenden Code, um der Gerüst Methode einen `try-catch`-Block und das [Bind-Attribut](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) hinzuzufügen: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Mit diesem Code wird dem `Students` Entitätenmenge die `Student` Entität hinzugefügt, die vom ASP.NET MVC-Modell Binder erstellt wurde, und die Änderungen werden dann in der Datenbank gespeichert. (*Modell Binder* bezieht sich auf die ASP.NET MVC-Funktionalität, die Ihnen das Arbeiten mit Daten erleichtert, die von einem Formular übermittelt werden. ein Modell Binder konvertiert die gesendeten Formular Werte in CLR-Typen und übergibt sie in Parametern an die Aktionsmethode. In diesem Fall instanziiert der Modell Binder eine `Student` Entität für Sie mithilfe von Eigenschafts Werten aus der `Form` Auflistung.)

    Das `ValidateAntiForgeryToken`-Attribut trägt dazu bei, [Site übergreifende Anforderungs Fälschungs](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) Angriffe zu verhindern.

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.cshtml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Führen Sie die Seite aus, indem Sie die Registerkarte **Studenten** auswählen und auf **neu erstellen**klicken.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Einige Daten Validierungen funktionieren standardmäßig. Geben Sie Namen und ein ungültiges Datum ein, und klicken Sie auf **Erstellen** , um die Fehlermeldung anzuzeigen.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Der folgende markierte Code zeigt die Überprüfung der Modell Validierung.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Ändern Sie das Datum in einen gültigen Wert, z. b. 9/1/2005, und klicken Sie auf **Erstellen** , um anzuzeigen, dass der neue Student auf der **Index** Seite angezeigt

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Aktualisieren der Seite "Post bearbeiten"

In *controllers\studentcontroller.cs*verwendet die `HttpGet` `Edit`-Methode (die-Methode ohne das `HttpPost`-Attribut) die `Find`-Methode, um die ausgewählte `Student` Entität abzurufen, wie Sie in der `Details`-Methode gesehen haben. Sie müssen diese Methode nicht ändern.

Ersetzen Sie jedoch die `HttpPost` `Edit` Aktionsmethode durch den folgenden Code, um einen `try-catch` Block und das [Bind-Attribut](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)hinzuzufügen:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Dieser Code ähnelt dem, was Sie in der `HttpPost` `Create`-Methode gesehen haben. Anstatt jedoch die Entität, die vom Modell Binder erstellt wurde, der Entitätenmenge hinzuzufügen, legt dieser Code ein Flag für die Entität fest, das angibt, dass es geändert wurde. Wenn die [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) -Methode aufgerufen wird, bewirkt das [geänderte](https://msdn.microsoft.com/library/system.data.entitystate.aspx) Flag, dass der Entity Framework SQL-Anweisungen erstellt, um die Datenbankzeile zu aktualisieren. Alle Spalten der Datenbankzeile werden aktualisiert, einschließlich derjenigen, die der Benutzer nicht geändert hat, und Parallelitäts Konflikte werden ignoriert. (In einem späteren Tutorial dieser Reihe erfahren Sie, wie Sie Parallelität verarbeiten.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Entitäts Zustände und die Attach-Methode und die SaveChanges-Methode

Der Datenbankkontext verfolgt, ob die Entitäten im Arbeitsspeicher mit ihren entsprechenden Zeilen in der Datenbank synchronisiert sind. Diese Information bestimmt, was passiert, wenn Sie die Methode `SaveChanges` aufrufen. Wenn Sie z. b. eine neue Entität an die [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) -Methode übergeben, wird der Zustand dieser Entität auf `Added`festgelegt. Wenn Sie dann die [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) -Methode aufzurufen, gibt der Daten Bank Kontext einen SQL `INSERT`-Befehl aus.

Eine Entität kann einen der[folgenden Zustände](https://msdn.microsoft.com/library/system.data.entitystate.aspx)aufweisen:

- `Added`. Die Entität ist noch nicht in der Datenbank vorhanden. Die `SaveChanges`-Methode muss eine `INSERT`-Anweisung ausgeben.
- `Unchanged`. Die Methode `SaveChanges` muss nichts mit dieser Entität tun. Wenn Sie eine Entität aus der Datenbank lesen, beginnt die Entität mit diesem Status.
- `Modified`. Einige oder alle Eigenschaftswerte der Entität wurden geändert. Die `SaveChanges`-Methode muss eine `UPDATE`-Anweisung ausgeben.
- `Deleted`. Die Entität wurde zum Löschen markiert. Die `SaveChanges`-Methode muss eine `DELETE`-Anweisung ausgeben.
- `Detached`. Die Entität wird nicht vom Datenbankkontext nachverfolgt.

Statusänderungen werden in einer Desktop-App in der Regel automatisch festgelegt. In einem desktoptyp der Anwendung lesen Sie eine Entität und nehmen Änderungen an einigen Eigenschafts Werten vor. Dadurch wird der Entitätsstatus automatisch auf `Modified` festgelegt. Wenn Sie `SaveChanges`aufgerufen haben, generiert der Entity Framework eine SQL `UPDATE`-Anweisung, die nur die tatsächlich geänderten Eigenschaften aktualisiert.

Die getrennte Art von Web-Apps lässt diese kontinuierliche Sequenz nicht zu. Der [dbcontext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , der eine Entität liest, wird verworfen, nachdem eine Seite gerendert wurde. Wenn die `HttpPost` `Edit` Aktionsmethode aufgerufen wird, wird eine neue Anforderung erstellt, und Sie verfügen über eine neue Instanz von [dbcontext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx). Daher müssen Sie den Entitäts Zustand manuell auf `Modified.` festlegen, wenn Sie `SaveChanges`aufrufen, aktualisiert der Entity Framework alle Spalten der Datenbankzeile, da der Kontext nicht wissen kann, welche Eigenschaften Sie geändert haben.

Wenn Sie möchten, dass die SQL `Update`-Anweisung nur die Felder aktualisiert, die der Benutzer tatsächlich geändert hat, können Sie die ursprünglichen Werte auf irgendeine Weise speichern (z. b. ausgeblendete Felder), damit Sie verfügbar sind, wenn die `HttpPost` `Edit`-Methode aufgerufen wird. Anschließend können Sie eine `Student` Entität mit den ursprünglichen Werten erstellen, die `Attach`-Methode mit der ursprünglichen Version der Entität aufzurufen, die Werte der Entität auf die neuen Werte aktualisieren und dann `SaveChanges.` abrufen, um weitere Informationen zu erhalten. Weitere Informationen finden Sie unter [Entitäts Zustände und SaveChanges](https://msdn.microsoft.com/data/jj592676) und [lokale Daten](https://msdn.microsoft.com/data/jj592872) im MSDN Data Developer Center.

Der Code in " *views\student\edit.cshtml* " ähnelt dem, was Sie in *Create. cshtml*gesehen haben, und es sind keine Änderungen erforderlich.

Führen Sie die Seite aus, indem Sie die Registerkarte **Studenten** auswählen und dann auf einen Link zum **Bearbeiten** klicken.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Ändern Sie einige der Daten, und klicken Sie auf **Speichern**. Die geänderten Daten werden auf der Index Seite angezeigt.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualisieren der Seite "Löschen"

In *controllers\studentcontroller.cs*verwendet der Vorlagen Code für die `HttpGet` `Delete`-Methode die `Find`-Methode, um die ausgewählte `Student` Entität abzurufen, wie Sie in den `Details`-und `Edit`-Methoden gesehen haben. Allerdings müssen Sie dieser Methode und der dazugehörigen Ansicht einige Funktionen hinzufügen, um eine benutzerdefinierte Fehlermeldung zu implementieren, wenn der Aufruf von `SaveChanges` fehlschlägt.

Wie Sie bereits bei den Vorgängen zum Aktualisieren und Erstellen gesehen haben, benötigen Löschvorgänge zwei Aktionsmethoden. Die Methode, die als Reaktion auf eine GET-Anforderung aufgerufen wird, zeigt eine Ansicht an, die dem Benutzer die Möglichkeit gibt, den Löschvorgang zu genehmigen oder abzubrechen. Wenn der Benutzer diesen Löschvorgang genehmigt, wird eine POST-Anforderung erstellt. Wenn dies der Fall ist, wird die `HttpPost` `Delete`-Methode aufgerufen, und dann führt diese Methode den Löschvorgang aus.

Fügen Sie der `HttpPost` `Delete`-Methode einen `try-catch`-Block hinzu, um alle Fehler zu behandeln, die beim Aktualisieren der Datenbank auftreten können. Wenn ein Fehler auftritt, ruft die `HttpPost` `Delete`-Methode die `HttpGet` `Delete`-Methode auf und übergibt ihr einen Parameter, der angibt, dass ein Fehler aufgetreten ist. Die `HttpGet Delete`-Methode zeigt dann die Bestätigungsseite und die Fehlermeldung erneut an, um dem Benutzer die Möglichkeit zu geben, ihn abzubrechen oder erneut zu versuchen.

1. Ersetzen Sie die `HttpGet` `Delete` Aktionsmethode durch den folgenden Code, der die Fehlerberichterstattung verwaltet: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Dieser Code akzeptiert einen [optionalen](https://msdn.microsoft.com/library/dd264739.aspx) booleschen Parameter, der angibt, ob er nach einem Fehler beim Speichern der Änderungen aufgerufen wurde. Dieser Parameter ist `false`, wenn die `HttpGet` `Delete`-Methode ohne vorherige Fehler aufgerufen wird. Wenn Sie von der `HttpPost` `Delete`-Methode als Reaktion auf einen Fehler bei der Datenbankaktualisierung aufgerufen wird, wird der-Parameter `true`, und eine Fehlermeldung wird an die Ansicht übergeben.
2. Ersetzen Sie die `HttpPost` `Delete` Aktionsmethode (mit dem Namen `DeleteConfirmed`) durch den folgenden Code, der den eigentlichen Löschvorgang ausführt und alle Daten Bank Aktualisierungs Fehler abfängt.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Dieser Code Ruft die ausgewählte Entität ab und ruft dann die [Remove](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) -Methode auf, um den Status der Entität auf `Deleted`festzulegen. Wenn `SaveChanges` aufgerufen wird, wird ein SQL `DELETE`-Befehl generiert. Ebenfalls haben Sie den Namen der Aktionsmethode `DeleteConfirmed` auf `Delete` geändert. Der Gerüst Code hat den `HttpPost` `Delete`-Methode genannt `DeleteConfirmed`, um der `HttpPost`-Methode eine eindeutige Signatur zu übergeben. (Die CLR erfordert überladene Methoden, um verschiedene Methoden Parameter zu haben.) Nachdem die Signaturen eindeutig sind, können Sie die MVC-Konvention verwenden und den gleichen Namen für die `HttpPost`-und `HttpGet` Delete-Methode verwenden.

     Wenn das Verbessern der Leistung in einer Anwendung mit hohem Volumen eine Priorität ist, können Sie eine unnötige SQL-Abfrage vermeiden, um die Zeile abzurufen, indem Sie die Codezeilen, die die `Find`-und `Remove` Methoden aufrufen, durch den folgenden Code ersetzen, wie in gelber Hervorhebung dargestellt:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Dieser Code instanziiert eine `Student` Entität nur mit dem Primärschlüssel Wert und legt dann den Entitäts Status auf `Deleted`fest. Das ist alles, was Entity Framework benötigt, um die Entität löschen zu können.

     Wie bereits erwähnt, werden die Daten von der `HttpGet` `Delete`-Methode nicht gelöscht. Das Ausführen eines Löschvorgangs als Reaktion auf eine GET-Anforderung (oder für die Ausführung eines Bearbeitungsvorgangs, eines Erstellungs Vorgangs oder eines anderen Vorgangs zum Ändern von Daten) führt zu einem Sicherheitsrisiko. Weitere Informationen finden Sie unter [ASP.NET MVC Tip #46 – verwenden Sie keine Lösch Links, da Sie Sicherheitslücken](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) im Blog von Stephen Walther erstellen.
3. Fügen Sie in *views\student\delete.cshtml*eine Fehlermeldung zwischen der `h2` Überschrift und der `h3` Überschrift hinzu, wie im folgenden Beispiel gezeigt:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Führen Sie die Seite aus, indem Sie die Registerkarte " **Students** " auswählen und auf den Link **Löschen**

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Klicken Sie auf **Löschen**. Die Indexseite wird ohne den gelöschten Student angezeigt. (Ein Beispiel für den Fehler Behandlungs Code in Aktion finden Sie im Tutorial zum [verarbeiten](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) von Parallelität später in dieser Reihe.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Sicherstellen, dass die Datenbankverbindungen nicht offen bleiben

Um sicherzustellen, dass die Datenbankverbindungen ordnungsgemäß geschlossen sind und die Ressourcen, die Sie freigegeben haben, freigegeben werden, sollten Sie sehen, dass die Kontext Instanz verworfen wird. Aus diesem Grund stellt der Gerüst Code eine verwerfen- [Methode am](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) Ende der `StudentController`-Klasse in *StudentController.cs*bereit, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

Die Basis `Controller` Klasse implementiert bereits die `IDisposable`-Schnittstelle, sodass dieser Code der `Dispose(bool)`-Methode einfach eine Überschreibung hinzufügt, um die Kontext Instanz explizit zu verwerfen.

## <a name="summary"></a>Summary

Sie verfügen jetzt über einen vollständigen Satz von Seiten, die einfache CRUD-Vorgänge für `Student` Entitäten ausführen. Sie haben MVC-Hilfsprogramme verwendet, um Benutzeroberflächen Elemente für Datenfelder zu generieren. Weitere Informationen zu MVC-Hilfsprogrammen finden [Sie unter Rendern eines Formulars mithilfe von HTML](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) -Hilfsprogrammen (die Seite ist für MVC 3, aber noch für MVC 4 relevant).

Im nächsten Tutorial erweitern Sie die Funktionalität der Index Seite durch Hinzufügen von sortieren und Paging.

Links zu anderen Entity Framework Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Weiter](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
