---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Erstellen eines Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung (1 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Eine neuere Version dieser tutorialreihe ist für Visual Studio 2013, Entity Framework 6 und MVC 5 verfügbar. Die Beispiel-Webanwendung der Configuration Manager-Website...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8ee5aa22b6b2329b01d41437f30508e28a2288b2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595968"
---
# <a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Erstellen eines Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung (1 von 10)

von [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > Eine [neuere Version dieser tutorialreihe](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) ist für Visual Studio 2013, Entity Framework 6 und MVC 5 verfügbar.
> 
> 
> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie ASP.NET MVC 4-Anwendungen mithilfe von Entity Framework 5 und Visual Studio 2012 erstellt werden. Bei der Beispiel-App handelt es sich um eine Website für die fiktive Contoso University. Sie enthält Funktionen wie die Zulassung von Studenten, die Erstellung von Kursen und Aufgaben von Dozenten. In dieser tutorialreihe wird erläutert, wie Sie die Beispielanwendung "" der Anwendung "" erstellen. Sie können [die abgeschlossene Anwendung herunterladen](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Code First
> 
> Es gibt drei Möglichkeiten, wie Sie mit Daten in der Entity Framework arbeiten können: *Database First*, *Model First*und *Code First*. Dieses Tutorial ist für Code First. Informationen zu den Unterschieden zwischen diesen Workflows und Anleitungen zum Auswählen des besten für Ihr Szenario finden Sie unter [Entity Framework Entwicklungs Workflows](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> Die Beispielanwendung basiert auf [ASP.NET MVC](../../../index.md). Wenn Sie lieber mit dem ASP.net-Web Forms Modell arbeiten möchten, finden Sie weitere Informationen unter [Modell Bindung und Web Forms](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) -tutorialreihe und [ASP.NET Data Access-Inhalts](../../../../whitepapers/aspnet-data-access-content-map.md)Zuordnung.
> 
> ## <a name="software-versions"></a>Software Versionen
> 
> | **Im Tutorial** | **Funktioniert auch mit** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express für Web. Dies wird automatisch vom Windows Azure SDK installiert, wenn Sie nicht bereits über vs 2012 oder vs 2012 Express für Web verfügen. Visual Studio 2013 sollte funktionieren, aber das Lernprogramm wurde nicht mit dem Tutorial getestet, und einige Menü Optionen und Dialogfelder unterscheiden sich. Die [vs 2013-Version des Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510) ist für die Windows Azure-Bereitstellung erforderlich. |
> | .NET 4.5 | Die meisten der gezeigten Features funktionieren in .NET 4, andere jedoch nicht. Beispielsweise erfordert die Aufzählungs Unterstützung in EF .NET 4,5. |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2,1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Wenn Sie die Schritte für die Windows Azure-Bereitstellung überspringen, benötigen Sie das SDK nicht. Wenn eine neue Version des SDK freigegeben wird, wird die neuere Version durch den Link installiert. In diesem Fall müssen Sie möglicherweise einige der Anweisungen an neue Benutzeroberfläche und Features anpassen. |
> 
> ## <a name="questions"></a>Fragen
> 
> Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im Forum [ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), im [Forum zu Entity Framework und LINQ to Entities](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.
> 
> ## <a name="acknowledgments"></a>Danksagungen
> 
> Im letzten Tutorial der Reihe finden [Sie Bestätigungen und einen Hinweis zu VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Ursprüngliche Version des Tutorials
> 
> Die ursprüngliche Version des Tutorials ist im [e-book EF 4,1/MVC 3](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC)verfügbar.

## <a name="the-contoso-university-web-application"></a>Die Webanwendung der "Web.

Bei der Anwendung, die Sie mithilfe dieser Tutorials erstellen, handelt es sich um eine einfache Universitätswebsite.

Benutzer können Informationen zu den Studenten, Kursen und Dozenten abrufen. Nachfolgend werden einige Anzeigen dargestellt, die erstellt werden sollen.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Der Benutzeroberflächenstil dieser Website orientiert sich an den integrierten Vorlagen, sodass Sie mit dieses Tutorial hauptsächlich auf die Verwendung von Entity Framework konzentriert.

## <a name="prerequisites"></a>Erforderliche Voraussetzungen

Bei den Anweisungen und Screenshots in diesem Tutorial wird davon ausgegangen, dass Sie [Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) oder [Visual Studio 2012 Express für Web](https://go.microsoft.com/fwlink/?LinkID=275131)verwenden, wobei das aktuellste Update und das Azure SDK für .net ab Juli 2013 installiert sind. Sie können all dies mit folgendem Link erreichen:

[Azure SDK für .net (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Wenn Sie Visual Studio installiert haben, werden mit dem obigen Link fehlende Komponenten installiert. Wenn Sie nicht über Visual Studio verfügen, wird der Link Visual Studio 2012 Express für Web installieren. Sie können Visual Studio 2013 verwenden, aber einige der erforderlichen Prozeduren und Bildschirme unterscheiden sich.

## <a name="create-an-mvc-web-application"></a>Erstellen einer MVC-Webanwendung

Öffnen Sie Visual Studio, und erstellen C# Sie ein neues Projekt mit dem Namen "contosouniversity" mithilfe der Vorlage **ASP.NET MVC 4-Webanwendung** . Stellen Sie sicher, dass Sie **.NET Framework 4,5** als Ziel verwenden (Sie verwenden [`enum` Eigenschaften](https://msdn.microsoft.com/data/hh859576.aspx)und benötigen .NET 4,5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Vorlage **Internet Anwendung** aus.

Lassen Sie die **Razor** -Ansichts-Engine aktiviert, und lassen Sie das Kontrollkästchen Komponenten **Testprojekt erstellen** deaktiviert.

Klicken Sie auf **OK**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Einrichten des Website Stils

Sie können das Websitemenü, das Layout und die Startseite über einige Änderungen einrichten.

Öffnen Sie *views\shared\\_Layout. cshtml*, und ersetzen Sie den Inhalt der Datei durch den folgenden Code. Die Änderungen werden hervorgehoben.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Durch diesen Code werden folgende Änderungen vorgenommen:

- Ersetzt die Vorlagen Instanzen von "My ASP.NET MVC Application" und "Your Logo here" durch "".
- Fügt mehrere Aktions Verknüpfungen hinzu, die später in diesem Tutorial verwendet werden.

Ersetzen Sie in *views\home\index.cshtml*den Inhalt der Datei durch den folgenden Code, um die Vorlagen Absätze zu ASP.net und MVC auszuschließen:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Ändern Sie in *controllers\homecontroller.cs*den Wert für `ViewBag.Message` in der `Index` Action-Methode in "Willkommen bei der Configuration Manager-Methode", wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Drücken Sie STRG + F5, um die Website auszuführen. Die Startseite wird mit dem Hauptmenü angezeigt.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Erstellen des Datenmodells

Als nächstes erstellen Sie Entitätsklassen für die Contoso University-Anwendung. Beginnen Sie mit den folgenden drei Entitäten:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Es besteht eine 1:n-Beziehung zwischen den Entitäten `Student` und `Enrollment`. Außerdem besteht eine 1:n-Beziehung zwischen den Entitäten `Course` und `Enrollment`. Das bedeutet, dass ein Student für beliebig viele Kurse angemeldet sein kann und sich für jeden Kurs eine beliebige Anzahl von Studenten anmelden kann.

In den folgenden Abschnitten erstellen Sie für jede dieser Entitäten eine Klasse.

> [!NOTE]
> Wenn Sie versuchen, das Projekt zu kompilieren, bevor Sie alle diese Entitäts Klassen erstellt haben, erhalten Sie Compilerfehler.

### <a name="the-student-entity"></a>Die Student-Entität

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Erstellen Sie im Ordner *Models* *Student.cs* , und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Die `StudentID`-Eigenschaft fungiert als Primärschlüsselspalte der Datenbanktabelle, die dieser Klasse entspricht. Standardmäßig interpretiert das Entity Framework eine Eigenschaft mit dem Namen `ID` oder *className* `ID` als Primärschlüssel.

Die `Enrollments`-Eigenschaft ist eine *Navigationseigenschaft*. Navigationseigenschaften enthalten andere Entitäten, die dieser Entität zugehörig sind. In diesem Fall enthält die `Enrollments`-Eigenschaft einer `Student`-Entität alle `Enrollment`-Entitäten, die mit dieser `Student` Entität verknüpft sind. Anders ausgedrückt: Wenn eine angegebene `Student` Zeile in der Datenbank über zwei verknüpfte `Enrollment` Zeilen verfügt (Zeilen, die den Primärschlüssel Wert dieses Schülers in der `StudentID` Fremdschlüssel Spalte enthalten), enthält die `Enrollments` Navigations Eigenschaft der `Student` Entität diese beiden `Enrollment` Entitäten.

Navigations Eigenschaften werden in der Regel als `virtual` definiert, damit Sie von bestimmten Entity Framework Funktionen wie *Lazy Loading*profitieren können. (Lazy Load wird später in diesem Artikel im Tutorial [Lesen verwandter Daten](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) erläutert.

Wenn eine Navigationseigenschaft mehrere Entitäten enthalten kann (wie bei m:n- oder 1:n-Beziehungen), muss dessen Typ aus einer Liste bestehen, in der Einträge hinzugefügt, gelöscht und aktualisiert werden können – z.B.: `ICollection`.

### <a name="the-enrollment-entity"></a>Die Entität "Registrierung"

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Erstellen Sie im Ordner *Models* (Modelle) die Datei *Enrollment.cs*, und ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Die Grade-Eigenschaft ist eine [Enumeration](https://msdn.microsoft.com/data/hh859576.aspx). Das Fragezeichen nach der `Grade`-Typdeklaration gibt an, dass die `Grade`-Eigenschaft [NULL-Werte zulässt](https://msdn.microsoft.com/library/2cf62fcy.aspx). Ein Grade, der NULL ist, unterscheidet sich von 0 (null) – NULL bedeutet, dass eine Klasse noch nicht bekannt ist oder noch nicht zugewiesen wurde.

Bei der `StudentID`-Eigenschaft handelt es sich um einen Fremdschlüssel und `Student` ist die entsprechende Navigationseigenschaft. Eine `Enrollment`-Entität wird einer `Student`-Entität zugeordnet, damit die Eigenschaft nur eine `Student`-Entität enthalten kann. Dies steht im Gegensatz zu der bereits erläuterten `Student.Enrollments`-Navigationseigenschaft, die mehrere `Enrollment`-Entitäten enthalten kann.

Bei der `CourseID`-Eigenschaft handelt es sich um einen Fremdschlüssel und `Course` ist die entsprechende Navigationseigenschaft. Die `Enrollment`-Entität wird einer `Course`-Entität zugeordnet.

### <a name="the-course-entity"></a>Die Entität „Course“

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Erstellen Sie im Ordner *Models* den Ordner *Course.cs*, und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

Die `Enrollments`-Eigenschaft ist eine Navigationseigenschaft. `Course`-Entitäten können sich auf jede beliebige Anzahl von `Enrollment`-Entitäten beziehen.

Weitere Informationen zu [[databasegenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([databasegeneratedoption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx)). None)]-Attribut im nächsten Tutorial. Im Grunde können Sie über dieses Attribut den Primärschlüssel für den Kurs angeben, anstatt ihn von der Datenbank generieren zu lassen.

## <a name="create-the-database-context"></a>Erstellen des Datenbankkontexts

Die Hauptklasse, die Entity Framework Funktionen für ein bestimmtes Datenmodell koordiniert, ist die *Daten Bank Kontext* Klasse. Sie erstellen diese Klasse durch Ableiten von der [System. Data. Entity. dbcontext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) -Klasse. Sie geben in Ihrem Code an, welche Entitäten im Datenmodell enthalten sind. Außerdem können Sie bestimmte Entity Framework-Verhalten anpassen. In diesem Projekt heißt die Klasse `SchoolContext`.

Erstellen Sie einen Ordner mit dem Namen *dal* (für die Datenzugriffs Ebene). Erstellen Sie in diesem Ordner eine neue Klassendatei mit dem Namen *SchoolContext.cs*, und ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Mit diesem Code wird eine [dbset](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) -Eigenschaft für jede Entitätenmenge erstellt. In Entity Framework-Terminologie entspricht eine *Entitätenmenge* in der Regel einer Datenbanktabelle, und eine *Entität* entspricht einer Zeile in der Tabelle.

Die `modelBuilder.Conventions.Remove`-Anweisung in der [onmodelcreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) -Methode verhindert, dass Tabellennamen pluralisiert werden. Wenn Sie dies nicht getan haben, würden die generierten Tabellen `Students`, `Courses`und `Enrollments`benannt werden. Stattdessen werden die Tabellennamen `Student`, `Course`und `Enrollment`. Entwickler sind sich uneinig darüber, ob Tabellennamen im Plural stehen sollten oder nicht. In diesem Tutorial wird das Singular-Formular verwendet, aber es ist wichtig, dass Sie das gewünschte Formular auswählen können, indem Sie diese Codezeile einschließen oder weglassen.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[Localdb](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) ist eine vereinfachte Version der SQL Server Express Datenbank-Engine, die bei Bedarf gestartet wird und im Benutzermodus ausgeführt wird. Localdb wird in einem speziellen Ausführungs Modus von SQL Server Express ausgeführt, der es Ihnen ermöglicht, mit Datenbanken als *MDF* -Dateien zu arbeiten. Normalerweise werden localdb-Datenbankdateien im *App-\_Daten* Ordner eines Webprojekts gespeichert. Mit der benutzerinstanzfunktion in SQL Server Express können Sie auch mit *MDF* -Dateien arbeiten, aber die benutzerinstanzfunktion ist veraltet. Daher wird localdb zum Arbeiten mit *MDF* -Dateien empfohlen.

In der Regel wird SQL Server Express nicht für produktionsweb Anwendungen verwendet. Localdb wird insbesondere für den Einsatz in der Produktion mit einer Webanwendung nicht empfohlen, da es nicht für die Verwendung mit IIS konzipiert ist.

In Visual Studio 2012 und höheren Versionen wird localdb standardmäßig mit Visual Studio installiert. In Visual Studio 2010 und früheren Versionen wird SQL Server Express (ohne localdb) standardmäßig mit Visual Studio installiert. Sie müssen es manuell installieren, wenn Sie Visual Studio 2010 verwenden.

In diesem Tutorial arbeiten Sie mit localdb, sodass die Datenbank im *App-\_Daten* Ordner als *MDF* -Datei gespeichert werden kann. Öffnen Sie die Datei root *Web. config* , und fügen Sie der `connectionStrings` Auflistung eine neue Verbindungs Zeichenfolge hinzu, wie im folgenden Beispiel gezeigt. (Stellen Sie sicher, dass Sie die Datei " *Web. config* " im Stamm Projektordner aktualisieren. Es gibt auch eine *Web. config* -Datei, die sich im Unterordner *views* befindet, den Sie nicht aktualisieren müssen.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Standardmäßig sucht die Entity Framework nach einer Verbindungs Zeichenfolge mit dem Namen, die mit der `DbContext`-Klasse identisch ist (`SchoolContext` für dieses Projekt). Die Verbindungs Zeichenfolge, die Sie hinzugefügt haben, gibt eine localdb-Datenbank mit dem Namen *condesouniversity. mdf* im Ordner *App\_Data* an. Weitere Informationen finden Sie unter [SQL Server Verbindungs Zeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx).

Sie müssen die Verbindungs Zeichenfolge nicht angeben. Wenn Sie keine Verbindungs Zeichenfolge angeben, wird Entity Framework eine für Sie erstellen. die Datenbank ist jedoch möglicherweise nicht im *App-\_Daten* Ordner der app. Informationen dazu, wo die Datenbank erstellt wird, finden [Sie unter Code First einer neuen Datenbank](https://msdn.microsoft.com/data/jj193542).

Die `connectionStrings`-Sammlung verfügt auch über eine Verbindungs Zeichenfolge mit dem Namen `DefaultConnection` die für die Mitgliedschafts Datenbank verwendet wird. In diesem Tutorial wird die Mitgliedschafts Datenbank nicht verwendet. Der einzige Unterschied zwischen den beiden Verbindungs Zeichenfolgen ist der Datenbankname und der Name-Attribut Wert.

## <a name="set-up-and-execute-a-code-first-migration"></a>Einrichten und Ausführen einer Code First Migration

Wenn Sie zum ersten Mal mit der Entwicklung einer Anwendung beginnen, ändert sich das Datenmodell häufig, und jedes Mal, wenn das Modell geändert wird, ist die Synchronisierung mit der Datenbank nicht mehr möglich. Sie können die Entity Framework so konfigurieren, dass die Datenbank jedes Mal, wenn Sie das Datenmodell ändern, automatisch gelöscht und neu erstellt wird. Dabei handelt es sich nicht um ein frühes Problem, da Testdaten problemlos neu erstellt werden können. nach der Bereitstellung in der Produktion möchten Sie das Datenbankschema jedoch in der Regel aktualisieren, ohne die Datenbank zu löschen. Die Migrations Funktion ermöglicht Code First, die Datenbank zu aktualisieren, ohne Sie zu löschen und neu zu erstellen. In den frühen Phasen des Entwicklungsprozesses eines neuen Projekts sollten Sie [dropupatedatabaseifmodelchanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) verwenden, um die Datenbank jedes Mal, wenn das Modell geändert wird, zu löschen, neu zu erstellen und neu zu erstellen. Wenn Sie die Anwendung bereitstellen möchten, können Sie Sie in den Migrationsansatz konvertieren. In diesem Tutorial verwenden Sie nur Migrationen. Weitere Informationen finden Sie unter [Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) -und [Migrations-screencastreihe](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Aktivieren von Code First-Migrationen

1. Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager** und dann auf Paket-Manager- **Konsole**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. Geben Sie an der `PM>` Eingabeaufforderung den folgenden Befehl ein:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![Enable-Migrations-Befehl](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Dieser Befehl erstellt einen *Migrations* Ordner im condesouniversity-Projekt und fügt diesen Ordner in den Ordner eine *Configuration.cs* -Datei ein, die Sie zum Konfigurieren von Migrationen bearbeiten können.

    ![Migrations Ordner](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    Die `Configuration`-Klasse enthält eine `Seed` Methode, die aufgerufen wird, wenn die Datenbank erstellt wird, und jedes Mal, wenn Sie nach einer Datenmodell Änderung aktualisiert wird.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    Der Zweck dieser `Seed` Methode besteht darin, dass Sie Testdaten in die Datenbank einfügen können, nachdem Code First Sie erstellt oder aktualisiert hat.

### <a name="set-up-the-seed-method"></a>Einrichten der Seed-Methode

Die [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) -Methode wird ausgeführt, wenn Code First-Migrationen die Datenbank erstellt und jedes Mal, wenn die Datenbank auf die neueste Migration aktualisiert wird. Der Zweck der Seed-Methode besteht darin, dass Sie Daten in die Tabellen einfügen können, bevor die Anwendung zum ersten Mal auf die Datenbank zugreift.

In früheren Versionen von Code First galt vor der Veröffentlichung von Migrationen häufig `Seed` Methoden, Testdaten einzufügen, da bei jeder Modell Änderung während der Entwicklung die Datenbank vollständig gelöscht und neu erstellt werden musste. Mit Code First-Migrationen werden Testdaten nach der Daten Bank Änderung beibehalten, sodass das Einschließen von Testdaten in die [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) -Methode in der Regel nicht erforderlich ist. Sie möchten nicht, dass die `Seed`-Methode Testdaten einfügt, wenn Sie Migrationen verwenden, um die Datenbank in der Produktionsumgebung bereitzustellen, da die `Seed`-Methode in der Produktionsumgebung ausgeführt wird. In diesem Fall möchten Sie, dass die `Seed`-Methode nur die Daten, die in die Produktion eingefügt werden sollen, in die Datenbank einfügen. Beispielsweise kann es vorkommen, dass die Datenbank tatsächliche Abteilungsnamen in die `Department` Tabelle einschließen soll, wenn die Anwendung in der Produktionsumgebung verfügbar wird.

In diesem Tutorial verwenden Sie Migrationen für die Bereitstellung, aber ihre `Seed`-Methode fügt trotzdem Testdaten ein, um die Funktionsweise der Anwendungs Funktionalität zu vereinfachen, ohne dass viele Daten manuell eingefügt werden müssen.

1. Ersetzen Sie den Inhalt der Datei *Configuration.cs* durch den folgenden Code, durch den Testdaten in die neue Datenbank geladen werden. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    Die [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) -Methode nimmt das Daten Bank Kontext Objekt als Eingabeparameter an, und der Code in der-Methode verwendet dieses Objekt, um der Datenbank neue Entitäten hinzuzufügen. Für jeden Entitätstyp erstellt der Code eine Auflistung von neuen Entitäten, fügt Sie der entsprechenden [dbset](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) -Eigenschaft hinzu und speichert die Änderungen dann in der Datenbank. Es ist nicht erforderlich, die [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) -Methode nach jeder Gruppe von Entitäten aufzurufen, wie hier beschrieben, aber damit können Sie die Ursache eines Problems ermitteln, wenn eine Ausnahme auftritt, während der Code in die Datenbank schreibt.

    Einige der Anweisungen, die Daten einfügen, verwenden die [addorupdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) -Methode, um einen Upsert-Vorgang auszuführen. Da die `Seed`-Methode bei jeder Migration ausgeführt wird, können Sie keine Daten einfügen, da die Zeilen, die Sie hinzufügen möchten, nach der ersten Migration, mit der die Datenbank erstellt wird, bereits vorhanden sind. Der Upsert-Vorgang verhindert Fehler, die auftreten würden, wenn Sie versuchen, eine Zeile einzufügen, die bereits vorhanden ist, aber alle Änderungen an Daten ***über*** schreibt, die Sie beim Testen der Anwendung möglicherweise vorgenommen haben. Mit Testdaten in einigen Tabellen sollten Sie dies möglicherweise nicht tun: in einigen Fällen, in denen Sie Daten während des Tests ändern, möchten Sie, dass Ihre Änderungen nach Datenbankaktualisierungen verbleiben. In diesem Fall möchten Sie einen bedingten Einfügevorgang durchführen: Fügen Sie eine Zeile nur ein, wenn Sie nicht bereits vorhanden ist. Die Seed-Methode verwendet beide Ansätze.

    Der erste Parameter, der an die [addorupdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) -Methode übergeben wird, gibt die Eigenschaft an, mit der überprüft wird, ob eine Zeile bereits vorhanden ist. Für die von Ihnen bereitgestellten Test Studenten Daten kann die `LastName`-Eigenschaft für diesen Zweck verwendet werden, da jeder Nachname in der Liste eindeutig ist:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Bei diesem Code wird davon ausgegangen, dass die Nachnamen eindeutig sind. Wenn Sie einen Studenten mit einem doppelten Nachnamen manuell hinzufügen, erhalten Sie die folgende Ausnahme, wenn Sie das nächste Mal eine Migration durchführen.

    Die Sequenz enthält mehr als ein Element.

    Weitere Informationen zur `AddOrUpdate`-Methode finden Sie im Blog von Julie Lerman im Blogbeitrag zur [EF 4,3 addorupdate-Methode](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) .

    Der Code, der `Enrollment` Entitäten hinzufügt, verwendet nicht die `AddOrUpdate`-Methode. Er überprüft, ob bereits eine Entität vorhanden ist, und fügt die Entität ein, wenn Sie nicht vorhanden ist Bei diesem Ansatz werden die Änderungen, die Sie an einer Registrierungs Qualität vornehmen, beim Ausführen von Migrationen beibehalten. Der Code durchläuft die einzelnen Mitglieder der `Enrollment`[Liste](https://msdn.microsoft.com/library/6sh2ey19.aspx) , und wenn die Registrierung nicht in der Datenbank gefunden wird, wird die Registrierung der Datenbank hinzugefügt. Wenn Sie die Datenbank zum ersten Mal aktualisieren, ist die Datenbank leer, sodass jede Registrierung hinzugefügt wird.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Informationen dazu, wie Sie die `Seed` Methode Debuggen und redundante Daten wie z. b. zwei Studenten mit dem Namen "Alexander Carson" behandeln, finden Sie unter [Seeding und Debuggen Entity Framework (EF)-DSB](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) im Blog von Rick Anderson.
2. Erstellen Sie das Projekt.

### <a name="create-and-execute-the-first-migration"></a>Erstellen und Ausführen der ersten Migration

1. Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle ein: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    Der `add-migration`-Befehl fügt dem Migrations Ordner eine *[DATESTAMP]\_InitialCreate.cs* -Datei hinzu, die den Code enthält, der die Datenbank erstellt. Der erste Parameter (`InitialCreate)` wird als Dateiname verwendet und kann von Ihnen verwendet werden. in der Regel wählen Sie ein Wort oder einen Ausdruck aus, das zusammenfasst, was bei der Migration erfolgt. Beispielsweise können Sie eine spätere Migration &quot;adddepartmenttable-&quot;benennen.

    ![Migrations Ordner mit erst Migration](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    Mit der `Up`-Methode der `InitialCreate`-Klasse werden die Datenbanktabellen erstellt, die den Datenmodell-Entitätenmengen entsprechen. Diese werden von der `Down`-Methode gelöscht. Die Migrationsfunktion ruft die Methode `Up` auf, um die Datenmodelländerungen für eine Migration zu implementieren. Wenn Sie einen Befehl eingeben, um ein Rollback für das Update auszuführen, ruft die Migrationsfunktion die Methode `Down` auf. Der folgende Code zeigt den Inhalt der `InitialCreate` Datei:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    Der `update-database`-Befehl führt die `Up`-Methode aus, um die Datenbank zu erstellen, und führt dann die `Seed`-Methode aus, um die Datenbank aufzufüllen.

Eine SQL Server Datenbank wurde nun für das Datenmodell erstellt. Der Name der Datenbank lautet *condesouniversity*, und die *MDF* -Datei befindet sich in der *App\_Daten* Ordners Ihres Projekts, weil Sie dies in der Verbindungs Zeichenfolge angegeben haben.

Sie können entweder **Server-Explorer** oder **SQL Server-Objekt-Explorer** (ssox) verwenden, um die Datenbank in Visual Studio anzuzeigen. In diesem Tutorial verwenden Sie **Server-Explorer**. In Visual Studio Express 2012 für Web wird **Server-Explorer** als **Datenbank-Explorer**bezeichnet.

1. Klicken Sie im Menü **Ansicht** auf **Server-Explorer**.
2. Klicken Sie auf das Symbol **Verbindung hinzufügen** .

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Wenn Sie im Dialogfeld **Datenquelle auswählen** aufgefordert werden, klicken Sie auf **Microsoft SQL Server**, und klicken Sie dann auf **weiter**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. Geben Sie im Dialogfeld **Verbindung hinzufügen** **(localdb) \v11.0** als **Server Namen**ein. Wählen Sie unter **Datenbanknamen eingeben oder**auswählen die Option **Conto souniversity aus.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Klicken Sie auf **OK.**
6. Erweitern Sie **schoolContext** und dann **Tabellen**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Klicken Sie mit der rechten Maustaste auf die Tabelle **Student** , und klicken Sie auf **Tabellendaten anzeigen** , um die erstellten Spalten und die in die Tabelle eingefügten Zeilen anzuzeigen.

    ![Tabelle "Student"](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Erstellen eines Studenten Controllers und von Ansichten

Der nächste Schritt besteht im Erstellen eines ASP.NET-MVC-Controllers und von Sichten in der Anwendung, die mit einer dieser Tabellen funktionieren können.

1. Klicken Sie zum Erstellen eines `Student` Controllers mit der rechten Maustaste auf den Ordner **Controllers** in **Projektmappen-Explorer**, wählen Sie **Hinzufügen**aus, und klicken Sie dann auf **Controller**. Wählen Sie im Dialogfeld **Controller hinzufügen** die folgenden Optionen aus, und klicken Sie dann auf **Hinzufügen**: 

   - Controller Name: **studentcontroller**.
   - Vorlage: **MVC-Controller mit Lese-/Schreibaktionen und Ansichten unter Verwendung Entity Framework**.
   - Modell Klasse: **Student (condesouniversity. Models)** . (Wenn diese Option nicht in der Dropdown Liste angezeigt wird, erstellen Sie das Projekt, und wiederholen Sie den Vorgang.)
   - Datenkontext Klasse: **schoolContext (condesouniversity. Models)** .
   - Views: **Razor (cshtml)** . (Die Standardeinstellung.)

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio öffnet die Datei " *controllers\studentcontroller.cs* ". Sie sehen, dass eine Klassen Variable erstellt wurde, die ein Daten Bank Kontext Objekt instanziiert:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     Die `Index` Aktionsmethode Ruft eine Liste von Schülern aus der Entitätenmenge " *Students* " ab, indem die `Students`-Eigenschaft der Daten Bank Kontext Instanz gelesen wird:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     In der Ansicht *student\index.cshtml* wird diese Liste in einer Tabelle angezeigt:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Drücken Sie STRG+F5, um das Projekt auszuführen.

     Klicken Sie auf die Registerkarte **Studenten** , um die Testdaten anzuzeigen, die von der `Seed` Methode eingefügt wurden.

     ![Index Seite "Student"](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Konventionen

Die Menge an Code, den Sie schreiben müssen, damit die Entity Framework eine komplette Datenbank erstellen kann, ist aufgrund der Verwendung von *Konventionen*oder Annahmen, die der Entity Framework vornimmt, minimal. Einige davon wurden bereits notiert:

- Die pluralisierten Formen von Entitäts Klassennamen werden als Tabellennamen verwendet.
- Eigenschaftennamen von Entitäten werden als Spaltennamen verwendet.
- Entitäts Eigenschaften, die `ID` oder *className* `ID` benannt werden, werden als Primärschlüssel Eigenschaften erkannt.

Sie haben gesehen, dass Konventionen überschrieben werden können (z. b. Wenn Sie festgelegt haben, dass Tabellennamen nicht pluralisiert werden sollen), und Sie erfahren mehr über Konventionen und wie Sie diese überschreiben, indem Sie später in dieser Serie das Tutorial zum [Erstellen eines komplexeren Datenmodells](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) ausführen. Weitere Informationen finden Sie unter [Code First Konventionen](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Summary

Sie haben jetzt eine einfache Anwendung erstellt, die die Entity Framework und SQL Server Express verwendet, um Daten zu speichern und anzuzeigen. Im folgenden Tutorial erfahren Sie, wie Sie grundlegende CRUD-Vorgänge (erstellen, lesen, aktualisieren, löschen) ausführen. Sie können das Feedback unten auf dieser Seite hinterlassen. Teilen Sie uns mit, wie Sie diesen Teil des Tutorials gefallen haben und wie wir ihn verbessern können.

Links zu anderen Entity Framework Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Nächste](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
