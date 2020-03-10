---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: Einführung in ASP.net Web Pages Anzeigen von Daten | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial wird gezeigt, wie Sie eine Datenbank in webmatrix erstellen und Datenbankdaten auf einer Seite anzeigen, wenn Sie ASP.net Web Pages (Razor) verwenden. Es wird davon ausgegangen, dass y...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 9e665ca8dd064c23a8b8bd3593014969d0c3da48
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421485"
---
# <a name="introducing-aspnet-web-pages---displaying-data"></a>Einführung in ASP.net Web Pages Anzeigen von Daten

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Tutorial wird gezeigt, wie Sie eine Datenbank in webmatrix erstellen und Datenbankdaten auf einer Seite anzeigen, wenn Sie ASP.net Web Pages (Razor) verwenden. Dabei wird davon ausgegangen, dass Sie die Reihe durch [Einführung in ASP.net Web Pages Programmierung](../introducing-razor-syntax-c.md)abgeschlossen haben.
> 
> Sie lernen Folgendes:
> 
> - Verwenden von webmatrix-Tools zum Erstellen einer Datenbank und von Datenbanktabellen.
> - Verwenden von webmatrix-Tools zum Hinzufügen von Daten zu einer Datenbank.
> - Anzeigen von Daten aus der Datenbank auf einer Seite.
> - Ausführen von SQL-Befehlen in ASP.net Web Pages.
> - Vorgehensweise beim Anpassen des `WebGrid` Hilfsprogramms zum Ändern der Datenanzeige und zum Hinzufügen von Paging und Sortierung.
>   
> 
> Erörterte Features und Technologien:
> 
> - Webmatrix-Datenbanktools.
> - `WebGrid`-Hilfsprogramm.

## <a name="what-youll-build"></a>Sie lernen Folgendes

Im vorherigen Tutorial haben Sie ASP.net Web Pages (*cshtml* -Dateien), die Grundlagen von Razor-Syntax und hilfshilfen eingeführt. In diesem Tutorial beginnen Sie mit dem Erstellen der eigentlichen Webanwendung, die Sie für die restliche Reihe verwenden. Bei der App handelt es sich um eine einfache Film Anwendung, mit der Sie Informationen zu Filmen anzeigen, hinzufügen, ändern und löschen können.

Wenn Sie mit diesem Tutorial fertig sind, können Sie eine Filmliste anzeigen, die wie folgt aussieht:

![WebGrid-Anzeige, die auf CSS-Klassennamen festgelegte Parameter enthält](displaying-data/_static/image1.png)

Zunächst müssen Sie jedoch eine Datenbank erstellen.

## <a name="a-very-brief-introduction-to-databases"></a>Eine sehr kurze Einführung in Datenbanken

In diesem Tutorial wird nur die kurze Einführung in die Datenbanken bereitgestellt. Wenn Sie über eine Datenbank verfügen, können Sie diesen kurzen Abschnitt überspringen.

Eine Datenbank enthält eine oder mehrere Tabellen, die Informationen &mdash; z. b. Tabellen für Kunden, Bestellungen und Lieferanten, oder für Schüler/Studenten, Lehrkräfte, Klassen und Noten enthalten. Strukturell gesehen ist eine Datenbanktabelle wie eine Tabelle. Stellen Sie sich ein typisches Adressbuch vor. Für jeden Eintrag im Adressbuch (d. h. für jede Person) haben Sie mehrere Informationen wie Vorname, Nachname, Adresse, e-Mail-Adresse und Telefonnummer.

![Beispiel einer Datenbanktabelle als einfaches Raster](displaying-data/_static/image2.png)

(Zeilen werden manchmal als *Datensätze*bezeichnet, und Spalten werden manchmal auch als *Felder*bezeichnet.)

Bei den meisten Datenbanktabellen muss die Tabelle über eine Spalte verfügen, die einen eindeutigen Wert enthält, wie z. b. eine Kundennummer, eine Kontonummer usw. Dieser Wert wird als *Primärschlüssel*der Tabelle bezeichnet und verwendet ihn, um jede Zeile in der Tabelle zu identifizieren. In diesem Beispiel ist die ID-Spalte der Primärschlüssel für das im vorherigen Beispiel gezeigte Adressbuch.

