---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Anzeigen einer Tabelle mit Datenbankdaten (VB) | Microsoft-Dokumentation
author: microsoft
description: In diesem Tutorial zeige ich zwei Methoden zum Anzeigen eines Satzes von Datenbankdaten Sätzen. Ich zeige zwei Methoden zum Formatieren eines Satzes von Datenbankdaten Sätzen in einer HTML-Ta...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: f2e2489ac8455913f55c746dbe05b9fe8272285b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78436737"
---
# <a name="displaying-a-table-of-database-data-vb"></a>Anzeigen einer Tabelle von Datenbankdaten (VB)

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> In diesem Tutorial zeige ich zwei Methoden zum Anzeigen eines Satzes von Datenbankdaten Sätzen. Ich zeige zwei Methoden zum Formatieren eines Satzes von Datenbankdaten Sätzen in einer HTML-Tabelle. Zuerst zeige ich, wie Sie die Datenbankeinträge direkt in einer Sicht formatieren können. Als nächstes zeige ich Ihnen, wie Sie beim Formatieren von Datenbankdaten Sätzen partiale nutzen können.

In diesem Tutorial wird erläutert, wie Sie eine HTML-Tabelle mit Datenbankdaten in einer ASP.NET MVC-Anwendung anzeigen können. Zunächst erfahren Sie, wie Sie mithilfe der in Visual Studio enthaltenen Gerüstbau Tools eine Ansicht generieren, in der automatisch eine Reihe von Datensätzen angezeigt werden. Als Nächstes erfahren Sie, wie Sie beim Formatieren von Datenbankdaten Sätzen eine partielle als Vorlage verwenden.

## <a name="create-the-model-classes"></a>Erstellen der Modellklassen

Wir werden den Satz von Datensätzen aus der Datenbanktabelle "Movies" anzeigen. Die "Movies"-Datenbanktabelle enthält die folgenden Spalten:

<a id="0.4_table01"></a>

| **Spaltenname** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | Int | False |
| Titel | Nvarchar (200) | False |
| Regisseur | Nvarchar (50) | False |
| Datereleasing | DateTime | False |

Um die Filme-Tabelle in unserer ASP.NET MVC-Anwendung darzustellen, müssen wir eine Modell Klasse erstellen. In diesem Tutorial verwenden wir die Microsoft-Entity Framework, um unsere Modellklassen zu erstellen.

> [!NOTE] 
> 
> In diesem Tutorial verwenden wir die Microsoft-Entity Framework. Es ist jedoch wichtig zu verstehen, dass Sie eine Vielzahl verschiedener Technologien verwenden können, um mit einer Datenbank aus einer ASP.NET MVC-Anwendung, einschließlich LINQ to SQL, NHibernate oder ADO.net, zu interagieren.

Führen Sie die folgenden Schritte aus, um den Entity Data Model Assistenten zu starten:

1. Klicken Sie im Fenster Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Modelle, und wählen Sie die Menüoption **hinzufügen, neues Element**aus.
2. Wählen Sie die Kategorie **Daten** aus, und wählen Sie die Vorlage **ADO.NET Entity Data Model** aus.
3. Geben Sie Ihrem Datenmodell den Namen " *moviesdbmodel. edmx* ", und klicken Sie auf die Schaltfläche **Hinzufügen** .

Nachdem Sie auf die Schaltfläche Hinzufügen geklickt haben, wird der Entity Data Model-Assistent angezeigt (siehe Abbildung 1). Führen Sie die folgenden Schritte aus, um den Assistenten abzuschließen:

1. Wählen Sie im Schritt **Modell Inhalt auswählen** die Option **aus Datenbank generieren aus** .
2. Verwenden Sie im Schritt **Wählen Sie Ihre Datenverbindung** aus die Datenverbindung " *moviesdb. mdf* " und den Namen " *moviesdbentities* " für die Verbindungseinstellungen. Klicken Sie auf die Schaltfläche **Weiter**.
3. Erweitern Sie im Schritt **Wählen Sie Ihre Datenbankobjekte** aus den Knoten Tabellen, und wählen Sie die Tabelle Movies aus. Geben Sie die Namespace *Modelle* ein, und klicken Sie auf **Fertig** stellen.

[![Erstellen von LINQ to SQL Klassen](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**Abbildung 01**: Erstellen von LINQ to SQL Klassen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-a-table-of-database-data-vb/_static/image2.png))

