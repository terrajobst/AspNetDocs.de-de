---
uid: web-pages/overview/data/5-working-with-data
title: Einführung in das Arbeiten mit einer Datenbank auf ASP.net Web Pages (Razor) Websites | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Kapitel wird beschrieben, wie Sie auf Daten aus einer Datenbank zugreifen und Sie mit ASP.net Web Pages anzeigen.
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 45e988d037465e59ad352bb9444af2c69fd3cd70
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474039"
---
# <a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Einführung in das Arbeiten mit einer Datenbank auf ASP.net Web Pages (Razor) Websites

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird beschrieben, wie Sie mit Microsoft webmatrix-Tools eine Datenbank auf einer ASP.net Web Pages-Website (Razor) erstellen und wie Sie Seiten erstellen, mit denen Sie Daten anzeigen, hinzufügen, bearbeiten und löschen können.
> 
> **Lernen Sie Folgendes:** 
> 
> - Erstellen einer Datenbank.
> - Vorgehensweise beim Herstellen einer Verbindung mit einer Datenbank
> - Anzeigen von Daten auf einer Webseite.
> - Vorgehensweise beim Einfügen, aktualisieren und Löschen von Datenbankdaten Sätzen.
> 
> Dies sind die im Artikel eingeführten Features:
> 
> - Arbeiten mit einer Microsoft SQL Server Compact Edition-Datenbank
> - Arbeiten mit SQL-Abfragen.
> - Die `Database`-Klasse.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Dieses Tutorial funktioniert auch mit webmatrix 3. Sie können ASP.net Web Pages 3 und Visual Studio 2013 (oder Visual Studio Express 2013 für Web) verwenden. die Benutzeroberfläche unterscheidet sich jedoch.

## <a name="introduction-to-databases"></a>Einführung in Datenbanken

Stellen Sie sich ein typisches Adressbuch vor. Für jeden Eintrag im Adressbuch (d. h. für jede Person) haben Sie mehrere Informationen wie Vorname, Nachname, Adresse, e-Mail-Adresse und Telefonnummer.

Eine typische Möglichkeit zum Bilddaten wie dieser ist die Tabelle mit Zeilen und Spalten. In Daten Bank begriffen wird jede Zeile häufig als Datensatz bezeichnet. Jede Spalte (manchmal auch als "Felder" bezeichnet) enthält einen Wert für jeden Datentyp: Vorname, Nachname usw.

| **ID** | **FirstName** | **Nachname** | **Adresse** | **E-Mail** | **Telefon** |
| --- | --- | --- | --- | --- | --- |
| 1 | Klaus | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234 Main St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

Bei den meisten Datenbanktabellen muss die Tabelle über eine Spalte verfügen, die einen eindeutigen Bezeichner enthält, wie z. b. eine Kundennummer, eine Kontonummer usw. Dies wird als *Primärschlüssel*der Tabelle bezeichnet, und Sie verwenden ihn, um jede Zeile in der Tabelle zu identifizieren. In diesem Beispiel ist die ID-Spalte der Primärschlüssel für das Adressbuch.

Mit diesem grundlegenden Verständnis der Datenbanken können Sie lernen, wie eine einfache Datenbank erstellt und Vorgänge wie das Hinzufügen, ändern und Löschen von Daten durchgeführt werden.

> [!TIP] 
> 
> **Relationale Datenbanken**
> 
> Sie können Daten auf vielfältige Weise speichern, einschließlich Textdateien und Kalkulations Tabellen. In den meisten geschäftlichen Verwendungs Fällen werden die Daten jedoch in einer relationalen Datenbank gespeichert.
> 
> Dieser Artikel geht nicht sehr tief in Datenbanken über. Allerdings ist es möglicherweise hilfreich, etwas über diese Informationen zu verstehen. In einer relationalen Datenbank werden die Informationen logisch in separate Tabellen aufgeteilt. Beispielsweise kann eine Datenbank für eine Schule separate Tabellen für Schüler und Studenten sowie für Klassen Angebote enthalten. Die Datenbanksoftware (z. b. SQL Server) unterstützt leistungsstarke Befehle, mit denen Sie dynamisch Beziehungen zwischen den Tabellen herstellen können. Beispielsweise können Sie die relationale Datenbank verwenden, um eine logische Beziehung zwischen Studenten und Klassen herzustellen, um einen Zeitplan zu erstellen. Das Speichern von Daten in separaten Tabellen reduziert die Komplexität der Tabellenstruktur und verringert die Notwendigkeit, redundante Daten in Tabellen zu speichern.

## <a name="creating-a-database"></a>Erstellen einer Datenbank