Ein Großteil der Arbeit in Webanwendungen besteht darin, Informationen aus der Datenbank zu lesen und auf einer Seite anzuzeigen. Außerdem werden Sie häufig Informationen von Benutzern erfassen und einer Datenbank hinzufügen, oder Sie ändern Datensätze, die bereits in der Datenbank vorhanden sind. (Im Verlauf dieses Tutorials werden alle diese Vorgänge behandelt.)

Datenbankaufgaben können enorm komplex sein und spezielle Kenntnisse erfordern. In diesem Lernprogramm Satz müssen Sie jedoch nur grundlegende Konzepte verstehen, die bei der Erläuterung erläutert werden.

## <a name="creating-a-database"></a>Erstellen einer Datenbank

Webmatrix enthält Tools, mit denen Sie problemlos eine Datenbank erstellen und Tabellen in der Datenbank erstellen können. (Die Struktur einer Datenbank wird als *Schema*der Datenbank bezeichnet.) Für dieses Tutorial erstellen Sie eine Datenbank, die nur eine einzige Tabelle &mdash; Filme enthält.

Öffnen Sie webmatrix, wenn Sie dies noch nicht getan haben, und öffnen Sie die Website Webwebsite-Filme, die Sie im vorherigen Tutorial erstellt haben.

Klicken Sie im linken Bereich auf den Arbeitsbereich **Datenbank** .

![Registerkarte "webmatrix-Datenbank"](displaying-data/_static/image3.png)

Das Menüband wird so geändert, dass datenbankbezogene Aufgaben angezeigt werden. Klicken Sie im Menüband auf **neue Datenbank**.

![Schaltfläche "neue Datenbank" im webmatrix-Menüband](displaying-data/_static/image4.png)

Webmatrix erstellt eine SQL Server CE-Datenbank (eine *sdf* -Datei) mit dem gleichen Namen wie Ihre Website &mdash; *webpgesmovies. sdf*. (Dies wird hier nicht beschrieben, aber Sie können die Datei in beliebige Dateien umbenennen, sofern Sie die Erweiterung " *. sdf* " haben.)

## <a name="creating-a-table"></a>Erstellen einer Tabelle

Klicken Sie im Menüband auf **neue Tabelle**. Webmatrix öffnet den Tabellen-Designer auf einer neuen Registerkarte. (wenn die Option neue Tabelle nicht verfügbar ist, stellen Sie sicher, dass die neue Datenbank in der Strukturansicht auf der linken Seite ausgewählt ist.)

![Schaltfläche "neue Tabelle" im webmatrix-Menüband](displaying-data/_static/image5.png)

Geben Sie im Textfeld am oberen Rand (in dem das Wasserzeichen "Enter Table Name" lautet) "Movies" ein.

![Eingeben eines Tabellennamens im webmatrix-Datenbankdesigner](displaying-data/_static/image6.png)

Der Bereich unterhalb des Tabellennamens besteht darin, dass Sie einzelne Spalten definieren. Für die Tabelle " *Filme* " in diesem Tutorial erstellen Sie nur einige Spalten: " *ID*", " *Title*", " *Genre*" und " *Jahr*".

Geben Sie im Feld **Name den Namen** "ID" ein. Wenn Sie hier einen Wert eingeben, werden alle Steuerelemente für die neue Spalte aktiviert.

Tabulator in die Liste **Datentyp** , und wählen Sie **int**aus. Dieser Wert gibt an, dass die ID-Spalte ganzzahlige Daten (Number) enthalten soll.

> [!NOTE]
> Wir werden Sie hier (viel) nicht mehr nennen, Sie können jedoch standardmäßige Windows-Tastatur Gesten verwenden, um in diesem Raster zu navigieren. Sie können z. b. die Tab-Taste zwischen den Feldern eingeben, um ein Element in einer Liste auszuwählen usw.

Tab-Taste hinter dem **Standardwert** Feld (d. h. leer lassen). Aktivieren Sie das Kontrollkästchen **is Primary Key** , und wählen Sie es aus. Mit dieser Option wird der Datenbank mitgeteilt, dass die Spalte *ID* die Daten enthält, mit denen einzelne Zeilen identifiziert werden. (Das heißt, jede Zeile weist einen eindeutigen Wert in der ID-Spalte auf, den Sie verwenden können, um diese Zeile zu finden.)

