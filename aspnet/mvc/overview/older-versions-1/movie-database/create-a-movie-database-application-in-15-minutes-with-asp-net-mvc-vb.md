---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: Erstellen einer Film Datenbankanwendung in 15 Minuten mit ASP.NET MVC (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther erstellt eine gesamte datenbankgesteuerte ASP.NET MVC-Anwendung von Anfang bis Ende. Dieses Tutorial ist eine gute Einführung für Personen, die neu sind...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ce8161d29a8ab4005e2b20462b08c9e10ee815a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435771"
---
# <a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a>Erstellen einer Filmdatenbankanwendung mit ASP.NET MVC in 15 Minuten (VB)

von [Stephen Walther](https://github.com/StephenWalther)

[Code herunterladen](https://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> Stephen Walther erstellt eine gesamte datenbankgesteuerte ASP.NET MVC-Anwendung von Anfang bis Ende. Dieses Tutorial ist eine gute Einführung für Personen, die mit dem ASP.NET MVC-Framework noch nicht vertraut sind und einen Eindruck von der Vorgehensweise zum Entwickeln einer ASP.NET MVC-Anwendung erhalten möchten.

Der Zweck dieses Tutorials besteht darin, Ihnen einen Eindruck davon zu vermitteln, wie Sie eine ASP.NET MVC-Anwendung erstellen können. In diesem Tutorial erstelle ich eine ganze ASP.NET MVC-Anwendung von Anfang bis Ende. Ich zeige Ihnen, wie Sie eine einfache datenbankgesteuerte Anwendung erstellen, die veranschaulicht, wie Sie Datenbankeinträge auflisten, erstellen und bearbeiten können.

Um den Prozess der Anwendungs Bildung zu vereinfachen, nutzen wir die Gerüstbau Features von Visual Studio 2008. Wir lassen zu, dass Visual Studio den anfänglichen Code und Inhalt für unsere Controller, Modelle und Ansichten generiert.

Wenn Sie mit Active Server Seiten oder ASP.net gearbeitet haben, sollten Sie ASP.NET MVC sehr vertraut finden. ASP.NET MVC-Ansichten sind den Seiten in einer Active Server Pages-Anwendung sehr ähnlich. Ebenso wie eine herkömmliche ASP.net-Web Forms Anwendung bietet ASP.NET MVC Ihnen vollständigen Zugriff auf die umfangreichen Sprachen und Klassen, die von .NET Framework bereitgestellt werden.

Meine Hoffnung ist, dass Sie in diesem Tutorial einen Eindruck davon erhalten, wie sich das Entwickeln einer ASP.NET MVC-Anwendung ähnlich und unterscheidet, als das Entwickeln einer Active Server Seiten oder einer ASP.net-Web Forms Anwendung.

## <a name="overview-of-the-movie-database-application"></a>Übersicht über die Movie Database-Anwendung

Da unser Ziel darin besteht, die Dinge einfach zu halten, erstellen wir eine sehr einfache Film Datenbankanwendung. Mit unserer Simple Movie Database-Anwendung können drei Dinge ausgeführt werden:

1. Auflisten eines Satzes von Movie Database-Datensätzen
2. Neuen Film Datenbankdaten Satz erstellen
3. Bearbeiten eines vorhandenen Movie Database-Datensatzes

Da wir die Dinge auf einfache Weise beibehalten möchten, profitieren wir von der minimalen Anzahl von Features des ASP.NET MVC-Frameworks, das zum Erstellen der Anwendung erforderlich ist. Beispielsweise wird die Test gesteuerte Entwicklung nicht genutzt.

Zum Erstellen der Anwendung müssen Sie die folgenden Schritte ausführen:

1. Erstellen des ASP.NET MVC-Webanwendungs Projekts
2. Erstellen der Datenbank
3. Erstellen des Datenbankmodells
4. Erstellen des ASP.NET-MVC-Controllers
5. Erstellen der ASP.NET-MVC-Ansichten

## <a name="preliminaries"></a>Vorbereitende Maßnahmen

Sie benötigen entweder Visual Studio 2008 oder Visual Web Developer 2008 Express, um eine ASP.NET MVC-Anwendung zu erstellen. Außerdem müssen Sie das ASP.NET-MVC-Framework herunterladen.

Wenn Sie nicht über Visual Studio 2008 verfügen, können Sie eine 90-Tage-Testversion von Visual Studio 2008 von dieser Website herunterladen:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Alternativ können Sie ASP.NET MVC-Anwendungen mit Visual Web Developer Express 2008 erstellen. Wenn Sie sich für die Verwendung von Visual Web Developer Express entscheiden, muss Service Pack 1 installiert sein. Sie können Visual Web Developer 2008 Express mit Service Pack 1 von dieser Website herunterladen:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;d isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Nachdem Sie entweder Visual Studio 2008 oder Visual Web Developer 2008 installiert haben, müssen Sie das ASP.NET-MVC-Framework installieren. Sie können das ASP.NET MVC-Framework von der folgenden Website herunterladen:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> Anstatt das ASP.NET Framework und das ASP.NET MVC-Framework einzeln herunterzuladen, können Sie den Webplattform-Installer nutzen. Der Webplattform-Installer ist eine Anwendung, mit der Sie die installierten Anwendungen auf einfache Weise verwalten können:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)

## <a name="creating-an-aspnet-mvc-web-application-project"></a>Erstellen eines ASP.NET-MVC-Webanwendungs Projekts

Erstellen Sie zunächst ein neues ASP.NET MVC-Webanwendungs Projekt in Visual Studio 2008. Wählen Sie die Menü Options **Datei, neues Projekt aus,** und das Dialogfeld Neues Projekt in Abbildung 1 wird angezeigt. Wählen Sie Visual Basic als Programmiersprache aus, und wählen Sie die Projektvorlage ASP.NET MVC-Webanwendung aus. Benennen Sie Ihr Projekt mit dem Namen "", und klicken Sie auf die Schaltfläche OK.

[![des Dialog Felds "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)

**Abbildung 01**: das Dialogfeld "Neues Projekt" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))

Stellen Sie sicher, dass Sie in der Dropdown Liste am oberen Rand des Dialog Felds "Neues Projekt" die Option "3,5" .NET Framework auswählen oder die Projektvorlage ASP.NET MVC-Webanwendung nicht angezeigt wird.

Wenn Sie ein neues MVC-Webanwendungs Projekt erstellen, werden Sie von Visual Studio aufgefordert, ein separates Komponenten Testprojekt zu erstellen. Das Dialogfeld in Abbildung 2 wird angezeigt. Da in diesem Tutorial aufgrund von Zeiteinschränkungen keine Tests erstellt werden (und, ja, wir sollten uns etwas schuldig machen), wählen Sie die Option **Nein** aus, und klicken Sie auf die Schaltfläche **OK** .

> [!NOTE] 
> 
> Visual Web Developer unterstützt keine Testprojekte.

[![des Dialog Felds "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)

**Abbildung 02**: das Dialogfeld zum Erstellen eines Komponenten Test Projekts ([Klicken Sie, um das Bild in voller Größe anzuzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))

Eine ASP.NET MVC-Anwendung verfügt über einen Standardsatz von Ordnern: einen Ordner "Models", "Views" und "Controllers". Sie können diesen Standardsatz von Ordnern im Projektmappen-Explorer Fenster sehen. Wir müssen jedem der Ordner "Models", "Views" und "Controllers" Dateien hinzufügen, um die Film Datenbankanwendung zu erstellen.

Wenn Sie eine neue MVC-Anwendung mit Visual Studio erstellen, erhalten Sie eine Beispielanwendung. Da wir von Grund auf neu beginnen möchten, müssen wir den Inhalt für diese Beispielanwendung löschen. Sie müssen die folgende Datei und den folgenden Ordner löschen:

- Controllers\homecontroller.vb
- Views\Home

## <a name="creating-the-database"></a>Erstellen der Datenbank

Wir müssen eine Datenbank erstellen, die unsere Movie Database-Datensätze enthält. Glücklicherweise enthält Visual Studio eine kostenlose Datenbank mit dem Namen SQL Server Express. Führen Sie diese Schritte aus, um die Datenbank zu erstellen:

1. Klicken Sie mit der rechten Maustaste auf den Ordner App\_Daten im Fenster Projektmappen-Explorer, und wählen Sie die Menüoption **hinzufügen, neues Element**aus.
2. Wählen Sie die Kategorie **Daten** aus, und wählen Sie die Vorlage **SQL Server Datenbanken** aus (siehe Abbildung 3).
3. Nennen Sie die neue Datenbank " *moviesdb. mdf* ", und klicken Sie auf die Schaltfläche **Hinzufügen**

Nachdem Sie die Datenbank erstellt haben, können Sie eine Verbindung mit der Datenbank herstellen, indem Sie auf die Datei "moviesdb. mdf" doppelklicken, die sich im Ordner App\_Data befindet. Wenn Sie auf die Datei "moviesdb. mdf" doppelklicken, wird das Server-Explorer Fenster geöffnet.

> [!NOTE] 
> 
> Das Fenster Server-Explorer wird im Fall von Visual Web Developer als Datenbank-Explorer Fenster bezeichnet.

[![des Dialog Felds "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)

**Abbildung 03**: Erstellen einer Microsoft SQL Server Datenbank ([Klicken Sie, um das Bild in voller Größe anzuzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))

Als nächstes müssen wir eine neue Datenbanktabelle erstellen. Klicken Sie im Fenster "Server-Explorer" mit der rechten Maustaste auf den Ordner Tabellen, und wählen Sie die Menüoption **neue Tabelle hinzufügen**aus. Durch Auswahl dieser Menüoption wird der Datenbanktabellen-Designer geöffnet. Erstellen Sie die folgenden Daten Bank Spalten:

<a id="0.2_table01"></a>

| **Spaltenname** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | Int | False |
| Titel | Nvarchar (100) | False |
| Regisseur | Nvarchar (100) | False |
| Datereleasing | DateTime | False |

Die erste Spalte, die ID-Spalte, verfügt über zwei spezielle Eigenschaften. Zuerst müssen Sie die ID-Spalte als Primärschlüssel Spalte markieren. Nachdem Sie die Spalte ID ausgewählt haben, klicken Sie auf die Schaltfläche **Primärschlüssel festlegen** (es handelt sich dabei um das Symbol, das wie ein Schlüssel aussieht). Zweitens müssen Sie die ID-Spalte als Identitäts Spalte markieren. Scrollen Sie in der Eigenschaftenfenster Spalte nach unten zum Abschnitt Identitäts Angabe, und erweitern Sie ihn. Ändern Sie die Eigenschaft **ist Identity** in den Wert **Ja**. Wenn Sie fertig sind, sollte die Tabelle wie in Abbildung 4 aussehen.

[![des Dialog Felds "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)

**Abbildung 04**: die Datenbanktabelle "Movies" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))

Der letzte Schritt ist das Speichern der neuen Tabelle. Klicken Sie auf die Schaltfläche Speichern (das Symbol der Diskette), und benennen Sie die neue Tabelle mit dem Namen Movies.

Nachdem Sie die Tabelle erstellt haben, fügen Sie der Tabelle einige Film Datensätze hinzu. Klicken Sie im Fenster Server-Explorer mit der rechten Maustaste auf die Tabelle Filme, und wählen Sie die Menüoption **Tabellendaten anzeigen**aus. Geben Sie eine Liste Ihrer bevorzugten Filme ein (siehe Abbildung 5).

[![des Dialog Felds "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)

**Abbildung 05**: eingeben von Movie-Datensätzen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))

## <a name="creating-the-model"></a>Erstellen des Modells

Wir müssen als nächstes eine Reihe von Klassen erstellen, die unsere Datenbank darstellen. Wir müssen ein Datenbankmodell erstellen. Wir nutzen die Microsoft-Entity Framework, um die Klassen für das Datenbankmodell automatisch zu generieren.

> [!NOTE] 
> 
> Das ASP.NET-MVC-Framework ist nicht an die Microsoft-Entity Framework gebunden. Sie können Ihre Datenbankmodell Klassen erstellen, indem Sie eine Vielzahl von Tools für die relationale Objekt Zuordnung (oder/M) nutzen, einschließlich LINQ to SQL, Subsonic und NHibernate.

Führen Sie die folgenden Schritte aus, um den Entity Data Model Assistenten zu starten:

1. Klicken Sie im Fenster Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Modelle, und wählen Sie die Menüoption **hinzufügen, neues Element**aus.
2. Wählen Sie die Kategorie **Daten** aus, und wählen Sie die Vorlage **ADO.NET Entity Data Model** aus.
3. Geben Sie Ihrem Datenmodell den Namen " *moviesdbmodel. edmx* ", und klicken Sie auf die Schaltfläche **Hinzufügen** .

Nachdem Sie auf die Schaltfläche Hinzufügen geklickt haben, wird der Entity Data Model-Assistent angezeigt (siehe Abbildung 6). Führen Sie die folgenden Schritte aus, um den Assistenten abzuschließen:

1. Wählen Sie im Schritt **Modell Inhalt auswählen** die Option **aus Datenbank generieren aus** .
2. Verwenden Sie im Schritt **Wählen Sie Ihre Datenverbindung** aus die Datenverbindung " *moviesdb. mdf* " und den Namen " *moviesdbentities* " für die Verbindungseinstellungen. Klicken Sie auf die Schaltfläche **Weiter**.
3. Erweitern Sie im Schritt **Wählen Sie Ihre Datenbankobjekte** aus den Knoten Tabellen, und wählen Sie die Tabelle Movies aus. Geben Sie den Namespace " *muvieapp. Models* " ein, und klicken Sie auf **Fertig** stellen.

[![des Dialog Felds "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)

**Abbildung 06**: Erstellen eines Datenbankmodells mit dem Assistenten für Entity Data Model ([Klicken Sie, um das Bild in voller Größe anzuzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))

Nachdem Sie den Entity Data Model-Assistenten beendet haben, wird der Entity Data Model-Designer geöffnet. Der Designer sollte die Datenbanktabelle "Movies" anzeigen (siehe Abbildung 7).

[![des Dialog Felds "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)

**Abbildung 07**: der Entity Data Model-Designer ([Klicken Sie, um das Bild in voller Größe anzuzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))

Wir müssen eine Änderung vornehmen, bevor wir fortfahren. Der Assistent für Entitäts Daten generiert eine Modell Klasse mit dem Namen Filme, die die Datenbanktabelle "Movies" darstellt Da wir die Klasse "Movies" verwenden, um einen bestimmten Film darzustellen, müssen wir den Namen der Klasse in *Movie* anstelle von *Movies* (Singular anstelle von Plural) ändern.

Doppelklicken Sie auf den Namen der Klasse auf der Designer Oberfläche, und ändern Sie den Namen der Klasse von Movies in Movie. Nachdem Sie diese Änderung vorgenommen haben, klicken Sie auf die Schaltfläche **Speichern** (das Symbol der Diskette), um die Movie-Klasse zu generieren.

## <a name="creating-the-aspnet-mvc-controller"></a>Erstellen des ASP.NET-MVC-Controllers

Der nächste Schritt besteht darin, den ASP.NET-MVC-Controller zu erstellen. Ein Controller ist dafür verantwortlich, zu steuern, wie ein Benutzer mit einer ASP.NET MVC-Anwendung interagiert.

Folgen Sie diesen Schritten:

1. Klicken Sie im Projektmappen-Explorer Fenster mit der rechten Maustaste auf den Ordner Controllers, und wählen Sie die Menüoption **Controller hinzufügen**aus.
2. Geben Sie im Dialogfeld Controller hinzufügen den Namen *HomeController* ein, und aktivieren Sie das Kontrollkästchen **Aktionsmethoden hinzufügen für Erstellungs-, Aktualisierungs-und Detail Szenarien** (siehe Abbildung 8).
3. Klicken Sie auf die Schaltfläche **Hinzufügen** , um dem Projekt den neuen Controller hinzuzufügen.

Nachdem Sie diese Schritte ausgeführt haben, wird der Controller in der Liste 1 erstellt. Beachten Sie, dass Sie Methoden namens Index, Details, CREATE und Edit enthält. In den folgenden Abschnitten fügen Sie den erforderlichen Code hinzu, damit diese Methoden funktionieren.

[![des Dialog Felds "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)

**Abbildung 08**: Hinzufügen eines neuen ASP.NET MVC-Controllers ([Klicken Sie, um das Bild in voller Größe anzuzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))

**Codebeispiel 1 – controllers\homecontroller.vb**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a>Auflisten von Datenbankeinträgen

Die Index ()-Methode des Home-Controllers ist die Standardmethode für eine ASP.NET MVC-Anwendung. Wenn Sie eine ASP.NET MVC-Anwendung ausführen, ist die Index ()-Methode die erste Controller Methode, die aufgerufen wird.

Wir verwenden die Index ()-Methode, um die Liste der Datensätze aus der Datenbanktabelle "Movies" anzuzeigen. Wir nutzen die Datenbankmodell Klassen, die wir zuvor erstellt haben, um die Movie Database-Datensätze mit der Index ()-Methode abzurufen.

Ich habe die HomeController-Klasse in der Liste 2 geändert, sodass Sie ein neues privates Feld mit dem Namen \_DB enthält. Die Klasse "moviesdbentities" repräsentiert unser Datenbankmodell, und wir verwenden diese Klasse, um mit unserer Datenbank zu kommunizieren.

Außerdem habe ich die Index ()-Methode in der Liste 2 geändert. Die Index ()-Methode verwendet die Klasse "moviesdbentities", um alle Film Datensätze aus der Datenbanktabelle "Movies" abzurufen. Der Ausdruck *\_DB. "Movieset. Start List ()* " gibt eine Liste aller Film Datensätze aus der Datenbanktabelle "Movies" zurück.

Die Liste der Filme wird an die Ansicht übermittelt. Alle Elemente, die an die View ()-Methode übermittelt werden, werden als Ansichts Daten an die Ansicht übermittelt.

**Listing 2 – Controllers/HomeController. vb (geänderte Index Methode)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

Die Index ()-Methode gibt eine Sicht mit dem Namen Index zurück. Wir müssen diese Ansicht erstellen, um die Liste der Movie Database-Datensätze anzuzeigen. Folgen Sie diesen Schritten:

Sie sollten das Projekt erstellen (Wählen Sie die Menüoption **erstellen,** Projekt Mappe erstellen), bevor Sie das Dialog **Feld Ansicht hinzufügen** öffnen. in der Dropdown Liste **Datenklasse anzeigen** werden keine Klassen angezeigt.

1. Klicken Sie mit der rechten Maustaste auf die Index ()-Methode im Code-Editor, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 9).
2. Überprüfen Sie im Dialogfeld Ansicht hinzufügen, ob das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen** aktiviert ist.
3. Wählen Sie in der Dropdown Liste **Inhalt anzeigen** die *Liste*Wert aus.
4. Wählen Sie in der Dropdown Liste **Datenklasse anzeigen** den Wert " *muvieapp. Movie*" aus.
5. Klicken Sie auf die Schaltfläche hinzufügen, um die neue Ansicht zu erstellen (siehe Abbildung 10).

Nachdem Sie diese Schritte ausgeführt haben, wird dem Ordner "views\home" eine neue Ansicht mit dem Namen "index. aspx" hinzugefügt. Der Inhalt der Index Ansicht ist in der Liste 3 enthalten.

[![des Dialog Felds "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)

**Abbildung 09**: Hinzufügen einer Ansicht aus einer Controller Aktion ([Klicken Sie, um das Bild in voller Größe anzuzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))

[![des Dialog Felds "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)

**Abbildung 10**: Erstellen einer neuen Ansicht mit dem Dialogfeld "Ansicht hinzufügen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

In der Index Sicht werden alle Film Datensätze aus der Datenbanktabelle "Movies" in einer HTML-Tabelle angezeigt. Die Sicht enthält eine for Each-Schleife, die die einzelnen Filme durchläuft, die von der ViewData. Model-Eigenschaft dargestellt werden. Wenn Sie die Anwendung durch Drücken der F5-Taste ausführen, sehen Sie die Webseite in Abbildung 11.

[![des Dialog Felds "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)

**Abbildung 11**: Index Ansicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))

## <a name="creating-new-database-records"></a>Erstellen neuer Datenbankdaten Sätze

Die Index Sicht, die wir im vorherigen Abschnitt erstellt haben, enthält einen Link zum Erstellen neuer Datenbankdaten Sätze. Nun implementieren wir die Logik und erstellen die Sicht, die zum Erstellen neuer Movie Database-Datensätze erforderlich ist.

Der Home-Controller enthält zwei Methoden mit dem Namen Create (). Die erste Create ()-Methode hat keine Parameter. Diese Überladung der Create ()-Methode wird verwendet, um das HTML-Formular zum Erstellen eines neuen Movie Database-Datensatzes anzuzeigen.

Die zweite Create ()-Methode verfügt über einen FormCollection-Parameter. Diese Überladung der Create ()-Methode wird aufgerufen, wenn das HTML-Formular zum Erstellen eines neuen Films an den Server gesendet wird. Beachten Sie, dass diese zweite Create ()-Methode über ein-Attribut verfügt, das verhindert, dass die Methode aufgerufen wird, es sei denn, es wird ein HTTP Post-Vorgang ausgeführt.

Diese zweite Create ()-Methode wurde in der aktualisierten HomeController-Klasse in der Liste 4 geändert. Die neue Version der Create ()-Methode akzeptiert einen Movie-Parameter und enthält die Logik für das Einfügen eines neuen Films in die "Movies"-Datenbanktabelle.

> [!NOTE] 
> 
> Beachten Sie das Bindungs Attribut. Da wir die Movie ID-Eigenschaft nicht aus dem HTML-Formular aktualisieren möchten, muss diese Eigenschaft explizit ausgeschlossen werden.

**Codebeispiel 4 – controllers\homecontroller.vb (geänderte Create-Methode)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

Mit Visual Studio können Sie das Formular zum Erstellen eines neuen Filmdaten Bank Datensatzes ganz einfach erstellen (siehe Abbildung 12). Folgen Sie diesen Schritten:

1. Klicken Sie mit der rechten Maustaste auf die Create ()-Methode im Code-Editor, und wählen Sie die Menüoption **Ansicht hinzufügen**.
2. Vergewissern Sie sich, dass das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen** aktiviert ist.
3. Wählen Sie in der Dropdown Liste **Inhalt anzeigen** den Wert *Erstellen*aus.
4. Wählen Sie in der Dropdown Liste **Datenklasse anzeigen** den Wert " *muvieapp. Movie*" aus.
5. Klicken Sie auf **Hinzufügen** , um die neue Ansicht zu erstellen.

[![des Dialog Felds "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)

**Abbildung 12**: Hinzufügen der Ansicht "erstellen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))

Visual Studio generiert die Ansicht automatisch in Auflistung 5. Diese Sicht enthält ein HTML-Formular, das die Felder enthält, die den einzelnen Eigenschaften der Movie-Klasse entsprechen.

**Codebeispiel 5 – views\home\kreate.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> Das HTML-Formular, das im Dialogfeld Ansicht hinzufügen generiert wird, generiert ein ID-Formularfeld. Da die ID-Spalte eine Identitäts Spalte ist, benötigen wir dieses Formularfeld nicht, und Sie können es problemlos entfernen.

Nachdem Sie die CREATE VIEW-Sicht hinzugefügt haben, können Sie der Datenbank neue Film Datensätze hinzufügen. Führen Sie die Anwendung aus, indem Sie die Taste F5 drücken, und klicken Sie auf den Link neu erstellen, um das Formular in Abbildung 13 anzuzeigen. Wenn Sie das Formular ausfüllen und übermitteln, wird ein neuer Film Datenbankdaten Satz erstellt.

Beachten Sie, dass Sie die Formular Validierung automatisch erhalten. Wenn Sie nicht vergessen, ein Veröffentlichungsdatum für einen Film einzugeben, oder wenn Sie ein ungültiges Veröffentlichungsdatum eingeben, wird das Formular erneut angezeigt, und das Feld Releasedatum wird hervorgehoben.

[![des Dialog Felds "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)

**Abbildung 13**: Erstellen eines neuen Films Database-Datensatzes ([Klicken Sie, um das Bild in voller Größe anzuzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))

## <a name="editing-existing-database-records"></a>Bearbeiten vorhandener Datenbankdaten Sätze

In den vorherigen Abschnitten wurde erläutert, wie Sie neue Datenbankeinträge auflisten und erstellen können. In diesem letzten Abschnitt wird erläutert, wie Sie vorhandene Datenbankeinträge bearbeiten können.

Zuerst muss das Bearbeitungs Formular generiert werden. Dieser Schritt ist einfach, da Visual Studio das Bearbeitungs Formular automatisch für uns generiert. Öffnen Sie die Klasse HomeController. vb im Code-Editor von Visual Studio, und führen Sie die folgenden Schritte aus:

1. Klicken Sie mit der rechten Maustaste auf die Edit ()-Methode im Code-Editor, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 14).
2. Aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**.
3. Wählen Sie in der Dropdown Liste **Inhalt anzeigen** den Wert *Bearbeiten*aus.
4. Wählen Sie in der Dropdown Liste **Datenklasse anzeigen** den Wert " *muvieapp. Movie*" aus.
5. Klicken Sie auf **Hinzufügen** , um die neue Ansicht zu erstellen.

Durch Ausführen dieser Schritte wird dem Ordner "views\home" eine neue Ansicht mit dem Namen "Edit. aspx" hinzugefügt. Diese Ansicht enthält ein HTML-Formular zum Bearbeiten eines Film Datensatzes.

[![des Dialog Felds "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)

**Abbildung 14**: Hinzufügen der Bearbeitungs Ansicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))

> [!NOTE] 
> 
> Die Bearbeitungs Ansicht enthält ein HTML-Formularfeld, das der Eigenschaft Movie ID entspricht. Da Sie nicht möchten, dass Benutzer den Wert der ID-Eigenschaft bearbeiten, sollten Sie dieses Formularfeld entfernen.

Schließlich müssen wir den Home-Controller so ändern, dass er das Bearbeiten eines Datenbankdaten Satzes unterstützt. Die aktualisierte HomeController-Klasse ist in der Liste 6 enthalten.

**Codebeispiel 6 – controllers\homecontroller.vb (Methoden bearbeiten)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

In der Liste 6 habe ich für beide über Ladungen der Edit ()-Methode zusätzliche Logik hinzugefügt. Die erste Edit ()-Methode gibt den Movie Database-Datensatz zurück, der dem an die Methode übergebenen ID-Parameter entspricht. Die zweite Überladung führt die Aktualisierungen an einem Film Daten Satz in der Datenbank aus.

Beachten Sie, dass Sie den ursprünglichen Film abrufen und dann ApplyPropertyChanges () aufrufen müssen, um den vorhandenen Film in der Datenbank zu aktualisieren.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie einen Eindruck von der Vorgehensweise beim Aufbau einer ASP.NET MVC-Anwendung gemacht. Ich hoffe, dass Sie festgestellt haben, dass das Entwickeln einer ASP.NET-MVC-Webanwendung dem Aufbau einer Active Server-oder ASP.NET-Anwendung sehr ähnlich ist.

In diesem Tutorial haben wir nur die grundlegendsten Features des ASP.NET-MVC-Frameworks untersucht. In zukünftigen Tutorials werden Themen wie Controller, Controller Aktionen, Ansichten, Ansichts Daten und HTML-Hilfsprogramme ausführlicher erläutert.

> [!div class="step-by-step"]
> [Previous](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
