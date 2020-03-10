---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Neuerungen in den Entity Framework 4,0 | Microsoft-Dokumentation
author: tdykstra
description: Diese tutorialreihe basiert auf der Webanwendung der Website von "Web", die in der tutorialreihe für die ersten Schritte mit Entity Framework 4,0 erstellt wurde. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 29d2bd61e8361b130e7b9415dad4fe1d5b847945
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78522147"
---
# <a name="whats-new-in-the-entity-framework-40"></a>Neue Funktionen in Entity Framework 4.0

von [Tom Dykstra](https://github.com/tdykstra)

> Diese tutorialreihe basiert auf der Webanwendung der Website "Web-so University", die durch die Reihe " [Getting Started with the Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) Tutorial Wenn Sie die vorherigen Tutorials nicht durchgearbeitet haben, können Sie die Anwendung, die Sie erstellt haben, als Ausgangspunkt für dieses Tutorial [herunterladen](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) . Sie können auch [die Anwendung herunterladen](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , die von der kompletten tutorialreihe erstellt wird. Wenn Sie Fragen zu den Tutorials haben, können Sie Sie im ASP.net- [Entity Framework Forum](https://forums.asp.net/1227.aspx)Posten.

Im vorherigen Tutorial haben Sie einige Methoden zum Maximieren der Leistung einer Webanwendung gesehen, die die Entity Framework verwendet. In diesem Tutorial werden einige der wichtigsten neuen Features in Version 4 der Entity Framework erläutert. Außerdem finden Sie hier Links zu Ressourcen, die eine umfassendere Einführung in alle neuen Features bieten. Die in diesem Tutorial markierten Features umfassen Folgendes:

- Fremdschlüssel Zuordnungen.
- Ausführen benutzerdefinierter SQL-Befehle.
- Modell First-Entwicklung.
- Poco-Unterstützung.

Außerdem führt das Lernprogramm eine kurze Einführung in die *Code First-Entwicklung*ein, ein Feature, das in der nächsten Version des Entity Framework zu finden ist.

Um das Lernprogramm zu starten, starten Sie Visual Studio, und öffnen Sie die Webanwendung der Website "Web-so University", die Sie im vorherigen Tutorial gearbeitet haben.

## <a name="foreign-key-associations"></a>Fremdschlüssel Zuordnungen

Version 3,5 der Entity Framework enthaltenen Navigations Eigenschaften, aber keine Fremdschlüssel Eigenschaften im Datenmodell enthalten. Beispielsweise würden die Spalten `CourseID` und `StudentID` der `StudentGrade` Tabelle in der `StudentGrade` Entität weggelassen werden.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

Der Grund für diese Vorgehensweise war, dass Fremdschlüssel eigentlich ein physisches Implementierungsdetail sind und nicht zu einem konzeptionellen Datenmodell gehören. Im praktischen Zusammenhang ist es jedoch häufig einfacher, mit Entitäten im Code zu arbeiten, wenn Sie direkten Zugriff auf die Fremdschlüssel haben.

Wenn Sie ein Beispiel dafür haben, wie Fremdschlüssel im Datenmodell Ihren Code vereinfachen, sollten Sie sich Gedanken darüber machen, wie Sie die *departmentsadd. aspx* -Seite ohne Sie codieren müssten. In der `Department`-Entität ist die `Administrator`-Eigenschaft ein Fremdschlüssel, der `PersonID` in der `Person` Entität entspricht. Um die Zuordnung zwischen einer neuen Abteilung und Ihrem Administrator herzustellen, mussten Sie lediglich den Wert für die `Administrator`-Eigenschaft im `ItemInserting`-Ereignishandler des Daten gebundenen Steuer Elements festlegen:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Ohne Fremdschlüssel im Datenmodell würden Sie das `Inserting`-Ereignis des Datenquellen-Steuer Elements behandeln, anstatt das `ItemInserting`-Ereignis des Daten gebundenen Steuer Elements, um einen Verweis auf die Entität selbst zu erhalten, bevor die Entität der Entitätenmenge hinzugefügt wird. Wenn Sie über diesen Verweis verfügen, können Sie die Zuordnung mithilfe von Code wie in den folgenden Beispielen einrichten:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Wie Sie im Blogbeitrag des Entity Framework Teams [zu Fremdschlüssel Zuordnungen](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx)sehen können, gibt es auch andere Fälle, in denen der Unterschied in der Komplexität des Codes wesentlich größer ist. Um die Anforderungen von Benutzern zu erfüllen, die im konzeptionellen Datenmodell für einen einfacheren Code mit Implementierungsdetails im konzeptionellen Datenmodell leben möchten, können Sie mit dem Entity Framework jetzt Fremdschlüssel in das Datenmodell einschließen.

Wenn Sie in Entity Framework Terminologie Fremdschlüssel in das Datenmodell einschließen, verwenden Sie *Fremdschlüssel Zuordnungen*, und wenn Sie Fremdschlüssel ausschließen, verwenden Sie *unabhängige Zuordnungen*.

## <a name="executing-user-defined-sql-commands"></a>Ausführen von benutzerdefinierten SQL-Befehlen

In früheren Versionen der Entity Framework gab es keine einfache Möglichkeit, eigene SQL-Befehle dynamisch zu erstellen und auszuführen. Entweder die Entity Framework dynamisch generierten SQL-Befehle für Sie, oder Sie mussten eine gespeicherte Prozedur erstellen und als Funktion importieren. In Version 4 werden `ExecuteStoreQuery` und `ExecuteStoreCommand` Methoden der `ObjectContext`-Klasse hinzugefügt, die es Ihnen erleichtern, jede beliebige Abfrage direkt an die Datenbank zu übergeben.

Nehmen wir an, dass die Administratoren von Administratoren in der Lage sein sollen, Massen Änderungen in der Datenbank auszuführen, ohne dass Sie eine gespeicherte Prozedur erstellen und in das Datenmodell importieren müssen. Die erste Anforderung ist eine Seite, auf der Sie die Anzahl der Gutschriften für alle Kurse in der Datenbank ändern können. Auf der Webseite möchten Sie in der Lage sein, eine Zahl einzugeben, die verwendet werden kann, um den Wert der `Credits` Spalte jeder `Course` Zeile zu multiplizieren.

Erstellen Sie eine neue Seite, auf der die Master Seite *Site. Master* verwendet wird, und nennen Sie Sie *updatecredits. aspx*. Fügen Sie dann dem `Content`-Steuerelement mit dem Namen `Content2`folgendes Markup hinzu:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Dieses Markup erstellt ein `TextBox` Steuerelement, in dem der Benutzer den Multiplikatorwert eingeben kann, ein `Button` Steuerelement, das zum Ausführen des Befehls klickt, und ein `Label` Steuerelement zum Angeben der Anzahl der betroffenen Zeilen.

Öffnen Sie *UpdateCredits.aspx.cs*, und fügen Sie die folgende `using`-Anweisung und einen Handler für das `Click`-Ereignis der Schaltfläche hinzu:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Dieser Code führt den SQL `Update`-Befehl mit dem Wert im Textfeld aus und verwendet die Bezeichnung, um die Anzahl der betroffenen Zeilen anzuzeigen. Führen Sie vor dem Ausführen der Seite die Seite " *Courses. aspx* " aus, um ein "Before"-Bild von einigen Daten zu erhalten.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Führen Sie *updatecredits. aspx*aus, geben Sie "10" als Multiplikator ein, und klicken Sie dann auf **Ausführen**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Führen Sie die Seite " *Courses. aspx* " erneut aus, um die geänderten Daten anzuzeigen.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Wenn Sie die Anzahl der Gutschriften auf ihre ursprünglichen Werte zurücksetzen möchten, können Sie in *UpdateCredits.aspx.cs* `Credits * {0}` ändern, um die Seite zu `Credits / {0}` und erneut auszuführen, indem Sie 10 als Divisor eingeben.)

