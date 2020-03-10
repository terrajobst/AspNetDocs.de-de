---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Einführung in ASP.net Web Pages-HTML-Formular Grundlagen | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial werden die Grundlagen der Erstellung eines Eingabeformulars und das Behandeln der Eingabe des Benutzers erläutert, wenn Sie ASP.net Web Pages (Razor) verwenden. Und jetzt...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: f57661077ec3bb13f3d4ec41b130bda4d2fb9070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463539"
---
# <a name="introducing-aspnet-web-pages---html-form-basics"></a>Einführung in ASP.net Web Pages-HTML-Formular Grundlagen

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Tutorial werden die Grundlagen der Erstellung eines Eingabeformulars und das Behandeln der Eingabe des Benutzers erläutert, wenn Sie ASP.net Web Pages (Razor) verwenden. Und nachdem Sie über eine Datenbank verfügen, verwenden Sie Ihre Formular Kenntnisse, um Benutzern das Auffinden bestimmter Filme in der Datenbank zu ermöglichen. Dabei wird davon ausgegangen, dass Sie die Reihe durch [Einführung in die Anzeige von Daten mit ASP.net Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data)abgeschlossen haben.
> 
> Sie lernen Folgendes:
> 
> - Erstellen eines Formulars mithilfe von HTML-Standardelementen.
> - Lesen der Benutzereingaben in einem Formular.
> - So erstellen Sie eine SQL-Abfrage, die Daten selektiv mithilfe eines vom Benutzer bereitgestellten Suchbegriffs abruft.
> - Gibt an, wie Felder auf der Seite "Speichern", was der Benutzer eingegeben hat.
>   
> 
> Erörterte Features und Technologien:
> 
> - Das `Request`-Objekt.
> - Die SQL `Where`-Klausel.

## <a name="what-youll-build"></a>Sie lernen Folgendes

Im vorherigen Tutorial haben Sie eine Datenbank erstellt, ihr Daten hinzugefügt und dann das `WebGrid` Hilfsprogramm verwendet, um die Daten anzuzeigen. In diesem Tutorial fügen Sie ein Suchfeld hinzu, mit dem Sie Filme eines bestimmten Genres suchen können oder dessen Titel das von Ihnen eingegebene Wort enthält. (Sie können z. b. alle Filme finden, deren Genre "action" ist oder deren Titel "Harry" oder "Adventure" enthält).

Wenn Sie mit diesem Tutorial fertig sind, verfügen Sie über eine Seite wie die folgende:

![Seite "Filme" mit Genre-und Titelsuche](form-basics/_static/image1.png)

Der Auflistungs Teil der Seite ist mit dem des letzten Tutorials &mdash; einem Raster identisch. Der Unterschied besteht darin, dass im Raster nur die Filme angezeigt werden, nach denen Sie suchen.

## <a name="about-html-forms"></a>Informationen über HTML-Formulare

(Wenn Sie Erfahrung mit der Erstellung von HTML-Formularen haben und den Unterschied zwischen `GET` und `POST`haben, können Sie diesen Abschnitt überspringen.)

Ein Formular verfügt über Benutzereingabe Elemente &mdash; Textfelder, Schaltflächen, Options Felder, Kontrollkästchen, Dropdown Listen usw. Benutzer füllen diese Steuerelemente aus oder treffen die Auswahl und senden dann das Formular durch Klicken auf eine Schaltfläche.

Die grundlegende HTML-Syntax eines Formulars wird in diesem Beispiel veranschaulicht:

[!code-html[Main](form-basics/samples/sample1.html)]

Wenn dieses Markup auf einer Seite ausgeführt wird, wird ein einfaches Formular erstellt, das wie in der folgenden Abbildung aussieht:

![Einfaches HTML-Formular, das im Browser gerendert wird](form-basics/_static/image2.png)

