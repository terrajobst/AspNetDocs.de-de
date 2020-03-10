---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Die ersten Schritte mit Entity Framework 4,0 Database First und ASP.NET 4 Web Forms | Microsoft-Dokumentation
author: tdykstra
description: Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie ASP.net-Web Forms Anwendungen mit den Entity Framework 4,0 und Visual Studio 2010 erstellen...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: fd88b32ad2a65e5b4c7ead15f0d6dc5dc6e97e75
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78511677"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Der Einstieg in Entity Framework 4,0 Database First und ASP.NET 4 Web Forms

von [Tom Dykstra](https://github.com/tdykstra)

> Die Beispiel-Webanwendung der Beispiel-Web-App veranschaulicht, wie Sie ASP.net-Web Forms Anwendungen mithilfe von Entity Framework 4,0 und Visual Studio 2010 erstellen. Die Beispielanwendung ist eine Website für eine fiktive. Sie enthält Funktionen wie die Zulassung von Studenten, die Erstellung von Kursen und Aufgaben von Dozenten.
> 
> Das Tutorial zeigt Beispiele in C#. Das [herunterladbare Beispiel](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) enthält Code C# sowohl in als auch in Visual Basic.
> 
> ## <a name="database-first"></a>Database First
> 
> Es gibt drei Möglichkeiten, wie Sie mit Daten in der Entity Framework arbeiten können: *Database First*, *Model First*und *Code First*. Dieses Tutorial ist für Database First. Informationen zu den Unterschieden zwischen diesen Workflows und Anleitungen zum Auswählen des besten für Ihr Szenario finden Sie unter [Entity Framework Entwicklungs Workflows](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> In dieser tutorialreihe wird das ASP.net-Web Forms Modell verwendet, und es wird davon ausgegangen, dass Sie mit der Arbeit mit ASP.net Web Forms in Visual Wenn Sie dies nicht tun, finden Sie weitere Informationen unter [Getting Started with ASP.NET 4,5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Wenn Sie lieber mit dem ASP.NET MVC-Framework arbeiten möchten, finden Sie weitere Informationen unter [Getting Started with the Entity Framework Using ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
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

## <a name="overview"></a>Übersicht

Die Anwendung, die Sie in diesen Tutorials aufbauen, ist eine einfache University-Website.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Benutzer können Informationen zu den Studenten, Kursen und Dozenten abrufen. Einige der Bildschirme, die Sie erstellen, sind unten dargestellt.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Erstellen der Webanwendung

Öffnen Sie Visual Studio, und erstellen Sie ein neues ASP.NET-Webanwendungs Projekt mithilfe der Vorlage **ASP.NET-Webanwendung** , um das Tutorial zu starten:

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Mit dieser Vorlage wird ein Webanwendungs Projekt erstellt, das bereits ein Stylesheet und Masterseiten enthält:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Öffnen Sie die Datei " *Site. Master* ", und ändern Sie "My ASP.NET Application" in "Configuration Manager".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Suchen Sie das *Menü* Steuerelement mit dem Namen `NavigationMenu`, und ersetzen Sie es durch das folgende Markup, mit dem Menü Elemente für die zu erstellenden Seiten hinzugefügt werden.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Öffnen Sie die Seite " *default. aspx* ", und ändern Sie das `Content`-Steuerelement mit dem Namen `BodyContent`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Sie verfügen jetzt über eine einfache Startseite mit Links zu den verschiedenen Seiten, die Sie erstellen:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Erstellen der Datenbank

Für diese Tutorials verwenden Sie den Entity Framework Data Model-Designer, um das Datenmodell basierend auf einer vorhandenen Datenbank automatisch zu erstellen (oftmals als *Database First-* Ansatz bezeichnet). Eine Alternative, die in dieser tutorialreihe nicht behandelt wird, besteht darin, das Datenmodell manuell zu erstellen und anschließend Skripts zu generieren, die die Datenbank erstellen (der *Model-First-* Ansatz).

Der nächste Schritt bei der in diesem Tutorial verwendeten Datenbank-First-Methode besteht im Hinzufügen einer Datenbank zum Standort. Die einfachste Möglichkeit besteht darin, zuerst das Projekt herunterzuladen, das in diesem Tutorial läuft. Klicken Sie dann mit der rechten Maustaste auf den Ordner *App\_Data* , wählen Sie **Vorhandenes Element hinzufügen**aus, und wählen Sie die Datenbankdatei *School. mdf* aus dem heruntergeladenen Projekt

Eine Alternative besteht darin, die Anweisungen unter [Erstellen der Beispieldatenbank "School](https://msdn.microsoft.com/library/bb399731.aspx)" zu befolgen. Unabhängig davon, ob Sie die Datenbank herunterladen oder erstellen, kopieren Sie die Datei " *School. mdf* " aus dem folgenden Ordner in den *App-\_Daten* Ordner Ihrer Anwendung:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Bei diesem Speicherort der *MDF* -Datei wird davon ausgegangen, dass Sie SQL Server 2008 Express verwenden.)

Wenn Sie die Datenbank aus einem Skript erstellen, führen Sie die folgenden Schritte aus, um ein Daten Bank Diagramm zu erstellen:

1. Erweitern Sie in **Server-Explorer**nacheinander **Datenverbindungen**und *School. mdf*, klicken Sie mit der rechten Maustaste auf Daten **Bank Diagramme**, und wählen Sie **Neues Diagramm hinzufügen**aus.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Wählen Sie alle Tabellen aus, und klicken Sie dann auf **Hinzufügen**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server erstellt ein Daten Bank Diagramm, in dem Tabellen, Spalten in den Tabellen und Beziehungen zwischen den Tabellen angezeigt werden. Sie können die Tabellen verschieben, um Sie beliebig zu organisieren.
3. Speichern Sie das Diagramm als "schooldiagram", und schließen Sie es.

Wenn Sie die Datei " *School. mdf* " herunterladen, die in diesem Tutorial läuft, können Sie das Daten Bank Diagramm anzeigen, indem Sie unter **Daten Bank Diagramme** in **Server-Explorer**auf **schooldiagram** doppelklicken.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Das Diagramm sieht in etwa wie folgt aus (die Tabellen befinden sich möglicherweise an verschiedenen Speicherorten, die hier angezeigt werden):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Erstellen des Entity Framework Datenmodells

Nun können Sie ein Entity Framework Datenmodell aus dieser Datenbank erstellen. Sie könnten das Datenmodell im Stamm Ordner der Anwendung erstellen, aber in diesem Tutorial platzieren Sie es in einem Ordner namens *dal* (für die Datenzugriffs Ebene).

Fügen Sie in **Projektmappen-Explorer**einen Projektordner mit dem Namen *dal* hinzu (stellen Sie sicher, dass er sich unter dem Projekt befindet, nicht unter der Projekt Mappe).

Klicken Sie mit der rechten Maustaste auf den Ordner *dal* , und wählen Sie **Hinzufügen** und **Neues Element**. Wählen Sie unter **installierte Vorlagen**die Option **Daten**aus, wählen Sie die Vorlage **ADO.NET Entity Data Model** aus, nennen Sie Sie *SchoolModel. edmx*, und klicken Sie dann auf **Hinzufügen**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Dadurch wird der Entity Data Model-Assistent gestartet. Im ersten Schritt des Assistenten ist die Option **aus Datenbank generieren** standardmäßig ausgewählt. Klicken Sie auf **Weiter**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

Überlassen Sie im Schritt **Wählen Sie Ihre Datenverbindung** aus die Standardwerte, und klicken Sie auf **weiter**. Die Datenbank "School" ist standardmäßig ausgewählt, und die Verbindungs Einstellung wird in der Datei " *Web. config* " als " **SchoolEntities**" gespeichert.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

Wählen Sie im Schritt **Wählen Sie Ihre Datenbankobjekte** aus alle Tabellen mit Ausnahme von `sysdiagrams` aus (die für das zuvor erstellte Diagramm erstellt wurde), und klicken Sie dann auf **Fertig**stellen.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Nachdem die Erstellung des Modells abgeschlossen ist, zeigt Visual Studio eine grafische Darstellung der Entity Framework Objekte (Entitäten) an, die den Datenbanktabellen entsprechen. (Wie beim Daten Bank Diagramm unterscheidet sich der Speicherort einzelner Elemente möglicherweise von dem, was Sie in dieser Abbildung sehen. Wenn Sie möchten, können Sie die Elemente so verschieben, dass Sie der Abbildung entsprechen.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Untersuchen des Entity Framework Datenmodells

Sie sehen, dass das Entitäts Diagramm dem Daten Bank Diagramm sehr ähnlich aussieht, mit einigen Unterschieden. Ein Unterschied besteht darin, dass am Ende jeder Zuordnung Symbole hinzugefügt werden, die den Typ der Zuordnung angeben (Tabellen Beziehungen werden als Entitäts Zuordnungen im Datenmodell bezeichnet):

- Eine eins-zu-NULL-or-one-Zuordnung wird durch "1" und "0.. 1" dargestellt.

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    In diesem Fall kann eine `Person` Entität einer `OfficeAssignment` Entität zugeordnet werden. Eine `OfficeAssignment` Entität muss einer `Person` Entität zugeordnet werden. Anders ausgedrückt: ein Dozent kann einem Büro zugewiesen werden oder nicht, und jedes Büro kann nur einem Dozenten zugewiesen werden.
- Eine 1: n-Zuordnung wird durch "1" und "\*" dargestellt.

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    In diesem Fall kann eine `Person` Entität `StudentGrade` Entitäten zugeordnet werden. Eine `StudentGrade` Entität muss einer `Person` Entität zugeordnet werden. `StudentGrade` Entitäten stellen in dieser Datenbank tatsächlich registrierte Kurse dar. Wenn ein Student an einem Kurs angemeldet ist und noch kein Wert vorhanden ist, ist die `Grade`-Eigenschaft NULL. Anders ausgedrückt: ein Student ist möglicherweise noch nicht in Kursen angemeldet, kann in einem Kurs registriert werden oder kann in mehreren Kursen registriert werden. Jede Qualität in einem registrierten Kurs gilt nur für einen Schüler/Student.
- Eine m:n-Zuordnung wird durch "\*" und "\*" dargestellt.

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    In diesem Fall kann eine `Person` Entität `Course` Entitäten zugeordnet werden, und umgekehrt ist auch true: eine `Course` Entität kann `Person` Entitäten zugeordnet werden. Anders ausgedrückt: ein Dozent kann mehrere Kurse unterrichten, und ein Kurs kann von mehreren Dozenten unterrichtet werden. (In dieser Datenbank gilt diese Beziehung nur für Dozenten; Schüler und Studenten werden nicht mit Kursen verknüpft. Schüler/Studenten sind über die Tabelle studentgrades mit Kursen verknüpft.)

Ein weiterer Unterschied zwischen dem Daten Bank Diagramm und dem Datenmodell ist der zusätzliche **Navigations Eigenschaften** Abschnitt für jede Entität. Eine Navigations Eigenschaft einer Entität verweist auf Verwandte Entitäten. Die `Courses`-Eigenschaft in einer `Person`-Entität enthält z. b. eine Auflistung aller `Course`-Entitäten, die mit dieser `Person` Entität verknüpft sind.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Ein weiterer Unterschied zwischen der Datenbank und dem Datenmodell besteht darin, dass die `CourseInstructor` Zuordnungs Tabelle nicht vorhanden ist, die in der Datenbank verwendet wird, um die `Person` und `Course` Tabellen in einer m:n-Beziehung zu verknüpfen. Die Navigations Eigenschaften ermöglichen es Ihnen, Verwandte `Course` Entitäten aus der Entität `Person` und verknüpfte `Person` Entitäten aus der `Course` Entität zu erhalten, sodass es nicht erforderlich ist, die Zuordnungs Tabelle im Datenmodell darzustellen.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Nehmen Sie für die Zwecke dieses Tutorials an, dass die Spalte `FirstName` der `Person` Tabelle tatsächlich den Vornamen und den Vornamen einer Person enthält. Der Name des Felds soll so geändert werden, dass der Datenbankadministrator (DBA) die Datenbank jedoch möglicherweise nicht ändern möchte. Sie können den Namen der `FirstName` Eigenschaft im Datenmodell ändern und gleichzeitig die Datenbank unverändert lassen.

Klicken Sie im Designer mit der rechten Maustaste auf **FirstName** in der `Person`-Entität, und wählen Sie dann **Umbenennen**aus.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Geben Sie den neuen Namen "firstmittelname" ein. Dies ändert die Art und Weise, in der Sie auf die Spalte im Code verweisen, ohne die Datenbank zu ändern.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

Der Modell Browser bietet eine weitere Möglichkeit, die Datenbankstruktur, die Datenmodell Struktur und die Zuordnung zwischen Ihnen anzuzeigen. Um dies anzuzeigen, klicken Sie mit der rechten Maustaste auf einen leeren Bereich im Entity Designer, und klicken Sie dann auf **Modell Browser**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

Der Bereich **Modell Browser** zeigt eine Strukturansicht an. (Der Bereich **Modell Browser** ist möglicherweise mit dem Bereich **Projektmappen-Explorer** angedockt.) Der **SchoolModel** -Knoten stellt die Datenmodell Struktur dar, und der **SchoolModel. Store** -Knoten stellt die Datenbankstruktur dar.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Erweitern Sie **SchoolModel. Store** , um die Tabellen anzuzeigen, **Tabellen/Sichten** zu sehen, um Tabellen anzuzeigen, und erweitern Sie dann **Kurs** , um die Spalten in einer Tabelle anzuzeigen.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Erweitern Sie **SchoolModel**, erweitern Sie **Entitäts Typen**, und erweitern Sie dann den Knoten **Kurs** , um die Entitäten und die Eigenschaften innerhalb der Entitäten anzuzeigen.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

Im Designer oder im **Modell Browser** Bereich können Sie sehen, wie die Entity Framework die Objekte der beiden Modelle miteinander verknüpft. Klicken Sie mit der rechten Maustaste auf die Entität `Person` und wählen Sie **Tabellen Zuordnung**

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Dadurch wird das Fenster **Mappingdetails** geöffnet. Beachten Sie, dass in diesem Fenster angezeigt wird, dass die Daten Bank Spalte `FirstName` `FirstMidName`zugeordnet ist, was Sie im Datenmodell umbenannt haben.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Der Entity Framework verwendet XML zum Speichern von Informationen über die Datenbank, das Datenmodell und die Zuordnungen zwischen Ihnen. Die Datei " *SchoolModel. edmx* " ist tatsächlich eine XML-Datei, die diese Informationen enthält. Der Designer rendert die Informationen in einem grafischen Format, aber Sie können die Datei auch als XML anzeigen, indem Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die *edmx* -Datei klicken, auf **Öffnen mit**klicken und den **XML-Editor (Text)** auswählen. (Der Datenmodell-Designer und ein XML-Editor sind nur zwei verschiedene Möglichkeiten, die gleiche Datei zu öffnen und zu bearbeiten, sodass Sie den Designer nicht öffnen und die Datei nicht gleichzeitig in einem XML-Editor öffnen können.)

Sie haben nun eine Website, eine Datenbank und ein Datenmodell erstellt. In der nächsten exemplarischen Vorgehensweise beginnen Sie mit der Arbeit mit Daten, indem Sie das Datenmodell und das ASP.net `EntityDataSource`-Steuerelement verwenden.

> [!div class="step-by-step"]
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-2.md)
