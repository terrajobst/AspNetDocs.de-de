---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Einführung in Entity Framework 4,0 Database First und ASP.NET 4 Web Forms-Teil 2 | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie mithilfe der Entity Framework Web Forms Anwendungen erstellen. Die Beispielanwendung ist...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: bd6a2e29e6f0df04e39be29160e2e08cc99c4706
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78456447"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Die ersten Schritte mit Entity Framework 4,0 Database First und ASP.NET 4 Web Forms-Teil 2

von [Tom Dykstra](https://github.com/tdykstra)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie ASP.net-Web Forms Anwendungen mithilfe von Entity Framework 4,0 und Visual Studio 2010 erstellen. Weitere Informationen zur tutorialreihe finden Sie [im ersten Tutorial der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md) .

## <a name="the-entitydatasource-control"></a>Das EntityDataSource-Steuerelement

Im vorherigen Tutorial haben Sie eine Website, eine Datenbank und ein Datenmodell erstellt. In diesem Tutorial arbeiten Sie mit dem `EntityDataSource`-Steuerelement, das ASP.net bereitstellt, um die Arbeit mit einem Entity Framework Datenmodell zu vereinfachen. Sie erstellen ein `GridView`-Steuerelement zum Anzeigen und Bearbeiten von Studenten Daten, ein `DetailsView` Steuerelement zum Hinzufügen neuer Studenten und ein `DropDownList` Steuerelement für die Auswahl einer Abteilung (die Sie später zum Anzeigen der zugeordneten Kurse verwenden).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Beachten Sie, dass Sie in dieser Anwendung keine Eingabevalidierung zu Seiten hinzufügen, die die Datenbank aktualisieren, und einige der Fehlerbehandlung sind nicht so robust, wie Sie in einer Produktionsanwendung erforderlich sind. Dadurch konzentriert sich das Lernprogramm auf die Entity Framework und hält es nicht mehr zu lange. Ausführliche Informationen dazu, wie Sie diese Funktionen zu Ihrer Anwendung hinzufügen, finden Sie unter [Validieren von Benutzereingaben in ASP.net Web Pages](https://msdn.microsoft.com/library/7kh55542.aspx) und [Fehlerbehandlung in ASP.net Pages and Applications](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Hinzufügen und Konfigurieren des EntityDataSource-Steuer Elements

Zunächst konfigurieren Sie ein `EntityDataSource`-Steuerelement, um `Person` Entitäten aus der `People` Entitätenmenge zu lesen.

Stellen Sie sicher, dass Sie Visual Studio geöffnet haben und dass Sie mit dem Projekt arbeiten, das Sie in Teil 1 erstellt haben. Wenn Sie das Projekt seit dem Erstellen des Datenmodells oder seit der letzten Änderung, die Sie vorgenommen haben, nicht erstellt haben, erstellen Sie das Projekt jetzt. Änderungen am Datenmodell werden dem Designer erst zur Verfügung gestellt, wenn das Projekt erstellt wurde.

Erstellen Sie eine neue Webseite mithilfe des **Webformulars unter Verwendung** der Vorlage "Master Seite", und nennen Sie Sie " *students. aspx*".

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Geben Sie *Site. Master* als Master Seite an. Auf allen Seiten, die Sie für diese Tutorials erstellen, wird diese Master Seite verwendet.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

Fügen Sie in der **Quell** Ansicht dem `Content` Steuerelement mit dem Namen `Content2`eine `h2` Überschrift hinzu, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Ziehen Sie auf der Registerkarte **Daten** der **Toolbox**ein `EntityDataSource`-Steuerelement auf die Seite, legen Sie es unterhalb der Überschrift ab, und ändern Sie die ID in `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Wechseln Sie zur **Entwurfs** Ansicht, klicken Sie auf das Smarttag des Datenquellen Steuer Elements, und klicken Sie dann auf **Datenquelle konfigurieren** , um den Assistenten zum **Konfigurieren von Datenquellen** zu starten.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

Wählen Sie im Schritt **configure ObjectContext** Wizard die Option **SchoolEntities** als Wert für **benannte Verbindung**aus, und wählen Sie **SchoolEntities** als Wert von **DefaultContainerName** aus. Klicken Sie dann auf **Weiter**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Hinweis: Wenn an dieser Stelle das folgende Dialogfeld angezeigt wird, müssen Sie das Projekt erstellen, bevor Sie den Vorgang fortsetzen.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

Wählen Sie im Schritt **Datenauswahl konfigurieren** die Option **People** als Wert für **entitySetName**aus. Vergewissern Sie sich unter **auswählen**, dass das Kontrollkästchen ll **auswählen** aktiviert ist. Wählen Sie dann die Optionen zum Aktivieren von Update und DELETE aus. Wenn Sie fertig sind, klicken Sie auf **Fertig**stellen.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Konfigurieren von Daten Bank Regeln, um das Löschen zuzulassen

Sie erstellen eine Seite, auf der Benutzer Studenten aus der `Person` Tabelle löschen können, die drei Beziehungen zu anderen Tabellen hat (`Course`, `StudentGrade`und `OfficeAssignment`). Standardmäßig verhindert die Datenbank, dass Sie eine Zeile in `Person` löschen, wenn in einer der anderen Tabellen Verwandte Zeilen vorhanden sind. Sie können die verknüpften Zeilen zuerst manuell löschen, oder Sie können die Datenbank so konfigurieren, dass Sie automatisch gelöscht werden, wenn Sie eine `Person` Zeile löschen. Für Student-Datensätze in diesem Tutorial konfigurieren Sie die Datenbank so, dass die zugehörigen Daten automatisch gelöscht werden. Da Schüler/Studenten Verwandte Zeilen nur in der `StudentGrade` Tabelle haben können, müssen Sie nur eine der drei Beziehungen konfigurieren.

Wenn Sie die Datei " *School. mdf* " verwenden, die Sie aus dem Projekt heruntergeladen haben, das in diesem Tutorial enthalten ist, können Sie diesen Abschnitt überspringen, da diese Konfigurationsänderungen bereits vorgenommen wurden. Wenn Sie die Datenbank durch Ausführen eines Skripts erstellt haben, konfigurieren Sie die Datenbank, indem Sie die folgenden Verfahren ausführen.

Öffnen Sie in **Server-Explorer**das Daten Bank Diagramm, das Sie in Teil 1 erstellt haben. Klicken Sie mit der rechten Maustaste auf die Beziehung zwischen `Person` und `StudentGrade` (die Zeile zwischen Tabellen), und wählen Sie dann **Eigenschaften**aus.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

Erweitern Sie im Fenster **Eigenschaften** die Option **INSERT-und Update Specification** , und legen Sie die **DeleteRule** -Eigenschaft auf **Cascade**fest.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Speichern und schließen Sie das Diagramm. Wenn Sie gefragt werden, ob Sie die Datenbank aktualisieren möchten, klicken Sie auf **Ja**.

Um sicherzustellen, dass das Modell Entitäten im Arbeitsspeicher synchron mit der Datenbank speichert, müssen Sie die entsprechenden Regeln im Datenmodell festlegen. Öffnen Sie *School Model. edmx*, klicken Sie mit der rechten Maustaste auf die Zuordnungs Linie zwischen `Person` und `StudentGrade`, und wählen Sie dann **Eigenschaften**aus.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

Legen Sie im Fenster **Eigenschaften** **End1 OnDelete** auf **Cascade**fest.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Speichern und schließen Sie die Datei *School Model. edmx* , und erstellen Sie dann das Projekt neu.

Im Allgemeinen haben Sie bei einer Änderung der Datenbank verschiedene Möglichkeiten, um das Modell zu synchronisieren:

- Für bestimmte Arten von Änderungen (z. b. das Hinzufügen oder Aktualisieren von Tabellen, Sichten oder gespeicherten Prozeduren) klicken Sie mit der rechten Maustaste in den Designer, und wählen Sie **Modell aus Datenbank aktualisieren aus** , damit der Designer die Änderungen automatisch vornimmt.
- Generieren Sie das Datenmodell neu.
- Nehmen Sie manuelle Updates wie diese vor.

In diesem Fall können Sie das Modell neu generieren oder die Tabellen aktualisieren, die von der Beziehungs Änderung betroffen sind. dann müssten Sie die Feldnamen Änderung erneut vornehmen (von `FirstName` bis `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Verwenden eines GridView-Steuer Elements zum Lesen und Aktualisieren von Entitäten

In diesem Abschnitt verwenden Sie ein `GridView`-Steuerelement, um Studenten anzuzeigen, zu aktualisieren oder zu löschen.

Öffnen Sie, oder wechseln Sie zu *students. aspx* , und wechseln Sie zur **Entwurfs** Ansicht. Ziehen Sie ein `GridView`-Steuerelement auf der Registerkarte **Daten** der **Toolbox**rechts neben das Steuerelement `EntityDataSource`, benennen Sie es `StudentsGridView`, klicken Sie auf das Smarttag, und wählen Sie dann **studentsentitydatasource** als Datenquelle aus.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Klicken Sie auf **Schema aktualisieren** (Klicken Sie auf **Ja** , wenn Sie zur Bestätigung aufgefordert werden), klicken Sie dann auf **Paging aktivieren**, **Sortierung aktivieren**, **Bearbeiten**aktivieren und **Löschen aktivieren**.

Klicken Sie auf **Spalten bearbeiten**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

Löschen Sie im Feld **Ausgewählte Felder** den Text **PersonID**, **LastName**und **HireDate**. In der Regel wird Benutzern kein Daten Satz Schlüssel angezeigt, das Einstellungs Datum ist für Schüler und Studenten nicht relevant, und Sie fügen beide Teile des Namens in einem Feld ein, sodass Sie nur eines der namens Felder benötigen.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Wählen Sie das Feld **firstmittelname** aus, und klicken Sie dann auf **dieses Feld in ein TemplateField konvertieren**.

Gehen Sie für " **anmelmentdate**" genauso vor.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Klicken Sie auf **OK** , und wechseln Sie dann zur **Quell** Ansicht. Die restlichen Änderungen werden im Markup einfacher durchzuführen. Das `GridView`-Steuerelement Markup sieht nun wie im folgenden Beispiel aus.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

Die erste Spalte nach dem Befehlsfeld ist ein Vorlagen Feld, in dem der Vorname derzeit angezeigt wird. Ändern Sie das Markup für dieses Vorlagen Feld so, dass es wie im folgenden Beispiel aussieht:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

Im Anzeigemodus zeigen zwei `Label` Steuerelemente den vor-und Nachnamen an. Im Bearbeitungsmodus werden zwei Textfelder bereitgestellt, sodass Sie den vor-und Nachnamen ändern können. Wie bei den `Label`-Steuerelementen im Anzeigemodus verwenden Sie `Bind` und `Eval` Ausdrücke genau so wie bei ASP.NET-Datenquellen-Steuerelementen, die direkt mit Datenbanken verbunden sind. Der einzige Unterschied besteht darin, dass Sie anstelle von Daten Bank Spalten Entitäts Eigenschaften angeben.

Die letzte Spalte ist ein Vorlagen Feld, das das Registrierungsdatum anzeigt. Ändern Sie das Markup für dieses Feld so, dass es wie im folgenden Beispiel aussieht:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

Im Anzeige-und Bearbeitungsmodus bewirkt die Format Zeichenfolge "{0, d}", dass das Datum im Format "kurzes Datum" angezeigt wird. (Ihr Computer kann so konfiguriert werden, dass dieses Format unterschiedlich angezeigt wird, und zwar von den in diesem Tutorial gezeigten Bildschirmbildern.)

Beachten Sie, dass der Designer in jedem dieser Vorlagen Felder standardmäßig einen `Bind` Ausdruck verwendet hat, Sie diesen jedoch in einen `Eval` Ausdruck in den `ItemTemplate` Elementen geändert haben. Der `Bind` Ausdruck macht die Daten in `GridView` Steuerelement Eigenschaften verfügbar, wenn Sie im Code auf die Daten zugreifen müssen. Auf dieser Seite benötigen Sie keinen Zugriff auf diese Daten im Code, sodass Sie `Eval`verwenden können, was effizienter ist. Weitere Informationen finden Sie unter [erhalten von Daten aus den Daten Steuerelementen](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Überarbeiten von EntityDataSource-Steuerelement Markup zur Verbesserung der Leistung

Entfernen Sie im Markup für das `EntityDataSource`-Steuerelement die Attribute `ConnectionString` und `DefaultContainerName`, und ersetzen Sie Sie durch ein `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` Attribut. Dies ist eine Änderung, die Sie jedes Mal vornehmen sollten, wenn Sie ein `EntityDataSource`-Steuerelement erstellen, es sei denn, Sie müssen eine Verbindung verwenden, die sich von der unterscheidet, die in der Objekt Kontext Klasse hart codiert ist. Die Verwendung des `ContextTypeName`-Attributs bietet die folgenden Vorteile:

- Bessere Leistung. Wenn das `EntityDataSource` Steuerelement das Datenmodell mit den Attributen `ConnectionString` und `DefaultContainerName` initialisiert, werden zusätzliche Aufgaben ausgeführt, um Metadaten für jede Anforderung zu laden. Dies ist nicht erforderlich, wenn Sie das `ContextTypeName`-Attribut angeben.
- Lazy Load ist in den generierten Objekt Kontext Klassen (z. b. `SchoolEntities` in diesem Tutorial) in Entity Framework 4,0 standardmäßig aktiviert. Dies bedeutet, dass Navigations Eigenschaften bei Bedarf automatisch mit verwandten Daten geladen werden. Lazy Load wird weiter unten in diesem Tutorial ausführlicher erläutert.
- Alle Anpassungen, die Sie auf die Objekt Kontext Klasse angewendet haben (in diesem Fall die `SchoolEntities`-Klasse), stehen für Steuerelemente zur Verfügung, die das `EntityDataSource`-Steuerelement verwenden. Das Anpassen der Objekt Kontext Klasse ist ein erweitertes Thema, das in dieser tutorialreihe nicht behandelt wird. Weitere Informationen finden Sie unter [Erweitern von Entity Framework generierten Typen](https://msdn.microsoft.com/library/dd456844.aspx).

Das Markup ähnelt nun dem folgenden Beispiel (die Reihenfolge der Eigenschaften kann abweichen):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

Das `EnableFlattening`-Attribut verweist auf eine Funktion, die in früheren Versionen der Entity Framework erforderlich war, da Fremdschlüssel Spalten nicht als Entitäts Eigenschaften verfügbar gemacht wurden. Die aktuelle Version ermöglicht die Verwendung von *Fremdschlüssel Zuordnungen*, d. h. Fremdschlüssel Eigenschaften werden für alle n:n-Zuordnungen offengelegt. Wenn Ihre Entitäten Fremdschlüssel Eigenschaften und keine [komplexen Typen](https://msdn.microsoft.com/library/bb738472.aspx)aufweisen, können Sie dieses Attribut auf `False`festlegen. Entfernen Sie das Attribut nicht aus dem Markup, da der Standardwert `True`ist. Weitere Informationen finden Sie unter Vereinfachen von [Objekten (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Führen Sie die Seite aus, und Sie sehen eine Liste der Schüler/Studenten und Mitarbeiter (im nächsten Tutorial können Sie nach nur Schülern Filtern). Der Vorname und Nachname werden gleichzeitig angezeigt.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Um die Anzeige zu sortieren, klicken Sie auf einen Spaltennamen.

Klicken Sie in einer beliebigen Zeile auf **Bearbeiten** . Text Felder werden angezeigt, in denen Sie den vor-und Nachnamen ändern können.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

Die Schaltfläche **Löschen** funktioniert ebenfalls. Klicken Sie für eine Zeile mit einem Registrierungsdatum auf löschen, um die Zeile zu löschen. (Zeilen ohne Anmeldungs Datum stellen Dozenten dar, und Sie erhalten möglicherweise einen Fehler bei der referenziellen Integrität. Im nächsten Tutorial Filtern Sie diese Liste so, dass Sie nur Studenten enthält.)

## <a name="displaying-data-from-a-navigation-property"></a>Anzeigen von Daten aus einer Navigations Eigenschaft

Nehmen Sie nun an, Sie möchten wissen, in wie vielen Kursen die einzelnen Studenten angemeldet sind. Der Entity Framework stellt diese Informationen in der `StudentGrades`-Navigations Eigenschaft der `Person`-Entität bereit. Da der Daten bankentwurf nicht zulässt, dass ein Student in einem Kurs registriert wird, ohne dass ein Grade zugewiesen ist, können Sie in diesem Tutorial davon ausgehen, dass das vorhanden sein einer Zeile in der `StudentGrade` Tabellenzeile, die einem Kurs zugeordnet ist, mit der Registrierung im Kurs identisch ist. (Die `Courses` Navigations Eigenschaft ist nur für Dozenten.)

Wenn Sie das `ContextTypeName`-Attribut des `EntityDataSource` Steuer Elements verwenden, ruft der Entity Framework automatisch Informationen für eine Navigations Eigenschaft ab, wenn Sie auf diese Eigenschaft zugreifen. Dies wird als *Lazy Loading*bezeichnet. Dies kann jedoch ineffizient sein, da dies zu einem separaten Aufruf der Datenbank führt, wenn zusätzliche Informationen benötigt werden. Wenn Sie für jede Entität, die vom `EntityDataSource`-Steuerelement zurückgegeben wird, Daten aus der Navigations Eigenschaft benötigen, ist es effizienter, die verknüpften Daten zusammen mit der Entität selbst in einem einzigen Daten Bank Abruf abzurufen. Dies wird als *Eager Loading*bezeichnet, und Sie geben Eager Loading für eine Navigations Eigenschaft an, indem Sie die `Include`-Eigenschaft des `EntityDataSource` Steuer Elements festlegen.

In *students. aspx*möchten Sie die Anzahl von Kursen für jeden Schüler/Student anzeigen, sodass Eager Loading die beste Wahl ist. Wenn Sie alle Schüler/Studenten anzeigen, aber die Anzahl der Kurse nur für einige davon anzeigen (was das Schreiben von Code zusätzlich zum Markup erfordern würde), ist Lazy Loading möglicherweise besser geeignet.

Öffnen Sie, oder wechseln Sie zu *students. aspx*, wechseln Sie zur **Entwurfs** Ansicht, wählen Sie `StudentsEntityDataSource`aus, und legen Sie im Fenster **Eigenschaften** die **include** -Eigenschaft auf **studentgrades**fest. (Wenn Sie mehrere Navigations Eigenschaften erhalten möchten, können Sie die Namen durch Kommas getrennt angeben – z. b. **studentgrades, Kurse**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Wechseln Sie zur **Quell** Ansicht. Fügen Sie im `StudentsGridView`-Steuerelement hinter dem letzten `asp:TemplateField`-Element das folgende neue Vorlagen Feld hinzu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

Im `Eval` Ausdruck können Sie auf die Navigations Eigenschaft `StudentGrades`verweisen. Da diese Eigenschaft eine Auflistung enthält, verfügt sie über eine `Count`-Eigenschaft, die Sie verwenden können, um die Anzahl von Kursen anzuzeigen, in denen der Student registriert ist. In einem späteren Tutorial erfahren Sie, wie Sie Daten aus Navigations Eigenschaften anzeigen, die einzelne Entitäten anstelle von Auflistungen enthalten. (Beachten Sie, dass Sie `BoundField` Elemente nicht zum Anzeigen von Daten aus Navigations Eigenschaften verwenden können.)

Führen Sie die Seite aus, und Sie sehen nun, wie viele Kurse die einzelnen Studenten angemeldet sind.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Verwenden eines DetailsView-Steuer Elements zum Einfügen von Entitäten

Der nächste Schritt besteht darin, eine Seite mit einem `DetailsView`-Steuerelement zu erstellen, mit dem Sie neue Studenten hinzufügen können. Schließen Sie den Browser, und erstellen Sie dann eine neue Webseite mithilfe der Master Seite *Site. Master* . Benennen Sie die Seite *studentsadd. aspx*, und wechseln Sie dann zur **Quell** Ansicht.

Fügen Sie das folgende Markup hinzu, um das vorhandene Markup für das `Content` Steuerelement mit dem Namen `Content2`zu ersetzen:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Dieses Markup erstellt ein `EntityDataSource` Steuerelement, das dem in *students. aspx*erstellten ähnelt, mit dem Unterschied, dass es Einfügevorgänge ermöglicht. Wie beim `GridView`-Steuerelement sind die gebundenen Felder des `DetailsView` Steuer Elements genau so codiert wie für ein Daten Steuerelement, das eine direkte Verbindung mit einer Datenbank herstellt, mit dem Unterschied, dass Sie auf Entitäts Eigenschaften verweisen. In diesem Fall wird das `DetailsView`-Steuerelement nur zum Einfügen von Zeilen verwendet, sodass Sie den Standardmodus auf `Insert`festgelegt haben.

Führen Sie die Seite aus, und fügen Sie einen neuen Studenten hinzu.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Nachdem Sie einen neuen Schüler/Student eingefügt haben, werden die Informationen zu den *neuen Schülern angezeigt*.

## <a name="displaying-data-in-a-drop-down-list"></a>Anzeigen von Daten in einer Dropdown Liste

In den folgenden Schritten wird ein `DropDownList`-Steuerelement mithilfe eines `EntityDataSource` Steuer Elements an eine Entitätenmenge übernehmen In diesem Teil des Tutorials wird diese Liste nicht wesentlich behandelt. In nachfolgenden Teilen verwenden Sie die Liste, um Benutzern die Auswahl von Kursen zu gestatten, die mit der Abteilung verknüpft sind.

Erstellen Sie eine neue Webseite namens " *Courses. aspx*". Fügen Sie in der **Quell** Ansicht dem `Content`-Steuerelement, das `Content2`heißt, eine Überschrift hinzu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

Fügen Sie der Seite in der **Entwurfs** Ansicht wie zuvor ein `EntityDataSource`-Steuerelement hinzu, mit der Ausnahme, dass Sie `DepartmentsEntityDataSource`. Wählen Sie **Abteilungen** als Wert für **entitySetName** aus, und wählen Sie nur die Eigenschaften **DepartmentID** und **Name** aus.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Ziehen Sie auf der Registerkarte **Standard** der **Toolbox**ein `DropDownList`-Steuerelement auf die Seite, benennen Sie es `DepartmentsDropDownList`, klicken Sie auf das Smarttag, und wählen Sie **Datenquelle auswählen** aus, um den **Assistenten zum Konfigurieren von Daten**Quellen zu starten.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

Wählen Sie im Schritt **Wählen Sie eine Datenquelle** aus die Option **departmentsentitydatasource** als Datenquelle aus, klicken Sie auf **Schema aktualisieren**, und wählen Sie dann **Name** als Datenfeld zum Anzeigen und **DepartmentID** als WertDatenfeld aus. Klicken Sie auf **OK**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Die Methode, die Sie verwenden, um das Steuerelement mit dem Entity Framework zu übernehmen, ist identisch mit anderen ASP.NET-Datenquellen-Steuerelementen, außer Sie geben Entitäten und Entitäts Eigenschaften an.

Wechseln Sie zur **Quell** Ansicht, und fügen Sie "Select a Department:" direkt vor dem `DropDownList`-Steuerelement ein.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Zur Erinnerung: Ändern Sie das Markup für das `EntityDataSource` Steuerelement an dieser Stelle, indem Sie die Attribute `ConnectionString` und `DefaultContainerName` durch ein `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` Attribut ersetzen. Es ist oft am besten, zu warten, bis Sie das Daten gebundene Steuerelement erstellt haben, das mit dem Datenquellen-Steuerelement verknüpft ist, bevor Sie das `EntityDataSource`-Steuerelement Markup ändern, denn nachdem Sie die Änderung vorgenommen haben, stellt der Designer im Daten gebundenen Steuerelement keine **Aktualisierungs Schema** Option bereit.

Führen Sie die Seite aus, und Sie können eine Abteilung aus der Dropdown Liste auswählen.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Dies schließt die Einführung in die Verwendung des `EntityDataSource`-Steuer Elements ein. Das Arbeiten mit diesem Steuerelement unterscheidet sich in der Regel nicht von der Arbeit mit anderen ASP.NET-Datenquellen-Steuerelementen, mit der Ausnahme, dass Sie anstelle von Tabellen und Spalten auf Entitäten Die einzige Ausnahme ist, wenn Sie auf Navigations Eigenschaften zugreifen möchten. Im nächsten Tutorial sehen Sie, dass sich die Syntax, die Sie mit `EntityDataSource`-Steuerelement verwenden, möglicherweise auch von anderen Datenquellen-Steuerelementen unterscheidet, wenn Sie Daten filtern, gruppieren und sortieren.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-3.md)