Wählen Sie die Option **ist Identity** . Mit dieser Option wird der Datenbank mitgeteilt, dass die nächste sequenzielle Zahl für jede neue Zeile automatisch generiert werden soll. (Die Option **ist ist** nur funktionsfähig, wenn Sie auch die Option **ist Primärschlüssel** ausgewählt haben.)

Klicken Sie in die nächste Raster Zeile, oder drücken Sie zweimal die Tab-Taste, um die aktuelle Zeile zu beenden. Eine der beiden Gesten speichert die aktuelle Zeile und startet die nächste Zeile. Beachten Sie, dass die Spalte **Standardwert** jetzt **null**lautet. (Null ist der Standardwert für den Standardwert, um zu sprechen.)

Wenn Sie die neue **ID** -Spalte definiert haben, sieht der Designer wie in der folgenden Abbildung aus:

![Webmatrix-Datenbankdesigner nach dem Definieren der ID-Spalte für die Movies-Tabelle](displaying-data/_static/image7.png)

Um die nächste Spalte zu erstellen, klicken Sie in das Feld in der Spalte **Name** . Geben Sie für die Spalte "Title" ein, und wählen Sie dann **nvarchar** als **Datentyp** Wert aus. Der "var"-Teil von **nvarchar** teilt der Datenbank mit, dass es sich bei den Daten für diese Spalte um eine Zeichenfolge handelt, deren Größe von Datensatz zu Datensatz variieren kann. (Das Präfix "n" steht für "National" und gibt an, dass das Feldzeichen Daten für jedes Alphabet oder das Schreiben von System enthalten kann, d. –., dass das Feld Unicode-Daten enthält.)

Wenn Sie **nvarchar**auswählen, wird ein weiteres Feld angezeigt, in dem Sie die maximale Länge für das Feld eingeben können. Geben Sie 50 ein, unter der Annahme, dass kein Filmtitel, mit dem Sie in diesem Tutorial arbeiten werden, länger als 50 Zeichen ist.

**Standardwert** überspringen und die Option NULL-Werte **zulassen deaktivieren** . Sie möchten nicht, dass in der Datenbank Filme in die Datenbank eingegeben werden dürfen, die keinen Titel haben.

Wenn Sie fertig sind und zur nächsten Zeile wechseln, sieht der Designer wie in der folgenden Abbildung aus:

![Webmatrix-Datenbankdesigner nach dem Definieren der Spalte Titel für die Tabelle Movies](displaying-data/_static/image8.png)

Wiederholen Sie diese Schritte zum Erstellen einer Spalte mit dem Namen "Genre", mit Ausnahme der Länge, legen Sie Sie nur auf 30 fest.

Erstellen Sie eine weitere Spalte mit dem Namen "Year". Wählen Sie als Datentyp **NCHAR** (nicht **nvarchar**) aus, und legen Sie die Länge auf 4 fest. Für das Jahr verwenden Sie eine vierstellige Zahl, wie z. b. "1995" oder "2010", sodass Sie keine Spalte mit variabler Größenangabe benötigen.

Der fertige Entwurf sieht wie folgt aus:

![Webmatrix-Datenbankdesigner, nachdem alle Felder für die Tabelle "Movies" definiert wurden](displaying-data/_static/image9.png)

Drücken Sie STRG + S, oder klicken Sie auf der Symbolleiste schnell Zugriff auf die Schaltfläche **Speichern** . Schließen Sie den Datenbank-Designer, indem Sie die Registerkarte schließen.

## <a name="adding-some-example-data"></a>Hinzufügen von Beispiel Daten

Später in dieser tutorialreihe erstellen Sie eine Seite, auf der Sie neue Filme in einem Formular eingeben können. Jetzt können Sie jedoch einige Beispiel Daten hinzufügen, die Sie dann auf einer Seite anzeigen können.