In diesem Verfahren wird gezeigt, wie Sie eine Datenbank mit dem Namen "smallbakery" erstellen, indem Sie das in webmatrix enthaltene SQL Server Compact Daten bankentwurfs Tool verwenden. Obwohl Sie eine Datenbank mit Code erstellen können, ist es typischer, die Datenbank-und Datenbanktabellen mit einem Entwurfs Tool wie webmatrix zu erstellen.

1. Starten Sie webmatrix, und klicken Sie auf der Seite Schnellstart auf **Site from Template**.
2. Wählen Sie **leere Website**aus, geben Sie im Feld **Site Name den Namen** "smallbakery" ein, und klicken Sie dann auf **OK**. Die Website wird erstellt und in webmatrix angezeigt.
3. Klicken Sie im linken Bereich auf den Arbeitsbereich **Datenbanken** .
4. Klicken Sie im Menüband auf **neue Datenbank**. Es wird eine leere Datenbank mit dem gleichen Namen wie die Website erstellt.
5. Erweitern Sie im linken Bereich den Knoten **smallbakery. sdf** , und klicken Sie dann auf **Tabellen**.
6. Klicken Sie im Menüband auf **neue Tabelle**. Webmatrix öffnet den Tabellen-Designer.

    ![Klang](5-working-with-data/_static/image1.jpg)
7. Klicken Sie in die Spalte **Name** , und geben Sie &quot;ID-&quot;ein.
8. Wählen Sie in der Spalte **Datentyp** die Option **int**aus.
9. Legen Sie den **is Primary Key fest?** und **ist identifium?** -Optionen auf **Yes**festzulegen.

    Wie der Name bereits vermuten lässt, teilt der **Primärschlüssel** der Datenbank mit, dass es sich hierbei um den Primärschlüssel der Tabelle handelt. **Ist Identity** weist die Datenbank an, automatisch eine ID-Nummer für jeden neuen Datensatz zu erstellen und ihr die nächste sequenzielle Zahl zuzuweisen (beginnend bei 1).
