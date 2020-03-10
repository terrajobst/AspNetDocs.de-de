---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: Erstellen von Modellklassen mit dem Entity Framework (VB) | Microsoft-Dokumentation
author: microsoft
description: In diesem Tutorial erfahren Sie, wie Sie ASP.NET MVC mit dem Microsoft Entity Framework verwenden. Sie erfahren, wie Sie mit dem Entity Wizard a ADO.NET Entity da...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: f6c896c6f5f6d898ac6f99d5998fb29cb73bcb10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437007"
---
# <a name="creating-model-classes-with-the-entity-framework-vb"></a>Erstellen von Modellklassen mit dem Entity Framework (VB)

von [Microsoft](https://github.com/microsoft)

> In diesem Tutorial erfahren Sie, wie Sie ASP.NET MVC mit dem Microsoft Entity Framework verwenden. Sie erfahren, wie Sie den Entitäts-Assistenten zum Erstellen eines ADO.NET-Entity Data Model verwenden. Im Rahmen dieses Tutorials erstellen wir eine Webanwendung, die veranschaulicht, wie Datenbankdaten mithilfe der Entity Framework ausgewählt, eingefügt, aktualisiert und gelöscht werden.

In diesem Tutorial wird erläutert, wie Sie beim Erstellen einer ASP.NET MVC-Anwendung mithilfe der Microsoft-Entity Framework Datenzugriffsklassen erstellen können. In diesem Tutorial wird davon ausgegangen, dass die Microsoft-Entity Framework nicht bekannt sind. Am Ende dieses Tutorials erfahren Sie, wie Sie die Entity Framework verwenden, um Datenbankeinträge auszuwählen, einzufügen, zu aktualisieren und zu löschen.

Beim Microsoft-Entity Framework handelt es sich um ein O/RM-Tool (Object Relational Mapping), mit dem Sie eine Datenzugriffs Ebene aus einer Datenbank automatisch generieren können. Mit der Entity Framework können Sie die mühsame Arbeit der Datenzugriffsklassen vermeiden.

> [!NOTE] 
> 
> Zwischen ASP.NET MVC und der Microsoft-Entity Framework ist keine wesentliche Verbindung vorhanden. Es gibt mehrere Alternativen zu den Entity Framework, die Sie mit ASP.NET MVC verwenden können. Beispielsweise können Sie Ihre MVC-Modellklassen mit anderen O/RM-Tools wie z. b. Microsoft LINQ to SQL, NHibernate oder Subsonic erstellen.

Um zu veranschaulichen, wie Sie die Microsoft-Entity Framework mit ASP.NET MVC verwenden können, erstellen wir eine einfache Beispielanwendung. Wir erstellen eine Movie Database-Anwendung, mit der Sie Movie Database-Datensätze anzeigen und bearbeiten können.

In diesem Tutorial wird davon ausgegangen, dass Sie über Visual Studio 2008 oder Visual Web Developer 2008 mit Service Pack 1 verfügen. Sie benötigen Service Pack 1, um die Entity Framework zu verwenden. Sie können Visual Studio 2008 Service Pack 1 oder Visual Web Developer mit Service Pack 1 unter folgender Adresse herunterladen:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)

## <a name="creating-the-movie-sample-database"></a>Erstellen der Movie-Beispieldatenbank

Die Film Datenbankanwendung verwendet eine Datenbanktabelle mit dem Namen Filme, die die folgenden Spalten enthält:

| Spaltenname | Datentyp | NULL-Werten zulassen? | Ist der Primärschlüssel? |
| --- | --- | --- | --- |
| Id | int | False | True |
| Titel | Nvarchar (100) | False | False |
| Regisseur | Nvarchar (100) | False | False |

Sie können diese Tabelle einem ASP.NET MVC-Projekt hinzufügen, indem Sie die folgenden Schritte ausführen:

1. Klicken Sie mit der rechten Maustaste auf den Ordner App\_Daten im Fenster Projektmappen-Explorer, und wählen Sie die Menüoption **hinzufügen, neues Element aus.**
2. Wählen Sie im Dialogfeld **Neues Element hinzufügen** **SQL Server Datenbank**aus, geben Sie der Datenbank den Namen "moviesdb. mdf", und klicken Sie auf die Schaltfläche " **Hinzufügen** ".
3. Doppelklicken Sie auf die Datei "moviesdb. mdf", um das Fenster Server-Explorer/Datenbank-Explorer zu öffnen.
4. Erweitern Sie die Datenbankverbindung moviesdb. mdf, klicken Sie mit der rechten Maustaste auf den Ordner Tabellen, und wählen Sie die Menüoption **neue Tabelle hinzufügen**aus.
5. Fügen Sie im Tabellen-Designer die Spalten "ID", "Title" und "Director" hinzu.
6. Klicken Sie auf die Schaltfläche **Speichern** (mit dem Symbol der Diskette), um die neue Tabelle mit dem Namen Movies zu speichern.

Nachdem Sie die Datenbanktabelle "Movies" erstellt haben, sollten Sie der Tabelle einige Beispiel Daten hinzufügen. Klicken Sie mit der rechten Maustaste auf die Tabelle Filme, und wählen Sie die Menüoption **Tabellendaten anzeigen**aus. Sie können gefälschte Filmdaten in das Raster eingeben, das angezeigt wird.

## <a name="creating-the-adonet-entity-data-model"></a>Erstellen der ADO.NET-Entity Data Model

Um die Entity Framework verwenden zu können, müssen Sie eine Entity Data Model erstellen. Sie können den Visual Studio-Entity Data Model- *Assistenten* nutzen, um automatisch eine Entity Data Model aus einer Datenbank zu generieren.

Folgen Sie diesen Schritten:

1. Klicken Sie im Projektmappen-Explorer Fenster mit der rechten Maustaste auf den Ordner Modelle, und wählen Sie die Menüoption **hinzufügen, neues Element**aus.
2. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Kategorie Daten aus (siehe Abbildung 1).
3. Wählen Sie die Vorlage **ADO.NET Entity Data Model** aus, geben Sie der Entity Data Model den Namen moviesdbmodel. edmx, und klicken Sie auf die Schaltfläche **Hinzufügen** . Durch Klicken auf die Schaltfläche **Hinzufügen** wird der Datenmodell-Assistent gestartet
4. Wählen Sie im Schritt **Modell Inhalt auswählen** die Option **aus Datenbank generieren aus** , und klicken Sie auf die Schaltfläche **weiter** (siehe Abbildung 2).
5. Wählen Sie im Schritt **Wählen Sie Ihre Datenverbindung** aus die Datenbankverbindung "moviesdb. mdf" aus, geben Sie den Namen der Verbindungseinstellungen der Entitäten ein, und klicken Sie auf die Schaltfläche " **weiter** " (siehe Abbildung 3).
6. Wählen Sie im Schritt **Wählen Sie Ihre Datenbankobjekte** aus die Tabelle Movie Database aus, und klicken Sie auf die Schaltfläche **Fertig** stellen (siehe Abbildung 4).

Nachdem Sie diese Schritte ausgeführt haben, wird der ADO.NET Entity Data Model-Designer (Entity Designer) geöffnet.

**Abbildung 1 – Erstellen eines neuen Entity Data Model**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**Abbildung 2 – Schritt "Modell Inhalte auswählen"**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**Abbildung 3 – Auswählen der Datenverbindung**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**Abbildung 4 – auswählen der Datenbankobjekte**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>Ändern des ADO.NET-Entity Data Model

Nachdem Sie einen Entity Data Model erstellt haben, können Sie das Modell ändern, indem Sie die Entity Designer nutzen (siehe Abbildung 5). Sie können den Entity Designer jederzeit öffnen, indem Sie im Fenster "Projektmappen-Explorer" auf die Datei "moviesdbmodel. edmx" doppelklicken, die im Ordner "Models" enthalten ist.

**Abbildung 5 – die ADO.NET-Entity Data Model-Designer**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