Beachten Sie im Arbeitsbereich **Datenbank** in webmatrix, dass es eine Struktur gibt, in der die zuvor erstellte *sdf* -Datei angezeigt wird. Öffnen Sie den Knoten für die neue *sdf* -Datei, und öffnen Sie dann den Knoten **Tabellen** .

![Webmatrix-Daten Bank Arbeitsbereich mit Tree Open for Movies-Tabelle](displaying-data/_static/image10.png)

Klicken Sie mit der rechten Maustaste auf den Knoten **Filme** , und wählen Sie **Daten**. Webmatrix öffnet ein Raster, in dem Sie Daten für die *Filme* -Tabelle eingeben können:

![Daten Bank Eingabe Raster in webmatrix (leer)](displaying-data/_static/image11.png)

Klicken Sie auf die Spalte **Titel** , und geben Sie "if Harry Met Sally" ein. Wechseln Sie zur **Genre** -Spalte (Sie können die Tab-Taste verwenden), und geben Sie "Romantik-Comedy" ein. Wechseln Sie in die Spalte **year** , und geben Sie "1989" ein:

![Daten Bank Eingabe Raster in webmatrix mit einem Datensatz](displaying-data/_static/image12.png)

Drücken Sie die EINGABETASTE, und webmatrix speichert den neuen Film. Beachten Sie, dass die **ID** -Spalte ausgefüllt wurde.

![Daten Bank Eingabe Raster in webmatrix mit einem Datensatz und einer automatisch generierten ID](displaying-data/_static/image13.png)

Geben Sie einen anderen Film ein (z. b. "Weg mit Wind", "Drama", "1939"). Die ID-Spalte wird erneut ausgefüllt:

![Daten Bank Eingabe Raster in webmatrix mit zwei Datensätzen und automatisch generierten IDs](displaying-data/_static/image14.png)

Geben Sie einen dritten Film ein (z. b. "Ghostbusters", "Comedy"). Lassen Sie als Experiment die **year** -Spalte leer, und drücken Sie dann die EINGABETASTE. Da Sie die Option **NULL-zulassen** deaktiviert haben, wird in der Datenbank ein Fehler angezeigt:

![Der Fehler "ungültige Daten" wird angezeigt, wenn ein erforderlicher Spaltenwert leer gelassen wird.](displaying-data/_static/image15.png)

Klicken Sie auf **OK** , um zurückzukehren und den Eintrag zu korrigieren (das Jahr für "Ghostbusters" ist 1984), und drücken Sie dann die EINGABETASTE.

Füllen Sie mehrere Filme aus, bis Sie 8 oder so haben. (Durch die Eingabe von 8 ist es einfacher, später mit dem Paging zu arbeiten. Wenn dies jedoch zu viele ist, geben Sie jetzt nur noch ein paar ein.) Die eigentlichen Daten sind unerheblich.

![Daten Bank Eingabe Raster in webmatrix mit beiden Datensätzen](displaying-data/_static/image16.png)

Wenn Sie alle Filme ohne Fehler eingegeben haben, sind die ID-Werte sequenziell. Wenn Sie versucht haben, einen unvollständigen Film Daten Satz zu speichern, sind die ID-Nummern möglicherweise nicht sequenziell. Wenn dies der Fall ist, ist das in Ordnung. Die Zahlen haben keine inhärente Bedeutung, und die einzige wichtige Bedeutung ist, dass Sie in der *Film* Tabelle eindeutig sind.

Schließen Sie die Registerkarte, die den Datenbank-Designer enthält.

Nun können Sie diese Daten auf einer Webseite anzeigen.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Anzeigen von Daten in einer Seite mithilfe des WebGrid-Hilfsprogramms

Zum Anzeigen von Daten auf einer Seite verwenden Sie das `WebGrid`-Hilfsprogramm. Diese Hilfsmethode erzeugt eine Anzeige in einem Raster oder einer Tabelle (Zeilen und Spalten). Wie Sie sehen werden, können Sie das Raster mit Formatierungen und anderen Features verfeinern.

Zum Ausführen des Rasters müssen Sie einige Zeilen Code schreiben. Diese paar Zeilen dienen als Muster für fast alle Daten Zugriffe, die Sie in diesem Tutorial ausführen.

