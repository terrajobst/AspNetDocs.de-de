---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: Einführung in ASP.net Web Pages Aktualisieren von Datenbankdaten | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial wird gezeigt, wie Sie einen vorhandenen Datenbankeintrag aktualisieren (ändern), wenn Sie ASP.net Web Pages (Razor) verwenden. Dabei wird davon ausgegangen, dass Sie die Reihe...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 8f8bcfb7d9d2416a2699776cadbdaae8e12415ba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463491"
---
# <a name="introducing-aspnet-web-pages---updating-database-data"></a>Einführung in ASP.net Web Pages Aktualisieren von Datenbankdaten

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Tutorial wird gezeigt, wie Sie einen vorhandenen Datenbankeintrag aktualisieren (ändern), wenn Sie ASP.net Web Pages (Razor) verwenden. Dabei wird davon ausgegangen, dass Sie die Reihe durch die [Eingabe von Daten mithilfe von Formularen mit ASP.net Web Pages](entering-data.md)abgeschlossen haben.
> 
> Sie lernen Folgendes:
> 
> - Auswählen eines einzelnen Datensatzes in der `WebGrid`-Hilfsprogramm.
> - Lesen eines einzelnen Datensatzes aus einer Datenbank.
> - Vorab Laden eines Formulars mit Werten aus dem Datenbankdaten Satz.
> - Aktualisieren eines vorhandenen Datensatzes in einer Datenbank.
> - Informationen zum Speichern von Informationen auf der Seite, ohne diese anzuzeigen.
> - Verwenden eines ausgeblendeten Felds zum Speichern von Informationen.
>   
> 
> Erörterte Features und Technologien:
> 
> - Das `WebGrid`-Hilfsprogramm.
> - Der SQL `Update`-Befehl.
> - Die `Database.Execute` -Methode.
> - Ausgeblendete Felder (`<input type="hidden">`).

## <a name="what-youll-build"></a>Sie lernen Folgendes

Im vorherigen Tutorial haben Sie gelernt, wie Sie einer Datenbank einen Datensatz hinzufügen. Hier erfahren Sie, wie Sie einen Datensatz für die Bearbeitung anzeigen. Auf der Seite *Filme* aktualisieren Sie das `WebGrid` Hilfsprogramm, sodass neben den einzelnen Filmen ein Link **Bearbeiten** angezeigt wird:

![WebGrid-Anzeige einschließlich eines "Bearbeiten"-Links für die einzelnen Filme](updating-data/_static/image1.png)

Wenn Sie auf den Link " **Bearbeiten** " klicken, gelangen Sie zu einer anderen Seite, auf der sich die Filminformationen bereits in einem Formular befinden:

![Movie Page bearbeiten, die den zu bearbeitenden Film anzeigt](updating-data/_static/image2.png)

Sie können alle Werte ändern. Wenn Sie die Änderungen übermitteln, aktualisiert der Code auf der Seite die Datenbank und führt Sie zurück zur filmauflistung.

Dieser Teil des Prozesses funktioniert fast genauso wie die Seite " *addmovie. cshtml* ", die Sie im vorherigen Tutorial erstellt haben. der Großteil dieses Lernprogramms wird Ihnen sehr vertraut sein.

Es gibt mehrere Möglichkeiten, wie Sie eine Möglichkeit zum Bearbeiten eines einzelnen Films implementieren können. Der angezeigte Ansatz wurde gewählt, da er einfach zu implementieren und leicht verständlich ist.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Hinzufügen eines Bearbeitungs Links zur Movie-Auflistung

Um zu beginnen, aktualisieren Sie die Seite " *Filme* ", sodass jede Filmliste auch einen **Bearbeitungs** Link enthält.

Öffnen Sie die Datei *Movies. cshtml* .

Ändern Sie im Text der Seite das `WebGrid` Markup, indem Sie eine Spalte hinzufügen. Hier ist das geänderte Markup:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

Die neue Spalte lautet wie folgt:

[!code-html[Main](updating-data/samples/sample2.html)]