Beispielsweise können Sie den Entity Designer verwenden, um die Namen der Klassen zu ändern, die vom Assistenten für Entity Model Data generiert werden. Der Assistent hat eine neue Datenzugriffs Klasse mit dem Namen "Movies" erstellt. Mit anderen Worten: der Assistent hat der Klasse denselben Namen wie die Datenbanktabelle gegeben. Da wir diese Klasse verwenden, um eine bestimmte Film Instanz darzustellen, sollten wir die Klasse von Movies in Movie umbenennen.

Wenn Sie eine Entitäts Klasse umbenennen möchten, können Sie im Entity Designer auf den Klassennamen doppelklicken und einen neuen Namen eingeben (siehe Abbildung 6). Alternativ können Sie den Namen einer Entität in der Eigenschaftenfenster ändern, nachdem Sie eine Entität im Entity Designer ausgewählt haben.

**Abbildung 6 – ändern eines Entitäts namens**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

Speichern Sie Ihre Entity Data Model, nachdem Sie eine Änderung vorgenommen haben, indem Sie auf die Schaltfläche Speichern klicken (das Symbol der Diskette). Im Hintergrund generiert das Entity Designer eine Reihe von Visual Basic .NET-Klassen. Sie können diese Klassen anzeigen, indem Sie die Datei "moviesdbmodel. Designer. vb" im Projektmappen-Explorer Fenster öffnen.

Ändern Sie den Code in der Datei "Designer. vb" nicht, da die Änderungen bei der nächsten Verwendung des Entity Designer überschrieben werden. Wenn Sie die Funktionalität der in der Datei "Designer. vb" definierten Entitäts Klassen erweitern möchten, können Sie *partielle Klassen* in separaten Dateien erstellen.

#### <a name="selecting-database-records-with-the-entity-framework"></a>Auswählen von Datensätzen mit der Entity Framework

Beginnen wir mit dem Erstellen der Film Datenbankanwendung, indem wir eine Seite erstellen, auf der eine Liste mit Film Datensätzen angezeigt wird. Der Home-Controller in der Liste 1 macht eine Aktion mit dem Namen Index () verfügbar. Die Index ()-Aktion gibt alle Film Datensätze aus der Film Datenbanktabelle zurück, indem Sie die Entity Framework nutzt.

**Codebeispiel 1 – controllers\homecontroller.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

Beachten Sie, dass der Controller in der Liste 1 einen Konstruktor enthält. Der Konstruktor initialisiert ein Feld auf Klassenebene mit dem Namen \_DB. Das Feld \_DB stellt die Daten Bank Entitäten dar, die vom Microsoft-Entity Framework generiert werden. Das \_DB-Feld ist eine Instanz der Klasse "moviesdbentities", die vom Entity Designer generiert wurde.

Das Feld \_DB wird innerhalb der Index ()-Aktion verwendet, um die Datensätze aus der Datenbanktabelle "Movies" abzurufen. Der Ausdruck \_DB. "Movieset" stellt alle Datensätze aus der Datenbanktabelle "Movies" dar. Die Methode "-Methode ()" wird verwendet, um den Satz von Filmen in eine generische Auflistung von Movie-Objekten zu konvertieren: List (of Movie).

Die Film Datensätze werden mithilfe von LINQ to Entities abgerufen. Die Index ()-Aktion in der Liste 1 verwendet die LINQ- *Methoden Syntax* , um den Satz von Datenbankdaten Sätzen abzurufen. Wenn Sie möchten, können Sie stattdessen die LINQ- *Abfrage Syntax* verwenden. Die folgenden beiden-Anweisungen haben dieselbe Sache:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

Verwenden Sie die gleiche LINQ-Syntax – Methoden Syntax oder Abfrage Syntax –, die Sie intuitiver finden. Es gibt keinen Unterschied in der Leistung zwischen den beiden Ansätzen – der einzige Unterschied besteht im Stil.

Die Ansicht in der Liste 2 wird verwendet, um die Film Datensätze anzuzeigen.

**Codebeispiel 2 – views\home\index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

