---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: Einführung in ASP.net Web Pages die Eingabe von Datenbankdaten mithilfe von Formularen | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial wird gezeigt, wie Sie ein Eingabeformular erstellen und dann die Daten, die Sie aus dem Formular erhalten, in eine Datenbanktabelle eingeben, wenn Sie ASP.net Web Pages (...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: b9354a7b97a7df9020a681f709e16a92650cfcf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78506601"
---
# <a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Einführung in ASP.net Web Pages Datenbankdaten mithilfe von Formularen

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Tutorial wird gezeigt, wie Sie ein Eingabeformular erstellen und dann die Daten, die Sie aus dem Formular erhalten, in eine Datenbanktabelle eingeben, wenn Sie ASP.net Web Pages (Razor) verwenden. Dabei wird davon ausgegangen, dass Sie die Reihe durch [Grundlagen von HTML-Formularen in ASP.net Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581)abgeschlossen haben.
> 
> Sie lernen Folgendes:
> 
> - Weitere Informationen zum Verarbeiten von Eingabe Formularen.
> - Vorgehensweise beim Hinzufügen (einfügen) von Daten in einer Datenbank.
> - Stellen Sie sicher, dass Benutzer einen erforderlichen Wert in einem Formular eingegeben haben (Überprüfen von Benutzereingaben).
> - Vorgehensweise beim Anzeigen von Validierungs Fehlern
> - Springen zu einer anderen Seite von der aktuellen Seite.
>   
> 
> Erörterte Features und Technologien:
> 
> - Die `Database.Execute` -Methode.
> - Die SQL `Insert Into`-Anweisung
> - Das `Validation`-Hilfsprogramm.
> - Die `Response.Redirect` -Methode.

## <a name="what-youll-build"></a>Sie lernen Folgendes

In dem zuvor gezeigten Tutorial zum Erstellen einer Datenbank haben Sie Datenbankdaten eingegeben, indem Sie die Datenbank direkt in webmatrix bearbeitet haben und im Arbeitsbereich **Datenbank** arbeiten. In den meisten apps ist dies keine praktische Möglichkeit, um Daten in die Datenbank einzufügen. In diesem Tutorial erstellen Sie also eine webbasierte Oberfläche, mit der Sie oder alle Benutzerdaten eingeben und in der Datenbank speichern können.

Sie erstellen eine Seite, auf der Sie neue Filme eingeben können. Die Seite enthält ein Eingabeformular mit Feldern (Textfeldern), in denen Sie einen Filmtitel, ein Genre und ein Jahr eingeben können. Die Seite sieht wie folgt aus:

![Seite "Film hinzufügen" im Browser](entering-data/_static/image1.png)

Die Textfelder sind HTML-`<input>` Elemente, die wie dieses Markup aussehen:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Erstellen des grundlegenden Eingabeformulars

Erstellen Sie eine Seite mit dem Namen *addmovie. cshtml*.

Ersetzen Sie die Elemente in der Datei durch das folgende Markup. Alles überschreiben Sie fügen oben einen Codeblock hinzu.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Dieses Beispiel zeigt typische HTML-Code zum Erstellen eines Formulars. Er verwendet `<input>` Elemente für die Textfelder und für die Schaltfläche "Senden". Die Beschriftungen für die Textfelder werden mit Standard `<label>` Elementen erstellt. Die `<fieldset>`-und `<legend>`-Elemente stellen ein schönes Feld um das Formular.

Beachten Sie, dass das `<form>`-Element auf dieser Seite `post` als Wert für das `method`-Attribut verwendet. Im vorherigen Tutorial haben Sie ein Formular erstellt, das die `get`-Methode verwendet hat. Das war richtig, denn obwohl das Formular Werte an den Server übermittelt hat, hat die Anforderung keine Änderungen vorgenommen. Alles konnte nicht auf unterschiedliche Weise abgerufen werden. Auf dieser Seite nehmen Sie jedoch Änderungen vor – *Sie werden neue* Datenbankdaten Sätze hinzufügen. Daher sollte in diesem Formular die `post`-Methode verwendet werden. (Weitere Informationen zu den Unterschieden zwischen `GET` und `POST` Vorgängen finden Sie in der Rand Leiste "[Get", "Post" und "http Verb Safety](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) " im vorherigen Tutorial.)

Beachten Sie, dass jedes Textfeld ein `name`-Element enthält (`title`, `genre`, `year`). Wie Sie im vorherigen Tutorial gesehen haben, sind diese Namen wichtig, da Sie diese Namen haben müssen, damit Sie die Eingabe des Benutzers später erhalten können. Sie können beliebige Namen verwenden. Es ist hilfreich, aussagekräftige Namen zu verwenden, die Ihnen helfen zu merken, mit welchen Daten Sie arbeiten.

Das `value`-Attribut jedes `<input>` Elements enthält ein wenig Razor-Code (z. b. `Request.Form["title"]`). Sie haben im vorherigen Tutorial eine Version dieses Tricks kennengelernt, um den im Textfeld (sofern vorhanden) eingegebenen Wert beizubehalten, nachdem das Formular übermittelt wurde.

## <a name="getting-the-form-values"></a>Die Formular Werte werden erhalten.

Als Nächstes fügen Sie Code hinzu, der das Formular verarbeitet. Im Umriss führen Sie die folgenden Schritte aus:

1. Überprüfen Sie, ob die Seite gesendet wird (wurde übermittelt). Sie möchten, dass Ihr Code nur ausgeführt wird, wenn Benutzer auf die Schaltfläche geklickt haben, nicht, wenn die Seite zum ersten Mal ausgeführt wird.
2. Gibt die Werte an, die der Benutzer in die Textfelder eingegeben hat. Da in diesem Fall das Formular das `POST` Verb verwendet, werden die Formular Werte aus der `Request.Form` Auflistung angezeigt.
3. Fügen Sie die Werte als neuen Datensatz in die Datenbanktabelle " *Movies* " ein.

Fügen Sie am Anfang der Datei den folgenden Code hinzu:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

In den ersten Zeilen werden Variablen (`title`, `genre`und `year`) erstellt, um die Werte aus den Textfeldern zu speichern. Die Zeile `if(IsPost)` stellt sicher, dass die Variablen *nur* festgelegt werden, wenn Benutzer auf die Schaltfläche **Film hinzufügen** klicken, d. –., wenn das Formular gepostet wurde.

Wie Sie in einem früheren Tutorial gesehen haben, können Sie den Wert eines Textfelds mit einem Ausdruck wie `Request.Form["name"]`erhalten, wobei *Name* der Name des `<input>` Elements ist.

Der Name der Variablen (`title`, `genre`und `year`) ist willkürlich. Wie die Namen, die Sie `<input>` Elementen zuweisen, können Sie Sie beliebig benennen. (Die Namen der Variablen müssen nicht den namens Attributen `<input>` Elemente im Formular entsprechen.) Wie bei den `<input>` Elementen empfiehlt es sich jedoch, Variablennamen zu verwenden, die die darin enthaltenen Daten widerspiegeln. Wenn Sie Code schreiben, können Sie mit konsistenten Namen leichter merken, mit welchen Daten Sie arbeiten.

## <a name="adding-data-to-the-database"></a>Hinzufügen von Daten zur Datenbank

Fügen Sie im soeben hinzugefügten Codeblock direkt *innerhalb* der schließenden geschweiften Klammer (`}`) des `if`-Blocks (nicht nur innerhalb des Code Blocks) den folgenden Code hinzu:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

Dieses Beispiel ähnelt dem Code, den Sie in einem vorherigen Tutorial zum Abrufen und Anzeigen von Daten verwendet haben. Die Zeile, die mit `db =` beginnt, öffnet die Datenbank, wie zuvor, und die nächste Zeile definiert eine SQL-Anweisung, wie Sie zuvor gesehen haben. Diesmal wird jedoch eine SQL `Insert Into`-Anweisung definiert. Das folgende Beispiel zeigt die allgemeine Syntax der `Insert Into`-Anweisung:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

Dies bedeutet, dass Sie die Tabelle angeben, in die eingefügt werden soll. Anschließend können Sie die einzufügenden Spalten auflisten und dann die einzufügenden Werte auflisten. (Wie bereits erwähnt, wird bei SQL die Groß-/Kleinschreibung nicht beachtet, aber einige Personen haben die Schlüsselwörter groß geschrieben, um das Lesen des Befehls zu vereinfachen.)

Die Spalten, in die Sie eingefügt werden, sind bereits im Befehl – `(Title, Genre, Year)`aufgeführt. Der interessante Teil ist, wie Sie die Werte aus den Textfeldern in den `VALUES` Teil des Befehls erhalten. Anstelle von tatsächlichen Werten sehen Sie `@0`, `@1`und `@2`. Dies sind natürlich Platzhalter. Wenn Sie den Befehl ausführen (in der `db.Execute` Zeile), übergeben Sie die Werte, die Sie in den Textfeldern erhalten haben.

**Wichtig!** Beachten Sie, dass die einzige Möglichkeit zum Einschließen von Daten, die von einem Benutzer in einer SQL-Anweisung Online eingegeben werden, die Verwendung von Platzhaltern ist, wie hier zu sehen ist (`VALUES(@0, @1, @2)`). Wenn Sie Benutzereingaben in einer SQL-Anweisung verketten, öffnen Sie sich bei einem SQL-Injection-Angriff, wie in den [Grundlagen der Grundlagen in ASP.net Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581) (dem vorherigen Tutorial) erläutert.

Fügen Sie im `if`-Block nach der `db.Execute` Zeile die folgende Zeile ein:

[!code-css[Main](entering-data/samples/sample4.css)]

Nachdem der neue Film in die Datenbank eingefügt wurde, springt Ihnen diese Zeile (Umleitungen) zur Seite " *Movies* " (Umleitungen), damit Sie den soeben eingegebenen Film sehen können. Der `~`-Operator bedeutet "Root of the Website". (Der `~`-Operator funktioniert nur in ASP.NET Seiten, im Allgemeinen nicht in HTML.)

Der gesamte Codeblock sieht wie im folgenden Beispiel aus:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Testen des Einfügebefehls (bisher)

Sie sind noch nicht fertig, aber jetzt ist es ein guter Zeitpunkt, zu testen.

Klicken Sie in der Strukturansicht der Dateien in webmatrix mit der rechten Maustaste auf die Seite *addmovie. cshtml* , und klicken Sie dann auf **im Browser starten**.

![Seite "Film hinzufügen" im Browser](entering-data/_static/image2.png)

(Wenn im Browser eine andere Seite angezeigt wird, stellen Sie sicher, dass die URL `http://localhost:nnnnn/AddMovie`) ist, wobei *NNNNN* die von Ihnen verwendete Portnummer ist.)