Nachdem Sie den Entity Data Model-Assistenten beendet haben, wird der Entity Data Model-Designer geöffnet. Der Designer sollte die Filme-Entität anzeigen (siehe Abbildung 2).

[![des Entity Data Model-Designers](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**Abbildung 02**: der Entity Data Model-Designer ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-a-table-of-database-data-vb/_static/image4.png))

Wir müssen eine Änderung vornehmen, bevor wir fortfahren. Der Assistent für Entitäts Daten generiert eine Modell Klasse mit dem Namen *Filme* , die die Datenbanktabelle "Movies" darstellt Da wir die Klasse "Movies" verwenden, um einen bestimmten Film darzustellen, müssen wir den Namen der Klasse in *Movie* anstelle von *Movies* (Singular anstelle von Plural) ändern.

Doppelklicken Sie auf den Namen der Klasse auf der Designer Oberfläche, und ändern Sie den Namen der Klasse von Movies in Movie. Nachdem Sie diese Änderung vorgenommen haben, klicken Sie auf die Schaltfläche **Speichern** (das Symbol der Diskette), um die Movie-Klasse zu generieren.

## <a name="create-the-movies-controller"></a>Erstellen des Filme Controllers

Nachdem wir nun über eine Möglichkeit verfügen, unsere Datenbankdaten Sätze darzustellen, können wir einen Controller erstellen, der die Sammlung von Filmen zurückgibt. Klicken Sie im Visual Studio-Projektmappen-Explorer Fenster mit der rechten Maustaste auf den Ordner Controllers, und wählen Sie die Menüoption **hinzufügen, Controller** (siehe Abbildung 3).