Die Ansicht in der Liste 2 enthält eine **for each** -Schleife, die jeden Film Daten Satz durchläuft und die Werte der Titel-und Director-Eigenschaften des Movie-Datensatzes anzeigt. Beachten Sie, dass neben jedem Datensatz ein Link zum Bearbeiten und löschen angezeigt wird. Außerdem wird unten in der Ansicht ein Link zum Hinzufügen von Movie angezeigt (siehe Abbildung 7).

**Abbildung 7 – Index Ansicht**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

Die Index Sicht ist eine *typisierte Sicht*. Die Index Sicht weist eine &lt;% @ Page%&gt;-Direktive auf, die ein erbt-Attribut enthält. Das erbt-Attribut wandelt die ViewData. Model-Eigenschaft in eine stark typisierte generische Listen Auflistung von Movie Objects um – eine Liste (of Movie).

## <a name="inserting-database-records-with-the-entity-framework"></a>Einfügen von Datenbankdaten Sätzen mit dem Entity Framework

Sie können den Entity Framework verwenden, um das Einfügen neuer Datensätze in eine Datenbanktabelle zu erleichtern. In der Liste 3 werden zwei neue Aktionen der Home Controller-Klasse hinzugefügt, die Sie zum Einfügen neuer Datensätze in die Film Datenbanktabelle verwenden können.

**Codebeispiel 3 – controllers\homecontroller.vb (Methoden hinzufügen)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

Die erste Aktion zum Hinzufügen () gibt einfach eine Ansicht zurück. Die Ansicht enthält ein Formular zum Hinzufügen eines neuen Film Datenbankdaten Satzes (siehe Abbildung 8). Wenn Sie das Formular senden, wird die zweite Aktion zum Hinzufügen () aufgerufen.

Beachten Sie, dass die zweite Aktion zum Hinzufügen () mit dem Attribut "Accept tverbs" ergänzt wird. Diese Aktion kann nur aufgerufen werden, wenn ein HTTP Post-Vorgang ausgeführt wird. Anders ausgedrückt: Diese Aktion kann nur aufgerufen werden, wenn Sie ein HTML-Formular veröffentlichen.

Die zweite Aktion zum Hinzufügen () erstellt eine neue Instanz der Entity Framework Movie-Klasse mit der Hilfe der ASP.NET MVC tryupdatemodel ()-Methode. Die tryupdatemodel ()-Methode nimmt die Felder in der FormCollection an, die an die Add ()-Methode übermittelt wurden, und weist die Werte dieser HTML-Formularfelder der Movie-Klasse zu.

Wenn Sie die Entity Framework verwenden, müssen Sie eine "weiße Liste" von Eigenschaften angeben, wenn Sie die Methoden "tryupdatemodel" oder "updatemodel" verwenden, um die Eigenschaften einer Entitäts Klasse zu aktualisieren.

Anschließend führt die Aktion hinzufügen () einige einfache Formular Überprüfungen durch. Die Aktion überprüft, ob die Titel-und Director-Eigenschaften Werte aufweisen. Wenn ein Validierungs Fehler vorliegt, wird eine Validierungs Fehlermeldung zu modelstate hinzugefügt.

Wenn keine Validierungs Fehler vorliegen, wird der Film Datenbanktabelle mithilfe der Entity Framework ein neuer Film Daten Satz hinzugefügt. Der neue Datensatz wird der Datenbank mit den folgenden zwei Codezeilen hinzugefügt:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

Die erste Codezeile fügt die neue Movie-Entität zu dem Satz von Filmen hinzu, der vom Entity Framework nachverfolgt wird. Die zweite Codezeile speichert alle Änderungen, die an den Filmen vorgenommen wurden, die an die zugrunde liegende Datenbank zurückverfolgt wurden.

**Abbildung 8 – Ansicht "hinzufügen"**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Aktualisieren von Datenbankdaten Sätzen mit dem Entity Framework