Weitere Informationen zum Ausführen von Abfragen, die Sie im Code definieren, finden Sie unter Gewusst [wie: Direktes Ausführen von Befehlen für die Datenquelle](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Modell-First-Entwicklung

In diesen exemplarischen Vorgehensweisen haben Sie zuerst die Datenbank erstellt und dann das Datenmodell basierend auf der Datenbankstruktur generiert. Im Entity Framework 4 können Sie stattdessen mit dem Datenmodell beginnen und die Datenbank basierend auf der Datenmodell Struktur generieren. Wenn Sie eine Anwendung erstellen, für die die Datenbank nicht bereits vorhanden ist, können Sie mit dem Model-First-Ansatz Entitäten und Beziehungen erstellen, die konzeptionell für die Anwendung sinnvoll sind, ohne sich Gedanken über die Details der physischen Implementierung machen zu müssen. . (Dies gilt jedoch nur für die anfänglichen Phasen der Entwicklung. Schließlich wird die Datenbank erstellt und enthält Produktionsdaten, und die erneute Erstellung aus dem Modell ist nicht mehr praktikabel. an diesem Punkt gehen Sie wieder zum Daten Bank ersten Ansatz.)

In diesem Abschnitt des Tutorials erstellen Sie ein einfaches Datenmodell und generieren die Datenbank aus der Datenbank.

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *dal* , und wählen Sie **Neues Element hinzufügen**aus. Wählen Sie im Dialogfeld **Neues Element hinzufügen** unter **installierte Vorlagen** die Option **Daten** aus, und wählen Sie dann die Vorlage **ADO.NET Entity Data Model** aus. Nennen Sie die neue Datei " *alumniassociationmodel. edmx* ", und klicken Sie auf **Hinzufügen**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Dadurch wird der Entity Data Model-Assistent gestartet. Wählen Sie im Schritt **Modell Inhalte auswählen** die Option **leeres Modell** aus, und klicken Sie dann auf **Fertig**stellen.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

Der **Entity Data Model-Designer** wird mit einer leeren Entwurfs Oberfläche geöffnet. Ziehen Sie ein **Entitäts** Element aus dem **Werkzeugkasten** auf die Entwurfs Oberfläche.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Ändern Sie den Entitäts Namen von `Entity1` in `Alumnus`, ändern Sie den Namen der `Id`-Eigenschaft in `AlumnusId`, und fügen Sie eine neue skalare Eigenschaft mit dem Namen `Name`hinzu. Um neue Eigenschaften hinzuzufügen, können Sie die EINGABETASTE drücken, nachdem Sie den Namen der `Id` Spalte geändert haben, oder mit der rechten Maustaste auf die Entität klicken und **skalare Eigenschaft hinzufügen**auswählen Der Standardtyp für neue Eigenschaften ist `String`, was für diese einfache Demonstration in Ordnung ist, aber natürlich können Sie im **Eigenschaften** Fenster Dinge wie den Datentyp ändern.

Erstellen Sie eine andere Entität auf dieselbe Weise, und benennen Sie Sie `Donation`. Ändern Sie die `Id`-Eigenschaft in `DonationId`, und fügen Sie eine skalare Eigenschaft mit dem Namen `DateAndAmount`hinzu.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Um eine Zuordnung zwischen diesen beiden Entitäten hinzuzufügen, klicken Sie mit der rechten Maustaste auf die `Alumnus` Entität, **und wählen Sie** **Hinzufügen**und dann Zuordnung aus.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Die Standardwerte im Dialogfeld "Zuordnung **Hinzufügen** " sind die gewünschten (1: n, Navigations Eigenschaften einschließen, Fremdschlüssel einschließen). Klicken Sie also einfach auf " **OK**".

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

Der Designer fügt eine Zuordnungs Linie und eine Fremdschlüssel Eigenschaft hinzu.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Nun sind Sie bereit, die Datenbank zu erstellen. Klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Datenbank aus Modell generieren**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Dadurch wird der Assistent zum Generieren von Datenbanken gestartet. (Wenn Warnungen angezeigt werden, die darauf hinweisen, dass die Entitäten nicht zugeordnet sind, können Sie diese für die Zeit ignorieren.)

Klicken Sie im Schritt **Wählen Sie Ihre Datenverbindung** aus auf **neue Verbindung**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

Wählen Sie im Dialogfeld **Verbindungs Eigenschaften** die lokale SQL Server Express Instanz aus, und benennen Sie die Datenbank `AlumniAssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Klicken Sie auf **Ja** , wenn Sie gefragt werden, ob Sie die Datenbank erstellen möchten. Wenn der Schritt **Wählen Sie Ihre Datenverbindung** aus angezeigt wird, klicken Sie auf **weiter**.

Klicken Sie im Schritt **Zusammenfassung und Einstellungen** auf **Fertig**stellen.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Eine *SQL* -Datei mit den DDL-Befehlen (Data Definition Language) wird erstellt, die Befehle wurden jedoch noch nicht ausgeführt.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Verwenden Sie ein Tool wie **SQL Server Management Studio** , um das Skript auszuführen und die Tabellen zu erstellen, wie Sie dies möglicherweise beim Erstellen der `School` Datenbank für [das erste Tutorial in der Reihe "erste](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)Schritte" durchgeführt haben. (Sofern Sie die Datenbank nicht heruntergeladen haben.)

Sie können jetzt das `AlumniAssociation` Datenmodell in ihren Webseiten auf die gleiche Weise wie das `School` Modell verwenden. Fügen Sie zu diesem Vorgang einige Daten zu den Tabellen hinzu, und erstellen Sie eine Webseite, auf der die Daten angezeigt werden.

Fügen Sie mithilfe **Server-Explorer**die folgenden Zeilen zu den Tabellen `Alumnus` und `Donation` hinzu.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Erstellen Sie eine neue Webseite namens " *Alumni. aspx* ", die die *Site. Master* -Master Seite verwendet. Fügen Sie das folgende Markup dem `Content`-Steuerelement mit dem Namen `Content2`hinzu:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Dieses Markup erstellt geschachtelte `GridView` Steuerelemente, das äußere zum Anzeigen von Namen der Alumni und das innere zum Anzeigen von Spenden Terminen und-Beträgen.

Öffnen Sie *Alumni.aspx.cs*. Fügen Sie eine `using`-Anweisung für die Datenzugriffs Ebene und einen Handler für das `RowDataBound` Ereignis des äußeren `GridView` Steuer Elements hinzu:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Mit diesem Code wird die innere `GridView` Steuerung mithilfe der `Donations`-Navigations Eigenschaft der `Alumnus` Entität der aktuellen Zeile databindiert.

Führen Sie die Seite aus.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Hinweis: Diese Seite ist im herunterladbaren Projekt enthalten, aber damit Sie funktioniert, müssen Sie die Datenbank in der lokalen SQL Server Express Instanz erstellen. die Datenbank ist nicht als *MDF* -Datei in der *App\_Daten* Ordner enthalten.)

Weitere Informationen zur Verwendung der Model First-Funktion des Entity Framework finden Sie unter [Model-First in the Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>POCO-Unterstützung

Wenn Sie die Domänen gesteuerte Entwurfsmethodik verwenden, entwerfen Sie Daten Klassen, die für die Geschäftsdomäne relevante Daten und Verhaltensweisen darstellen. Diese Klassen sollten unabhängig von einer bestimmten Technologie sein, die verwendet wird, um die Daten zu speichern (persistent zu speichern). Dies bedeutet *, dass Sie*persistent sein sollten. Durch das Ignorieren der Persistenz kann auch eine Klasse für den Komponenten Test vereinfacht werden, da das Komponenten Testprojekt die für Tests am einfachsten geeignete Persistenztechnologie verwenden kann. Frühere Versionen des Entity Framework boten eingeschränkte Unterstützung für Persistenz bei der Persistenz, da Entitäts Klassen von der `EntityObject`-Klasse erben mussten und somit viele Entity Framework spezifische Funktionalität enthielt.

Der Entity Framework 4 führt die Möglichkeit ein, Entitäts Klassen zu verwenden, die nicht von der `EntityObject`-Klasse erben und daher persistent sind. Im Kontext des Entity Framework werden Klassen wie diese normalerweise als *einfache alte CLR-Objekte* (poco oder POCOS) bezeichnet. Poco-Klassen können manuell geschrieben werden, oder Sie können basierend auf einem vorhandenen Datenmodell mithilfe der vom Entity Framework bereitgestellten Vorlagen des Text Template Transformation Toolkit (T4) automatisch generiert werden.

Weitere Informationen zum Verwenden von POCOS im Entity Framework finden Sie in den folgenden Ressourcen:

- [Arbeiten mit poco-Entitäten](https://msdn.microsoft.com/library/dd456853.aspx). Dies ist ein MSDN-Dokument, das eine Übersicht über POCOS und Links zu anderen Dokumenten mit detaillierteren Informationen ist.
- Exemplarische Vorgehensweise [: POCO-Vorlage für die Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) Dies ist ein Blogbeitrag vom Entity Framework Entwicklungsteam mit Links zu anderen Blogbeiträgen zu POCOS.

## <a name="code-first-development"></a>Code-First-Entwicklung

Die poco-Unterstützung in Entity Framework 4 erfordert weiterhin, dass Sie ein Datenmodell erstellen und die Entitäts Klassen mit dem Datenmodell verknüpfen. Die nächste Version der Entity Framework enthält eine Funktion namens " *Code First Development*". Diese Funktion ermöglicht es Ihnen, die Entity Framework mit ihren eigenen poco-Klassen zu verwenden, ohne entweder den Datenmodell-Designer oder eine Datenmodell-XML-Datei verwenden zu müssen. (Aus diesem Grund wurde diese Option auch als *nur Code*bezeichnet. " *Code First* " und " *nur Code* " beziehen sich auf dieselbe Entity Framework Funktion.)

Weitere Informationen zur Verwendung des Code First-Ansatzes für die Entwicklung finden Sie in den folgenden Ressourcen:

- [Code-First-Entwicklung mit Entity Framework 4](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Dies ist ein Blogbeitrag von Scott Guthrie, der die Code First-Entwicklung einführt.
- [Blog des Entity Framework-Entwicklungsteams-Beiträge mit markierten Code](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Entity Framework-Entwicklungs Team Blog-Beiträge mit Tags versehen Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [MVC Music Store-Tutorial-Teil 4: Modelle und Datenzugriff](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Erste Schritte mit MVC 3-Part 4: Entity Framework Code-First-Entwicklung](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Außerdem wird ein neues MVC-Code-First-Tutorial, mit dem eine Anwendung erstellt 2011 wird, die der Anwendung "" der Anwendung "die Anwendung" der Anwendung "der Anwendung" ähnelt, auf [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Weitere Informationen

Dadurch wird der Überblick über die Neuerungen in der Entity Framework und die Entity Framework tutorialreihe fortgesetzt. Weitere Informationen zu den neuen Features in den Entity Framework 4, die hier nicht behandelt werden, finden Sie in den folgenden Ressourcen:

- [Neues in ADO.net](https://msdn.microsoft.com/library/ex6y04yf.aspx) MSDN-Thema zu neuen Features in Version 4 der Entity Framework.
- [Ankündigung der Veröffentlichung von Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) Der Blogbeitrag des Entity Framework Development-Teams zu den neuen Features in Version 4.

> [!div class="step-by-step"]
> [Previous](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