Der Punkt dieser Spalte besteht darin, einen Link (`<a>`-Element) anzuzeigen, dessen Text "Edit" lautet. Nachfolgend wird ein Link erstellt, der wie folgt aussieht, wenn die Seite ausgeführt wird, wobei der `id` Wert für die einzelnen Filme anders ist:

[!code-css[Main](updating-data/samples/sample3.css)]

Mit diesem Link wird eine Seite mit dem Namen " *editmovie*" aufgerufen, die die Abfrage Zeichenfolge `?id=7` an diese Seite übergibt.

Die Syntax für die neue Spalte kann etwas kompliziert aussehen, aber das ist nur, weil Sie mehrere Elemente vereint. Jedes einzelne Element ist unkompliziert. Wenn Sie sich nur auf das `<a>` Element konzentrieren, sehen Sie dieses Markup:

[!code-html[Main](updating-data/samples/sample4.html)]

Hintergrundinformationen zur Funktionsweise des Rasters: im Raster werden Zeilen angezeigt, eine für jeden Daten Bank Datensatz und Spalten für jedes Feld im Datenbankdaten Satz. Während die einzelnen Raster Zeilen erstellt werden, enthält das `item` Objekt den Datenbankdaten Satz (Item) für diese Zeile. Diese Anordnung ermöglicht es Ihnen, die Daten für diese Zeile im Code zu erhalten. Dies sehen Sie hier: der Ausdruck `item.ID` den ID-Wert des aktuellen Daten Bank Elements erhält. Mit `item.Title`, `item.Genre`oder `item.Year`können Sie alle Daten Bankwerte (Titel, Genre oder Jahr) auf dieselbe Weise erhalten.

Der Ausdruck `"~/EditMovie?id=@item.ID` kombiniert den hart codierten Teil der Ziel-URL (`~/EditMovie?id=`) mit dieser dynamisch abgeleiteten ID. (Im vorherigen Tutorial haben Sie den `~`-Operator gesehen; es handelt sich um einen ASP.net-Operator, der den aktuellen Website Stamm darstellt.)

Das Ergebnis ist, dass dieser Teil des Markups in der-Spalte zur Laufzeit einfach etwas wie das folgende Markup erzeugt:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Natürlich ist der tatsächliche Wert `id` für jede Zeile unterschiedlich.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Erstellen einer benutzerdefinierten Anzeige für eine Raster Spalte

Nun zurück zur Raster Spalte. Die drei Spalten, die Sie ursprünglich im Raster besaßen, haben nur Datenwerte (Titel, Genre und Jahr) angezeigt. Sie haben diese Anzeige angegeben, indem Sie den Namen der Daten Bank Spalte übergeben &mdash; z. b. `grid.Column("Title")`.

Diese neue Spalte zum **Bearbeiten** des Links ist unterschiedlich. Anstatt einen Spaltennamen anzugeben, übergeben Sie einen `format` Parameter. Mithilfe dieses Parameters können Sie Markup definieren, das vom `WebGrid`-Hilfsprogramm zusammen mit dem `item` Wert dargestellt wird, um die Spaltendaten Fett oder grün oder in einem beliebigen Format anzuzeigen. Wenn Sie z. b. möchten, dass der Titel fett angezeigt wird, können Sie eine Spalte wie im folgenden Beispiel erstellen:

[!code-html[Main](updating-data/samples/sample6.html)]

(Die verschiedenen `@` Zeichen, die Sie in der `format`-Eigenschaft sehen, markieren den Übergang zwischen Markup und einem Codewert.)

Wenn Sie die `format`-Eigenschaft kennen, ist es leichter zu verstehen, wie die neue Spalte zum **Bearbeiten** von Links zusammengestellt wird:

[!code-html[Main](updating-data/samples/sample7.html)]

Die Spalte besteht *nur* aus dem Markup, das den Link rendert, sowie aus einigen Informationen (der ID), die aus dem Datenbankdaten Satz für die Zeile extrahiert werden.