Sie können fast denselben Ansatz verwenden, um einen Datenbankdaten Satz mit dem Entity Framework zu bearbeiten, als den Ansatz, den wir soeben befolgt haben, um einen neuen Datenbankdaten Satz einzufügen. Codebeispiel 4 enthält zwei neue Controller Aktionen mit dem Namen "Edit ()". Die erste Edit ()-Aktion gibt ein HTML-Formular zum Bearbeiten eines Film Datensatzes zurück. Die zweite Aktion bearbeiten () versucht, die Datenbank zu aktualisieren.

**Codebeispiel 4 – controllers\homecontroller.vb (Methoden bearbeiten)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

Die zweite Aktion bearbeiten () beginnt mit dem Abrufen des Film Datensatzes aus der Datenbank, der mit der ID des bearbeiteten Films übereinstimmt. Mit der folgenden LINQ to Entities-Anweisung wird der erste Daten Bank Datensatz erfasst, der mit einer bestimmten ID übereinstimmt:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

Im nächsten Schritt wird die tryupdatemodel ()-Methode verwendet, um die Werte der HTML-Formularfelder den Eigenschaften der Movie-Entität zuzuweisen. Beachten Sie, dass eine Zulassungsliste bereitgestellt wird, um die genaue Eigenschaften aktualisieren anzugeben.

Im nächsten Schritt wird eine einfache Überprüfung ausgeführt, um zu überprüfen, ob die Titel-und Director-Eigenschaften des Films über Werte verfügen Wenn einer der Eigenschaften ein Wert fehlt, wird modelstate eine Validierungs Fehlermeldung hinzugefügt, und modelstate. IsValid gibt den Wert false zurück.

Wenn keine Validierungs Fehler vorliegen, wird die zugrunde liegende Filme-Datenbanktabelle durch Aufrufen der SaveChanges ()-Methode mit allen Änderungen aktualisiert.

Wenn Sie Datenbankeinträge bearbeiten, müssen Sie die ID des bearbeiteten Datensatzes an die Controller Aktion übergeben, die das Datenbankupdate ausführt. Andernfalls weiß die Controller Aktion nicht, welcher Datensatz in der zugrunde liegenden Datenbank aktualisiert werden soll. Die Bearbeitungs Ansicht, die in der Liste 5 enthalten ist, enthält ein ausgeblendetes Formularfeld, das die ID des bearbeiteten Datenbankdaten Satzes darstellt.

**Codebeispiel 5 – views\home\edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Löschen von Datenbankdaten Sätzen mit dem Entity Framework

Der letzte Daten Bank Vorgang, den wir in diesem Tutorial angehen müssen, ist das Löschen von Datenbankdaten Sätzen. Sie können die Controller Aktion in der Liste 6 verwenden, um einen bestimmten Datenbankdaten Satz zu löschen.

**Codebeispiel 6: \controllers\homecontroller.vb (DELETE-Aktion)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

Die Delete ()-Aktion ruft zuerst die Film Entität ab, die der an die Aktion übergebenen ID entspricht. Als nächstes wird der Film aus der Datenbank gelöscht, indem die DeleteObject ()-Methode gefolgt von der SaveChanges ()-Methode aufgerufen wird. Schließlich wird der Benutzer zurück zur Index Ansicht umgeleitet.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wird veranschaulicht, wie Sie datenbankgesteuerte Webanwendungen erstellen können, indem Sie ASP.NET MVC und die Microsoft-Entity Framework nutzen. Sie haben gelernt, wie Sie eine Anwendung erstellen, die es Ihnen ermöglicht, Datenbankeinträge auszuwählen, einzufügen, zu aktualisieren und zu löschen.

Zunächst wurde erläutert, wie Sie mit dem Entity Data Model-Assistenten innerhalb von Visual Studio eine Entity Data Model generieren können. Als Nächstes erfahren Sie, wie Sie mit LINQ to Entities einen Satz von Datenbankdaten Sätzen aus einer Datenbanktabelle abrufen. Zum Schluss haben wir die Entity Framework verwendet, um Datenbankdaten Sätze einzufügen, zu aktualisieren und zu löschen.

> [!div class="step-by-step"]
> [Zurück](validation-with-the-data-annotation-validators-cs.md)
> [Weiter](creating-model-classes-with-linq-to-sql-vb.md)
