---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: Erstellen von Modellklassen mit LINQ to SQL (VB) | Microsoft-Dokumentation
author: microsoft
description: Ziel dieses Tutorials ist es, eine Methode zum Erstellen von Modellklassen für eine ASP.NET MVC-Anwendung zu erläutern. In diesem Tutorial erfahren Sie, wie Sie das Modell erstellen...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 88a5f1037d93ef3bdc95bf60b6005ebb254ab440
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74588588"
---
# <a name="creating-model-classes-with-linq-to-sql-vb"></a>Erstellen von Modellklassen mit LINQ to SQL (VB)

von [Microsoft](https://github.com/microsoft)

[PDF herunterladen](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> Ziel dieses Tutorials ist es, eine Methode zum Erstellen von Modellklassen für eine ASP.NET MVC-Anwendung zu erläutern. In diesem Tutorial erfahren Sie, wie Sie Modellklassen erstellen und Datenbankzugriff durchführen, indem Sie die Vorteile von Microsoft LINQ to SQL nutzen.

Ziel dieses Tutorials ist es, eine Methode zum Erstellen von Modellklassen für eine ASP.NET MVC-Anwendung zu erläutern. In diesem Tutorial erfahren Sie, wie Sie Modellklassen erstellen und Datenbankzugriff durchführen, indem Sie die Vorteile von Microsoft LINQ to SQL nutzen.

In diesem Tutorial erstellen wir eine einfache Film Datenbankanwendung. Wir beginnen damit, die Movie Database-Anwendung möglichst schnell und am einfachsten zu erstellen. Wir führen den gesamten Datenzugriff direkt aus den Controller Aktionen aus.

Als Nächstes erfahren Sie, wie Sie das Repository-Muster verwenden. Die Verwendung des Repository-Musters erfordert etwas mehr Arbeit. Der Vorteil der Übernahme dieses Musters besteht jedoch darin, dass Sie Anwendungen erstellen können, die angepasst werden können und problemlos getestet werden können.

## <a name="what-is-a-model-class"></a>Was ist eine Modell Klasse?

Ein MVC-Modell enthält die gesamte Anwendungslogik, die nicht in einer MVC-Ansicht oder einem MVC-Controller enthalten ist. Ein MVC-Modell enthält insbesondere alle Anwendungs Geschäfts-und Datenzugriffs Logik.

Sie können eine Vielzahl verschiedener Technologien verwenden, um Ihre Datenzugriffs Logik zu implementieren. Beispielsweise können Sie die Datenzugriffsklassen mithilfe der Klassen Microsoft Entity Framework, NHibernate, Subsonic oder ADO.NET erstellen.

In diesem Tutorial verwende ich LINQ to SQL, um die Datenbank abzufragen und zu aktualisieren. LINQ to SQL stellt eine sehr einfache Methode für die Interaktion mit einer Microsoft SQL Server-Datenbank bereit. Es ist jedoch wichtig zu wissen, dass das ASP.NET-MVC-Framework nicht an LINQ to SQL gebunden ist. ASP.NET MVC ist mit allen Datenzugriffs Technologien kompatibel.

## <a name="create-a-movie-database"></a>Erstellen einer Filmdatenbank

In diesem Tutorial: um zu veranschaulichen, wie Modellklassen erstellt werden können, erstellen wir eine einfache Film Datenbankanwendung. Der erste Schritt besteht darin, eine neue Datenbank zu erstellen. Klicken Sie mit der rechten Maustaste auf den Ordner App\_Daten im Fenster Projektmappen-Explorer, und wählen Sie die Menüoption **hinzufügen, neues Element**aus. Wählen Sie die Vorlage SQL Server Datenbank aus, geben Sie Ihr den Namen "moviesdb. mdf", und klicken Sie auf die Schaltfläche **Hinzufügen** (siehe Abbildung 1).

[![Hinzufügen einer neuen SQL Server Datenbank](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**Abbildung 01**: Hinzufügen einer neuen SQL Server Datenbank ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))

Nachdem Sie die neue Datenbank erstellt haben, können Sie die Datenbank öffnen, indem Sie im Ordner App\_Data auf die Datei "moviesdb. mdf" doppelklicken. Durch Doppelklicken auf die Datei "moviesdb. mdf" wird das Fenster "Server-Explorer" geöffnet (siehe Abbildung 2).

|   | Das Fenster Server-Explorer wird bei Verwendung von Visual Web Developer als Datenbank-Explorer Fenster bezeichnet. |
|---|----------------------------------------------------------------------------------------------------|
|   |                                                                                                    |

[![mithilfe des Server-Explorer Fensters](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**Abbildung 02**: Verwenden des Fensters "Server-Explorer" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))

Wir müssen unserer Datenbank eine Tabelle hinzufügen, die unsere Filme repräsentiert. Klicken Sie mit der rechten Maustaste auf den Ordner Tabellen, und wählen Sie die Menüoption **neue Tabelle hinzufügen**. Wenn Sie diese Menüoption auswählen, wird der Tabellen-Designer geöffnet (siehe Abbildung 3).

[![mithilfe des Server-Explorer Fensters](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**Abbildung 03**: der Tabellen-Designer ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))

Wir müssen der Datenbanktabelle die folgenden Spalten hinzufügen:

| **Spalten Name** | **Datentyp** | **NULL-Werten zulassen** |
| --- | --- | --- |
| Id | Int | Falsch |
| Title | Nvarchar (200) | Falsch |
| Direktors | Nvarchar (50) | Falsch |

Sie müssen für die ID-Spalte zwei besondere Dinge tun. Zuerst müssen Sie die ID-Spalte als Primärschlüssel Spalte markieren, indem Sie die Spalte in der Tabellen-Designer auswählen und auf das Symbol einer Taste klicken. LINQ to SQL müssen Sie beim Ausführen von Einfügungen oder Updates für die Datenbank Ihre Primärschlüssel Spalten angeben.

Als nächstes müssen Sie die ID-Spalte als Identitäts Spalte markieren, indem Sie der **is Identity** -Eigenschaft den Wert Ja zuweisen (siehe Abbildung 3). Eine Identitäts Spalte ist eine Spalte, der automatisch eine neue Nummer zugewiesen wird, wenn Sie eine neue Daten Zeile zu einer Tabelle hinzufügen.

Nachdem Sie diese Änderungen vorgenommen haben, speichern Sie die Tabelle mit dem Namen "tblmovie". Sie können die Tabelle speichern, indem Sie auf die Schaltfläche Speichern klicken.

## <a name="create-linq-to-sql-classes"></a>Erstellen von LINQ to SQL Klassen

Unser MVC-Modell enthält LINQ to SQL Klassen, die die tblmovie-Datenbanktabelle darstellen. Die einfachste Möglichkeit zum Erstellen dieser LINQ to SQL Klassen ist, dass Sie mit der rechten Maustaste auf den Ordner Modelle klicken, **hinzufügen, neues Element**, die Vorlage LINQ to SQL Klassen auswählen, den Klassen den Namen Movie. dbml geben und dann auf die Schaltfläche **Hinzufügen** klicken (siehe Abbildung 4).

[![Erstellen von LINQ to SQL Klassen](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**Abbildung 04**: Erstellen von LINQ to SQL Klassen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))

Unmittelbar nach dem Erstellen der Movie LINQ to SQL-Klassen wird der objektrelationaler Designer angezeigt. Sie können Datenbanktabellen aus dem Server-Explorer Fenster auf die objektrelationaler Designer ziehen, um LINQ to SQL Klassen zu erstellen, die bestimmte Datenbanktabellen darstellen. Wir müssen die tblmovie-Datenbanktabelle dem objektrelationaler Designer hinzufügen (siehe Abbildung 4).

[![mithilfe des objektrelationaler Designer](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**Abbildung 05**: Verwenden des objektrelationaler Designer ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))

Standardmäßig erstellt das objektrelationaler Designer eine Klasse mit dem gleichen Namen wie die Datenbanktabelle, die Sie auf den Designer ziehen. Wir möchten jedoch nicht unsere Klasse "tblmovie" anrufen. Klicken Sie daher im Designer auf den Namen der Klasse, und ändern Sie den Namen der Klasse in Movie.

Klicken Sie abschließend auf die Schaltfläche **Speichern** (das Bild der Diskette), um die LINQ to SQL Klassen zu speichern. Andernfalls werden die LINQ to SQL Klassen nicht vom objektrelationaler Designer generiert.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Verwenden von LINQ to SQL in einer Controller Aktion

Nachdem wir nun über unsere LINQ to SQL Klassen verfügen, können wir diese Klassen verwenden, um Daten aus der Datenbank abzurufen. In diesem Abschnitt erfahren Sie, wie Sie LINQ to SQL-Klassen direkt innerhalb einer Controller Aktion verwenden. Die Liste der Filme wird aus der Datenbanktabelle tblmovies in einer MVC-Ansicht angezeigt.

Zuerst müssen wir die HomeController-Klasse ändern. Diese Klasse befindet sich im Ordner Controllers Ihrer Anwendung. Ändern Sie die Klasse so, dass Sie wie die Klasse in der Liste 1 aussieht.

**Codebeispiel 1 – `Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

Die Index ()-Aktion in der Liste 1 verwendet eine LINQ to SQL DataContext-Klasse (moviedatacontext), um die Datenbank "moviesdb" darzustellen. Die Klasse "muvedatacontext" wurde vom Visual Studio-objektrelationaler Designer generiert.

Eine LINQ-Abfrage wird für den DataContext ausgeführt, um alle Filme aus der tblmovies-Datenbanktabelle abzurufen. Die Liste der Filme wird einer lokalen Variablen mit dem Namen Filme zugewiesen. Schließlich wird die Liste der Filme durch Ansichts Daten an die Ansicht übermittelt.

Um die Filme anzuzeigen, müssen wir das nächste Mal die Index Ansicht ändern. Die Index Sicht finden Sie im Ordner "views\home\". Aktualisieren Sie die Index Sicht, sodass Sie wie die Ansicht in der Liste 2 aussieht.

**Codebeispiel 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

Beachten Sie, dass die geänderte Index Sicht eine &lt;% @ Import-Namespace%&gt;-Direktive am Anfang der Sicht enthält. Diese Direktive importiert den MvcApplication1-Namespace. Wir benötigen diesen Namespace, um mit den Modellklassen zu arbeiten – insbesondere die Movie-Klasse in der Ansicht.

Die Ansicht in der Liste 2 enthält eine for Each-Schleife, die alle Elemente durchläuft, die von der ViewData. Model-Eigenschaft dargestellt werden. Der Wert der Title-Eigenschaft wird für jeden Film angezeigt.

Beachten Sie, dass der Wert der ViewData. Model-Eigenschaft in ein IEnumerable-Objekt umgewandelt wird. Dies ist erforderlich, um den Inhalt von "ViewData. Model" zu durchlaufen. Eine weitere Möglichkeit besteht darin, eine stark typisierte Ansicht zu erstellen. Wenn Sie eine stark typisierte Ansicht erstellen, wandeln Sie die ViewData. Model-Eigenschaft in einen bestimmten Typ in der Code Behind-Klasse einer Ansicht um.

Wenn Sie die Anwendung nach dem Ändern der HomeController-Klasse und der Index Ansicht ausführen, wird eine leere Seite angezeigt. Sie erhalten eine leere Seite, weil in der Datenbanktabelle "tblmovies" keine Film Datensätze vorhanden sind.

Klicken Sie im Fenster Server-Explorer (Datenbank-Explorer Fenster in Visual Web Developer) mit der rechten Maustaste auf die Datenbanktabelle tblmovies (Fenster in Visual Web Developer), und wählen Sie die Menüoption **Tabellendaten anzeigen**aus. Sie können Film Datensätze mit dem angezeigten Raster einfügen (siehe Abbildung 5).

[![Einfügen von Filmen](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**Abbildung 06**: Einfügen von Filmen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))

Nachdem Sie der Tabelle tblmovies einige Datenbankeinträge hinzugefügt und die Anwendung ausgeführt haben, wird die Seite in Abbildung 7 angezeigt. Alle Movie Database-Datensätze werden in einer Auflistungs Liste angezeigt.

[![Anzeigen von Filmen mit der Index Ansicht](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**Abbildung 07**: Anzeigen von Filmen mit der Index Ansicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))

## <a name="using-the-repository-pattern"></a>Verwenden des Repository-Musters

Im vorherigen Abschnitt haben wir LINQ to SQL-Klassen direkt innerhalb einer Controller Aktion verwendet. Wir haben die Klasse "muviedatacontext" direkt aus der Index ()-Controller Aktion verwendet. Im Fall einer einfachen Anwendung gibt es nichts falsches. Das direkte Arbeiten mit LINQ to SQL in einer Controller Klasse führt jedoch zu Problemen, wenn Sie eine komplexere Anwendung erstellen müssen.

Wenn Sie LINQ to SQL innerhalb einer Controller Klasse verwenden, ist es schwierig, Datenzugriffs Technologien in Zukunft zu wechseln. Beispielsweise können Sie sich entscheiden, von der Verwendung von Microsoft LINQ to SQL zur Verwendung der Microsoft-Entity Framework als Datenzugriffs Technologie zu wechseln. In diesem Fall müssen Sie jeden Controller neu schreiben, der in Ihrer Anwendung auf die Datenbank zugreift.

Die Verwendung von LINQ to SQL in einer Controller Klasse erschwert auch das Erstellen von Komponententests für Ihre Anwendung. Normalerweise möchten Sie beim Ausführen von Komponententests nicht mit einer Datenbank interagieren. Sie möchten die-Komponententests verwenden, um Ihre Anwendungslogik und nicht den Datenbankserver zu testen.

Um eine MVC-Anwendung zu erstellen, die besser an zukünftige Änderungen angepasst werden kann und die leichter getestet werden kann, sollten Sie das Repository-Muster verwenden. Wenn Sie das Repository-Muster verwenden, erstellen Sie eine separate Repository-Klasse, die die gesamte Datenbankzugriffs Logik enthält.

Wenn Sie die Repository-Klasse erstellen, erstellen Sie eine Schnittstelle, die alle Methoden darstellt, die von der Repository-Klasse verwendet werden. Innerhalb ihrer Controller schreiben Sie den Code für die Schnittstelle statt für das Repository. Auf diese Weise können Sie das Repository in Zukunft mithilfe verschiedener Datenzugriffs Technologien implementieren.

Die Schnittstelle in der Liste 3 heißt imuvierepository und stellt eine einzelne Methode mit dem Namen ListAll () dar.

**Codebeispiel 3 – `Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

Die Repository-Klasse in der Auflistung 4 implementiert die imuvierepository-Schnittstelle. Beachten Sie, dass Sie eine Methode namens ListAll () enthält, die der Methode entspricht, die für die imuvierepository-Schnittstelle erforderlich ist.

**Codebeispiel 4 – `Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

Zum Schluss verwendet die Klasse "moviescontroller" in Auflistung 5 das Repository-Muster. LINQ to SQL-Klassen werden nicht mehr direkt verwendet.

**Codebeispiel 5 – `Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

Beachten Sie, dass die Klasse "moviescontroller" in der Auflistung 5 über zwei Konstruktoren verfügt. Der erste Konstruktor, der Parameter lose Konstruktor, wird aufgerufen, wenn die Anwendung ausgeführt wird. Dieser Konstruktor erstellt eine Instanz der Klasse "muvierepository" und übergibt sie an den zweiten Konstruktor.

Der zweite Konstruktor verfügt über einen einzelnen Parameter: einen imuvierepository-Parameter. Dieser Konstruktor weist einfach den Wert des Parameters einem Feld auf Klassenebene mit dem Namen \_Repository zu.

Die Klasse "moviescontroller" nutzt ein Software Entwurfsmuster, das als Abhängigkeits Injection-Muster bezeichnet wird. Insbesondere wird ein Konstruktor-Abhängigkeitsinjektion als Konstruktor verwendet. Weitere Informationen zu diesem Muster finden Sie im folgenden Artikel von Martin Fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Beachten Sie, dass der gesamte Code in der Klasse "moviescontroller" (mit Ausnahme des ersten Konstruktors) mit der imovierepository-Schnittstelle und nicht mit der eigentlichen movierepository-Klasse interagiert. Der Code interagiert mit einer abstrakten Schnittstelle und nicht mit einer konkreten Implementierung der-Schnittstelle.

Wenn Sie die von der Anwendung verwendete Datenzugriffs Technologie ändern möchten, können Sie einfach die imuvierepository-Schnittstelle mit einer Klasse implementieren, die die Alternative Datenbankzugriffs Technologie verwendet. Beispielsweise können Sie eine entityframeworkmuvierepository-Klasse oder eine subsonicmuvierepository-Klasse erstellen. Da die Controller Klasse mit der-Schnittstelle programmiert wird, können Sie eine neue imuvierepository-Implementierung an die Controller Klasse übergeben, und die-Klasse würde weiterhin funktionieren.

Wenn Sie die Klasse "moviescontroller" testen möchten, können Sie außerdem eine Klasse "Fake Movie Repository" an den "moviescontroller" übergeben. Sie können die imuvierepository-Klasse mit einer Klasse implementieren, die nicht auf die Datenbank zugreift, sondern alle erforderlichen Methoden der imuvierepository-Schnittstelle enthält. Auf diese Weise können Sie einen Komponenten Test für die Klasse "moviescontroller" durchlaufen, ohne tatsächlich auf eine echte Datenbank zuzugreifen.

## <a name="summary"></a>Summary

In diesem Tutorial wird veranschaulicht, wie Sie MVC-Modellklassen erstellen können, indem Sie die Vorteile von Microsoft LINQ to SQL nutzen. Wir haben zwei Strategien zum Anzeigen von Datenbankdaten in einer ASP.NET MVC-Anwendung untersucht. Zuerst haben wir LINQ to SQL Klassen erstellt und die Klassen direkt innerhalb einer Controller Aktion verwendet. Die Verwendung von LINQ to SQL Klassen in einem Controller ermöglicht das schnelle und einfache Anzeigen von Datenbankdaten in einer MVC-Anwendung.

Als nächstes wurde ein etwas schwierigerer, aber definitiv ausführlichere Pfad zum Anzeigen von Datenbankdaten untersucht. Wir haben das Repository-Muster genutzt und die gesamte Datenbankzugriffs Logik in einer separaten Repository-Klasse platziert. In unserem Controller haben wir den gesamten Code für eine Schnittstelle anstelle einer konkreten Klasse geschrieben. Der Vorteil des Repository-Musters besteht darin, dass es uns ermöglicht, Datenbankzugriffs Technologien in Zukunft problemlos zu ändern. so können wir unsere Controller Klassen problemlos testen.

> [!div class="step-by-step"]
> [Zurück](creating-model-classes-with-the-entity-framework-vb.md)
> [Weiter](displaying-a-table-of-database-data-vb.md)