Haben Sie eine Fehlerseite erhalten? Wenn dies der Fall ist, lesen Sie ihn sorgfältig durch, und stellen Sie sicher, dass der Code genau so aussieht wie zuvor.

Geben Sie im Formular einen Film ein, &mdash; z. b. "Bürger-Kane", "Drama" und "1941" zu verwenden. (Oder was auch immer.) Klicken Sie dann auf **Film hinzufügen**.

Wenn alles gut funktioniert, werden Sie zur Seite " *Filme* " umgeleitet. Stellen Sie sicher, dass Ihr neuer Film aufgeführt ist.

![Filme Seite mit neu hinzugefügten Film](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Überprüfen der Benutzereingabe

Wechseln Sie zurück zur Seite " *addmovie* ", oder führen Sie Sie erneut aus. Geben Sie einen anderen Film ein, aber geben Sie diesmal nur den Titel ein, &mdash; z. b. "singini" in "Regen" eingeben. Klicken Sie dann auf **Film hinzufügen**.

Sie werden erneut zur Seite " *Filme* " umgeleitet. Sie finden den neuen Film, aber er ist unvollständig.

![Seite "Filme" mit neuem Film, für den einige Werte fehlen](entering-data/_static/image4.png)

Beim Erstellen der *Filme* -Tabelle haben Sie explizit gesagt, dass keines der Felder NULL sein kann. Hier haben Sie ein Eingabeformular für neue Filme, und Sie lassen Felder leer. Das ist ein Fehler.

In diesem Fall hat die Datenbank keinen Fehler ausgelöst *(oder ausgelöst*). Sie haben kein Genre oder Jahr angegeben, daher hat der Code auf der *addmovie* -Seite diese Werte als so genannte *leere*Zeichen folgen behandelt. Wenn der SQL `Insert Into`-Befehl ausgeführt wurde, hatten die Felder Genre und Year keine nützlichen Daten, aber Sie waren nicht NULL.

Natürlich sollten Sie nicht zulassen, dass Benutzer halb leere Filminformationen in die Datenbank eingeben. Die Lösung besteht darin, die Eingabe des Benutzers zu überprüfen. Anfänglich wird bei der Überprüfung einfach sichergestellt, dass der Benutzer einen Wert für alle Felder eingegeben hat (d. h., dass keiner der Felder eine leere Zeichenfolge enthält).

> [!TIP]
> 
> **NULL und leere Zeichen folgen**
> 
> Beim Programmieren gibt es einen Unterschied zwischen den verschiedenen Begriffen "kein Wert". Im Allgemeinen ist ein Wert *null* , wenn er nie auf irgendeine Weise festgelegt oder initialisiert wurde. Im Gegensatz dazu kann eine Variable, die Zeichendaten (Zeichen folgen) erwartet, auf eine *leere Zeichenfolge*festgelegt werden. In diesem Fall ist der Wert nicht NULL. Sie wurde nur explizit auf eine Zeichenfolge festgelegt, deren Länge 0 (null) ist. Diese beiden Anweisungen zeigen den Unterschied:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> Es ist etwas komplizierter als das, aber der wichtigste Punkt ist, dass `null` eine Art nicht ermittelte Zustands darstellt.
> 
> Nun ist es wichtig, genau zu verstehen, wenn ein Wert NULL ist und wenn es sich nur um eine leere Zeichenfolge handelt. Im Code für die *addmovie* -Seite erhalten Sie die Werte der Textfelder, indem Sie `Request.Form["title"]` usw. verwenden. Wenn die Seite zum ersten Mal ausgeführt wird (bevor Sie auf die Schaltfläche klicken), ist der Wert von `Request.Form["title"]` NULL. Wenn Sie jedoch das Formular senden, wird `Request.Form["title"]` den Wert des `title` Textfelds abrufen. Es ist nicht offensichtlich, aber ein leeres Textfeld ist nicht NULL. darin ist nur eine leere Zeichenfolge angegeben. Wenn der Code als Reaktion auf den Klick auf die Schaltfläche ausgeführt wird, enthält `Request.Form["title"]` eine leere Zeichenfolge.
> 
> Warum ist dieser Unterschied wichtig? Beim Erstellen der *Filme* -Tabelle haben Sie explizit gesagt, dass keines der Felder NULL sein kann. Hier haben Sie jedoch ein Eingabeformular für neue Filme, und Sie lassen Felder leer. Wenn Sie versuchen, neue Filme zu speichern, bei denen keine Werte für Genre oder Jahr vorhanden waren, sollten Sie davon ausgehen, dass sich die Datenbank beschwert. Aber das ist der Punkt &mdash; auch wenn Sie diese Textfelder leer lassen, sind die Werte nicht NULL. Sie sind leere Zeichen folgen. Daher können Sie neue Filme in der Datenbank speichern, wenn diese Spalten leer &mdash;, aber nicht NULL sind! &mdash;-Werte sind. Daher müssen Sie sicherstellen, dass Benutzer keine leere Zeichenfolge einreichen, was Sie tun können, indem Sie die Eingabe des Benutzers validieren.

### <a name="the-validation-helper"></a>Das Validierungs Hilfsprogramm

ASP.net Web Pages enthält eine Hilfsobjekt &mdash; den `Validation` Helper-&mdash;, mit dem Sie sicherstellen können, dass Benutzerdaten eingeben, die Ihren Anforderungen entsprechen. Das `Validation`-Hilfsprogramm ist eine der in ASP.net Web Pages integrierten Hilfsprogramme, sodass Sie es nicht als Paket mithilfe von nuget installieren müssen, wie Sie das Gravatar-Hilfsprogramm in einem früheren Tutorial installiert haben.

Um die Eingabe des Benutzers zu überprüfen, gehen Sie wie folgt vor:

- Verwenden Sie Code, um anzugeben, dass Sie Werte in den Textfeldern auf der Seite benötigen.
- Fügen Sie einen Test in den Code ein, sodass die Filminformationen nur dann zur Datenbank hinzugefügt werden, wenn alles ordnungsgemäß überprüft wird.
- Fügen Sie Code in das Markup ein, um Fehlermeldungen anzuzeigen.

Fügen Sie im Codeblock auf der *addmovie* -Seite am oberen Rand vor den Variablen Deklarationen den folgenden Code hinzu:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Sie können `Validation.RequireField` einmal für jedes Feld (`<input>` Element) abrufen, in dem Sie einen Eintrag benötigen. Sie können auch eine benutzerdefinierte Fehlermeldung für jeden-Befehl hinzufügen, wie hier zu sehen. (Wir haben die Nachrichten unterschiedlich geändert, um anzuzeigen, dass Sie dort beliebige Inhalte ablegen können.)

Wenn ein Problem vorliegt, sollten Sie verhindern, dass die neuen Filminformationen in die Datenbank eingefügt werden. Verwenden Sie im `if(IsPost)`-Block `&&` (logisches and), um eine weitere Bedingung hinzuzufügen, mit der `Validation.IsValid()`getestet wird. Wenn Sie fertig sind, sieht der gesamte `if(IsPost)`-Block wie der folgende Code aus:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Wenn ein Überprüfungs Fehler mit einem der Felder vorliegt, die Sie mithilfe des `Validation`-Hilfsprogramms registriert haben, gibt die `Validation.IsValid`-Methode false zurück. Und in diesem Fall wird kein Code in diesem Block ausgeführt, sodass keine ungültigen Film Einträge in die Datenbank eingefügt werden. Und natürlich werden Sie nicht zur Seite " *Filme* " umgeleitet.

Der gesamte Codeblock, einschließlich des Validierungs Codes, sieht nun wie im folgenden Beispiel aus:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Anzeigen von Validierungs Fehlern

Der letzte Schritt besteht darin, Fehlermeldungen anzuzeigen. Sie können einzelne Meldungen für jeden Validierungs Fehler anzeigen, oder Sie können eine Zusammenfassung oder beides anzeigen. In diesem Tutorial führen Sie beides aus, damit Sie sehen können, wie es funktioniert.

Nennen Sie neben jedem `<input>` Element, das Sie überprüfen, die `Html.ValidationMessage`-Methode, und übergeben Sie den Namen des `<input>` Elements, das Sie überprüfen. Fügen Sie die `Html.ValidationMessage`-Methode an die Stelle, an der die Fehlermeldung angezeigt werden soll. Wenn die Seite ausgeführt wird, rendert die `Html.ValidationMessage`-Methode ein `<span>` Element, bei dem der Überprüfungs Fehler auftritt. (Wenn kein Fehler vorliegt, wird das `<span>` Element gerendert, aber es ist kein Text vorhanden.)

Ändern Sie das Markup auf der Seite so, dass es eine `Html.ValidationMessage`-Methode für jedes der drei `<input>` Elemente auf der Seite enthält, wie in diesem Beispiel:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Fügen Sie das folgende Markup und den Code direkt hinter dem `<h1>Add a Movie</h1>`-Element auf der Seite ein, um zu sehen, wie die Zusammenfassung funktioniert:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Standardmäßig zeigt die `Html.ValidationSummary`-Methode alle Validierungs Nachrichten in einer Liste an (ein `<ul>` Element, das sich innerhalb eines `<div>`-Elements befindet). Wie bei der `Html.ValidationMessage`-Methode wird das Markup für die Validierungs Zusammenfassung immer gerendert. Wenn keine Fehler vorliegen, werden keine Listenelemente gerendert.

Die Zusammenfassung kann eine alternative Möglichkeit zum Anzeigen von Validierungs Nachrichten anstelle von verwendet werden, um `Html.ValidationMessage` die einzelnen feldspezifischen Fehler anzuzeigen. Sie können auch eine Zusammenfassung und die Details verwenden. Oder Sie können die `Html.ValidationSummary`-Methode verwenden, um einen generischen Fehler anzuzeigen und dann einzelne `Html.ValidationMessage` Aufrufe zum Anzeigen von Details zu verwenden.

Die Seite "Fertig" sieht nun wie im folgenden Beispiel aus:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

Das ist alles. Sie können die Seite jetzt testen, indem Sie einen Film hinzufügen, aber ein oder mehrere Felder verlassen. Wenn Sie dies tun, wird die folgende Fehleranzeige angezeigt:

![Hinzufügen einer Filmseite mit Validierungs Fehlermeldungen](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Formatieren der Validierungs Fehlermeldungen

Sie sehen, dass Fehlermeldungen vorliegen, aber Sie sind nicht wirklich gut hervorragend. Es gibt jedoch eine einfache Möglichkeit, die Fehlermeldungen zu formatieren.

Um die einzelnen Fehlermeldungen zu formatieren, die von `Html.ValidationMessage`angezeigt werden, erstellen Sie eine CSS-Formatklasse mit dem Namen `field-validation-error`. Um das Suchen nach der Validierungs Zusammenfassung zu definieren, erstellen Sie eine CSS-Formatklasse mit dem Namen `validation-summary-errors`.

Um zu sehen, wie diese Technik funktioniert, fügen Sie im `<head>` Abschnitt der Seite ein `<style>` Element hinzu. Definieren Sie dann Stil Klassen mit dem Namen `field-validation-error` und `validation-summary-errors`, die die folgenden Regeln enthalten:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Normalerweise würden Sie Formatierungsinformationen in eine separate *CSS* -Datei einfügen, aber aus Gründen der Einfachheit können Sie Sie jetzt auf der Seite ablegen. (Später in diesem Tutorial verschieben Sie die CSS-Regeln in eine separate *CSS* -Datei.)

Wenn ein Validierungs Fehler vorliegt, rendert die `Html.ValidationMessage`-Methode ein `<span>` Element, das `class="field-validation-error"`enthält. Indem Sie eine Format Definition für diese Klasse hinzufügen, können Sie konfigurieren, wie die Nachricht aussieht. Wenn Fehler auftreten, rendert die `ValidationSummary`-Methode das Attribut ebenfalls dynamisch `class="validation-summary-errors"`.

Führen Sie die Seite erneut aus, und lassen Sie absichtlich einige Felder aus. Die Fehler sind nun auffälliger. (Tatsächlich sind Sie überladen, aber das ist nur, um zu zeigen, was Sie tun können.)

![Hinzufügen einer Filmseite mit Validierungs Fehlern, die formatiert wurden](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Hinzufügen eines Links zur Seite "Filme"

Der letzte Schritt besteht darin, den Einstieg in die Seite " *addmovie* " aus der ursprünglichen Filmliste zu vereinfachen.

Öffnen Sie die Seite *Filme* erneut. Fügen Sie nach dem schließenden `</div>` Tag, das auf das `WebGrid`-Hilfsprogramm folgt, das folgende Markup hinzu:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Wie Sie bereits gesehen haben, interpretiert ASP.NET den `~` Operator als das Stammverzeichnis der Website. Sie müssen nicht den `~`-Operator verwenden. Sie können das Markup `<a href="./AddMovie">Add a movie</a>` oder eine andere Methode verwenden, um den Pfad zu definieren, den HTML versteht. Der `~` Operator ist jedoch ein guter allgemeiner Ansatz, wenn Sie Verknüpfungen für Razor Pages erstellen, da die Website flexibler ist – wenn Sie die aktuelle Seite in einen Unterordner verschieben, wird der Link weiterhin zur *addmovie* -Seite weitergeleitet. (Beachten Sie, dass der `~`-Operator nur auf *. cshtml* -Seiten funktioniert. ASP.net versteht sie, aber es handelt sich nicht um Standard-HTML.)

Wenn Sie fertig sind, führen Sie die Seite *Filme* aus. Diese Seite sieht wie folgt aus:

![Seite "Filme" mit Link zur Seite "Filme hinzufügen"](entering-data/_static/image7.png)

Klicken Sie auf den Link **Film hinzufügen** , um sicherzustellen, dass er auf die Seite *addmovie* wechselt.

## <a name="coming-up-next"></a>Nächste nächste

Im nächsten Tutorial erfahren Sie, wie Sie Benutzern das Bearbeiten von Daten ermöglichen, die bereits in der Datenbank vorhanden sind.

## <a name="complete-listing-for-addmovie-page"></a>Vervollständigen der Liste für die addmovie-Seite

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in die ASP.net-Webprogrammierung mit der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL INSERT INTO-Anweisung](http://www.w3schools.com/sql/sql_insert.asp) auf der W3Schools-Website
- Überprüfen [der Benutzereingaben an ASP.net Web Pages Websites](https://go.microsoft.com/fwlink/?LinkId=253002). Weitere Informationen zum Arbeiten mit dem `Validation`-Hilfsprogramm.

> [!div class="step-by-step"]
> [Zurück](form-basics.md)
> [Weiter](updating-data.md)