> [!TIP]
> 
> **Benannte Parameter und Positions Parameter für eine Methode**
> 
> Viele Male, wenn Sie eine Methode aufgerufen und Parameter an diese übergeben haben, haben Sie einfach die durch Kommas getrennten Parameterwerte aufgelistet. Hier sind einige Beispiele angegeben:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Wir haben das Problem beim ersten Mal in diesem Code nicht erwähnt, aber in jedem Fall übergeben Sie Parameter an die Methoden in einer bestimmten Reihenfolge &mdash; nämlich die Reihenfolge, in der die Parameter in dieser Methode definiert werden. Wenn Sie für `db.Execute` und `Validation.RequireFields`die Reihenfolge der übergebenen Werte gemischt haben, erhalten Sie eine Fehlermeldung, wenn die Seite ausgeführt wird, oder zumindest einige seltsame Ergebnisse. Natürlich müssen Sie die Reihenfolge kennen, in der die Parameter übergeben werden. (In webmatrix kann IntelliSense Ihnen helfen, den Namen, den Typ und die Reihenfolge der Parameter zu ermitteln.)
> 
> Anstatt Werte in der Reihenfolge zu übergeben, können Sie *benannte Parameter*verwenden. (Das Übergeben von Parametern in der Reihenfolge wird als Verwenden von *Positions Parametern*bezeichnet.) Bei benannten Parametern fügen Sie den Namen des Parameters explizit ein, wenn Sie seinen Wert übergeben. In diesen Tutorials haben Sie benannte Parameter bereits mehrmals verwendet. Beispiel:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> und
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Benannte Parameter sind in einigen Situationen praktisch, insbesondere dann, wenn eine Methode viele Parameter annimmt. Eine ist, wenn Sie nur einen oder zwei Parameter übergeben möchten, aber die Werte, die Sie übergeben möchten, nicht zu den ersten Positionen in der Parameterliste gehören. Eine andere Situation ist, wenn Sie den Code besser lesbar machen möchten, indem Sie die Parameter in der Reihenfolge übergeben, die für Sie am sinnvollsten ist.
> 
> Um benannte Parameter zu verwenden, müssen Sie die Namen der Parameter kennen. Webmatrix IntelliSense kann die Namen *anzeigen* , aber Sie kann Sie zurzeit nicht für Sie ausfüllen.

## <a name="creating-the-edit-page"></a>Erstellen der Seite "Bearbeiten"

Nun können Sie die *editmovie* -Seite erstellen. Wenn Benutzer auf den Link " **Bearbeiten** " klicken, werden Sie auf dieser Seite angezeigt.

Erstellen Sie eine Seite mit dem Namen " *editmovie. cshtml* ", und ersetzen Sie die Elemente in der Datei durch das folgende Markup:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Dieses Markup und dieser Code ähneln denen, die Sie auf der Seite " *addmovie* " haben. Es gibt einen kleinen Unterschied im Text für die Schaltfläche "Senden". Wie bei der *addmovie* -Seite gibt es einen `Html.ValidationSummary`-Befehl, der Validierungs Fehler anzeigt, sofern vorhanden. Diesmal lassen wir Aufrufe an `Validation.Message`aus, da Fehler in der Validierungs Zusammenfassung angezeigt werden. Wie bereits im vorherigen Tutorial erwähnt, können Sie die Validierungs Zusammenfassung und die einzelnen Fehlermeldungen in verschiedenen Kombinationen verwenden.

Beachten Sie, dass das `method`-Attribut des `<form>`-Elements auf `post`festgelegt ist. Wie bei der *addmovie. cshtml* -Seite nimmt diese Seite Änderungen an der-Datenbank vor. Daher sollte in diesem Formular ein `POST` Vorgang durchgeführt werden. (Weitere Informationen zum Unterschied zwischen `GET` und `POST` Vorgängen finden Sie in der Rand Leiste " [Get", "Post" und "http Verb Safety](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) " im Tutorial zu HTML-Formularen.)