10. Klicken Sie in die nächste Zeile. Der Editor startet eine neue Spaltendefinition.
11. Geben Sie unter Name den Namen &quot;&quot;ein.
12. Wählen Sie unter **Datentyp**die Option &quot;nvarchar&quot; aus, und legen Sie die Länge auf 50 fest. Der *var* -Teil `nvarchar` teilt der Datenbank mit, dass es sich bei den Daten für diese Spalte um eine Zeichenfolge handelt, deren Größe von Datensatz zu Datensatz variieren kann. (Das *n* -Präfix repräsentiert *National*und gibt an, dass das Feldzeichen Daten enthalten kann, die ein &#8212; beliebiges Alphabet oder ein Schreibsystem darstellen, das heißt, dass das Feld Unicode-Daten enthält.)
13. Legen Sie die Option NULL-Wert **zulassen** auf **Nein**fest. Dadurch wird erzwungen, dass die Spalte *Name* nicht leer ist.
14. Erstellen Sie mit diesem Prozess eine Spalte namens *Description*. Legen Sie den **Datentyp** für die Länge auf "nvarchar" und 50 fest, und legen Sie NULL-Wert **zulassen** auf false fest.
15. Erstellen Sie eine Spalte mit dem Namen *Price*. Legen **Sie den Datentyp auf "Money"** fest, und legen Sie **Nullen zulassen** auf false fest.
16. Benennen Sie die Tabelle im oberen Feld &quot;Product&quot;.

    Wenn Sie fertig sind, sieht die Definition wie folgt aus:

    ![Klang](5-working-with-data/_static/image2.png)
17. Drücken Sie STRG + S, um die Tabelle zu speichern.

## <a name="adding-data-to-the-database"></a>Hinzufügen von Daten zur Datenbank

Nun können Sie der Datenbank, mit der Sie später in diesem Artikel arbeiten, einige Beispiel Daten hinzufügen.

1. Erweitern Sie im linken Bereich den Knoten **smallbakery. sdf** , und klicken Sie dann auf **Tabellen**.
2. Klicken Sie mit der rechten Maustaste auf die Tabelle Product und anschließend auf **Daten**.
3. Geben Sie im Bearbeitungsbereich die folgenden Einträge ein:

    | **Name** | **Beschreibung** | **Sonderpreis** |
    | --- | --- | --- |
    | Lai | Jeden Tag frisch gebacken. | 2,99 |
    | Erdbeer Shortcake | Durch die Verwendung von Bio-Erdbeeren aus unserem Garten. | 9.99 |
    | Apple-Kreis | Zweitens nur für Ihren MOM-Kreis. | 12.99 |
    | Pecan-Kreis | Wenn Sie pecane wünschen, ist dies für Sie. | 10.99 |
    | Zitronen Kreis | Mit den besten lemons auf der Welt. | 11.99 |
    | Cupcakes | Ihre Kinder und das Kind in werden Sie gerne begeistern. | 7.99 |

    Denken Sie daran, dass Sie für die *ID* -Spalte nichts eingeben müssen. Wenn Sie die *ID* -Spalte erstellt haben, legen Sie die Eigenschaft **ist Identity** auf true fest. Dies bewirkt, dass Sie automatisch ausgefüllt wird.

    Wenn Sie mit der Eingabe der Daten fertig sind, sieht der Tabellen-Designer wie folgt aus:

    ![Klang](5-working-with-data/_static/image3.jpg)
4. Schließen Sie die Registerkarte, die die Datenbankdaten enthält.

## <a name="displaying-data-from-a-database"></a>Anzeigen von Daten aus einer Datenbank

Wenn Sie über eine Datenbank mit Daten verfügen, können Sie die Daten auf einer ASP.NET-Webseite anzeigen. Zum Auswählen der anzuzeigenden Tabellenzeilen verwenden Sie eine SQL-Anweisung, bei der es sich um einen Befehl handelt, den Sie an die Datenbank übergeben.

1. Klicken Sie im linken Bereich auf den Arbeitsbereich **Dateien** .
2. Erstellen Sie im Stammverzeichnis der Website eine neue cshtml-Seite mit dem Namen *listproducts. cshtml*.
3. Ersetzen Sie das vorhandene Markup durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    Im ersten Codeblock öffnen Sie die Datei " *smallbakery. sdf* " (Datenbank), die Sie zuvor erstellt haben. Bei der `Database.Open`-Methode wird davon ausgegangen, dass sich die *sdf* -Datei in der *App\_Daten* Ordner Ihrer Website befindet. (Beachten Sie, dass Sie die Erweiterung &#8212; " *. sdf* " nicht angeben müssen, wenn Sie dies tun, funktioniert die `Open` Methode nicht.)

    > [!NOTE]
    > Der *App-\_Daten* Ordner ist ein spezieller Ordner in ASP.net, der zum Speichern von Datendateien verwendet wird. Weitere Informationen finden Sie weiter unten in diesem Artikel unter [Herstellen einer Verbindung mit einer Datenbank](#SB_ConnectingToADatabase) .

    Anschließend führen Sie eine Anforderung zum Abfragen der Datenbank mithilfe der folgenden SQL `Select`-Anweisung aus:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    In der-Anweisung identifiziert `Product` die abzufragende Tabelle. Das `*` Zeichen gibt an, dass die Abfrage alle Spalten aus der Tabelle zurückgeben soll. (Sie können Spalten auch einzeln auflisten, die durch Kommas getrennt sind, wenn Sie nur einige der Spalten anzeigen möchten.) Die `Order By`-Klausel gibt an, wie die Daten &#8212; in diesem Fall in der Spalte *Name* sortiert werden sollen. Dies bedeutet, dass die Daten alphabetisch sortiert werden, basierend auf dem Wert der *Name* -Spalte für jede Zeile.

    Im Text der Seite erstellt das Markup eine HTML-Tabelle, die zum Anzeigen der Daten verwendet wird. Innerhalb des `<tbody>`-Elements verwenden Sie eine `foreach`-Schleife, um jede Daten Zeile, die von der Abfrage zurückgegeben wird, einzeln zu erhalten. Für jede Daten Zeile erstellen Sie eine HTML-Tabellenzeile (`<tr>`-Element). Anschließend erstellen Sie HTML-Tabellenzellen (`<td>` Elemente) für jede Spalte. Jedes Mal, wenn Sie die Schleife durchlaufen, befindet sich die nächste verfügbare Zeile aus der Datenbank in der `row`-Variablen (Sie richten Sie in der `foreach`-Anweisung ein). Um eine einzelne Spalte aus der Zeile zu erhalten, können Sie `row.Name` oder `row.Description` oder den Namen der gewünschten Spalte verwenden.
4. Führen Sie die Seite in einem Browser aus. (Stellen Sie sicher, dass die Seite im Arbeitsbereich " **Dateien** " ausgewählt ist, bevor Sie Sie ausführen.) Auf der Seite wird eine Liste wie die folgende angezeigt:

    ![Klang](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Structured Query Language (SQL) (Strukturierte Abfragesprache (SQL))**
> 
> SQL ist eine Sprache, die in den meisten relationalen Datenbanken zum Verwalten von Daten in einer Datenbank verwendet wird. Sie enthält Befehle, mit denen Sie Daten abrufen und aktualisieren und Datenbanktabellen erstellen, ändern und verwalten können. SQL unterscheidet sich von einer Programmiersprache (wie die, die Sie in webmatrix verwenden), denn bei SQL besteht die Idee darin, dass Sie der Datenbank mitteilen, was Sie möchten, und es ist die Aufgabe der Datenbank, herauszufinden, wie Sie die Daten erhalten oder die Aufgabe ausführen. Im folgenden finden Sie Beispiele für einige SQL-Befehle und deren Funktionsweise:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Dadurch werden die Spalten " *ID*", " *Name*" und " *Preis* " aus den Datensätzen in der Tabelle " *Product* " abgerufen, wenn der Wert von " *Price* " größer als 10 ist, und die Ergebnisse in alphabetischer Reihenfolge basierend auf den Werten der *namens* Spalte zurückgegeben Dieser Befehl gibt ein Resultset zurück, das die Datensätze enthält, die die Kriterien erfüllen, oder eine leere Menge, wenn keine Datensätze übereinstimmen.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Dadurch wird ein neuer Datensatz in die Tabelle " *Product* " eingefügt, und die Spalte " *Name* " wird auf &quot;Croissant&quot;, die *Beschreibungs* Spalte, &quot;um eine schlanke Freude&quot;und den Preis auf 1,99 festzulegen.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Mit diesem Befehl werden Datensätze in der Tabelle " *Product* " gelöscht, deren Ablaufdatum-Spalte vor dem 1. Januar 2008 liegt. (Hierbei wird davon ausgegangen, dass die Tabelle " *Product* " natürlich eine solche Spalte enthält.) Das Datum wird hier im Format mm/dd/yyyy eingegeben, sollte jedoch in dem Format eingegeben werden, das für Ihr Gebiets Schema verwendet wird.
> 
> Die Befehle `Insert Into` und `Delete` geben keine Resultsets zurück. Stattdessen geben Sie eine Zahl zurück, die Aufschluss darüber gibt, wie viele Datensätze vom Befehl betroffen sind.
> 
> Für einige dieser Vorgänge (z. b. das Einfügen und Löschen von Datensätzen) muss der Prozess, der den Vorgang anfordert, über die entsprechenden Berechtigungen in der Datenbank verfügen. Aus diesem Grund müssen Sie bei Produktionsdatenbanken häufig einen Benutzernamen und ein Kennwort angeben, wenn Sie eine Verbindung mit der Datenbank herstellen.
> 
> Es gibt Dutzende von SQL-Befehlen, aber alle folgen einem Muster wie diesem. Sie können SQL-Befehle zum Erstellen von Datenbanktabellen, zum zählen der Anzahl von Datensätzen in einer Tabelle, zum Berechnen der Preise und zum Ausführen von vielen weiteren Vorgängen verwenden.

## <a name="inserting-data-in-a-database"></a>Einfügen von Daten in eine Datenbank

In diesem Abschnitt wird gezeigt, wie Sie eine Seite erstellen, mit der Benutzer der *Product* Database-Tabelle ein neues Produkt hinzufügen können. Nachdem ein neuer Produktdaten Satz eingefügt wurde, zeigt die Seite die aktualisierte Tabelle mithilfe der Seite *listproducts. cshtml* an, die Sie im vorherigen Abschnitt erstellt haben.

Die Seite enthält eine Überprüfung, um sicherzustellen, dass die vom Benutzer eingegebenen Daten für die Datenbank gültig sind. Beispielsweise stellt der Code auf der Seite sicher, dass ein Wert für alle erforderlichen Spalten eingegeben wurde.

1. Erstellen Sie auf der Website eine neue cshtml-Datei mit dem Namen *insertproducts. cshtml*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    Der Text der Seite enthält ein HTML-Formular mit drei Textfeldern, in denen Benutzer einen Namen, eine Beschreibung und einen Preis eingeben können. Wenn Benutzer auf die Schaltfläche **Einfügen** klicken, öffnet der Code oben auf der Seite eine Verbindung mit der Datenbank *smallbakery. sdf* . Anschließend erhalten Sie die vom Benutzer übermittelten Werte mithilfe des `Request` Objekts und weisen diese Werte lokalen Variablen zu.

    Um zu überprüfen, ob der Benutzer einen Wert für jede erforderliche Spalte eingegeben hat, registrieren Sie jedes `<input>` Element, das Sie überprüfen möchten:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    Das `Validation`-Hilfsprogramm prüft, ob ein Wert in jedem der Felder vorhanden ist, die Sie registriert haben. Sie können testen, ob alle Felder die Validierung bestanden haben, indem Sie `Validation.IsValid()`überprüfen, was Sie in der Regel tun, bevor Sie die Informationen verarbeiten, die Sie vom Benutzer erhalten haben:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (Der `&&`-Operator bedeutet, und – dieser Test ist, *Wenn es sich um eine Formular Übermittlung handelt und alle Felder die Validierung bestanden haben*.)

    Wenn alle Spalten überprüft wurden (keine waren leer), erstellen Sie eine SQL-Anweisung, um die Daten einzufügen, und führen Sie Sie dann wie folgt aus:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Um die einzufügenden Werte einzuschließen, fügen Sie Parameter Platzhalter (`@0`, `@1``@2`) ein.

    > [!NOTE]
    > Übergeben Sie als Sicherheitsmaßnahme immer Werte mithilfe von Parametern an eine SQL-Anweisung, wie im vorherigen Beispiel zu sehen. Dadurch haben Sie die Möglichkeit, die Daten des Benutzers zu überprüfen, und Sie schützen vor versuchen, bösartige Befehle an Ihre Datenbank zu senden (manchmal auch als Einschleusung von SQL-Befehlen bezeichnet).

    Verwenden Sie diese Anweisung, um die Abfrage auszuführen, und übergeben Sie dabei die Variablen, die die Werte enthalten, die für die Platzhalter ersetzt werden sollen:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Nachdem die `Insert Into`-Anweisung ausgeführt wurde, senden Sie den Benutzer an die Seite, auf der die Produkte aufgeführt werden, die diese Zeile verwenden:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Wenn die Überprüfung nicht erfolgreich war, überspringen Sie die Einfügung. Stattdessen verfügen Sie über ein Hilfsprogramm auf der Seite, auf der die akkumulierten Fehlermeldungen (falls vorhanden) angezeigt werden können:

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Beachten Sie, dass der Stilblock im Markup eine CSS-Klassendefinition mit dem Namen `.validation-summary-errors`enthält. Dies ist der Name der CSS-Klasse, die standardmäßig für das `<div>` Element verwendet wird, das Validierungs Fehler enthält. In diesem Fall gibt die CSS-Klasse an, dass Validierungs Zusammenfassungs Fehler rot und fett angezeigt werden. Sie können jedoch die `.validation-summary-errors` Klasse definieren, um beliebige Formatierungen anzuzeigen.

### <a name="testing-the-insert-page"></a>Testen der Einfügeseite

1. Anzeigen der Seite in einem Browser. Auf der Seite wird ein Formular angezeigt, das dem in der folgenden Abbildung dargestellten Formular ähnelt.

    ![Klang](5-working-with-data/_static/image5.jpg)
2. Geben Sie Werte für alle Spalten ein, stellen Sie jedoch sicher, dass Sie die *Preis* Spalte leer lassen.
3. Klicken Sie auf **Einfügen**. Die Seite zeigt eine Fehlermeldung an, wie in der folgenden Abbildung dargestellt. (Es wird kein neuer Datensatz erstellt.)

    ![Klang](5-working-with-data/_static/image6.jpg)
4. Füllen Sie das Formular vollständig aus, und klicken Sie dann auf **Einfügen**. Dieses Mal wird die Seite *listproducts. cshtml* angezeigt und zeigt den neuen Datensatz.

## <a name="updating-data-in-a-database"></a>Aktualisieren von Daten in einer Datenbank

Nachdem Daten in eine Tabelle eingegeben wurden, müssen Sie Sie möglicherweise aktualisieren. In diesem Verfahren wird gezeigt, wie Sie zwei Seiten erstellen, die denen ähneln, die Sie zuvor für das Einfügen von Daten erstellt haben. Die erste Seite zeigt Produkte an und ermöglicht es Benutzern, eine zu ändernde auszuwählen. Auf der zweiten Seite können die Benutzer die Änderungen vornehmen und speichern.

> [!NOTE] 
> 
> **Wichtig** In einer Produktions Website beschränken Sie in der Regel, wer Änderungen an den Daten vornehmen darf. Informationen zum Einrichten der Mitgliedschaft und zu Methoden zum Autorisieren von Benutzern für die Ausführung von Aufgaben auf dem Standort finden Sie unter [Hinzufügen von Sicherheit und Mitgliedschaft zu einem ASP.net Web Pages-Standort](https://go.microsoft.com/fwlink/?LinkId=202904).

1. Erstellen Sie auf der Website eine neue cshtml-Datei mit dem Namen *editproducts. cshtml*.
2. Ersetzen Sie das vorhandene Markup in der Datei durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    Der einzige Unterschied zwischen dieser Seite und der *listproducts. cshtml* -Seite von früher besteht darin, dass die HTML-Tabelle auf dieser Seite eine zusätzliche Spalte enthält, in der ein **Bearbeitungs** Link angezeigt wird. Wenn Sie auf diesen Link klicken, gelangen Sie zur Seite *updateproducts. cshtml* (die Sie als Nächstes erstellen), auf der Sie den ausgewählten Datensatz bearbeiten können.

    Sehen Sie sich den Code an, der den **Bearbeitungs** Link erstellt:

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Dadurch wird ein HTML-`<a>` Element erstellt, dessen `href`-Attribut dynamisch festgelegt wird. Das `href`-Attribut gibt die Seite an, die angezeigt wird, wenn der Benutzer auf den Link klickt. Außerdem übergibt Sie den `Id` Wert der aktuellen Zeile an den Link. Wenn die Seite ausgeführt wird, enthält die Seitenquelle möglicherweise Links wie diese:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Beachten Sie, dass das `href`-Attribut auf `UpdateProducts/n`festgelegt ist, wobei *n* eine Produktnummer ist. Wenn ein Benutzer auf einen dieser Links klickt, sieht die resultierende URL etwa wie folgt aus:

    `http://localhost:18816/UpdateProducts/6`

    Das heißt, die Produktnummer, die bearbeitet werden soll, wird in der URL übermittelt.
3. Anzeigen der Seite in einem Browser. Auf der Seite werden die Daten in einem Format wie dem folgenden angezeigt:

    ![Klang](5-working-with-data/_static/image7.jpg)

    Als Nächstes erstellen Sie die Seite, mit der Benutzer die Daten tatsächlich aktualisieren können. Die Seite Update enthält eine Überprüfung, um die vom Benutzer eingegebenen Daten zu validieren. Beispielsweise stellt der Code auf der Seite sicher, dass ein Wert für alle erforderlichen Spalten eingegeben wurde.
4. Erstellen Sie auf der Website eine neue cshtml-Datei mit dem Namen *updateproducts. cshtml*.
5. Ersetzen Sie das vorhandene Markup in der Datei durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    Der Text der Seite enthält ein HTML-Formular, in dem ein Produkt angezeigt wird und in dem Benutzer es bearbeiten können. Um das Produkt anzuzeigen, verwenden Sie diese SQL-Anweisung:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Dadurch wird das Produkt ausgewählt, dessen ID mit dem Wert übereinstimmt, der im `@0`-Parameter übergeben wird. (Da die *ID* der Primärschlüssel ist und daher eindeutig sein muss, kann immer nur ein Produktdaten Satz ausgewählt werden.) Um den ID-Wert zu erhalten, der an diese `Select`-Anweisung übergeben werden soll, können Sie den Wert, der an die Seite übergeben wird, als Teil der URL mithilfe der folgenden Syntax lesen:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Um den Produktdaten Satz tatsächlich abzurufen, verwenden Sie die `QuerySingle`-Methode, die nur einen Datensatz zurückgibt:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    Die einzelne Zeile wird an die `row` Variable zurückgegeben. Sie können Daten aus jeder Spalte heraus erhalten und Sie wie folgt lokalen Variablen zuweisen:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    Im Markup für das Formular werden diese Werte automatisch in einzelnen Textfeldern angezeigt, indem eingebetteter Code wie der folgende verwendet wird:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Dieser Teil des Codes zeigt den zu aktualisierenden Produktdaten Satz an. Nachdem der Datensatz angezeigt wurde, kann der Benutzer einzelne Spalten bearbeiten.

    Wenn der Benutzer das Formular durch Klicken auf die Schaltfläche **Aktualisieren** sendet, wird der Code im `if(IsPost)`-Block ausgeführt. Dadurch werden die Werte des Benutzers aus dem `Request` Objekt abgerufen, die Werte werden in Variablen gespeichert, und es wird überprüft, ob die einzelnen Spalten ausgefüllt wurden. Wenn die Überprüfung bestanden wird, erstellt der Code die folgende SQL-Update-Anweisung:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    In einer SQL `Update`-Anweisung geben Sie jede zu Aktualisier gende Spalte und den Wert an, auf den Sie festgelegt werden soll. In diesem Code werden die Werte mithilfe der Parameter Platzhalter `@0`, `@1`, `@2`usw. angegeben. (Wie bereits erwähnt, sollten Sie aus Sicherheitsgründen immer Werte mithilfe von Parametern an eine SQL-Anweisung übergeben.)

    Wenn Sie die `db.Execute`-Methode aufzurufen, übergeben Sie die Variablen, die die Werte enthalten, in der Reihenfolge, die den Parametern in der SQL-Anweisung entspricht:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Nachdem die `Update`-Anweisung ausgeführt wurde, wird die folgende Methode aufgerufen, um den Benutzer zurück zur Bearbeitungsseite zu leiten:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    Dies hat den Effekt, dass dem Benutzer eine aktualisierte Auflistung der Daten in der Datenbank angezeigt wird und ein anderes Produkt bearbeitet werden kann.
6. Speichern Sie die Seite.
7. Führen Sie die Seite *editproducts. cshtml* (nicht die Seite aktualisieren) aus, und klicken Sie dann auf **Bearbeiten** , um das zu bearbeitende Produkt auszuwählen. Die Seite *updateproducts. cshtml* wird angezeigt und zeigt den Datensatz an, den Sie ausgewählt haben.

    ![Klang](5-working-with-data/_static/image8.jpg)
8. Nehmen Sie eine Änderung vor, und klicken Sie auf **Aktualisieren**. Die Produktliste wird erneut mit den aktualisierten Daten angezeigt.

## <a name="deleting-data-in-a-database"></a>Löschen von Daten in einer Datenbank

In diesem Abschnitt wird gezeigt, wie Sie Benutzern das Löschen eines Produkts aus der Tabelle " *Product* Database" gestatten. Das Beispiel besteht aus zwei Seiten. Auf der ersten Seite wählen Benutzer einen Datensatz aus, der gelöscht werden soll. Der zu löschende Datensatz wird dann auf einer zweiten Seite angezeigt, auf der Sie bestätigen können, dass Sie den Datensatz löschen möchten.

> [!NOTE] 
> 
> **Wichtig** In einer Produktions Website beschränken Sie in der Regel, wer Änderungen an den Daten vornehmen darf. Weitere Informationen zum Einrichten der Mitgliedschaft und zu den Möglichkeiten, den Benutzer zum Ausführen von Aufgaben auf dem Standort zu autorisieren, finden Sie unter [Hinzufügen von Sicherheit und Mitgliedschaft zu einem ASP.net Web Pages-Standort](https://go.microsoft.com/fwlink/?LinkId=202904).

1. Erstellen Sie auf der Website eine neue cshtml-Datei mit dem Namen *listproductfordelete. cshtml*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Diese Seite ähnelt der Seite *editproducts. cshtml* von oben. Anstatt jedoch für jedes Produkt einen **Bearbeitungs** Link anzuzeigen, wird ein Link zum **Löschen** angezeigt. Der Link **Löschen** wird mithilfe des folgenden eingebetteten Codes im Markup erstellt:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Dadurch wird eine URL erstellt, die wie folgt aussieht, wenn Benutzer auf den Link klicken:

    `http://<server>/DeleteProduct/4`

    Die URL Ruft eine Seite mit dem Namen *deleteProduct. cshtml* auf (die Sie als Nächstes erstellen) und übergibt sie an die ID des zu löschenden Produkts (hier 4).
3. Speichern Sie die Datei, lassen Sie Sie jedoch geöffnet.
4. Erstellen Sie eine weitere cHTML-Datei mit dem Namen *deleteProduct. cshtml*. Ersetzen Sie den vorhandenen Inhalt durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Diese Seite wird von *listproductfordelete. cshtml* aufgerufen und ermöglicht Benutzern die Bestätigung, dass Sie ein Produkt löschen möchten. Um das zu löschende Produkt aufzulisten, erhalten Sie die ID des Produkts, das aus der URL gelöscht werden soll, indem Sie den folgenden Code verwenden:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    Die Seite fordert den Benutzer auf, auf eine Schaltfläche zu klicken, um den Datensatz tatsächlich zu löschen. Dies ist eine wichtige Sicherheitsmaßnahme: Wenn Sie auf Ihrer Website vertrauliche Vorgänge ausführen, wie z. b. das Aktualisieren oder Löschen von Daten, sollten diese Vorgänge immer mit einem Post-Vorgang erfolgen, nicht mit einem Get-Vorgang. Wenn Ihre Website so eingerichtet ist, dass ein Löschvorgang mit einem Get-Vorgang ausgeführt werden kann, kann jeder Benutzer eine URL wie `http://<server>/DeleteProduct/4` übergeben und alle gewünschten Elemente aus der Datenbank löschen. Indem Sie die Bestätigung hinzufügen und die Seite so codieren, dass der Löschvorgang nur mithilfe eines Beitrags durchgeführt werden kann, fügen Sie Ihrer Website ein Maß an Sicherheit hinzu.

    Der tatsächliche Löschvorgang wird mithilfe des folgenden Codes durchgeführt, der zuerst bestätigt, dass es sich um einen Post-Vorgang handelt und die ID nicht leer ist:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    Der Code führt eine SQL-Anweisung aus, mit der der angegebene Datensatz gelöscht wird, und leitet den Benutzer dann zurück zur Auflistungs Seite.
5. Führen Sie *listproductfordelete. cshtml* in einem Browser aus.

    ![Klang](5-working-with-data/_static/image9.jpg)
6. Klicken Sie auf den Link **Löschen** für eines der Produkte. Die Seite *deleteProduct. cshtml* wird angezeigt, um zu bestätigen, dass Sie diesen Datensatz löschen möchten.
7. Klicken Sie auf die Schaltfläche **Löschen** . Der Produktdaten Satz wurde gelöscht, und die Seite wird mit einer aktualisierten Produktliste aktualisiert.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Herstellen einer Verbindung mit einer Datenbank
> 
> Sie können auf zwei Arten eine Verbindung mit einer Datenbank herstellen. Der erste besteht darin, die `Database.Open`-Methode zu verwenden und den Namen der Datenbankdatei anzugeben (abzüglich der Erweiterung *. sdf* ):
> 
> `var db = Database.Open("SmallBakery");`
> 
> Die `Open`-Methode geht davon aus, dass die. die *sdf* -Datei befindet sich im *App-\_Daten* Ordner der Website. Dieser Ordner wurde speziell für die Speicherung von Daten entwickelt. Sie verfügt z. b. über die entsprechenden Berechtigungen, damit die Website Daten lesen und schreiben kann. als Sicherheitsmaßnahme gestattet webmatrix keinen Zugriff auf Dateien aus diesem Ordner.
> 
> Die zweite Möglichkeit besteht darin, eine Verbindungs Zeichenfolge zu verwenden. Eine Verbindungs Zeichenfolge enthält Informationen zum Herstellen einer Verbindung mit einer Datenbank. Dies kann einen Dateipfad enthalten, oder er kann den Namen einer SQL Server Datenbank auf einem lokalen Server oder Remote Server enthalten, zusammen mit einem Benutzernamen und einem Kennwort, um eine Verbindung mit dem Server herzustellen. (Wenn Sie Daten in einer zentral verwalteten Version von SQL Server aufbewahren, z. b. auf der Website eines hostinganbieters, verwenden Sie immer eine Verbindungs Zeichenfolge, um die Daten bankverbindungs Informationen anzugeben.)
> 
> In webmatrix werden Verbindungs Zeichenfolgen in der Regel in einer XML-Datei mit dem Namen *Web. config*gespeichert. Wie der Name schon sagt, können Sie eine *Web. config* -Datei im Stammverzeichnis Ihrer Website verwenden, um die Konfigurationsinformationen für die Website zu speichern, einschließlich aller Verbindungs Zeichenfolgen, die für Ihre Website erforderlich sind. Ein Beispiel für eine Verbindungs Zeichenfolge in einer *Web. config* -Datei könnte wie folgt aussehen:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> In dem Beispiel verweist die Verbindungs Zeichenfolge auf eine Datenbank in einer Instanz von SQL Server die auf einem Server irgendwo ausgeführt wird (im Gegensatz zu einer lokalen *sdf* -Datei). Sie müssen die entsprechenden Namen für `myServer` und `myDatabase`ersetzen und SQL Server Anmelde Werte für `username` und `password`angeben. (Die Werte für Benutzername und Kennwort sind nicht notwendigerweise identisch mit Ihren Windows-Anmelde Informationen oder den Werten, die Ihr Hostinganbieter Ihnen zur Anmeldung bei ihren Servern erteilt hat. Wenden Sie sich an den Administrator, um genau die benötigten Werte zu ermitteln.)
> 
> Die `Database.Open`-Methode ist flexibel, da Sie entweder den Namen einer Datenbank *. sdf* -Datei oder den Namen einer Verbindungs Zeichenfolge übergeben können, die in der Datei *Web. config* gespeichert ist. Im folgenden Beispiel wird gezeigt, wie mit der Verbindungs Zeichenfolge, die im vorherigen Beispiel veranschaulicht wurde, eine Verbindung mit der Datenbank hergestellt wird
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Wie bereits erwähnt, können Sie mit der `Database.Open`-Methode entweder einen Datenbanknamen oder eine Verbindungs Zeichenfolge übergeben und ermitteln, welche verwendet werden soll. Dies ist sehr nützlich, wenn Sie Ihre Website bereitstellen (veröffentlichen). Wenn Sie Ihre Website entwickeln und testen, können Sie eine *sdf* -Datei im *\_Daten* Ordner der App verwenden. Wenn Sie Ihre Website dann auf einen Produktionsserver verschieben, können Sie eine Verbindungs Zeichenfolge in der Datei " *Web. config* " verwenden, die den gleichen Namen wie die *sdf* -Datei hat, aber auf die &#8212; Datenbank des hostinganbieters verweist, ohne den Code ändern zu müssen.
> 
> Wenn Sie zum Schluss direkt mit einer Verbindungs Zeichenfolge arbeiten möchten, können Sie die `Database.OpenConnectionString`-Methode und die tatsächliche Verbindungs Zeichenfolge anstelle des Namens eines in der Datei " *Web. config* " übergeben. Dies kann in Situationen nützlich sein, in denen Sie aus irgendeinem Grund keinen Zugriff auf die Verbindungs Zeichenfolge haben (oder Werte darin, wie z *. b. den. sdf* -Dateinamen), bis die Seite ausgeführt wird. In den meisten Szenarien können Sie jedoch `Database.Open` verwenden, wie in diesem Artikel beschrieben.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Herstellen einer Verbindung mit einer SQL Server-oder MySQL-Datenbank in webmatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Überprüfen der Benutzereingabe in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=253002)