[![dem Menü "Controller hinzufügen"](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**Abbildung 03**: das Menü "Controller hinzufügen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-a-table-of-database-data-vb/_static/image6.png))

Wenn das Dialogfeld **Controller hinzufügen** angezeigt wird, geben Sie den Controller Namen "muviecontroller" ein (siehe Abbildung 4). Klicken Sie zum Hinzufügen des neuen Controllers auf die Schaltfläche **Hinzufügen** .

[Dialogfeld "Controller hinzufügen" ![](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**Abbildung 04**: Dialogfeld "Controller hinzufügen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-a-table-of-database-data-vb/_static/image8.png))

Wir müssen die vom Movie Controller verfügbar gemachte Index ()-Aktion ändern, damit Sie den Satz von Datenbankdaten Sätzen zurückgibt. Ändern Sie den Controller so, dass er wie der Controller in der Liste 1 aussieht.

**Codebeispiel 1 – controllers\muviecontroller.vb**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

In der Liste 1 wird die Klasse "moviesdbentities" verwendet, um die Datenbank "moviesdb" darzustellen. Die Ausdrucks *Entitäten. "Movieset. Start List ()* " gibt den Satz aller Filme aus der Datenbanktabelle "Movies" zurück.

## <a name="create-the-view"></a>Erstellen der Sicht

Die einfachste Möglichkeit, einen Satz von Datenbankdaten Sätzen in einer HTML-Tabelle anzuzeigen, besteht darin, das von Visual Studio bereitgestellte Gerüstbau zu nutzen.

Erstellen Sie die Anwendung, indem Sie die Menüoption **Build,** Projekt Mappe erstellen auswählen. Sie müssen die Anwendung erstellen, bevor Sie das Dialog **Feld Ansicht hinzufügen** öffnen, oder die Daten Klassen werden nicht im Dialogfeld angezeigt.

Klicken Sie mit der rechten Maustaste auf die Aktion Index (), und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 5).

[![Hinzufügen einer Ansicht](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**Abbildung 05**: Hinzufügen einer Ansicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-a-table-of-database-data-vb/_static/image10.png))

Aktivieren Sie im Dialog **Feld Ansicht hinzufügen** das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**. Wählen Sie die Klasse Movie als **Ansichts Datenklasse**aus. Wählen Sie *Liste* als **Inhalt der Ansicht** aus (siehe Abbildung 6). Wenn Sie diese Optionen auswählen, wird eine stark typisierte Ansicht generiert, in der eine Liste der Filme angezeigt wird.

[Dialogfeld "Ansicht hinzufügen" ![](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**Abbildung 06**: Dialogfeld "Ansicht hinzufügen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-a-table-of-database-data-vb/_static/image12.png))

Nachdem Sie auf die Schaltfläche **Hinzufügen** geklickt haben, wird die Ansicht in der Liste 2 automatisch generiert. Diese Sicht enthält den Code, der zum Durchlaufen der Auflistung von Filmen und zum Anzeigen der einzelnen Eigenschaften eines Films erforderlich ist.

**Codebeispiel 2 – views\muvie\index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

Sie können die Anwendung ausführen, indem Sie die Menüoption **Debuggen, Debuggen starten** (oder Drücken der Taste F5) auswählen. Wenn Sie die Anwendung ausführen, wird Internet Explorer gestartet. Wenn Sie zur/Movie-URL navigieren, wird die Seite in Abbildung 7 angezeigt.

[![einer Tabelle mit Filmen](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**Abbildung 07**: eine Tabelle mit Filmen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-a-table-of-database-data-vb/_static/image14.png))

Wenn Sie nichts über die Darstellung des Rasters von Datenbankdaten Sätzen in Abbildung 7 erfahren möchten, können Sie einfach die Index Ansicht ändern. Beispielsweise können Sie den *datereleasing* -Header in das *Date* -Format ändern, indem Sie die Index Ansicht ändern.

## <a name="create-a-template-with-a-partial"></a>Erstellen einer Vorlage mit einem partiellen

Wenn eine Ansicht zu kompliziert wird, empfiehlt es sich, die Ansicht in partiale zu zerlegen. Wenn Sie partiale verwenden, werden Ihre Ansichten leichter zu verstehen und zu warten. Wir erstellen eine partielle, die wir als Vorlage zum Formatieren der einzelnen Movie Database-Datensätze verwenden können.

Führen Sie die folgenden Schritte aus, um den partiellen

1. Klicken Sie mit der rechten Maustaste auf den Ordner views\movie, und wählen Sie die Menüoption **Ansicht hinzufügen**.
2. Aktivieren Sie das Kontrollkästchen als *Teilansicht erstellen (. ascx)* .
3. Benennen Sie die partielle " *mavietemplate*".
4. Aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**.
5. Wählen Sie Movie als *Ansichts Datenklasse*aus.
6. Wählen Sie leer als *Inhalt der Sicht*aus.
7. Klicken Sie auf die Schaltfläche **Hinzufügen** , um dem Projekt die partielle hinzuzufügen.

Nachdem Sie diese Schritte ausgeführt haben, ändern Sie die Datei "muvietemplate" so, dass Sie wie in der Liste 3

**Codebeispiel 3 – views\muvie\muvietemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

Die partielle in der Liste 3 enthält eine Vorlage für eine einzelne Zeile mit Datensätzen.

Die geänderte Index Sicht in der Liste 4 verwendet die "muvietemplate"-partielle.

**Codebeispiel 4 – views\muvie\index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

Die Ansicht in der Liste 4 enthält eine for Each-Schleife, die alle Filme durchläuft. Für jeden Film wird der "mavietemplate"-Teil verwendet, um den Film zu formatieren. Die "muvietemplate" wird durch Aufrufen der renderpartial ()-Hilfsmethode gerendert.

Die geänderte Index Ansicht rendert dieselbe HTML-Tabelle von Datenbankdaten Sätzen. Allerdings wurde die Ansicht erheblich vereinfacht.

Die renderpartial ()-Methode unterscheidet sich von den meisten anderen Hilfsmethoden, da Sie keine Zeichenfolge zurückgibt. Daher müssen Sie die renderpartial ()-Methode mit &lt;% HTML. renderpartial ()%&gt; anstelle von &lt;% = HTML. renderpartial ()%&gt;aufgerufen werden.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wurde erläutert, wie Sie einen Satz von Datenbankdaten Sätzen in einer HTML-Tabelle anzeigen können. Zuerst haben Sie gelernt, wie Sie einen Satz von Datenbankdaten Sätzen aus einer Controller Aktion zurückgeben, indem Sie den Microsoft-Entity Framework nutzen. Als nächstes haben Sie gelernt, wie Sie mit dem Visual Studio-Gerüst eine Ansicht generieren, in der automatisch eine Auflistung von Elementen angezeigt wird. Schließlich haben Sie erfahren, wie Sie die Ansicht vereinfachen, indem Sie eine partielle nutzen. Sie haben gelernt, wie Sie eine partielle als Vorlage verwenden, damit Sie jeden Datenbankdaten Satz formatieren können.

> [!div class="step-by-step"]
> [Zurück](creating-model-classes-with-linq-to-sql-vb.md)
> [Weiter](performing-simple-validation-vb.md)