> [!NOTE]
> Sie haben tatsächlich viele Optionen zum Anzeigen von Daten auf einer Seite. Das `WebGrid`-Hilfsprogramm ist nur eins. Wir haben uns für dieses Tutorial entschieden, da es die einfachste Möglichkeit zum Anzeigen von Daten ist, und da es recht flexibel ist. Im nächsten Tutorial wird erläutert, wie Sie eine "Manuelle" Methode zum Arbeiten mit Daten auf der Seite verwenden, mit der Sie die Anzeige der Daten besser steuern können.

Klicken Sie im linken Bereich von webmatrix auf den Arbeitsbereich **Dateien** .

Die neue Datenbank, die Sie erstellt haben, befindet sich im Ordner *App\_Data* . Wenn der Ordner nicht bereits vorhanden ist, wurde er von webmatrix für die neue Datenbank erstellt. (Der Ordner ist möglicherweise bereits vorhanden, wenn Sie zuvor hilfshilfen installiert haben.)

Wählen Sie in der Strukturansicht den Stamm der Website aus. Sie müssen den Stamm der Website auswählen. Andernfalls wird die neue Datei möglicherweise dem App-\_Datenordner hinzugefügt.

Klicken Sie im Menüband auf **neu**. Wählen Sie im Feld **Wählen Sie einen Dateityp** aus die Option **cshtml**aus.

Benennen Sie im Feld **Name** die neue Seite "Movies. cshtml":

![Dialogfeld "Dateityp auswählen" zeigt die Seite "Filme" an](displaying-data/_static/image17.png)

Klicken Sie auf die Schaltfläche **OK** . Webmatrix öffnet eine neue Datei mit einigen Skeleton-Elementen. Zuerst schreiben Sie Code, um die Daten aus der Datenbank zu erhalten. Dann fügen Sie der Seite Markup hinzu, um die Daten tatsächlich anzuzeigen.

### <a name="writing-the-data-query-code"></a>Schreiben des Datenabfrage Codes

Geben Sie oben auf der Seite zwischen den `@{`-und `}` Zeichen den folgenden Code ein. (Stellen Sie sicher, dass Sie diesen Code zwischen öffnenden und schließenden geschweiften Klammern eingeben.)

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

In der ersten Zeile wird die Datenbank geöffnet, die Sie zuvor erstellt haben. Dies ist immer der erste Schritt, bevor Sie einen Vorgang mit der Datenbank ausführen. Sie können den Namen der `Database.Open` Methode der zu öffnenden Datenbank angeben. Beachten Sie, dass Sie " *. sdf* " nicht in den Namen einschließen. Bei der `Open`-Methode wird davon ausgegangen, dass Sie nach einer *sdf* -Datei (d. h *. "webpgesmovies. sdf*") sucht und dass sich die *sdf* -Datei im Ordner " *App\_Data* " befindet. (Früher haben wir angemerkt, dass der *App-\_Daten* Ordner reserviert ist. dieses Szenario ist eine der Orte, an denen ASP.net Annahmen zu diesem Namen trifft.)

Wenn die Datenbank geöffnet wird, wird ein Verweis auf die Variable mit dem Namen "`db`" eingefügt. (Die als beliebiger Name bezeichnet werden können.) Die `db` Variable gibt an, wie Sie mit der Datenbank interagieren.

Die zweite Zeile Ruft die Datenbankdaten tatsächlich mithilfe der `Query`-Methode ab. Beachten Sie, dass dieser Code funktioniert: die `db` Variable enthält einen Verweis auf die geöffnete Datenbank, und Sie rufen die `Query`-Methode mithilfe der `db`-Variablen (`db.Query`) auf.

Die Abfrage selbst ist eine SQL `Select`-Anweisung. (Einen kleinen Hintergrund zu SQL finden Sie in der Erläuterung weiter unten.) In der-Anweisung identifiziert `Movies` die abzufragende Tabelle. Das `*` Zeichen gibt an, dass die Abfrage alle Spalten aus der Tabelle zurückgeben soll. (Sie können Spalten auch einzeln auflisten, getrennt durch Kommas.)

Die Ergebnisse der Abfrage werden, sofern vorhanden, zurückgegeben und in der `selectedData` Variablen verfügbar gemacht. Auch hier kann die Variable beliebig benannt werden.