Wie Sie in einem früheren Tutorial gesehen haben, werden die `value` Attribute der Textfelder mit Razor-Code festgelegt, um Sie vorab zu laden. Dieses Mal verwenden Sie jedoch Variablen wie `title` und `genre` für diese Aufgabe anstelle `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Wie zuvor lädt dieses Markup die Textfeld-Werte mit den Film Werten vorab. Sie werden in Kürze sehen, warum es praktisch ist, Variablen zu verwenden, anstatt das `Request` Objekt zu verwenden.

Auf dieser Seite ist auch ein `<input type="hidden">`-Element vorhanden. Dieses Element speichert die Film-ID, ohne Sie auf der Seite sichtbar zu machen. Die ID wird zunächst mithilfe eines Abfrage Zeichenfolgen-Werts (`?id=7` oder ähnlich in der URL) an die Seite übermittelt. Wenn Sie den ID-Wert in ein ausgeblendetes Feld einfügen, können Sie sicherstellen, dass es verfügbar ist, wenn das Formular übermittelt wird, auch wenn Sie nicht mehr auf die ursprüngliche URL zugreifen können, mit der die Seite aufgerufen wurde.

Im Gegensatz zur *addmovie* -Seite verfügt der Code für die *editmovie* -Seite über zwei unterschiedliche Funktionen. Die erste Funktion ist, dass der Code die Film-ID aus der Abfrage Zeichenfolge abruft, wenn die Seite zum ersten Mal angezeigt wird (und *nur* dann). Der Code verwendet die ID dann, um den entsprechenden Film aus der Datenbank zu lesen und ihn in den Textfeldern anzuzeigen (vorab zu laden).

Die zweite Funktion besteht darin, dass der Code, wenn der Benutzer auf die Schaltfläche " **Änderungen übermitteln** " klickt, die Werte der Textfelder lesen und diese überprüfen muss. Der Code muss auch das Daten Bank Element mit den neuen Werten aktualisieren. Dieses Verfahren ähnelt dem Hinzufügen eines Datensatzes, wie Sie in *addmovie*gesehen haben.

## <a name="adding-code-to-read-a-single-movie"></a>Hinzufügen von Code zum Lesen eines einzelnen Films

Fügen Sie den folgenden Code am Anfang der Seite ein, um die erste Funktion auszuführen:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

Der größte Teil dieses Codes befindet sich in einem Block, der `if(!IsPost)`startet. Der `!`-Operator bedeutet "Not". der Ausdruck bedeutet, dass *es sich bei dieser Anforderung nicht um eine Post-Übermittlung handelt*. Dies ist eine indirekte Methode, um zu sagen, *ob diese Anforderung zum ersten Mal ausgeführt*wurde. Wie bereits erwähnt, sollte dieser Code *nur* bei der erstmaligen Ausführung der Seite ausgeführt werden. Wenn Sie den Code nicht in `if(!IsPost)`eingeschlossen haben, wird er jedes Mal ausgeführt, wenn die Seite aufgerufen wird, egal ob beim ersten Mal oder als Reaktion auf einen Klick auf eine Schaltfläche.

Beachten Sie, dass der Code einen `else` Block This Time enthält. Wie bereits erwähnt, wenn wir `if` Blöcke eingeführt haben, möchten Sie manchmal alternativen Code ausführen, wenn die zu testende Bedingung nicht wahr ist. Das ist hier der Fall. Wenn die Bedingung erfüllt ist (d. h., wenn die an die Seite übergebenen ID in Ordnung ist), lesen Sie eine Zeile aus der Datenbank. Wenn die Bedingung jedoch nicht erfüllt ist, wird der `else`-Block ausgeführt, und der Code legt eine Fehlermeldung fest.

## <a name="validating-a-value-passed-to-the-page"></a>Validieren eines an die Seite übergebenen Werts

Der Code verwendet `Request.QueryString["id"]`, um die ID zu erhalten, die an die Seite übermittelt wird. Der Code stellt sicher, dass für die ID tatsächlich ein Wert übermittelt wurde. Wenn kein Wert bestanden wurde, legt der Code einen Validierungs Fehler fest.

Dieser Code zeigt eine andere Möglichkeit zum Überprüfen von Informationen. Im vorherigen Tutorial haben Sie mit dem `Validation`-Hilfsprogramm gearbeitet. Sie haben die zu validierenden Felder registriert, und ASP.net hat die Überprüfung automatisch durchgeführt und die Fehler angezeigt, indem Sie `Html.ValidationMessage` und `Html.ValidationSummary` In diesem Fall überprüfen Sie jedoch nicht wirklich die Benutzereingaben. Stattdessen validieren Sie einen Wert, der von an anderer Stelle an die Seite übermittelt wurde. Das `Validation`-Hilfsprogramm führt dies nicht für Sie aus.

Daher überprüfen Sie den Wert selbst, indem Sie ihn mit `if(!Request.QueryString["ID"].IsEmpty()`) testen. Wenn ein Problem vorliegt, können Sie den Fehler wie beim `Validation`-Hilfsprogramm mithilfe von `Html.ValidationSummary`anzeigen. Dazu wird `Validation.AddFormError` aufgerufen und eine Meldung an die Anzeige übergeben. `Validation.AddFormError` ist eine integrierte Methode, mit der Sie benutzerdefinierte Meldungen definieren können, die mit dem bereits vertrauten Validierungssystem verknüpft sind. (Später in diesem Tutorial wird erläutert, wie Sie diesen Validierungsprozess etwas stabiler machen.)

Nachdem Sie sichergestellt haben, dass eine ID für den Film vorhanden ist, liest der Code die Datenbank und sucht nur nach einem einzelnen Daten Bank Element. (Wahrscheinlich haben Sie das allgemeine Muster für Daten Bank Vorgänge bemerkt: Öffnen Sie die Datenbank, definieren Sie eine SQL-Anweisung, und führen Sie die Anweisung aus.) Dieses Mal enthält die SQL `Select`-Anweisung `WHERE ID = @0`. Da die ID eindeutig ist, kann nur ein Datensatz zurückgegeben werden.

Die Abfrage wird mit `db.QuerySingle` ausgeführt (nicht `db.Query`, wie Sie für die filmauflistung verwendet haben), und der Code fügt das Ergebnis in die `row` Variable ein. Der Name `row` ist willkürlich. Sie können die Variablen beliebig benennen. Die Variablen, die oben initialisiert werden, werden dann mit den Filmdetails gefüllt, sodass diese Werte in den Textfeldern angezeigt werden können.

## <a name="testing-the-edit-page-so-far"></a>Testen der Bearbeitungsseite (bisher)

Wenn Sie Ihre Seite testen möchten, führen Sie jetzt die Seite *Filme* aus, und klicken Sie auf einen Link **Bearbeiten** neben jedem Film. Die Seite *editmovie* wird mit den Details für den ausgewählten Film angezeigt:

![Movie Page bearbeiten, die den zu bearbeitenden Film anzeigt](updating-data/_static/image3.png)

Beachten Sie, dass die URL der Seite etwas wie `?id=10` (oder eine andere Zahl) enthält. Bisher haben Sie das **Bearbeiten** von Links auf der Seite " *Movie* " getestet, dass die Seite die ID aus der Abfrage Zeichenfolge liest und dass die Datenbankabfrage zum erhalten eines einzelnen Film Datensatzes funktioniert.

Sie können die Filminformationen ändern, aber nichts passiert, wenn Sie auf **Änderungen übermitteln**klicken.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Hinzufügen von Code zum Aktualisieren des Films mit den Änderungen des Benutzers

Fügen Sie in der Datei *editmovie. cshtml* zum Implementieren der zweiten Funktion (Speichern von Änderungen) den folgenden Code direkt innerhalb der schließenden geschweiften Klammer des `@` Blocks hinzu. (Wenn Sie nicht genau wissen, wo der Code abgelegt werden soll, sehen Sie sich die [komplette Codeliste für die Seite "Movie bearbeiten](#Complete_Page_Listing_for_EditMovie) " an, die am Ende dieses Lernprogramms angezeigt wird.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Dieses Markup und dieser Code ähneln dem Code in *addmovie*. Der Code befindet sich in einem `if(IsPost)`-Block, da dieser Code nur ausgeführt wird, wenn der Benutzer auf die Schaltfläche " **Änderungen übermitteln** " klickt &mdash; d. h., wann (und nur wenn) das Formular gepostet wurde. In diesem Fall verwenden Sie keinen Test wie `if(IsPost && Validation.IsValid())`– d. h. Sie kombinieren nicht beide Tests mithilfe von und. Auf dieser Seite legen Sie zuerst fest, ob eine Formular Übermittlung (`if(IsPost)`) vorhanden ist, und registrieren dann nur die Felder für die Validierung. Anschließend können Sie die Validierungs Ergebnisse (`if(Validation.IsValid()`) testen. Der Flow unterscheidet sich geringfügig von der Seite " *addmovie. cshtml* ", aber der Effekt ist identisch.

Sie erhalten die Werte der Textfelder, indem Sie `Request.Form["title"]` und ähnlichen Code für die anderen `<input>` Elemente verwenden. Beachten Sie, dass der Code die Film-ID aus dem ausgeblendeten Feld (`<input type="hidden">`) abruft. Wenn die Seite zum ersten Mal ausgeführt wurde, erhielt der Code die ID aus der Abfrage Zeichenfolge. Sie erhalten den Wert aus dem ausgeblendeten Feld, um sicherzustellen, dass Sie die ID des Films erhalten, der ursprünglich angezeigt wurde, für den Fall, dass die Abfrage Zeichenfolge seitdem geändert wurde.

Der wirklich wichtige Unterschied zwischen dem *addmovie* -Code und diesem Code besteht darin, dass Sie in diesem Code die SQL `Update`-Anweisung anstelle der `Insert Into`-Anweisung verwenden. Das folgende Beispiel zeigt die Syntax der SQL `Update`-Anweisung:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Sie können beliebige Spalten in beliebiger Reihenfolge angeben, und Sie müssen nicht unbedingt jede Spalte während eines `Update` Vorgangs aktualisieren. (Sie können die ID selbst nicht aktualisieren, da dadurch der Datensatz als neuer Datensatz gespeichert wird, was für einen `Update` Vorgang nicht zulässig ist.)

> [!NOTE] 
> 
> **Wichtig** Die `Where`-Klausel mit der ID ist sehr wichtig, da die Datenbank weiß, welcher Daten Bank Datensatz Sie aktualisieren möchten. Wenn Sie die `Where`-Klausel ausgelassen haben, aktualisiert die Datenbank *jeden* Datensatz in der Datenbank. In den meisten Fällen wäre dies ein Notfall.

Im Code werden die zu aktualisierenden Werte mithilfe von Platzhaltern an die SQL-Anweisung übermittelt. Um zu wiederholen, was wir vorhergesagt haben: aus Sicherheitsgründen verwenden Sie *nur* Platzhalter, um Werte an eine SQL-Anweisung zu übergeben.

Nachdem der Code `db.Execute` verwendet hat, um die `Update` Anweisung auszuführen, wird er zurück zur Listenseite umgeleitet, auf der Sie die Änderungen sehen können.

> [!TIP] 
> 
> **Verschiedene SQL-Anweisungen, verschiedene Methoden**
> 
> Möglicherweise haben Sie bemerkt, dass Sie leicht unterschiedliche Methoden verwenden, um verschiedene SQL-Anweisungen auszuführen. Um eine `Select` Abfrage auszuführen, die potenziell mehrere Datensätze zurückgibt, verwenden Sie die `Query`-Methode. Um eine `Select` Abfrage auszuführen, von der Sie wissen, dass nur ein Daten Bank Element zurückgegeben wird, verwenden Sie die `QuerySingle`-Methode. Zum Ausführen von Befehlen, die Änderungen vornehmen, aber keine Datenbankelemente zurückgeben, verwenden Sie die `Execute`-Methode.
> 
> Sie müssen über verschiedene Methoden verfügen, da jeder von Ihnen andere Ergebnisse zurückgibt, wie Sie bereits im Unterschied zwischen `Query` und `QuerySingle`gesehen haben. (Die `Execute`-Methode gibt auch einen-Wert zurück, der die Anzahl der vom &mdash; Befehl betroffenen Daten Bank Zeilen &mdash;, aber Sie haben das bisher ignoriert.)
> 
> Natürlich kann die `Query`-Methode nur eine Datenbankzeile zurückgeben. ASP.NET behandelt die Ergebnisse der `Query` Methode jedoch immer als eine Auflistung. Auch wenn die Methode nur eine Zeile zurückgibt, müssen Sie diese einzelne Zeile aus der Auflistung extrahieren. Daher ist es in Situationen, in denen Sie *wissen, dass* Sie nur eine Zeile zurückerhalten, etwas bequemer, `QuerySingle`zu verwenden.
> 
> Es gibt einige andere Methoden, mit denen bestimmte Typen von Daten Bank Vorgängen durchgeführt werden. Eine Liste der datenbankmethoden finden Sie in der [ASP.net Web Pages-API-kurz](../../api-reference/asp-net-web-pages-api-reference.md#Data)Übersicht.

## <a name="making-validation-for-the-id-more-robust"></a>Die Überprüfung der ID ist robuster.

Wenn die Seite das erste Mal ausgeführt wird, erhalten Sie die Film-ID aus der Abfrage Zeichenfolge, sodass Sie diesen Film aus der Datenbank holen können. Sie haben sichergestellt, dass es tatsächlich einen Wert gibt, den Sie mit diesem Code erstellt haben:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Sie haben diesen Code verwendet, um sicherzustellen, dass die Seite eine benutzerfreundliche Fehlermeldung anzeigt, wenn ein Benutzer auf die *editmovies* -Seite gelangt, ohne zuvor einen Film auf der Seite " *Filme* " auszuwählen. (Andernfalls wird Benutzern eine Fehlermeldung angezeigt, die Sie wahrscheinlich nur verwirren würde.)

Diese Validierung ist jedoch nicht sehr robust. Die Seite kann auch mit folgenden Fehlern aufgerufen werden:

- Die ID ist keine Zahl. Beispielsweise könnte die Seite mit einer URL wie `http://localhost:nnnnn/EditMovie?id=abc`aufgerufen werden.
- Die ID ist eine Zahl, verweist jedoch auf einen Film, der nicht vorhanden ist (z. b. `http://localhost:nnnnn/EditMovie?id=100934`).

Wenn Sie die Fehler, die sich aus diesen URLs ergeben, anzeigen möchten, führen Sie die Seite *Filme* aus. Wählen Sie einen zu bearbeitenden Film aus, und ändern Sie dann die URL der *editmovie* -Seite in eine URL, die eine alphabetische ID oder die ID eines nicht vorhandenen Films enthält.

Was sollten Sie also tun? Der erste Fix besteht darin, sicherzustellen, dass nicht nur eine ID an die Seite, sondern eine ganze Zahl ist. Ändern Sie den Code für den `!IsPost` Test so, dass er wie in diesem Beispiel aussieht:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Sie haben dem `IsEmpty` Test eine zweite Bedingung hinzugefügt, die mit `&&` (logisches and) verknüpft ist:

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Möglicherweise erinnern Sie sich von der [Einführung in ASP.net Web Pages Programming](../introducing-razor-syntax-c.md) Tutorial, dass Methoden wie `AsBool` ein `AsInt` eine Zeichenfolge in einen anderen Datentyp konvertieren. Die `IsInt`-Methode (und andere, wie `IsBool` und `IsDateTime`) sind ähnlich. Allerdings wird nur getestet, ob Sie die Zeichenfolge konvertieren *können* , ohne die Konvertierung tatsächlich auszuführen. Hier sagen Sie im Grunde *, ob der Wert der Abfrage Zeichenfolge in eine ganze Zahl konvertiert werden kann...*

Das andere potenzielle Problem ist die Suche nach einem Film, der nicht vorhanden ist. Der Code zum erhalten eines Films sieht wie der folgende Code aus:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Wenn Sie einen `movieId` Wert an die `QuerySingle`-Methode übergeben, die keinem tatsächlichen Film entspricht, wird nichts zurückgegeben, und die folgenden Anweisungen (z. b. `title=row.Title`) führen zu Fehlern.

Auch hier gibt es eine einfache Lösung. Wenn die `db.QuerySingle`-Methode keine Ergebnisse zurückgibt, ist die `row` Variable NULL. Sie können also überprüfen, ob die `row` Variable NULL ist, bevor Sie versuchen, Werte daraus zu erhalten. Der folgende Code fügt einen `if`-Block um die-Anweisungen hinzu, die die Werte aus dem `row`-Objekt erhalten:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Mit diesen beiden zusätzlichen Validierungstests wird die Seite stärker Aufzählungs Zeichen. Der gesamte Code für den `!IsPost` Branch sieht nun wie im folgenden Beispiel aus:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Beachten Sie, dass diese Aufgabe eine gute Verwendung für einen `else`-Block ist. Wenn die Tests nicht bestanden werden, werden vom `else` Blöcke Fehlermeldungen festgelegt.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Hinzufügen eines Links zum zurückkehren zur Seite "Filme"

Ein letztes und hilfreiches Detail ist das Hinzufügen eines Links zurück zur Seite " *Filme* ". Im normalen Ablauf von Ereignissen beginnen Benutzer auf der Seite " *Filme* " und klicken auf den Link " **Bearbeiten** ". Damit gelangen Sie auf die Seite *editmovie* , wo Sie den Film bearbeiten und auf die Schaltfläche klicken können. Nachdem der Code die Änderung verarbeitet hat, wird er zurück an die Seite " *Movies* " umgeleitet.

Dabei gilt jedoch Folgendes:

- Der Benutzer kann sich entscheiden, nichts zu ändern.
- Der Benutzer ist möglicherweise auf diese Seite gelangt, ohne zuvor auf der Seite " *Filme* " auf den Link " **Bearbeiten** " zu klicken.

In jedem Fall möchten Sie es Ihnen erleichtern, zur Haupt Auflistung zurückzukehren. Es ist eine einfache Lösung, &mdash; das folgende Markup direkt hinter dem schließenden `</form>`-Tag im Markup hinzuzufügen:

[!code-html[Main](updating-data/samples/sample19.html)]

Dieses Markup verwendet die gleiche Syntax für ein `<a>` Element, das Sie an anderer Stelle gesehen haben. Die URL enthält `~` "Stamm der Website".

## <a name="testing-the-movie-update-process"></a>Testen des Film Aktualisierungsprozesses

Jetzt können Sie testen. Führen Sie die Seite *Filme* aus, und klicken Sie auf **Bearbeiten** neben einem Film. Wenn die Seite *editmovie* angezeigt wird, nehmen Sie Änderungen am Film vor, und klicken Sie auf **Änderungen übermitteln**. Wenn die Film Auflistung angezeigt wird, stellen Sie sicher, dass Ihre Änderungen angezeigt werden.

Um sicherzustellen, dass die Überprüfung funktioniert, klicken Sie auf **Bearbeiten** für einen anderen Film. Wenn Sie auf die Seite *editmovie* gelangen, löschen Sie das Feld **Genre** (oder das Feld **Jahr** oder beides), und versuchen Sie, die Änderungen zu übermitteln. Wie Sie erwarten, wird ein Fehler angezeigt:

![Movie Page bearbeiten zeigt Validierungs Fehler an](updating-data/_static/image4.png)

Klicken Sie auf den Link **zum Listen Link zurückkehren** , um die Änderungen zu verwerfen und zur Seite *Filme* zurückzukehren.

## <a name="coming-up-next"></a>Nächste nächste

Im nächsten Tutorial erfahren Sie, wie Sie einen Film Daten Satz löschen.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Vervollständigen der Auflistung für Movie Page (aktualisiert mit Links bearbeiten)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Complete Page Listing for Movie Page Page

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in die ASP.net-Webprogrammierung mithilfe der Razor-Syntax](../../getting-started/introducing-razor-syntax-c.md)
- [SQL Update-Anweisung](http://www.w3schools.com/sql/sql_update.asp) auf der W3Schools-Website

> [!div class="step-by-step"]
> [Zurück](entering-data.md)
> [Weiter](deleting-data.md)
