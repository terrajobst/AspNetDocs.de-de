---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Mit den Entity Framework 4,0 und dem ObjectDataSource-Steuerelement, Teil 1: ersten Schritte | Microsoft-Dokumentation'
author: tdykstra
description: Diese tutorialreihe basiert auf der Webanwendung der Website "Web-so University", die durch die Reihe "Getting Started with the Entity Framework Tutorial Wenn yo...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 2f14707eb058d438495dd2bc4c17b976c471fc97
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440403"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Mit den Entity Framework 4,0 und dem ObjectDataSource-Steuerelement, Teil 1: ersten Schritten

von [Tom Dykstra](https://github.com/tdykstra)

> Diese tutorialreihe basiert auf der Webanwendung der Website von "Web", die in der tutorialreihe für die ersten Schritte [mit Entity Framework 4,0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) erstellt wurde. Wenn Sie die vorherigen Tutorials nicht durchgearbeitet haben, können Sie die Anwendung, die Sie erstellt haben, als Ausgangspunkt für dieses Tutorial [herunterladen](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) . Sie können auch [die Anwendung herunterladen](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , die von der kompletten tutorialreihe erstellt wird.
> 
> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie ASP.net-Web Forms Anwendungen mithilfe von Entity Framework 4,0 und Visual Studio 2010 erstellen. Die Beispielanwendung ist eine Website für eine fiktive. Sie enthält Funktionen wie die Zulassung von Studenten, die Erstellung von Kursen und Aufgaben von Dozenten.
> 
> Das Tutorial zeigt Beispiele in C#. Das [herunterladbare Beispiel](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) enthält Code C# sowohl in als auch in Visual Basic.
> 
> ## <a name="database-first"></a>Database First
> 
> Es gibt drei Möglichkeiten, wie Sie mit Daten in der Entity Framework arbeiten können: *Database First*, *Model First*und *Code First*. Dieses Tutorial ist für Database First. Informationen zu den Unterschieden zwischen diesen Workflows und Anleitungen zum Auswählen des besten für Ihr Szenario finden Sie unter [Entity Framework Entwicklungs Workflows](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> Wie die Reihe "Getting Started" verwendet diese tutorialreihe das ASP.net-Web Forms Modell und geht davon aus, dass Sie mit der Verwendung von ASP.net Web Forms in Visual Studio vertraut sind. Wenn Sie dies nicht tun, finden Sie weitere Informationen unter [Getting Started with ASP.NET 4,5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Wenn Sie lieber mit dem ASP.NET MVC-Framework arbeiten möchten, finden Sie weitere Informationen unter [Getting Started with the Entity Framework Using ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Software Versionen
> 
> | **Im Tutorial** | **Funktioniert auch mit** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express für Web. Das Tutorial wurde nicht mit neueren Versionen von Visual Studio getestet. Es gibt viele Unterschiede bei der Menü Auswahl, Dialogfeldern und Vorlagen. |
> | .NET 4 | .NET 4,5 ist abwärts kompatibel mit .NET 4, aber das Tutorial wurde nicht mit .NET 4,5 getestet. |
> | Entity Framework 4 | Das Tutorial wurde nicht mit neueren Versionen von Entity Framework getestet. Ab Entity Framework 5 verwendet EF standardmäßig den `DbContext API`, der mit EF 4,1 eingeführt wurde. Das EntityDataSource-Steuerelement wurde entwickelt, um die `ObjectContext`-API zu verwenden. Informationen zur Verwendung des EntityDataSource-Steuer Elements mit der `DbContext`-API finden Sie in [diesem Blogbeitrag](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Fragen
> 
> Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im Forum [ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), im [Forum zu Entity Framework und LINQ to Entities](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.

Mit dem `EntityDataSource`-Steuerelement können Sie eine Anwendung sehr schnell erstellen, aber in der Regel müssen Sie eine beträchtliche Menge an Geschäftslogik und Datenzugriffs Logik auf Ihren *aspx* -Seiten aufbewahren. Wenn Sie davon ausgehen, dass Ihre Anwendung komplexer wird und eine kontinuierliche Wartung erforderlich ist, können Sie mehr Entwicklungszeit in den Vordergrund investieren, um eine *n-* *schichtige oder geschichtete* Anwendungs Struktur zu erstellen, die besser verwaltierbar ist. Zum Implementieren dieser Architektur trennen Sie die Darstellungs Schicht von der Geschäftslogik Schicht (Business Logic Layer, BLL) und der Datenzugriffs Schicht (Data Access Layer, DAL). Eine Möglichkeit, diese Struktur zu implementieren, besteht darin, das `ObjectDataSource`-Steuerelement anstelle des `EntityDataSource` Steuer Elements zu verwenden. Wenn Sie das `ObjectDataSource`-Steuerelement verwenden, implementieren Sie Ihren eigenen Datenzugriffs Code und rufen ihn dann in *aspx* -Seiten mithilfe eines Steuer Elements auf, das viele der gleichen Funktionen wie andere Datenquellen-Steuerelemente aufweist. Auf diese Weise können Sie die Vorteile eines n-Tier-Ansatzes mit den Vorteilen der Verwendung eines Web Forms-Steuer Elements für den Datenzugriff kombinieren.

Das `ObjectDataSource` Steuerelement bietet Ihnen auch mehr Flexibilität auf andere Weise. Da Sie Ihren eigenen Datenzugriffs Code schreiben, ist es einfacher, einen bestimmten Entitätstyp zu lesen, einzufügen, zu aktualisieren oder zu löschen. dabei handelt es sich um die Aufgaben, die das `EntityDataSource` Steuerelement ausführen soll. Sie können z. b. die Protokollierung jedes Mal ausführen, wenn eine Entität aktualisiert wird. Sie können Daten immer dann archivieren, wenn eine Entität gelöscht wird, oder die zugehörigen Daten beim Einfügen einer Zeile mit einem Fremdschlüssel Wert automatisch überprüfen und aktualisieren.

## <a name="business-logic-and-repository-classes"></a>Geschäftslogik-und Repository-Klassen

Ein `ObjectDataSource` Steuerelement funktioniert, indem eine von Ihnen erstellte Klasse aufgerufen wird. Die-Klasse enthält Methoden, mit denen Daten abgerufen und aktualisiert werden, und die Namen dieser Methoden werden dem `ObjectDataSource`-Steuerelement im Markup bereitgestellt. Während der Rendering-oder Postback Verarbeitung ruft der `ObjectDataSource` die von Ihnen angegebenen Methoden auf.

Neben grundlegenden CRUD-Vorgängen muss die Klasse, die Sie für die Verwendung mit dem `ObjectDataSource`-Steuerelement erstellen, möglicherweise Geschäftslogik ausführen, wenn die `ObjectDataSource` Daten liest oder aktualisiert. Wenn Sie z. b. eine Abteilung aktualisieren, müssen Sie möglicherweise überprüfen, ob keine anderen Abteilungen denselben Administrator haben, da eine Person nicht als Administrator für mehr als eine Abteilung angemeldet sein darf.

In einigen `ObjectDataSource` Dokumentation, wie z. [b. der ObjectDataSource-Klasse](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), ruft das-Steuerelement eine-Klasse auf, die als *Geschäftsobjekt* bezeichnet wird, das sowohl Geschäftslogik-als auch Datenzugriffs Logik umfasst. In diesem Tutorial erstellen Sie separate Klassen für die Geschäftslogik und die Datenzugriffs Logik. Die Klasse, die Datenzugriffs Logik kapselt, wird als *Repository*bezeichnet. Die Geschäftslogik Klasse umfasst sowohl Geschäftslogik Methoden als auch Datenzugriffs Methoden, aber die Datenzugriffs Methoden aufrufen das Repository, um Datenzugriffs Aufgaben auszuführen.

Außerdem erstellen Sie eine Abstraktions Ebene zwischen Ihrer BLL-und Dal-Komponente, die automatisierte Unittests der BLL ermöglicht. Diese Abstraktions Ebene wird implementiert, indem eine Schnittstelle erstellt und die-Schnittstelle verwendet wird, wenn Sie das Repository in der Geschäftslogik Klasse instanziieren. Dies ermöglicht es Ihnen, die Geschäftslogik Klasse mit einem Verweis auf jedes Objekt bereitzustellen, das die Repository-Schnittstelle implementiert. Für den normalen Betrieb geben Sie ein Repository-Objekt an, das mit dem Entity Framework funktioniert. Zum Testen stellen Sie ein Repository-Objekt bereit, das mit gespeicherten Daten auf eine Weise funktioniert, die Sie leicht bearbeiten können, z. b. als Auflistungen definierte Klassen Variablen.

Die folgende Abbildung zeigt den Unterschied zwischen einer Geschäftslogik Klasse, die Datenzugriffs Logik ohne ein Repository und eine, die ein Repository verwendet, enthält.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Zunächst erstellen Sie Webseiten, in denen das `ObjectDataSource`-Steuerelement direkt an ein Repository gebunden ist, da nur grundlegende Datenzugriffs Aufgaben ausgeführt werden. Im nächsten Tutorial erstellen Sie eine Geschäftslogik Klasse mit Validierungs Logik und binden das `ObjectDataSource`-Steuerelement an diese Klasse anstelle an die Repository-Klasse. Außerdem erstellen Sie Komponententests für die Validierungs Logik. Im dritten Tutorial dieser Reihe fügen Sie der Anwendung Sortier-und Filterfunktionen hinzu.

Die Seiten, die Sie in diesem Tutorial erstellen, funktionieren mit dem `Departments` Entitätenmenge des Datenmodells, das Sie in der [Reihe "Getting Started Tutorial](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)" erstellt haben.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Aktualisieren der Datenbank und des Datenmodells

Sie beginnen mit diesem Tutorial, indem Sie zwei Änderungen an der Datenbank vornehmen. beide erfordern entsprechende Änderungen am Datenmodell, das Sie in den Tutorials erste Schritte [mit den Entity Framework und Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) erstellt haben. In einem dieser Tutorials haben Sie Änderungen im Designer manuell vorgenommen, um das Datenmodell nach einer Daten Bank Änderung mit der Datenbank zu synchronisieren. In diesem Tutorial verwenden Sie das Tool **Update Model von Database** des Designers, um das Datenmodell automatisch zu aktualisieren.

### <a name="adding-a-relationship-to-the-database"></a>Hinzufügen einer Beziehung zur Datenbank

Öffnen Sie in Visual Studio die Webanwendung der Website "Web", die Sie in der Reihe " [Getting Started with the Entity Framework and Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) Tutorial" erstellt haben, und öffnen Sie dann das `SchoolDiagram` Daten Bank Diagramm.

Wenn Sie die `Department` Tabelle im Daten Bank Diagramm betrachten, sehen Sie, dass Sie über eine `Administrator` Spalte verfügt. Diese Spalte ist ein Fremdschlüssel für die `Person` Tabelle, aber in der Datenbank ist keine Fremdschlüssel Beziehung definiert. Sie müssen die Beziehung erstellen und das Datenmodell aktualisieren, damit das Entity Framework diese Beziehung automatisch verarbeiten kann.

Klicken Sie im Daten Bank Diagramm mit der rechten Maustaste auf die `Department` Tabelle, und wählen Sie **Beziehungen**aus.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

Klicken Sie im Feld **Fremdschlüssel Beziehungen** auf **Hinzufügen**, und klicken Sie dann auf die Auslassungs Punkte für **Tabellen-und Spaltenspezifikation**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

Legen Sie im Dialogfeld **Tabellen und Spalten** die Tabelle und das Feld Primärschlüssel auf `Person` und `PersonID`fest, und legen Sie die Fremdschlüssel Tabelle und das Feld auf `Department` und `Administrator`fest. (Wenn Sie dies tun, ändert sich der Beziehungs Name von `FK_Department_Department` in `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Klicken Sie im Feld **Tabellen und Spalten** auf **OK** , klicken Sie im Feld **Fremdschlüssel Beziehungen** auf **Schließen** , und speichern Sie die Änderungen. Wenn Sie gefragt werden, ob Sie die `Person`-und `Department` Tabellen speichern möchten, klicken Sie auf **Ja**.

> [!NOTE]
> Wenn Sie `Person` Zeilen gelöscht haben, die den Daten entsprechen, die bereits in der Spalte `Administrator` vorhanden sind, können Sie diese Änderung nicht speichern. Verwenden Sie in diesem Fall den Tabellen-Editor in **Server-Explorer** , um sicherzustellen, dass der `Administrator` Wert in jeder `Department` Zeile die ID eines Datensatzes enthält, der in der `Person` Tabelle tatsächlich vorhanden ist.
> 
> Nachdem Sie die Änderung gespeichert haben, können Sie keine Zeile aus der `Person` Tabelle löschen, wenn diese Person ein Abteilungs Administrator ist. In einer Produktionsanwendung würden Sie eine bestimmte Fehlermeldung angeben, wenn eine Daten Bank Einschränkung einen Löschvorgang verhindert, oder Sie geben ein kaskadierliches löschen an. Ein Beispiel für die Angabe eines kaskadierenden Löschvorgang finden Sie unter den ersten Schritten für [Entity Framework und ASP.net –, Teil 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).

### <a name="adding-a-view-to-the-database"></a>Hinzufügen einer Ansicht zur Datenbank

Auf der Seite "neue *Abteilungen. aspx* ", die Sie erstellen möchten, können Sie eine Dropdown Liste mit Dozenten mit Namen im Format "Letztes, erstes" bereitstellen, sodass Benutzer Abteilungs Administratoren auswählen können. Um dies zu vereinfachen, erstellen Sie eine Sicht in der Datenbank. Die Ansicht besteht lediglich aus den Daten, die in der Dropdown Liste benötigt werden: der vollständige Name (ordnungsgemäß formatiert) und der Daten Satz Schlüssel.

Erweitern Sie in **Server-Explorer**den Eintrag *School. mdf*, klicken Sie mit der rechten Maustaste auf den Ordner **views** , und wählen Sie **neue Ansicht hinzufügen**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Klicken Sie im angezeigten Dialogfeld **Tabelle hinzufügen** auf **Schließen** , und fügen Sie die folgende SQL-Anweisung in den SQL-Bereich ein:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Speichern Sie die Ansicht als `vInstructorName`.

### <a name="updating-the-data-model"></a>Aktualisieren des Datenmodells

Öffnen Sie im Ordner *dal* die Datei *School Model. edmx* , klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Modell aus Datenbank aktualisieren aus**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

Wählen Sie im Dialogfeld **Wählen Sie Ihre Datenbankobjekte** aus die Registerkarte **Hinzufügen** aus, und wählen Sie die soeben erstellte Ansicht aus.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Klicken Sie auf **Fertig stellen**.

Im Designer sehen Sie, dass das Tool eine `vInstructorName` Entität und eine neue Zuordnung zwischen den Entitäten `Department` und `Person` erstellt hat.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> Im Fenster **Ausgabe** und **Fehlerliste** wird möglicherweise eine Warnmeldung angezeigt, in der Sie darüber informiert werden, dass das Tool automatisch einen Primärschlüssel für die neue `vInstructorName` Ansicht erstellt hat. Dieses Verhalten wird erwartet.

Wenn Sie im Code auf die neue `vInstructorName` Entität verweisen, sollten Sie die Daten Bank Konvention "v" nicht als präfixvorgang verwenden. Daher benennen Sie die Entität und Entitätenmenge im Modell um.

Öffnen Sie den **Modell Browser**. `vInstructorName` als Entitätstyp und Ansicht aufgeführt.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

Klicken Sie unter **SchoolModel** (nicht **SchoolModel. Store**) mit der rechten Maustaste auf **vinstructor Name** , und wählen Sie **Eigenschaften**aus. Ändern Sie im Eigenschaften Fenster die **Name** -Eigenschaft in "Instructor Name", und ändern Sie die Eigenschaft " **entitätenmengenname** " in "Instructor Names".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Speichern und schließen Sie das Datenmodell, und erstellen Sie dann das Projekt neu.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Verwenden einer Repository-Klasse und eines ObjectDataSource-Steuer Elements

Erstellen Sie eine neue Klassendatei im Ordner *dal* , benennen Sie Sie *SchoolRepository.cs*, und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Dieser Code stellt eine einzelne `GetDepartments`-Methode bereit, die alle Entitäten in der `Departments` Entitätenmenge zurückgibt. Da Sie wissen, dass Sie für jede zurückgegebene Zeile auf die `Person` Navigations Eigenschaft zugreifen werden, geben Sie Eager Loading für diese Eigenschaft an, indem Sie die `Include`-Methode verwenden. Die Klasse implementiert auch die `IDisposable`-Schnittstelle, um sicherzustellen, dass die Datenbankverbindung freigegeben wird, wenn das Objekt verworfen wird.

> [!NOTE]
> Eine gängige Vorgehensweise besteht darin, eine Repository-Klasse für jeden Entitätstyp zu erstellen. In diesem Tutorial wird eine Repository-Klasse für mehrere Entitäts Typen verwendet. Weitere Informationen zum Repository-Muster finden Sie in den Beiträgen im [Blog des Entity Framework Teams](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) und im [Blog von Julie Lerman](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).

Die `GetDepartments`-Methode gibt ein `IEnumerable` Objekt anstelle eines `IQueryable`-Objekts zurück, um sicherzustellen, dass die zurückgegebene Auflistung auch dann verwendbar ist, nachdem das Repository-Objekt selbst verworfen wurde. Ein `IQueryable`-Objekt kann Datenbankzugriffe auslösen, wenn darauf zugegriffen wird. das Repository-Objekt kann jedoch von dem Zeitpunkt verworfen werden, an dem ein Daten gebundene-Steuerelement versucht, die Daten zu erzeugen. Sie können einen anderen Sammlungstyp, z. b. ein `IList` Objekt, anstelle eines `IEnumerable`-Objekts zurückgeben. Wenn Sie jedoch ein `IEnumerable` Objekt zurückgeben, stellen Sie sicher, dass Sie typische Aufgaben für die schreibgeschützte Listen Verarbeitung ausführen können, wie z. b. `foreach` Schleifen und LINQ-Abfragen, aber Sie können keine Elemente in der Auflistung hinzufügen oder entfernen, was darauf hindeuten könnte, dass solche Änderungen in der Datenbank persistent gespeichert werden

Erstellen Sie eine Seite " *Departments. aspx* ", die die Master Seite " *Site. Master* " verwendet, und fügen Sie das folgende Markup im `Content` Steuerelement mit dem Namen `Content2`hinzu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Dieses Markup erstellt ein `ObjectDataSource` Steuerelement, das die soeben erstellte Repository-Klasse verwendet, sowie ein `GridView` Steuerelement zum Anzeigen der Daten. Das `GridView`-Steuerelement gibt Befehle zum **Bearbeiten** und **Löschen** an, aber Sie haben noch keinen Code zur Unterstützung hinzugefügt.

Mehrere Spalten verwenden `DynamicField` Steuerelemente, damit Sie die automatische Datenformatierung und Validierungs Funktionalität nutzen können. Damit diese funktionieren, müssen Sie die `EnableDynamicData`-Methode im `Page_Init`-Ereignishandler aufzurufen. (`DynamicControl` Steuerelemente werden nicht im Feld `Administrator` verwendet, da Sie nicht mit Navigations Eigenschaften funktionieren.)

Die `Vertical-Align="Top"` Attribute werden später wichtig, wenn Sie dem Raster eine Spalte hinzufügen, die über ein `GridView`-Steuerelement verfügt.

Öffnen Sie die Datei *Departments.aspx.cs* , und fügen Sie die folgende `using`-Anweisung hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Fügen Sie dann den folgenden Handler für das `Init`-Ereignis der Seite hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

Erstellen Sie im Ordner *dal* eine neue Klassendatei mit dem Namen *Department.cs* , und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Mit diesem Code werden dem Datenmodell Metadaten hinzugefügt. Er gibt an, dass die `Budget`-Eigenschaft der `Department` Entität tatsächlich Currency darstellt, obwohl der Datentyp `Decimal`ist, und gibt an, dass der Wert zwischen 0 und $1.000.000,00 liegen muss. Außerdem wird angegeben, dass die `StartDate`-Eigenschaft als Datum im Format mm/dd/yyyy formatiert werden soll.

Führen Sie die Seite " *Departments. aspx* " aus.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Obwohl Sie keine Format Zeichenfolge im Seiten Markup " *Departments. aspx* " für die **Budget** -oder **Start Datums** Spalten angegeben haben, wurden die standardmäßigen Währungs-und Datums Formatierung durch die `DynamicField`-Steuerelemente mithilfe der Metadaten, die Sie in der *Department.cs* -Datei angegeben haben, auf diese angewendet.

## <a name="adding-insert-and-delete-functionality"></a>Hinzufügen und Löschen von Funktionen

Öffnen Sie *SchoolRepository.cs*, und fügen Sie den folgenden Code hinzu, um eine `Insert`-Methode und eine `Delete`-Methode zu erstellen. Der Code enthält außerdem eine Methode mit dem Namen `GenerateDepartmentID`, die den nächsten verfügbaren Daten Satz Schlüsselwert zur Verwendung durch die `Insert`-Methode berechnet. Dies ist erforderlich, da die Datenbank nicht so konfiguriert ist, dass Sie automatisch für die `Department` Tabelle berechnet wird.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Die Attach-Methode

Die `DeleteDepartment`-Methode ruft die `Attach`-Methode auf, um den Link, der im Objektstatus-Manager des Objekt Kontexts verwaltet wird, zwischen der Entität im Arbeitsspeicher und der Datenbankzeile, die Sie darstellt, wiederherzustellen. Dies muss eintreten, bevor die-Methode die `SaveChanges`-Methode aufruft.

Der Begriff *Objekt Kontext* verweist auf die Entity Framework Klasse, die von der `ObjectContext`-Klasse abgeleitet wird, die Sie für den Zugriff auf die Entitätenmengen und Entitäten verwenden. Im Code für dieses Projekt heißt die Klasse `SchoolEntities`, und eine Instanz davon wird immer `context`benannt. Der *Objekt Zustands-Manager* des Objekt Kontexts ist eine Klasse, die von der `ObjectStateManager`-Klasse abgeleitet wird. Die Objekt Verbindung verwendet den Objekt Zustands-Manager, um Entitäts Objekte zu speichern, und um nachzuverfolgen, ob beide mit der entsprechenden Tabellenzeile oder den Zeilen in der Datenbank synchronisiert sind.

Wenn Sie eine Entität lesen, speichert der Objekt Kontext Sie im Objekt Zustands-Manager und verfolgt, ob die Darstellung des Objekts mit der Datenbank synchronisiert ist. Wenn Sie z. b. einen Eigenschafts Wert ändern, wird ein Flag festgelegt, um anzugeben, dass die geänderte Eigenschaft nicht mehr mit der Datenbank synchronisiert ist. Wenn Sie dann die `SaveChanges`-Methode aufzurufen, weiß der Objekt Kontext, was in der Datenbank zu tun ist, da der Objekt Zustands-Manager genau weiß, was sich zwischen dem aktuellen Zustand der Entität und dem Status der Datenbank unterscheidet.

Allerdings funktioniert dieser Prozess in der Regel nicht in einer Webanwendung, da die Objekt Kontext Instanz, die eine Entität liest, zusammen mit allen Elementen im Objekt Zustands-Manager verworfen wird, nachdem eine Seite gerendert wurde. Die Objekt Kontext Instanz, die Änderungen anwenden muss, ist eine neue, die für die Post Back Verarbeitung instanziiert wird. Im Fall der `DeleteDepartment`-Methode erstellt das `ObjectDataSource`-Steuerelement die ursprüngliche Version der Entität von Werten im Ansichts Zustand neu, aber diese neu `Department` erstellte Entität ist im Objekt Zustands-Manager nicht vorhanden. Wenn Sie die `DeleteObject`-Methode für diese neu erstellte Entität aufgerufen haben, schlägt der Aufruf fehl, da der Objekt Kontext nicht weiß, ob die Entität mit der Datenbank synchronisiert ist. Wenn Sie jedoch die `Attach`-Methode aufrufen, wird die gleiche Überwachung zwischen der neu erstellten Entität und den Werten in der Datenbank wieder hergestellt, die ursprünglich automatisch erfolgt sind, als die Entität in einer früheren Instanz des Objekt Kontexts gelesen wurde.

Es gibt Zeiten, in denen Sie nicht möchten, dass der Objekt Kontext Entitäten im Objekt Zustands-Manager nachverfolgt, und Sie können Flags festlegen, um dies zu verhindern. Beispiele hierfür finden Sie in späteren Tutorials in dieser Reihe.

### <a name="the-savechanges-method"></a>Die SaveChanges-Methode

Diese einfache Repository-Klasse veranschaulicht grundlegende Prinzipien der Durchführung von CRUD-Vorgängen. In diesem Beispiel wird die `SaveChanges`-Methode unmittelbar nach jedem Update aufgerufen. In einer Produktionsanwendung empfiehlt es sich, die `SaveChanges`-Methode aus einer separaten Methode aufzurufen, um Ihnen mehr Kontrolle darüber zu verschaffen, wann die Datenbank aktualisiert wird. (Am Ende des nächsten Tutorials finden Sie einen Link zu einem Whitepaper, in dem das Arbeitseinheits Muster erläutert wird, das ein Ansatz zum koordinieren verwandter Updates ist.) Beachten Sie auch, dass die `DeleteDepartment`-Methode im Beispiel keinen Code zum Behandeln von Parallelitäts Konflikten enthält. zu verwendende Code wird in einem späteren Tutorial dieser Reihe hinzugefügt.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Abrufen von beim Einfügen zu ausgewäfnenden Dozenten Namen

Benutzer müssen in der Lage sein, in einer Dropdown Liste einen Administrator aus einer Liste von Dozenten auszuwählen, wenn Sie neue Abteilungen erstellen. Fügen Sie daher *SchoolRepository.cs* den folgenden Code hinzu, um eine Methode zum Abrufen der Liste der Dozenten mithilfe der Ansicht zu erstellen, die Sie zuvor erstellt haben:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Erstellen einer Seite zum Einfügen von Abteilungen

Erstellen Sie eine Seite " *departmentsadd. aspx* ", die die *Site. Master* -Seite verwendet, und fügen Sie das folgende Markup im `Content`-Steuerelement mit dem Namen `Content2`hinzu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Dieses Markup erstellt zwei `ObjectDataSource` Steuerelemente, eine zum Einfügen neuer `Department` Entitäten und eine zum Abrufen von Dozenten Namen für das `DropDownList`-Steuerelement, das zum Auswählen von Abteilungs Administratoren verwendet wird. Das Markup erstellt ein `DetailsView` Steuerelement für die Eingabe neuer Abteilungen und gibt einen Handler für das `ItemInserting` Ereignis des Steuer Elements an, sodass Sie den `Administrator` Fremdschlüssel Wert festlegen können. Am Ende befindet sich ein `ValidationSummary`-Steuerelement zum Anzeigen von Fehlermeldungen.

Öffnen Sie *DepartmentsAdd.aspx.cs* , und fügen Sie die folgende `using`-Anweisung hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Fügen Sie die folgende Klassen Variable und die folgenden Methoden hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

Die `Page_Init`-Methode ermöglicht dynamische Daten Funktionalität. Der-Handler für das `Init` Ereignis des `DropDownList`-Steuer Elements speichert einen Verweis auf das-Steuerelement, und der-Handler für das `Inserting` Ereignis des `DetailsView`-Steuer Elements verwendet diesen Verweis, um den `PersonID` Wert des ausgewählten Dozenten zu erhalten und die `Administrator` Fremdschlüssel Eigenschaft der `Department` Entität zu aktualisieren.

Führen Sie die Seite aus, fügen Sie Informationen für eine neue Abteilung hinzu, und klicken Sie dann auf den Link **Einfügen** .

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Geben Sie Werte für eine andere neue Abteilung ein. Geben Sie im Feld **Budget** eine Zahl größer als 1.000.000,00 ein, und wechseln Sie zum nächsten Feld. Im Feld wird ein Sternchen angezeigt, und wenn Sie den Mauszeiger darüber halten, wird die Fehlermeldung angezeigt, die Sie in den Metadaten für dieses Feld eingegeben haben.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Klicken Sie auf **Einfügen**, um die Fehlermeldung anzuzeigen, die vom `ValidationSummary`-Steuerelement am unteren Rand der Seite angezeigt wird.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Schließen Sie dann den Browser, und öffnen Sie die Seite " *Departments. aspx* ". Fügen Sie der Seite " *Departments. aspx* " die Löschfunktion hinzu, indem Sie ein `DeleteMethod` Attribut zum `ObjectDataSource`-Steuerelement und ein `DataKeyNames`-Attribut zum `GridView` Steuerelement hinzufügen. Die öffnenden Tags für diese Steuerelemente ähneln nun dem folgenden Beispiel:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Führen Sie die Seite aus.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Löschen Sie die Abteilung, die Sie beim Durchlaufen der Seite *departmentsadd. aspx* hinzugefügt haben.

## <a name="adding-update-functionality"></a>Hinzufügen von Update Funktionen

Öffnen Sie *SchoolRepository.cs* , und fügen Sie die folgende `Update`-Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Wenn Sie auf der Seite " *Departments. aspx* " auf **Aktualisieren** klicken, erstellt das `ObjectDataSource`-Steuerelement zwei `Department` Entitäten, die an die `UpdateDepartment`-Methode übergeben werden. Eine enthält die ursprünglichen Werte, die im Ansichts Zustand gespeichert wurden, und die andere enthält die neuen Werte, die im `GridView` Steuerelement eingegeben wurden. Der Code in der `UpdateDepartment`-Methode übergibt die `Department` Entität mit den ursprünglichen Werten an die `Attach`-Methode, um die Überwachung zwischen der Entität und den Daten in der Datenbank herzustellen. Anschließend übergibt der Code die `Department` Entität mit den neuen Werten an die `ApplyCurrentValues`-Methode. Der Objekt Kontext vergleicht die alten und neuen Werte. Wenn ein neuer Wert von einem alten Wert abweicht, ändert der Objekt Kontext den Eigenschafts Wert. Die `SaveChanges`-Methode aktualisiert dann nur die geänderten Spalten in der Datenbank. (Wenn die Update-Funktion für diese Entität einer gespeicherten Prozedur zugeordnet wurde, würde jedoch unabhängig davon, welche Spalten geändert wurden, die gesamte Zeile aktualisiert.)

Öffnen Sie die Datei " *Departments. aspx* ", und fügen Sie dem `DepartmentsObjectDataSource`-Steuerelement die folgenden Attribute hinzu:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Dies bewirkt, dass alte Werte im Ansichts Zustand gespeichert werden, sodass Sie mit den neuen Werten in der `Update`-Methode verglichen werden können.
- `OldValuesParameterFormatString="orig{0}"`   
 Dadurch wird dem Steuerelement mitgeteilt, dass der Name des ursprünglichen Values-Parameters `origDepartment` ist.

Das Markup für das öffnende Tag des `ObjectDataSource`-Steuer Elements ähnelt nun dem folgenden Beispiel:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Fügen Sie ein `OnRowUpdating="DepartmentsGridView_RowUpdating"`-Attribut zum `GridView`-Steuerelement hinzu. Sie verwenden diese Eigenschaft, um den Wert der `Administrator`-Eigenschaft basierend auf der Zeile festzulegen, die der Benutzer in einer Dropdown Liste auswählt. Das `GridView` öffnende Tag ähnelt nun dem folgenden Beispiel:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Fügen Sie dem `GridView`-Steuerelement unmittelbar nach dem `ItemTemplate` Steuerelement für diese Spalte ein `EditItemTemplate`-Steuerelement für die Spalte `Administrator` hinzu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Dieses `EditItemTemplate` Steuerelement ähnelt dem `InsertItemTemplate`-Steuerelement auf der Seite *departmentsadd. aspx* . Der Unterschied besteht darin, dass der Anfangswert des-Steuer Elements mit dem `SelectedValue`-Attribut festgelegt wird.

Fügen Sie vor dem `GridView`-Steuerelement wie auf der Seite *departmentsadd. aspx* ein `ValidationSummary`-Steuerelement hinzu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Öffnen Sie *Departments.aspx.cs* , und fügen Sie unmittelbar nach der Deklaration der partiellen Klasse den folgenden Code hinzu, um ein privates Feld zu erstellen, das auf das `DropDownList` Steuerelement verweist

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Fügen Sie dann Handler für das `Init` Ereignis des `DropDownList`-Steuer Elements und das `RowUpdating` Ereignis des `GridView`-Steuer Elements hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Der-Handler für das `Init`-Ereignis speichert einen Verweis auf das `DropDownList`-Steuerelement im-Feld der Klasse. Der-Handler für das `RowUpdating`-Ereignis verwendet den-Verweis, um den vom Benutzer eingegebenen Wert zu erhalten und ihn auf die `Administrator`-Eigenschaft der `Department`-Entität anzuwenden.

Verwenden Sie die Seite *departmentsadd. aspx* , um eine neue Abteilung hinzuzufügen, führen Sie dann die Seite *Departments. aspx* aus, und klicken Sie in der hinzugefügten Zeile auf **Bearbeiten** .

> [!NOTE]
> Aufgrund ungültiger Daten in der Datenbank können keine Zeilen bearbeitet werden, die Sie nicht hinzugefügt haben (d. h., Sie waren bereits in der Datenbank vorhanden). die Administratoren für die Zeilen, die mit der-Datenbank erstellt wurden, sind Schüler/Studenten. Wenn Sie versuchen, eines dieser Elemente zu bearbeiten, wird eine Fehlerseite angezeigt, die einen Fehler meldet, wie `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`

[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Wenn Sie einen ungültigen **Budget** Betrag eingeben und dann auf **Aktualisieren**klicken, sehen Sie das gleiche Sternchen und die Fehlermeldung, die Sie auf der Seite " *Departments. aspx* " gesehen haben.

Ändern Sie einen Feldwert, oder wählen Sie einen anderen Administrator, und klicken Sie auf **Aktualisieren**. Die Änderung wird angezeigt.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Dadurch wird die Einführung in die Verwendung des `ObjectDataSource`-Steuer Elements für grundlegende CRUD-Vorgänge (erstellen, lesen, aktualisieren, löschen) mit dem Entity Framework abgeschlossen. Sie haben eine einfache n-Tier-Anwendung erstellt, aber die Geschäftslogik Ebene ist weiterhin eng an die Datenzugriffs Ebene gekoppelt, die automatisierte Komponententests erschwert. Im folgenden Tutorial erfahren Sie, wie Sie das Repository-Muster implementieren, um Unittests zu vereinfachen.

> [!div class="step-by-step"]
> [Weiter](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