Zum Schluss weist die dritte Zeile ASP.net an, dass Sie eine Instanz des `WebGrid`-Hilfsprogramms verwenden möchten. Sie erstellen (*instanziieren*) Sie das Hilfsobjekt mit dem `new`-Schlüsselwort, und übergeben Sie die Abfrageergebnisse über die `selectedData` Variable. Das neue `WebGrid` Objekt wird zusammen mit den Ergebnissen der Datenbankabfrage in der `grid`-Variablen verfügbar gemacht. Sie benötigen dieses Ergebnis in einem Moment, um die Daten auf der Seite anzuzeigen.

In dieser Phase wurde die Datenbank geöffnet, Sie haben die gewünschten Daten abgerufen, und Sie haben das `WebGrid`-Hilfsprogramm mit diesen Daten vorbereitet. Als Nächstes erstellen Sie das Markup auf der Seite.

> [!TIP] 
> 
> **Structured Query Language (SQL) (Strukturierte Abfragesprache (SQL))**
> 
> SQL ist eine Sprache, die in den meisten relationalen Datenbanken zum Verwalten von Daten in einer Datenbank verwendet wird. Sie enthält Befehle, mit denen Sie Daten abrufen und aktualisieren können, und mit denen Sie Daten in Datenbanktabellen erstellen, ändern und verwalten können. SQL unterscheidet sich von einer Programmiersprache ( C#z. b.). Mit SQL teilen Sie der Datenbank mit, was Sie möchten, und es ist die Aufgabe der Datenbank, herauszufinden, wie Sie die Daten erhalten oder die Aufgabe ausführen. Im folgenden finden Sie Beispiele für einige SQL-Befehle und deren Funktionsweise:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Die erste `Select`-Anweisung ruft alle Spalten (angegeben durch `*`) aus der *Movies* -Tabelle ab.
> 
> Die zweite `Select`-Anweisung ruft die Spalten "ID", "Name" und "Price" aus den Datensätzen in der *Product* -Tabelle ab, deren Preis Spaltenwert größer als 10 ist. Der Befehl gibt die Ergebnisse in alphabetischer Reihenfolge basierend auf den Werten der Namensspalte zurück. Wenn keine Datensätze mit den Preiskriterien übereinstimmen, gibt der Befehl eine leere Menge zurück.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Mit diesem Befehl wird ein neuer Datensatz in die Tabelle " *Product* " eingefügt, wobei die Spalte "Name" auf "Croissant", die Spalte "Beschreibung" auf "a unzuverlässigen Delight" und der Preis auf 1,99 festgelegt wird.
> 
> Beachten Sie Folgendes: Wenn Sie einen nicht numerischen Wert angeben, wird der Wert in einfache Anführungszeichen eingeschlossen (in doppelte Anführungszeichen, wie in C#). Sie verwenden diese Anführungszeichen um Text-oder Datumswerte, aber nicht um Zahlen.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Mit diesem Befehl werden Datensätze in der Tabelle " *Product* " gelöscht, deren Ablaufdatum-Spalte vor dem 1. Januar 2008 liegt. (Der Befehl geht davon aus, dass die *Product* -Tabelle natürlich eine solche Spalte aufweist.) Das Datum wird hier im Format mm/dd/yyyy eingegeben, sollte jedoch in dem Format eingegeben werden, das für Ihr Gebiets Schema verwendet wird.
> 
> Die Befehle `Insert` und `Delete` geben keine Resultsets zurück. Stattdessen geben Sie eine Zahl zurück, die Aufschluss darüber gibt, wie viele Datensätze vom Befehl betroffen sind.
> 
> Für einige dieser Vorgänge (z. b. das Einfügen und Löschen von Datensätzen) muss der Prozess, der den Vorgang anfordert, über die entsprechenden Berechtigungen in der Datenbank verfügen. Daher müssen Sie bei Produktionsdatenbanken häufig einen Benutzernamen und ein Kennwort angeben, wenn Sie eine Verbindung mit der Datenbank herstellen.
> 
> Es gibt Dutzende von SQL-Befehlen, aber alle folgen einem Muster wie den hier aufgeführten Befehlen. Sie können SQL-Befehle zum Erstellen von Datenbanktabellen, zum zählen der Anzahl von Datensätzen in einer Tabelle, zum Berechnen der Preise und zum Ausführen von vielen weiteren Vorgängen verwenden.

### <a name="adding-markup-to-display-the-data"></a>Hinzufügen von Markup zum Anzeigen der Daten

Legen Sie im `<head>`-Element den Inhalt des `<title>`-Elements auf "Movies" fest:

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

Fügen Sie im `<body>`-Element der Seite Folgendes hinzu:

[!code-html[Main](displaying-data/samples/sample3.html)]

Das ist alles. Die `grid` Variable ist der Wert, den Sie erstellt haben, als Sie das `WebGrid`-Objekt zuvor in Code erstellt haben.

Klicken Sie in der webmatrix-Strukturansicht mit der rechten Maustaste auf die Seite, und wählen Sie **im Browser starten aus**. Sie sehen etwa diese Seite:

![Standardausgabe des WebGrid-Hilfsprogramms aus der Movies-Tabelle](displaying-data/_static/image18.png)

Klicken Sie auf den Link Spaltenüberschrift, um nach dieser Spalte zu sortieren. Wenn Sie durch Klicken auf eine Überschrift sortieren möchten, ist dies ein Feature, das in das **WebGrid** -Hilfsprogramm integriert ist.

Die `GetHtml`-Methode, wie der Name vermuten lässt, generiert Markup, mit dem die Daten angezeigt werden. Standardmäßig generiert die `GetHtml`-Methode ein HTML-`<table>` Element. (Wenn Sie möchten, können Sie das Rendering überprüfen, indem Sie sich die Quelle der Seite im Browser ansehen.)

## <a name="modifying-the-look-of-the-grid"></a>Ändern des Aussehens des Rasters

Die Verwendung des `WebGrid`-Hilfsprogramms ist einfach, aber die resultierende Anzeige ist einfach. Das `WebGrid`-Hilfsprogramm verfügt über alle möglichen Optionen, mit denen Sie steuern können, wie die Daten angezeigt werden. In diesem Tutorial gibt es viel zu viele Möglichkeiten, aber in diesem Abschnitt werden einige dieser Optionen erläutert. In späteren Tutorials dieser Reihe werden einige zusätzliche Optionen behandelt.

### <a name="specifying-individual-columns-to-display"></a>Angeben einzelner Anzuzeigende Spalten

Zum starten können Sie angeben, dass Sie nur bestimmte Spalten anzeigen möchten. Standardmäßig zeigt das Raster alle vier Spalten aus der *Film* Tabelle an.

Ersetzen Sie in der Datei *Movies. cshtml* das `@grid.GetHtml()` Markup, das Sie soeben hinzugefügt haben, durch Folgendes:

[!code-css[Main](displaying-data/samples/sample4.css)]

Um dem Hilfsprogramm mitzuteilen, welche Spalten angezeigt werden sollen, fügen Sie einen `columns`-Parameter für die `GetHtml`-Methode ein, und übergeben Sie eine Auflistung von Spalten. Geben Sie in der-Auflistung jede einzuschließgende Spalte an. Sie geben eine einzelne Spalte an, die angezeigt werden soll, indem Sie ein `grid.Column` Objekt einschließen, und übergeben den Namen der gewünschten Datenspalte. (Diese Spalten müssen in den SQL-Abfrage Ergebnissen enthalten sein – die `WebGrid`-Hilfsprogramm kann keine Spalten anzeigen, die nicht von der Abfrage zurückgegeben wurden.)

Starten Sie die Seite *Movies. cshtml* erneut im Browser, und dieses Mal erhalten Sie eine Anzeige wie die folgende (Beachten Sie, dass keine ID-Spalte angezeigt wird):

![WebGrid-Anzeige, die nur ausgewählte Spalten anzeigt](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Ändern des Aussehens des Rasters

Es gibt einige weitere Optionen zum Anzeigen von Spalten, von denen einige in späteren Tutorials in diesem Satz untersucht werden. In diesem Abschnitt erfahren Sie, wie Sie das Raster als Ganzes formatieren können.

Fügen Sie im `<head>` Abschnitt der Seite direkt vor dem schließenden `</head>`-Tag das folgende `<style>`-Element hinzu:

[!code-css[Main](displaying-data/samples/sample5.css)]

Dieses CSS-Markup definiert Klassen mit dem Namen "`grid`", "`head`" usw. Diese Format Definitionen können auch in einer separaten *CSS* -Datei abgelegt und mit der Seite verknüpft werden. (Tatsächlich wird dies später in diesem Lernprogramm Satz durchführen.) Um die Arbeit für dieses Tutorial zu vereinfachen, befinden Sie sich jedoch auf derselben Seite, auf der die Daten angezeigt werden.

Nun können Sie das `WebGrid`-Hilfsprogramm zum Verwenden dieser Stil Klassen erhalten. Das Hilfsprogramm verfügt über eine Reihe von Eigenschaften (z. b. `tableStyle`) zu diesem Zweck – Sie weisen diesen einen CSS-Stil Klassennamen zu, und dieser Klassenname wird als Teil des Markups gerendert, das vom Hilfsprogramm gerendert wird.

Ändern Sie das `grid.GetHtml` Markup so, dass es jetzt wie der folgende Code aussieht:

[!code-css[Main](displaying-data/samples/sample6.css)]

Neues hier ist, dass Sie der `GetHtml`-Methode `tableStyle`-, `headerStyle`-und `alternatingRowStyle`-Parameter hinzugefügt haben. Diese Parameter wurden auf die Namen der CSS-Stile festgelegt, die Sie vor einem Moment eingefügt haben.

Führen Sie die Seite aus, und dieses Mal wird ein Raster angezeigt, das deutlich weniger genau als zuvor aussieht:

![WebGrid-Anzeige, die auf CSS-Klassennamen festgelegte Parameter enthält](displaying-data/_static/image20.png)

Um zu sehen, was die `GetHtml` Methode generiert hat, können Sie sich die Quelle der Seite im Browser ansehen. Wir werden hier nicht ausführlich darauf eingehen, aber der wichtigste Punkt ist, dass Sie durch die Angabe von Parametern wie `tableStyle`dazu geführt haben, dass das Raster HTML-Tags wie die folgende generiert:

`<table class="grid">`

Dem `<table>`-Tag wurde ein `class` Attribut hinzugefügt, das auf eine der zuvor hinzugefügten CSS-Regeln verweist. In diesem Code wird das grundlegende Muster &mdash; verschiedenen Parametern für die `GetHtml`-Methode gezeigt, mit denen Sie auf CSS-Klassen verweisen können, die von der Methode generiert werden, und das Markup Was Sie mit den CSS-Klassen tun, ist Ihnen überlassen.

## <a name="adding-paging"></a>Hinzufügen von Paging

Als letzte Aufgabe für dieses Tutorial fügen Sie Paging zum Raster hinzu. Momentan ist es kein Problem, alle Filme gleichzeitig anzuzeigen. Wenn Sie jedoch Hunderte von Filmen hinzugefügt haben, wird die Seiten Anzeige lang angezeigt.

Ändern Sie im Seitencode die Zeile, die das `WebGrid`-Objekt erstellt, in den folgenden Code:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

Der einzige Unterschied von before besteht darin, dass Sie einen `rowsPerPage`-Parameter hinzugefügt haben, der auf 3 festgelegt ist.

Führen Sie die Seite aus. Im Raster werden drei Zeilen gleichzeitig angezeigt, sowie Navigationslinks, mit denen Sie die Filme in der Datenbank durchlaufen können:

![WebGrid-Anzeige mit Paging](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Nächste nächste

Im nächsten Tutorial erfahren Sie, wie Sie Razor und C# Code verwenden, um Benutzereingaben in einem Formular zu erhalten. Sie fügen der Seite "Filme" ein Suchfeld hinzu, damit Sie Filme nach Titel oder Genre suchen können.

## <a name="complete-listing-for-movies-page"></a>Seite "Auflistung für Filme vervollständigen"

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in die ASP.net-Webprogrammierung mit der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Zurück](intro-to-web-pages-programming.md)
> [Weiter](form-basics.md)
