---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Teil 4: Modelle und Datenzugriff | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. In Teil 4 werden Modelle und der Datenzugriff behandelt.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 402be340f1ea3344675e7b859cea8c5130cfc8ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451017"
---
# <a name="part-4-models-and-data-access"></a>Teil 4: Modelle und Datenzugriff

von [Jon Galloway](https://github.com/jongalloway)

> Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.  
>   
> Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.
> 
> In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. In Teil 4 werden Modelle und der Datenzugriff behandelt.

Bisher haben wir soeben "Dummy-Daten" von unseren Controllern an unsere Ansichts Vorlagen übergeben. Jetzt sind wir bereit, eine echte Datenbank zu verbinden. In diesem Tutorial wird erläutert, wie Sie SQL Server Compact Edition (häufig als SQL CE bezeichnet) als Datenbank-Engine verwenden. Bei SQL CE handelt es sich um eine kostenlose, eingebettete, dateibasierte Datenbank, für die keine Installation oder Konfiguration erforderlich ist, was für die lokale Entwicklung praktisch ist.

## <a name="database-access-with-entity-framework-code-first"></a>Datenbankzugriff mit Entity Framework Code First

Wir verwenden die Unterstützung von Entity Framework (EF), die in ASP.NET MVC 3-Projekten enthalten ist, um die Datenbank abzufragen und zu aktualisieren. EF ist eine flexible ORM-Daten-API (Object Relational Mapping), mit der Entwickler in einer Datenbank gespeicherte Daten in objektorientierter Weise Abfragen und aktualisieren können.

Entity Framework Version 4 unterstützt ein Entwicklungsparadigma namens "Code First". Code First ermöglicht das Erstellen eines Modell Objekts, indem einfache Klassen geschrieben werden (auch als poco aus "Plain-Old" CLR-Objekten bezeichnet), und die Datenbank kann sogar dynamisch von ihren Klassen erstellt werden.

### <a name="changes-to-our-model-classes"></a>Änderungen an unseren Modellklassen

Wir nutzen das Daten Bank Erstellungs Feature in Entity Framework in diesem Tutorial. Bevor wir dies tun, nehmen wir jedoch einige geringfügige Änderungen an unseren Modellklassen vor, um einige Dinge hinzuzufügen, die wir später verwenden werden.

#### <a name="adding-the-artist-model-classes"></a>Hinzufügen der Klassen des Modell Modells

Unsere Alben werden Künstlern zugeordnet, daher fügen wir eine einfache Modell Klasse hinzu, um einen Künstler zu beschreiben. Fügen Sie dem Ordner Models mithilfe des unten gezeigten Codes eine neue Klasse mit dem Namen Artist.cs hinzu.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Aktualisieren der Modellklassen

Aktualisieren Sie die-Album Klasse wie unten gezeigt.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Nehmen Sie als nächstes die folgenden Aktualisierungen an der Genre-Klasse vor.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-app_data-folder"></a>Hinzufügen der APP\_Datenordner

Wir fügen dem Projekt eine APP\_Datenverzeichnis hinzu, um unsere SQL Server Express Datenbankdateien zu speichern. App-\_Daten sind ein spezielles Verzeichnis in ASP.net, das bereits über die erforderlichen Sicherheits Zugriffsberechtigungen für den Datenbankzugriff verfügt. Wählen Sie im Menü Projekt die Option ASP.NET-Ordner hinzufügen und dann App-\_Daten aus.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Erstellen einer Verbindungs Zeichenfolge in der Datei "Web. config"

Wir fügen der Konfigurationsdatei der Website einige Zeilen hinzu, damit Entity Framework weiß, wie eine Verbindung mit der Datenbank hergestellt wird. Doppelklicken Sie auf die Datei "Web. config", die sich im Stammverzeichnis des Projekts befindet.

![](mvc-music-store-part-4/_static/image2.png)

Scrollen Sie zum Ende dieser Datei, und fügen Sie einen &lt;connectionStrings&gt; Abschnitt direkt oberhalb der letzten Zeile hinzu, wie unten gezeigt.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Hinzufügen einer Kontext Klasse

Klicken Sie mit der rechten Maustaste auf den Ordner Modelle, und fügen Sie eine neue Klasse namens MusicStoreEntities.cs

![](mvc-music-store-part-4/_static/image3.png)

Diese Klasse stellt den Daten Bank Kontext Entity Framework dar und übernimmt die Erstellungs-, Lese-, Aktualisierungs-und Löschvorgänge für uns. Der Code für diese Klasse wird unten dargestellt.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

Das ist das, es gibt keine andere Konfiguration, keine speziellen Schnittstellen usw. Durch die Erweiterung der dbcontext-Basisklasse kann unsere Klasse "musicstoreentities" unsere Daten Bank Vorgänge für uns verarbeiten. Nachdem wir nun damit verknüpft sind, fügen wir unseren Modellklassen einige weitere Eigenschaften hinzu, um einige der zusätzlichen Informationen in unserer Datenbank zu nutzen.

### <a name="adding-our-store-catalog-data"></a>Hinzufügen von Daten zum Store-Katalog

Wir nutzen eine Funktion in Entity Framework, die einer neu erstellten Datenbank "Seed"-Daten hinzufügt. Dadurch wird der Store-Katalog mit einer Liste von Genres, Künstlern und Alben vorab aufgefüllt. Der MvcMusicStore-Assets. zip-Download, der unsere zuvor in diesem Tutorial verwendeten Website Designdateien enthielt, verfügt über eine Klassendatei mit diesen Seed-Daten, die sich in einem Ordner mit dem Namen "Code" befindet.

Suchen Sie im Ordner Code/Models die Datei SampleData.cs, und legen Sie Sie im Ordner Models in unserem Projekt ab, wie unten gezeigt.

![](mvc-music-store-part-4/_static/image4.png)

Nun müssen wir eine Codezeile hinzufügen, um Entity Framework über die SampleData-Klasse zu informieren. Doppelklicken Sie auf die Datei Global. asax im Stammverzeichnis des Projekts, um es zu öffnen, und fügen Sie die folgende Zeile am Anfang der Anwendung\_Start-Methode hinzu.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

An diesem Punkt haben wir die notwendigen Schritte zum Konfigurieren von Entity Framework für das Projekt abgeschlossen.

## <a name="querying-the-database"></a>Abfragen der Datenbank

Nun aktualisieren wir den StoreController, sodass anstelle der Verwendung von "Dummy-Daten" stattdessen die Datenbank aufgerufen wird, um alle Informationen abzufragen. Wir beginnen mit der Deklaration eines Felds auf dem **StoreController** , das eine Instanz der Klasse "musicstoreentities" mit dem Namen "storedb" enthält:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Aktualisieren des Speicher Indexes zum Abfragen der Datenbank

Die Klasse "musicstoreentities" wird vom Entity Framework verwaltet und macht eine Auflistungs Eigenschaft für jede Tabelle in der Datenbank verfügbar. Wir aktualisieren nun die Index Aktion von StoreController, um alle Genres in unserer Datenbank abzurufen. Zuvor haben wir die Zeichen folgen Daten hart codiert. Nun können Sie einfach die Entity Framework Kontext Generations Auflistung verwenden:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Es müssen keine Änderungen an der Ansichts Vorlage vorgenommen werden, da wir immer noch das gleiche storeindexviewmodel zurückgeben, das wir zuvor zurückgegeben haben. Wir geben jetzt nur noch Livedaten aus unserer Datenbank zurück.

Wenn wir das Projekt erneut ausführen und die URL "/Store" aufrufen, sehen wir nun eine Liste aller Genres in unserer Datenbank:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Aktualisieren von Store-durchsuchen und Details zur Verwendung von Livedaten

Mit der Action-Methode/Store/Browse? Genre = *[some-Genre]* suchen wir nach einem Genre anhand des Namens. Wir erwarten nur ein Ergebnis, da wir niemals zwei Einträge für denselben Genre Namen haben sollten, und wir können also das verwenden. Single ()-Erweiterung in LINQ, um das entsprechende Genre Objekt wie folgt abzufragen (geben Sie dies noch nicht ein):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

Die Single-Methode nimmt einen Lambda-Ausdruck als Parameter an, der angibt, dass ein einzelnes Genre Objekt so ist, dass sein Name mit dem definierten Wert übereinstimmt. Im obigen Fall laden wir ein einzelnes Genre Objekt mit einem namens Wert, der Disco entspricht.

Wir nutzen ein Entity Framework Feature, das es uns ermöglicht, andere Verwandte Entitäten anzugeben, die wir ebenfalls laden möchten, wenn das Genre Objekt abgerufen wird. Diese Funktion wird als Abfrageergebnis Strukturierung bezeichnet und ermöglicht es uns, die Anzahl der Zugriffe auf die Datenbank zu reduzieren, um alle benötigten Informationen abzurufen. Wir möchten die Alben für das Genre, das wir abrufen, vorab abrufen. daher aktualisieren wir unsere Abfrage so, dass Sie aus Genres. include ("Alben") enthalten ist, um anzugeben, dass wir auch Verwandte Alben benötigen. Dies ist effizienter, da sowohl unsere Genre-als auch die Album Daten in einer einzelnen Daten Bank Anforderung abgerufen werden.

Nachfolgend finden Sie eine Erläuterung dazu, wie unsere aktualisierte Aktion zum Durchsuchen von Controllern aussieht:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Wir können nun die Ansicht "Store-durchsuchen" aktualisieren, um die in den einzelnen Genres verfügbaren Alben anzuzeigen. Öffnen Sie die Ansichts Vorlage (in/views/Store/Browse.cshtml), und fügen Sie eine Auflistungs Liste der Alben hinzu, wie unten gezeigt.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Wenn wir unsere Anwendung ausführen und zu/Store/Browse? Genre = Jazz navigieren, sehen wir, dass unsere Ergebnisse jetzt aus der Datenbank abgerufen werden, sodass alle Alben in unserem ausgewählten Genre angezeigt werden.

![](mvc-music-store-part-4/_static/image2.jpg)

Wir nehmen dieselbe Änderung an der/Store/Details/-URL (ID]) vor und ersetzen unsere Dummydaten durch eine Datenbankabfrage, die ein Album lädt, dessen ID mit dem Parameterwert übereinstimmt.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Wenn Sie die Anwendung ausführen und zu/Store/Details/1 navigieren, sehen Sie, dass unsere Ergebnisse jetzt aus der Datenbank abgerufen werden.

![](mvc-music-store-part-4/_static/image5.png)

Nachdem die Seite "Store-Details" so eingerichtet ist, dass Sie ein Album anhand der Album-ID anzeigt, aktualisieren wir nun die **browseinsicht** , um eine Verknüpfung mit der Detailansicht zu erhalten. Wir verwenden "HTML. Action Link" genau wie bei der Verknüpfung aus dem Store-Index zum Speichern von durchsuchen am Ende des vorherigen Abschnitts. Die gesamte Quelle für die browseinsicht wird unten angezeigt.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Wir können nun von unserer Store-Seite zu einer Genre Seite navigieren, die die verfügbaren Alben auflistet, und durch Klicken auf ein Album können wir Details zu diesem Album anzeigen.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-3.md)
> [Weiter](mvc-music-store-part-5.md)