Das `<form>` Element schließt HTML-Elemente ein, die gesendet werden sollen. (Ein einfacher Fehler besteht darin, der Seite Elemente hinzuzufügen, diese jedoch in einem `<form>` Element abzulegen. In diesem Fall wird nichts übermittelt.) Das `method`-Attribut weist den Browser an, die Benutzereingaben zu übermitteln. Sie legen dies auf `post` fest, wenn Sie ein Update auf dem Server ausführen oder `get`, wenn Sie nur Daten vom Server abrufen.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **Get-, Post-und HTTP-Verb Sicherheit**
> 
> HTTP, das Protokoll, das Browser und Server zum Austauschen von Informationen verwenden, ist in den grundlegenden Vorgängen erstaunlich einfach. Browser verwenden nur wenige Verben, um Anforderungen an Server zu senden. Wenn Sie Code für das Web schreiben, ist es hilfreich, diese Verben zu verstehen und zu erfahren, wie Sie von Browser und Server verwendet werden. Die am häufigsten verwendeten Verben sind die folgenden:
> 
> - [https://login.microsoftonline.com/consumers/](`GET`). Der Browser verwendet dieses Verb zum Abrufen von Informationen vom Server. Wenn Sie z. b. eine URL in Ihren Browser eingeben, führt der Browser einen `GET` Vorgang aus, um die gewünschte Seite anzufordern. Wenn die Seite Grafiken enthält, führt der Browser zusätzliche `GET` Vorgänge aus, um die Bilder zu erhalten. Wenn der `GET` Vorgang Informationen an den Server übergeben muss, werden die Informationen als Teil der URL in der Abfrage Zeichenfolge übergeben.
> - [https://login.microsoftonline.com/consumers/](`POST`). Der Browser sendet eine `POST` Anforderung, um Daten zu übermitteln, die auf dem Server hinzugefügt oder geändert werden sollen. Das `POST` Verb wird z. b. verwendet, um Datensätze in einer Datenbank zu erstellen oder vorhandene zu ändern. In den meisten Fällen führt der Browser einen `POST` Vorgang aus, wenn Sie ein Formular ausfüllen und auf die Schaltfläche "Senden" klicken. Bei einem `POST` Vorgang befinden sich die Daten, die an den Server übermittelt werden, im Text der Seite.
> 
> Ein wichtiger Unterschied zwischen diesen Verben besteht darin, dass bei einem `GET` Vorgang nichts auf dem Server geändert werden soll – oder dass ein `GET` Vorgang etwas abstrakter Weise erfolgt, wenn ein Vorgang nicht zu einer Zustandsänderung auf dem Server führt. Sie können einen `GET` Vorgang für dieselben Ressourcen so oft wie gewünscht ausführen, und diese Ressourcen werden nicht geändert. (Ein `GET` Vorgang wird häufig als "sicher" bezeichnet oder ist *idempotent*, um einen technischen Begriff zu verwenden.) Im Gegensatz dazu ändert eine `POST` Anforderung auf dem Server jedes Mal, wenn Sie den Vorgang ausführen.
> 
> Zwei Beispiele helfen Ihnen, diesen Unterschied zu veranschaulichen. Wenn Sie eine Suche mit einer Engine wie z. b. "z" oder Google ausführen, füllen Sie ein Formular aus, das aus einem Textfeld besteht, und klicken Sie dann auf die Schaltfläche "suchen". Der Browser führt einen `GET` Vorgang mit dem Wert aus, den Sie in das Feld eingegeben haben, das als Teil der URL eingegeben wurde. Die Verwendung eines `GET` Vorgangs für diese Art von Formular ist in Ordnung, da bei einem Suchvorgang keine Ressourcen auf dem Server geändert werden, sondern lediglich Informationen abgerufen werden.
> 
> Sehen Sie sich nun den Prozess der Bestellung von Online an. Füllen Sie die Bestelldetails aus, und klicken Sie dann auf die Schaltfläche Senden. Bei diesem Vorgang handelt es sich um eine `POST` Anforderung, da der Vorgang zu Änderungen auf dem Server führt, z. b. ein neuer Bestelldaten Satz, eine Änderung der Kontoinformationen und möglicherweise noch viele andere Änderungen. Im Gegensatz zum `GET` Vorgang können Sie Ihre `POST` Anforderung nicht wiederholen – Wenn Sie dies getan haben, würden Sie bei jeder erneuten Übermittlung der Anforderung eine neue Bestellung auf dem Server generieren. (In solchen Fällen warnen Websites häufig darauf, dass Sie nicht mehrmals auf die Schaltfläche "Senden" klicken oder die Schaltfläche "Senden" deaktivieren, um das Formular nicht versehentlich erneut zu senden.)
> 
> Im Rahmen dieses Tutorials verwenden Sie sowohl einen `GET` Vorgang als auch einen `POST` Vorgang, um mit HTML-Formularen zu arbeiten. In jedem Fall wird erläutert, warum das Verb, das Sie verwenden, das richtige ist.
> 
> (Weitere Informationen zu HTTP-Verben finden Sie im Artikel [Methoden Definitionen](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) auf der W3C-Website.)

Die meisten Benutzereingabe Elemente sind HTML-`<input>` Elemente. Sie sehen wie `<input type="type" name="name">,`, wobei der *Typ* die gewünschte Art von Benutzereingabe-Steuerelement angibt. Dabei handelt es sich um die folgenden Elemente:

- Textfeld: `<input type="text">`
- Kontrollkästchen: `<input type="check">`
- Optionsfeld: `<input type="radio">`
- Schaltfläche: `<input type="button">`
- Schaltfläche "Senden": `<input type="submit">`

Sie können auch das `<textarea>`-Element verwenden, um ein mehrzeilige Textfeld zu erstellen, und das `<select>`-Element, um eine Dropdown Liste oder scrollbare Liste zu erstellen. (Weitere Informationen zu HTML-Formular Elementen finden Sie unter [HTML Forms and Input](http://www.w3schools.com/html/html_forms.asp) auf der W3Schools-Website.)

Das `name`-Attribut ist sehr wichtig, da der Name ist, wie Sie den Wert des-Elements später erhalten, wie Sie es in Kürze sehen werden.

Der interessante Teil ist das, was Sie, der Seiten Entwickler, mit der Eingabe des Benutzers tun. Diesen Elementen ist kein integriertes Verhalten zugeordnet. Stattdessen müssen Sie die Werte erhalten, die der Benutzer eingegeben oder ausgewählt hat, und damit eine Aktion ausführen. Das ist das, was Sie in diesem Tutorial erlernen werden.

> [!TIP] 
> 
> **HTML5 und Eingabeformulare**
> 
> Wie Sie vielleicht wissen, befindet sich HTML im Übergang, und die neueste Version (HTML5) umfasst die Unterstützung intuitiver Möglichkeiten für die Eingabe von Informationen durch Benutzer. Beispielsweise können Sie (der Seiten Entwickler) in HTML5 der Seite mitteilen, dass der Benutzer ein Datum eingeben soll. Der Browser kann dann automatisch einen Kalender anzeigen, anstatt den Benutzer manuell eingeben zu müssen. HTML5 ist jedoch neu und wird in allen Browsern noch nicht unterstützt.
> 
> ASP.net Web Pages unterstützt HTML5-Eingaben in dem Umfang, in dem der Browser des Benutzers dies tut. Eine Vorstellung der neuen Attribute für das `<input>`-Element in HTML5 finden Sie unter [HTML-&lt;Eingabe&gt; Type-Attribut](http://www.w3schools.com/html/html_form_input_types.asp) auf der W3Schools-Website.

## <a name="creating-the-form"></a>Erstellen des Formulars

Öffnen Sie in webmatrix im Arbeitsbereich " **Dateien** " die Seite " *Movies. cshtml* ".

Fügen Sie nach dem schließenden `</h1>`-Tag und vor dem öffnenden `<div>`-Tag des `grid.GetHtml`-Aufrufes das folgende Markup hinzu:

[!code-html[Main](form-basics/samples/sample2.html)]

Dieses Markup erstellt ein Formular, das über ein Textfeld mit dem Namen `searchGenre` und eine Schaltfläche zum Senden verfügt. Das Textfeld und die Schaltfläche Senden sind in ein `<form>` Element eingeschlossen, dessen `method`-Attribut auf `get`festgelegt ist. (Beachten Sie, dass beim Klicken auf die Schaltfläche nichts übermittelt wird, wenn Sie das Textfeld und die Schaltfläche "Senden" nicht in einem `<form>` Element ablegen.) Sie verwenden das `GET` Verb hier, da Sie ein Formular erstellen, das keine Änderungen auf dem Server vornimmt – es führt lediglich zu einer Suche. (Im vorherigen Tutorial haben Sie eine `post`-Methode verwendet, mit der Sie Änderungen an den Server übermitteln. Dies wird im nächsten Tutorial wieder angezeigt.)

Führen Sie die Seite aus. Obwohl Sie kein Verhalten für das Formular definiert haben, können Sie sehen, wie es aussieht:

![Seite "Filme" mit Suchfeld für Genre](form-basics/_static/image3.png)

Geben Sie einen Wert in das Textfeld ein, z. b. "Comedy". Klicken Sie dann auf **Genre suchen**.

Notieren Sie sich die URL der Seite. Da Sie das `method`-Attribut des `<form>` Elements auf `get`festlegen, ist der eingegebene Wert nun Teil der Abfrage Zeichenfolge in der URL, wie folgt:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Lesen von Formular Werten

Die Seite enthält bereits Code, mit dem Datenbankdaten abgerufen und die Ergebnisse in einem Raster angezeigt werden. Nun müssen Sie Code hinzufügen, der den Wert des Textfelds liest, damit Sie eine SQL-Abfrage ausführen können, die den Suchbegriff enthält.

Da Sie die-Methode des Formulars auf `get`festgelegt haben, können Sie den eingegebenen Wert in das Textfeld lesen, indem Sie Code wie den folgenden verwenden:

`var searchTerm = Request.QueryString["searchGenre"];`

Das `Request.QueryString`-Objekt (die `QueryString`-Eigenschaft des `Request`-Objekts) enthält die Werte der Elemente, die im Rahmen des `GET` Vorgangs übermittelt wurden. Die `Request.QueryString`-Eigenschaft enthält eine *Auflistung (eine* Liste) der Werte, die im Formular übermittelt werden. Um einen einzelnen Wert zu erhalten, geben Sie den Namen des gewünschten Elements an. Aus diesem Grund müssen Sie über ein `name`-Attribut für das `<input>` Element verfügen (`searchTerm`), das das Textfeld erstellt. (Weitere Informationen zum `Request`-Objekt finden Sie in der Rand [Leiste](#BKMK_TheRequestObject) später.)

Es ist einfach genug, um den Wert des Textfelds zu lesen. Wenn der Benutzer jedoch überhaupt nichts in das Textfeld eingegeben hat, aber auf " **Suche** " geklickt hat, können Sie diesen Klick ignorieren, da nichts zu suchen ist.

Der folgende Code ist ein Beispiel, das zeigt, wie diese Bedingungen implementiert werden. (Sie müssen diesen Code noch nicht hinzufügen. Sie müssen dies in einem Moment tun.)

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Der Test wird auf diese Weise aufgegliedert:

- Sie erhalten den Wert `Request.QueryString["searchGenre"]`, nämlich den Wert, der in das `<input>` Element mit dem Namen `searchGenre`eingegeben wurde.
- Mit der `IsEmpty`-Methode ermitteln, ob Sie leer ist. Diese Methode ist die Standardmethode, um zu bestimmen, ob etwas (z. b. ein Formular Element) einen Wert enthält. Aber es ist nur wichtig, wenn es *nicht* leer ist...
- Fügen Sie den `!`-Operator vor dem `IsEmpty` Test hinzu. (Der `!`-Operator bedeutet logisches NOT).

In Klartext übersetzt sich die gesamte `if` Bedingung in Folgendes: *Wenn das searchgenre-Element des Formulars nicht leer ist, dann.* .

Mit diesem Block wird die Phase zum Erstellen einer Abfrage festgelegt, die den Suchbegriff verwendet. Dies erledigen Sie im nächsten Abschnitt.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **Das Anforderungs Objekt.**
> 
> Das `Request`-Objekt enthält alle Informationen, die der Browser an Ihre Anwendung sendet, wenn eine Seite angefordert oder übermittelt wird. Dieses Objekt enthält alle Informationen, die der Benutzer bereitstellt, z. b. Text Feldwerte oder eine hoch zuladende Datei. Es umfasst auch alle möglichen zusätzlichen Informationen, wie z. b. Cookies, Werte in der URL-Abfrage Zeichenfolge (sofern vorhanden), den Dateipfad der Seite, die ausgeführt wird, den Browsertyp, den der Benutzer verwendet, die Liste der Sprachen, die im Browser festgelegt sind. und vieles mehr.
> 
> Das `Request`-Objekt ist *eine Auflistung* (Liste) von Werten. Sie erhalten einen einzelnen Wert aus der Auflistung, indem Sie den Namen angeben:
> 
> `var someValue = Request["name"];`
> 
> Das `Request`-Objekt macht tatsächlich mehrere Teilmengen verfügbar. Beispiel:
> 
> - `Request.Form` liefert Werte aus Elementen innerhalb des übermittelten `<form>` Elements, wenn die Anforderung eine `POST` Anforderung ist.
> - `Request.QueryString` gibt Ihnen lediglich die Werte in der Abfrage Zeichenfolge der URL. (In einer URL wie `http://mysite/myapp/page?searchGenre=action&page=2`ist der `?searchGenre=action&page=2` Abschnitt der URL die Abfrage Zeichenfolge.)
> - `Request.Cookies` Sammlung ermöglicht Ihnen den Zugriff auf Cookies, die der Browser gesendet hat.
> 
> Um einen Wert zu erhalten, den Sie sich im übermittelten Formular befinden, können Sie `Request["name"]`verwenden. Alternativ können Sie die spezifischeren Versionen `Request.Form["name"]` (bei `POST` Anforderungen) oder `Request.QueryString["name"]` (für `GET` Anforderungen) verwenden. Natürlich ist *Name* der Name des zu entzurufenden Elements.
> 
> Der Name des Elements, das Sie erhalten möchten, muss innerhalb der von Ihnen verwendeten Sammlung eindeutig sein. Aus diesem Grund stellt das `Request` Objekt die Teilmengen wie `Request.Form` und `Request.QueryString`bereit. Angenommen, die Seite enthält ein Formular Element mit dem Namen `userName` und enthält *auch* ein Cookie mit dem Namen `userName`. Wenn Sie `Request["userName"]`erhalten, ist es nicht eindeutig, ob Sie den Formular Wert oder das Cookie benötigen. Wenn Sie jedoch `Request.Form["userName"]` oder `Request.Cookie["userName"]`erhalten, werden Sie explizit feststellen, welcher Wert ermittelt werden soll.
> 
> Es empfiehlt sich, bestimmte `Request` zu verwenden und die Teilmenge der zu verwenden, an der Sie interessiert sind, wie z. b. `Request.Form` oder `Request.QueryString`. Bei den einfachen Seiten, die Sie in diesem Tutorial erstellen, macht es wahrscheinlich keinen Unterschied. Wenn Sie jedoch komplexere Seiten erstellen, können Sie mithilfe der expliziten Version `Request.Form` oder `Request.QueryString` Probleme vermeiden, die auftreten können, wenn die Seite ein Formular (oder mehrere Formulare), Cookies, Werte von Abfrage Zeichenfolgen usw. enthält.

## <a name="creating-a-query-by-using-a-search-term"></a>Erstellen einer Abfrage mithilfe eines Suchbegriffs

Nachdem Sie nun wissen, wie Sie den Suchbegriff erhalten, den der Benutzer eingegeben hat, können Sie eine Abfrage erstellen, die diese verwendet. Denken Sie daran, dass Sie eine SQL-Abfrage verwenden, die wie folgt aussieht, um alle Movie-Elemente aus der Datenbank zu erhalten:

`SELECT * FROM Movies`

Um nur bestimmte Filme zu erhalten, müssen Sie eine Abfrage verwenden, die eine `Where`-Klausel enthält. Mit dieser Klausel können Sie eine Bedingung festlegen, nach der Zeilen von der Abfrage zurückgegeben werden. Im Folgenden ein Beispiel:

`SELECT * FROM Movies WHERE Genre = 'Action'`

Das grundlegende Format ist `WHERE column = value`. Sie können andere Operatoren neben nur `=`wie `>` (größer als), `<` (kleiner als), `<>` (nicht gleich), `<=` (kleiner oder gleich) usw. verwenden. Dies hängt davon ab, was Sie suchen.

Wenn Sie sich Fragen, wird die Groß-/Kleinschreibung von SQL-Anweisungen nicht beachtet, &mdash; `SELECT` mit `Select` (oder sogar mit `select`) identisch ist. Viele Schlüsselwörter in einer SQL-Anweisung, wie `SELECT` und `WHERE`, werden jedoch häufig groß geschrieben, um Sie leichter lesbar zu machen.

### <a name="passing-the-search-term-as-a-parameter"></a>Übergeben des Suchbegriffs als Parameter

Das Suchen nach einem bestimmten Genre ist einfach genug (`WHERE Genre = 'Action'`), aber Sie möchten nach einem beliebigen Genre suchen können, das der Benutzer eingibt. Zu diesem Zweck erstellen Sie als SQL-Abfrage, die einen Platzhalter für den zu durchsuchenden Wert enthält. Dieser Befehl sieht wie folgt aus:

`SELECT * FROM Movies WHERE Genre = @0`

Der Platzhalter ist das `@` Zeichen gefolgt von 0 (null). Wie Sie vielleicht vermuten, kann eine Abfrage mehrere Platzhalter enthalten, und Sie würden `@0`, `@1`, `@2`usw. benannt werden.

Wenn Sie die Abfrage einrichten und den Wert tatsächlich übergeben möchten, verwenden Sie den Code wie den folgenden:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Dieser Code ähnelt dem, was Sie bereits zum Anzeigen von Daten im Raster getan haben. Die einzigen Unterschiede sind:

- Die Abfrage enthält einen Platzhalter (`WHERE Genre = @0"`).
- Die Abfrage wird in eine Variable (`selectCommand`) eingefügt. vor haben Sie die Abfrage direkt an die `db.Query`-Methode übermittelt.
- Wenn Sie die `db.Query`-Methode aufzurufen, übergeben Sie sowohl die Abfrage als auch den Wert, der für den Platzhalter verwendet werden soll. (Wenn die Abfrage mehrere Platzhalter enthielt, würden Sie alle als separate Werte an die-Methode übergeben.)

Wenn Sie alle diese Elemente kombinieren, erhalten Sie den folgenden Code:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Wichtig!** Die Verwendung von Platzhaltern (wie `@0`) zum Übergeben von Werten an einen SQL-Befehl ist für die Sicherheit *äußerst wichtig* . Wie Sie hier sehen, ist die einzige Möglichkeit, SQL-Befehle mit Platzhaltern für Variablen Daten zu erstellen.
> 
> Erstellen Sie niemals eine SQL-Anweisung, indem Sie den Literaltext und die Werte, die Sie vom Benutzer erhalten, zusammenfügen (Verketten). Durch das Verketten von Benutzereingaben in eine SQL-Anweisung wird die Website mit einem *SQL-Injection-Angriff* geöffnet, bei dem ein böswilliger Benutzer Werte an Ihre Seite sendet, die Ihre Datenbank hacken. (Weitere Informationen finden Sie im Artikel [SQL Injection](https://msdn.microsoft.com/library/ms161953.aspx) der MSDN-Website.)

## <a name="updating-the-movies-page-with-search-code"></a>Aktualisieren der Seite "Filme" mit suchcode

Nun können Sie den Code in der Datei " *Movies. cshtml* " aktualisieren. Ersetzen Sie zunächst den Code im Codeblock am oberen Rand der Seite durch folgenden Code:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

Der Unterschied besteht darin, dass Sie die Abfrage in die `selectCommand` Variable eingefügt haben, die Sie später an `db.Query` übergeben werden. Wenn Sie die SQL-Anweisung in eine Variable einfügen, können Sie die Anweisung ändern, was Sie tun, um die Suche auszuführen.

Sie haben auch diese beiden Zeilen entfernt, die Sie später wieder hinzufügen:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Sie möchten die Abfrage noch nicht ausführen (d. h., `db.Query`), und Sie möchten die `WebGrid`-Hilfsobjekt noch nicht initialisieren. Sie müssen diese Schritte ausführen, nachdem Sie ermittelt haben, welche SQL-Anweisung ausgeführt werden muss.

Nach diesem umgeschriebenen Block können Sie die neue Logik zum Behandeln der Suche hinzufügen. Der abgeschlossene Code sieht wie folgt aus. Aktualisieren Sie den Code auf der Seite, damit er mit diesem Beispiel übereinstimmt:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

Die Seite funktioniert nun wie folgt. Jedes Mal, wenn die Seite ausgeführt wird, öffnet der Code die Datenbank, und die `selectCommand` Variable wird auf die SQL-Anweisung festgelegt, mit der alle Datensätze aus der `Movies` Tabelle abgerufen werden. Der Code initialisiert außerdem die `searchTerm` Variable.

Wenn die aktuelle Anforderung jedoch einen Wert für das `searchGenre`-Element enthält, legt der Code `selectCommand` auf eine andere Abfrage fest – d. auf eine, die die `Where`-Klausel zum Suchen nach einem Genre enthält. Außerdem wird `searchTerm` auf den Wert festgelegt, der für das Suchfeld (möglicherweise Nothing) weitergegeben wurde.

Unabhängig davon, welche SQL-Anweisung in `selectCommand`ist, ruft der Code `db.Query` auf, um die Abfrage auszuführen, und übergibt sie an `searchTerm`. Wenn `searchTerm`ist, ist es unerheblich, denn in diesem Fall gibt es keinen Parameter, um den Wert an `selectCommand` zu übergeben.

Zum Schluss initialisiert der Code das `WebGrid`-Hilfsprogramm, indem die Abfrageergebnisse wie zuvor verwendet werden.

Sie können sehen, dass Sie, indem Sie die SQL-Anweisung und den Suchbegriff in Variablen einfügen, dem Codeflexibilität hinzugefügt haben. Wie Sie später in diesem Tutorial sehen werden, können Sie dieses grundlegende Framework verwenden und weiterhin Logik für verschiedene Arten von Such Vorgängen hinzufügen.

## <a name="testing-the-search-by-genre-feature"></a>Testen der Funktion für die Suche nach Genre

Führen Sie in webmatrix die Seite *Movies. cshtml* aus. Die Seite mit dem Textfeld für Genre wird angezeigt.

Geben Sie ein Genre ein, das Sie für einen ihrer Testdatensätze eingegeben haben, und klicken Sie dann auf **Suchen**. Dieses Mal wird eine Liste der Filme angezeigt, die mit diesem Genre identisch sind:

![Liste der Filme nach dem Suchen nach Genre "comeals"](form-basics/_static/image4.png)

Geben Sie ein anderes Genre ein, und suchen Sie nach. Versuchen Sie, das Genre in Kleinbuchstaben oder in Großbuchstaben einzugeben, damit Sie feststellen können, dass bei der Suche die Groß-/Kleinschreibung nicht beachtet wird.

## <a name="remembering-what-the-user-entered"></a>"Erinnern", was der Benutzer eingegeben hat

Sie haben vielleicht bemerkt, dass Sie nach dem Eingeben eines Genres und dem Klicken auf das **suchgenre**eine Auflistung für dieses Genre gesehen haben. Das Suchtextfeld war jedoch leer &mdash; anders ausgedrückt: Es hat nicht vergessen, was Sie eingegeben haben.

Es ist wichtig zu verstehen, warum dieses Verhalten auftritt. Wenn Sie eine Seite übermitteln, sendet der Browser eine Anforderung an den Webserver. Wenn ASP.net die Anforderung erhält, erstellt Sie eine ganz neue Instanz der Seite, führt den Code darin aus und rendert die Seite dann erneut im Browser. Tatsächlich weiß die Seite jedoch nicht, dass Sie gerade mit einer früheren Version von selbst gearbeitet haben. Alles weiß, dass es eine Anforderung mit einigen Formulardaten erhalten hat.

Jedes Mal, wenn Sie eine Seite anfordern &mdash;, ob Sie zum ersten Mal oder übermittelt haben, &mdash; Sie eine neue Seite erhalten. Der Webserver weist keinen Arbeitsspeicher für die letzte Anforderung auf. Weder ASP.net noch den Browser. Die einzige Verbindung zwischen diesen separaten Instanzen der Seite sind alle Daten, die zwischen Ihnen übertragen werden. Wenn Sie z. b. eine Seite übermitteln, kann die neue Seiten Instanz die Formulardaten erhalten, die von der früheren Instanz gesendet wurden. (Eine andere Möglichkeit zum Übergeben von Daten zwischen Seiten ist die Verwendung von Cookies.)

Eine formale Möglichkeit, diese Situation zu beschreiben, besteht darin, zu sagen, dass Webseiten zustandslos *sind.* Webserver und die Seiten selbst und die Elemente auf der Seite behalten keine Informationen über den vorherigen Zustand einer Seite bei. Das Web wurde auf diese Weise entworfen, weil die Wartung des Zustands einzelner Anforderungen schnell die Ressourcen von Webservern erschöpft, die häufig Tausende, vielleicht sogar Hunderttausende von Anforderungen pro Sekunde verarbeiten.

Daher war das Textfeld leer. Nachdem Sie die Seite übermittelt haben, hat ASP.net eine neue Instanz der Seite erstellt und den Code und das Markup ausgeführt. In diesem Code gab es nichts, das ASP.net einen Wert in das Textfeld eingefügt hat. ASP.net hat keine Aktion durchführen, und das Textfeld wurde ohne einen Wert gerendert.

Es gibt eigentlich eine einfache Möglichkeit, dieses Problem zu umgehen. Das Genre, das Sie in das Textfeld eingegeben haben, *steht* Ihnen im Code &mdash; in `Request.QueryString["searchGenre"]`zur Verfügung.

Aktualisieren Sie das Markup für das Textfeld, damit das `value`-Attribut seinen Wert aus `searchTerm`abruft, wie im folgenden Beispiel gezeigt:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Auf dieser Seite können Sie auch das `value`-Attribut auf die `searchTerm` Variable festlegen, da diese Variable auch das Genre enthält, das Sie eingegeben haben. Die Verwendung des `Request` Objekts zum Festlegen des `value` Attributs, wie hier gezeigt, ist die Standardmethode zum Ausführen dieser Aufgabe. (Vorausgesetzt, dass Sie dies in einigen Situationen auch &mdash; möchten, können Sie die Seite *ohne* Werte in den Feldern darstellen. Alles hängt davon ab, was mit Ihrer APP passiert.)

> [!NOTE]
> Sie können den Wert eines Textfelds, das für Kenn Wörter verwendet wird, nicht "merken". Es wäre eine Sicherheitslücke, die es Benutzern ermöglicht, ein Kenn Wort Feld mithilfe von Code auszufüllen.

Führen Sie die Seite erneut aus, geben Sie ein Genre ein, und klicken Sie auf **Genre suchen**. Dieses Mal sehen Sie nicht nur die Ergebnisse der Suche, sondern das Textfeld speichert das letzte Mal, was Sie eingegeben haben:

![Die Seite, die anzeigt, dass das Textfeld den vorherigen Eintrag "gespeichert" hat](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Suchen nach einem Wort im Titel

Sie können jetzt nach beliebigen Genres suchen, aber Sie möchten möglicherweise auch nach einem Titel suchen. Es ist schwierig, bei der Suche genau einen Titel zu erhalten. Sie können also stattdessen nach einem Wort suchen, das an einer beliebigen Stelle innerhalb eines Titels erscheint. Verwenden Sie dazu den `LIKE` Operator und die Syntax wie folgt:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Mit diesem Befehl werden alle Filme abgerufen, deren Titel "Adventure" enthalten. Wenn Sie den `LIKE`-Operator verwenden, schließen Sie das Platzhalter Zeichen `%` in den Suchbegriff ein. Das Such `LIKE 'adventure%'` bedeutet "" beginnend mit "Adventure". (Technisch gesehen bedeutet dies "die Zeichenfolge" Adventure ", gefolgt von etwas.") Ebenso bedeutet der Suchbegriff `LIKE '%adventure'` "alles, gefolgt von der Zeichenfolge" Adventure ". Dies ist eine weitere Möglichkeit," mit "" auf "Adventure" zu beenden.

Der Suchbegriff `LIKE '%adventure%'` daher "mit ' Adventure ' an beliebiger Stelle im Titel". (Technisch gesehen "alles im Titel, gefolgt von" Adventure ", gefolgt von etwas.")

Fügen Sie im `<form>`-Element das folgende Markup direkt unter dem schließenden `</div>`-Tag für die Genre Suche (unmittelbar vor dem schließenden `</form>`-Element) hinzu:

[!code-html[Main](form-basics/samples/sample10.html)]

Der Code für diese Suche ähnelt dem Code für die Genre Suche, mit dem Unterschied, dass Sie die `LIKE` Suche zusammenstellen müssen. Fügen Sie im Codeblock am oberen Rand der Seite diesen `if` Block direkt hinter dem `if`-Block für die Genre Suche ein:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

In diesem Code wird die gleiche Logik verwendet, die Sie zuvor gesehen haben, mit der Ausnahme, dass die Suche einen `LIKE` Operator verwendet und der Code "`%`" vor und nach dem Suchbegriff einfügt.

Beachten Sie, dass es einfach ist, der Seite eine weitere Suche hinzuzufügen. Alles, was Sie tun mussten:

- Erstellen Sie einen `if` Block, der testet, ob das relevante Suchfeld über einen Wert verfügt.
- Legen Sie die `selectCommand` Variable auf eine neue SQL-Anweisung fest.
- Legen Sie die `searchTerm` Variable auf den Wert fest, der an die Abfrage übergeben werden soll.

Hier ist der gesamte Codeblock, der die neue Logik für eine Titelsuche enthält:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Im folgenden finden Sie eine Zusammenfassung der Funktionsweise dieses Codes:

- Die Variablen `searchTerm` und `selectCommand` werden oben initialisiert. Sie legen diese Variablen auf den entsprechenden Suchbegriff (sofern vorhanden) und den entsprechenden SQL-Befehl fest, je nachdem, was der Benutzer auf der Seite bewirkt. Die Standardsuche ist der einfache Fall, alle Filme aus der Datenbank zu erhalten.
- In den Tests für `searchGenre` und `searchTitle`legt der Code `searchTerm` auf den Wert fest, nach dem Sie suchen möchten. Diese Code Blöcke legen auch `selectCommand` auf einen entsprechenden SQL-Befehl für diese Suche fest.
- Die `db.Query`-Methode wird nur einmal aufgerufen. dabei wird der SQL-Befehl verwendet `selectedCommand` und welcher Wert in `searchTerm`ist. Wenn kein Suchbegriff (kein Genre und kein Titel Wort) vorhanden ist, ist der Wert von `searchTerm` eine leere Zeichenfolge. Dies ist jedoch nicht von Bedeutung, da in diesem Fall die Abfrage keinen Parameter erfordert.

## <a name="testing-the-title-search-feature"></a>Testen der Titel Suchfunktion

Nun können Sie Ihre abgeschlossene Suchseite testen. Führen Sie *Movies. cshtml*aus.

Geben Sie ein Genre ein, und klicken Sie auf **Suche Genre** Das Raster zeigt Filme dieses Genres an, wie zuvor.

Geben Sie ein Titelwort ein, und klicken Sie auf **Titel suchen**. Im Raster werden Filme angezeigt, die das Wort im Titel enthalten.

![Liste der Filme nach "The" im Titel suchen](form-basics/_static/image6.png)

Lassen Sie beide Textfelder leer, und klicken Sie auf die Schaltfläche. Im Raster werden alle Filme angezeigt.

## <a name="combining-the-queries"></a>Kombinieren der Abfragen

Sie werden möglicherweise feststellen, dass die Suchvorgänge exklusiv sind. Sie können den Titel und das Genre nicht gleichzeitig durchsuchen, auch wenn beide Suchfelder Werte enthalten. Beispielsweise können Sie nicht nach allen Aktions Filmen suchen, deren Titel "Adventure" enthält. (Da die Seite jetzt codiert ist, erhält die Titelsuche Vorrang, wenn Sie Werte für Genre und Titel eingeben.) Zum Erstellen einer Suche, die die Bedingungen kombiniert, müssten Sie eine SQL-Abfrage erstellen, die eine Syntax wie die folgende hat:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

Und Sie müssen die Abfrage wie folgt ausführen, indem Sie eine Anweisung wie die folgende verwenden:

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Das Erstellen von Logik, um viele Permutationen von Suchkriterien zuzulassen, kann ein wenig an der Stelle stehen, wie Sie sehen können. Daher werden wir hier nicht mehr Unternehmen.

## <a name="coming-up-next"></a>Nächste nächste

Im nächsten Tutorial erstellen Sie eine Seite, die ein Formular verwendet, mit dem Benutzer der Datenbank Filme hinzufügen können.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Vervollständigen der Auflistung für Movie Page (aktualisiert mit Search)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in die ASP.net-Webprogrammierung mit der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL-WHERE-Klausel](http://www.w3schools.com/sql/sql_where.asp) auf der W3Schools-Website
- Artikel zu [Methoden Definitionen](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) auf der W3C-Website

> [!div class="step-by-step"]
> [Zurück](displaying-data.md)
> [Weiter](entering-data.md)
