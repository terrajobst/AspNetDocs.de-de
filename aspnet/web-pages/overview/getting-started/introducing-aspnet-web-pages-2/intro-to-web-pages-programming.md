---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: Einführung in die Grundlagen der ASP.net Web Pages Programmierung | Microsoft-Dokumentation
author: Rick-Anderson
description: 'In diesem Tutorial erhalten Sie eine Übersicht darüber, wie Sie in ASP.net Web Pages mit Razor-Syntax programmieren können. Lernen Sie Folgendes: die grundlegende Razor-Syntax, die Sie für PR verwenden...'
ms.author: riande
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 474de7671ac2931e5ba9ff635d77385403644521
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509979"
---
# <a name="introducing-aspnet-web-pages---programming-basics"></a>Einführung in die Grundlagen der ASP.net Web Pages Programmierung

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Tutorial erhalten Sie eine Übersicht darüber, wie Sie in ASP.net Web Pages mit Razor-Syntax programmieren können.
> 
> Sie lernen Folgendes:
> 
> - Die grundlegende "Razor"-Syntax, die Sie für die Programmierung in ASP.net Web Pages verwenden.
> - Einige Grund C#Legende, das ist die Programmiersprache, die Sie verwenden werden.
> - Einige grundlegende Programmier Konzepte für Webseiten.
> - Installieren von Paketen (Komponenten, die vorkonfigurierte Codes enthalten) für die Verwendung mit dem-Standort.
> - *Verwenden von* Hilfsprogrammen zum Ausführen allgemeiner Programmieraufgaben.
>   
> 
> Erörterte Features und Technologien:
> 
> - Nuget und der Paket-Manager.
> - Das `Gravatar`-Hilfsprogramm.

Dieses Tutorial ist in erster Linie eine Übung in der Einführung in die Programmier Syntax, die Sie für ASP.net Web Pages verwenden. Sie erfahren mehr über *Razor-Syntax* und Code, der in der C# Programmiersprache geschrieben ist. Sie haben im vorherigen Tutorial einen Einblick in diese Syntax erhalten. in diesem Tutorial wird die Syntax ausführlicher erläutert.

Wir versprechen, dass dieses Lernprogramm die meisten Programmierungs Programme enthält, die Sie in einem einzigen Tutorial sehen werden, und dass es sich um das einzige Lernprogramm handelt, das *nur* das Programmieren umfasst. In den verbleibenden Tutorials in diesem Satz werden tatsächlich Seiten erstellt, die interessante Dinge bewirken.

Außerdem *erfahren Sie mehr über Hilfsprogramme*. Ein Hilfsobjekt ist eine Komponente – ein gepacktes Codestück –, das Sie einer Seite hinzufügen können. Das Hilfsprogramm führt Aufgaben für Sie aus, die andernfalls mühsam oder komplex sein könnten.

## <a name="creating-a-page-to-play-with-razor"></a>Erstellen einer Seite zur Wiedergabe mit Razor

In diesem Abschnitt experimentieren Sie mit Razor, damit Sie einen Eindruck von der grundlegenden Syntax erhalten.

Starten Sie webmatrix, wenn Sie nicht bereits ausgeführt wird. Sie verwenden die Website, die Sie im vorherigen Tutorial erstellt haben ([Getting Started with Web Pages](https://go.microsoft.com/fwlink/?LinkId=251578)). Klicken Sie zum erneuten Öffnen auf **Meine Websites** , und wählen Sie **webpagemovies**aus:

![Webmatrix-Startbildschirm mit hervorgehobenen Optionen für geöffnete Websites und meine Websites](intro-to-web-pages-programming/_static/image1.png)

Wählen Sie den Arbeitsbereich **Dateien** aus.

Klicken Sie im Menüband auf **neu** , um eine Seite zu erstellen. Wählen Sie **cshtml** aus, und nennen Sie die neue Seite " *testrazor. cshtml*".

Klicken Sie auf **OK**.

Kopieren Sie Folgendes in die Datei, und ersetzen Sie vollständig, was bereits vorhanden ist.

> [!NOTE]
> Wenn Sie Code oder Markup aus den Beispielen in eine Seite kopieren, sind der Einzug und die Ausrichtung möglicherweise nicht identisch mit denen im Tutorial. Einzug und Ausrichtung haben jedoch keine Auswirkung darauf, wie der Code ausgeführt wird.

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Untersuchen der Beispielseite

Die meisten Elemente, die Sie sehen, sind normales HTML. Im oberen Bereich gibt es jedoch den folgenden Codeblock:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Beachten Sie die folgenden Punkte zu diesem Codeblock:

- Das @-Zeichen teilt ASP.net mit, dass es sich dabei um Razor-Code und nicht um HTML handelt. ASP.NET behandelt alles nach dem @-Zeichen als Code, bis es wieder in HTML-Code ausgeführt wird. (In diesem Fall ist das &lt;! DOCTYPE&gt; Element.
- Die geschweiften Klammern ({und}) schließen einen Block von Razor-Code ein, wenn der Code mehr als eine Zeile enthält. Die geschweiften Klammern Teilen ASP.net, wo der Code für diesen Block beginnt und endet.
- Die//-Zeichen markieren einen Kommentar, d. –. einen Teil des Codes, der nicht ausgeführt wird.
- Jede Anweisung muss mit einem Semikolon (;)) enden. (Keine Kommentare, jedoch.)
- Sie können Werte in *Variablen*speichern, die Sie mit dem Schlüsselwort var erstellen (*deklarieren*). Wenn Sie eine Variable erstellen, geben Sie Ihr einen Namen, der Buchstaben, Ziffern und Unterstriche (\_) enthalten kann. Variablennamen dürfen nicht mit einer Zahl beginnen und können nicht den Namen eines Programmier Schlüsselworts (wie z. b. var) verwenden.
- Zeichen folgen (z. b. "ASP.net" und "Web Pages") werden in Anführungszeichen eingeschlossen. (Sie müssen doppelte Anführungszeichen aufweisen.) Zahlen sind nicht in Anführungszeichen.
- Leerzeichen außerhalb von Anführungszeichen sind unerheblich. Zeilenumbrüche sind meistens nicht wichtig. die Ausnahme besteht darin, dass Sie eine Zeichenfolge nicht in Anführungszeichen aufteilen können. Einzug und Ausrichtung sind nicht von Bedeutung.

In diesem Beispiel ist dies nicht offensichtlich, weil bei allen Codes die Groß-/Kleinschreibung beachtet wird. Dies bedeutet, dass es sich bei der Variablen thesum um eine andere Variable als Variablen handelt, die den Namen thesum oder thesum haben können. Ebenso ist var ein Schlüsselwort, aber var ist nicht.

### <a name="objects-and-properties-and-methods"></a>Objekte und Eigenschaften und Methoden

Dann gibt es den Ausdruck "DateTime. Now". In einfachen Begriffen ist DateTime ein *Objekt*. Ein Objekt ist ein Objekt, mit dem Sie programmieren können – eine Seite, ein Textfeld, eine Datei, ein Bild, eine Webanforderung, eine e-Mail-Nachricht, einen Kundendaten Satz usw. -Objekte verfügen über eine oder mehrere *Eigenschaften* , die ihre Eigenschaften beschreiben. Ein Textfeld-Objekt verfügt über eine Text-Eigenschaft (unter anderem), ein Request-Objekt verfügt über eine URL-Eigenschaft (und andere), eine e-Mail-Nachricht hat eine From-Eigenschaft und eine to-Eigenschaft usw. -Objekte verfügen auch über *Methoden* , die die "Verben" sind, die Sie ausführen können. Sie arbeiten mit-Objekten sehr viel.

Wie Sie im Beispiel sehen können, ist DateTime ein Objekt, mit dem Sie Datumsangaben und Uhrzeiten programmieren können. Sie verfügt über eine Eigenschaft mit dem Namen now, die das aktuelle Datum und die Uhrzeit zurückgibt.

### <a name="using-code-to-render-markup-in-the-page"></a>Verwenden von Code zum Rendering von Markup auf der Seite

Beachten Sie im Text der Seite Folgendes:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Auch hier teilt das @-Zeichen ASP.net mit, dass es sich dabei um Code, nicht um HTML handelt. Im Markup können Sie @ gefolgt von einem Code Ausdruck hinzufügen, und ASP.NET führt den Wert dieses Ausdrucks direkt an diesem Punkt aus. Im Beispiel wird @a den Wert der Variablen mit dem Namen a rendern, @product rendert, was in der Variablen mit dem Namen Product usw. ist.

Sie sind jedoch nicht auf Variablen beschränkt. In einigen Fällen liegt das @-Zeichen vor einem Ausdruck:

- @ (a\*b) rendert das Produkt von dem, was in den Variablen a und b liegt. (Der \*-Operator bedeutet Multiplikation.)
- @ (Technology + "" + Product) rendert die Werte in der Variablen Technologie und im Produkt, nachdem Sie verkettet und ein Leerzeichen dazwischen hinzugefügt wurden. Der Operator (+) zum Verketten von Zeichen folgen ist mit dem Operator zum Hinzufügen von Zahlen identisch. ASP.net kann normalerweise feststellen, ob Sie mit Zahlen oder mit Zeichen folgen arbeiten und mit dem Operator + das richtige tun.
- @Request.Url rendert die URL-Eigenschaft des Request-Objekts. Das Anforderungs Objekt enthält Informationen über die aktuelle Anforderung aus dem Browser, und natürlich enthält die URL-Eigenschaft die URL der aktuellen Anforderung.

Das Beispiel soll Ihnen auch zeigen, dass Sie die Arbeit auf unterschiedliche Weise erledigen können. Sie können im Codeblock am oberen Rand Berechnungen durchführen, die Ergebnisse in eine Variable einfügen und die Variable dann im Markup darstellen. Oder Sie können Berechnungen in einem Ausdruck direkt im Markup ausführen. Welche Vorgehensweise Sie verwenden, hängt davon ab, was Sie tun, und zu einem gewissen Grad.

### <a name="seeing-the-code-in-action"></a>Anzeigen des Codes in Aktion

Klicken Sie mit der rechten Maustaste auf den Namen der Datei, und wählen Sie dann **im Browser starten**aus. Die Seite im Browser wird mit allen auf der Seite aufgelösten Werten und Ausdrücken angezeigt.

![Die Seite "testrazor" wird im Browser ausgeführt.](intro-to-web-pages-programming/_static/image2.png)

Sehen Sie sich die Quelle im Browser an.

!["Razor"-Seitenquelle im Browser](intro-to-web-pages-programming/_static/image3.png)

Wie Sie im vorherigen Tutorial von ihrer Arbeit erwarten, befindet sich kein Razor-Code auf der Seite. Alles, was Sie sehen, sind die tatsächlichen Anzeigewerte. Wenn Sie eine Seite ausführen, stellen Sie tatsächlich eine Anforderung an den Webserver, der in webmatrix integriert ist. Wenn die Anforderung empfangen wird, löst ASP.net alle Werte und Ausdrücke auf und rendert ihre Werte auf der Seite. Die Seite wird dann an den Browser gesendet.

> [!TIP] 
> 
> **Razor undC#**
> 
> Bis jetzt haben wir schon gesagt, dass Sie mit Razor-Syntax arbeiten. Das ist richtig, aber das ist nicht die komplette Story. Die tatsächliche Programmiersprache, die Sie verwenden, *C#* wird aufgerufen. C#wurde vor mehr als zehn Jahren von Microsoft erstellt und wurde zu einer der primären Programmiersprachen zum Erstellen von Windows-apps. Alle Regeln, die Sie gesehen haben, wie Sie eine Variable benennen und Anweisungen erstellen, sind eigentlich alle Regeln der C# Sprache.
> 
> Razor verweist genauer auf den kleinen Satz von Konventionen, wie Sie diesen Code in eine Seite einbetten. Beispielsweise ist die Konvention der Verwendung von @ zum Markieren von Code auf der Seite und zum Verwenden von @ {} zum Einbetten eines Codeblocks der Razor-Aspekt einer Seite. Hilfsprogramme werden auch als Teil von Razor behandelt. Razor-Syntax wird an mehr stellen als nur in ASP.net Web Pages verwendet. (Sie wird z. b. auch in ASP.NET MVC-Ansichten verwendet.)
> 
> Wir erwähnen dies, denn wenn Sie nach Informationen zum Programmieren ASP.net Web Pages suchen, finden Sie viele Verweise auf Razor. Viele dieser Verweise gelten jedoch nicht für das, was Sie tun, und könnten daher verwirrend sein. Und in der Tat werden viele ihrer Programmierungs Fragen in Bezug auf die Arbeit mit C# ASP.net. Wenn Sie also speziell nach Informationen zu Razor suchen, finden Sie möglicherweise nicht die benötigten Antworten.

## <a name="adding-some-conditional-logic"></a>Hinzufügen von bedingter Logik

Eines der großartigen Features bei der Verwendung von Code auf einer Seite besteht darin, dass Sie je nach Bedingungen ändern können, was passiert. In diesem Teil des Tutorials werden Sie einige Möglichkeiten zum Ändern der auf der Seite angezeigten Optionen spielen.

Das Beispiel ist einfach und etwas so konstruiert, dass wir uns auf die bedingte Logik konzentrieren können. Die Seite, die Sie erstellen, wird dies tun:

- Zeigen Sie unterschiedliche Text auf der Seite an, je nachdem, ob die Seite das erste Mal angezeigt wird oder ob Sie auf eine Schaltfläche geklickt haben, um die Seite zu übermitteln. Dies ist der erste bedingte Test.
- Zeigen Sie die Meldung nur an, wenn ein bestimmter Wert in der Abfrage Zeichenfolge der URL (http://...? Show = true) übermittelt wird. Dies ist der zweite bedingte Test.

Erstellen Sie in webmatrix eine Seite, und nennen Sie Sie *TestRazorPart2. cshtml*. (Klicken Sie im Menüband auf **neu**, wählen Sie **cshtml**aus, benennen Sie die Datei, und klicken Sie dann auf **OK**.)

Ersetzen Sie den Inhalt dieser Seite durch Folgendes:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

Der Codeblock am Anfang Initialisiert eine Variable mit dem Namen Message mit Text. Im Text der Seite wird der Inhalt der Nachrichten Variablen in einem &lt;p&gt;-Element angezeigt. Das Markup enthält auch ein &lt;Eingabe&gt;-Element, um eine Schaltfläche " **senden** " zu erstellen.

Führen Sie die Seite aus, um zu sehen, wie Sie jetzt funktioniert. Vorerst handelt es sich im Grunde um eine statische Seite, auch wenn Sie auf die Schaltfläche " **senden** " klicken.

Wechseln Sie zurück zu webmatrix. Fügen Sie im Codeblock den folgenden hervorgehobenen Code hinter der Zeile hinzu, *die die Nachricht* initialisiert:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>Der If {}-Block

Was Sie soeben hinzugefügt haben, war eine if-Bedingung. Im Code weist die if-Bedingung eine Struktur wie die folgende auf:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

Die zu überprüfende Bedingung wird in Klammern angegeben. Es muss sich um einen Wert oder einen Ausdruck handeln, der "true" oder "false" zurückgibt. Wenn die Bedingung true ist, führt ASP.net die Anweisung bzw. die Anweisungen in den geschweiften Klammern aus. (Diese sind der *Then* -Teil der *if-then-* Logik.) Wenn die Bedingung false ist, wird der Codeblock übersprungen.

Im folgenden sind einige Beispiele für Bedingungen aufgeführt, die Sie in einer if-Anweisung testen können:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Sie können Variablen anhand von Werten oder mit Ausdrücken testen, indem Sie einen *logischen Operator* oder *Vergleichs Operator*verwenden: gleich (= =), größer als (&gt;), kleiner als (&lt;), größer als oder gleich (&gt;=) und kleiner als oder gleich (&lt;=). Der! =-Operator bedeutet, dass nicht gleich – ist, z. b. wenn (a! = 0) bedeutet, dass *ein ungleich 0*(null) ist.

> [!NOTE]
> Stellen Sie sicher, dass der Vergleichs Operator für gleich (= =) nicht mit = übereinstimmt. Der Operator = wird nur zum Zuweisen von Werten verwendet (var a = 2). Wenn Sie diese Operatoren nach oben mischen, erhalten Sie entweder eine Fehlermeldung, oder Sie erhalten einige seltsame Ergebnisse.

Um zu testen, ob etwas true ist, ist die gesamte Syntax if (isdone = = true). Sie können jedoch auch die Verknüpfung if (isdone) verwenden. Wenn kein Vergleichs Operator vorhanden ist, geht ASP.net davon aus, dass Sie auf true testen.

Der der Operator allein bedeutet ein logisches NOT. Beispiel: die Bedingung if (! Ispost) bedeutet, dass *ispost nicht*den Wert true hat.

Sie können Bedingungen mithilfe eines logischen AND-Operators (&amp;&amp; Operator) oder eines logischen OR-Operators (| |) kombinieren. Die letzte der if-Bedingungen in den vorangehenden Beispielen bedeutet beispielsweise, dass *fileprocessingisdone nicht auf true und Display Sample auf false*festgelegt ist.

### <a name="the-else-block"></a>Der Else-Block

Ein letztes Ergebnis von if-Blöcken: ein If-Block kann von einem Else-Block gefolgt werden. Ein Else-Block ist nützlich, wenn Sie einen anderen Code ausführen müssen, wenn die Bedingung "false" ist. Hier ist ein einfaches Beispiel:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

In späteren Tutorials in dieser Reihe werden einige Beispiele angezeigt, in denen die Verwendung eines Else-Blocks nützlich ist.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Testen, ob es sich bei der Anforderung um eine Übermittlung handelt (Post)

Es gibt noch mehr, aber wir kehren zum Beispiel zurück, das die Bedingung "if (ispost) {...}" hat. Ispost ist tatsächlich eine Eigenschaft der aktuellen Seite. Wenn die Seite das erste Mal angefordert wird, gibt ispost false zurück. Wenn Sie jedoch auf eine Schaltfläche klicken oder die Seite auf andere Weise übermitteln – d. h., Sie veröffentlichen Sie, – ispost gibt true zurück. Mit ispost können Sie also ermitteln, ob Sie mit einer Formular Übermittlung arbeiten. (Im Hinblick auf HTTP-Verben gibt ispost false zurück, wenn die Anforderung ein Get-Vorgang ist. Wenn es sich bei der Anforderung um einen Post-Vorgang handelt, gibt ispost true zurück.) In einem späteren Tutorial arbeiten Sie mit Eingabe Formularen, bei denen dieser Test besonders nützlich ist.

Führen Sie die Seite aus. Da dies das erste Mal ist, dass Sie die Seite angefordert haben, sehen Sie "Dies ist das erste Mal, dass Sie die Seite angefordert haben". Diese Zeichenfolge ist der Wert, mit dem Sie die Nachrichten Variable initialisiert haben. Es gibt einen if-Test (ispost), der jedoch im Moment false zurückgibt, sodass der Code im If-Block nicht ausgeführt wird.

Klicken Sie auf die Schaltfläche **senden** . Die Seite wird erneut angefordert. Wie zuvor ist die Nachrichten Variable auf "This is the First Time..." festgelegt. Diesmal gibt der Test if (ispost) "true" zurück, sodass der Code im If-Block ausgeführt wird. Der Code ändert den Wert der Message-Variablen in einen anderen Wert, der im Markup gerendert wird.

Fügen Sie nun eine if-Bedingung im Markup hinzu. Fügen Sie unterhalb des &lt;p&gt;-Elements, das die Schaltfläche **senden** enthält, das folgende Markup hinzu:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Sie fügen Code im Markup hinzu, damit Sie mit @beginnen müssen. Dann gibt es einen if-Test ähnlich dem, den Sie zuvor im Codeblock hinzugefügt haben. In den geschweiften Klammern fügen Sie jedoch normales HTML hinzu – zumindest ist es normal, bis es zu @DateTime.Nowwird. Dies ist ein weiteres wenig Razor-Code. Sie müssen also wieder @ vor dem Code hinzufügen.

Der Punkt hier ist, dass Sie If-Bedingungen sowohl im Codeblock oben als auch im Markup hinzufügen können. Wenn Sie eine if-Bedingung im Text der Seite verwenden, kann es sich bei den Zeilen innerhalb des Blocks um Markup oder Code handeln. In diesem Fall müssen Sie, wenn Sie Markup und Code kombinieren, den Wert "@" verwenden, um das ASP.net zu löschen, wo der Code ist.

Führen Sie die Seite aus, und klicken Sie auf **senden**. Dieses Mal wird nicht nur eine andere Meldung angezeigt, wenn Sie übermittelt werden ("jetzt haben Sie bereits übermittelt..."), es wird jedoch eine neue Meldung angezeigt, in der das Datum und die Uhrzeit aufgeführt sind.

![Die Seite "Test Razor 2" wird im Browser ausgeführt, und der Zeitstempel wird nach dem Senden angezeigt.](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Testen des Werts einer Abfrage Zeichenfolge

Ein weiteren Test. Dieses Mal fügen Sie einen If-Block hinzu, der einen Wert mit dem Namen Show testet, der in der Abfrage Zeichenfolge weitergegeben werden könnte. (Wie folgt: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) Sie ändern die Seite so, dass die Meldung, die Sie angezeigt haben ("Dies ist das erste Mal..." usw.), nur angezeigt wird, wenn der Wert von "Show" auf "true" festgelegt ist.

Fügen Sie am unteren Rand (aber innerhalb) den Codeblock am oberen Rand der Seite hinzu:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

Der gesamte Codeblock sieht nun wie im folgenden Beispiel aus. (Beachten Sie, dass der Einzug beim Kopieren des Codes in die Seite anders aussehen könnte. Dies wirkt sich jedoch nicht darauf aus, wie der Code ausgeführt wird.)

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

Der neue Code im-Block initialisiert eine Variable mit dem Namen ShowMessage mit false. Anschließend wird ein if-Test durchführt, um in der Abfrage Zeichenfolge nach einem Wert zu suchen. Wenn Sie die Seite zum ersten Mal anfordern, wird eine URL wie die folgende angezeigt:

`http://localhost:43097/TestRazorPart2.cshtml`

Der Code bestimmt, ob die URL eine Variable namens "Show" in der Abfrage Zeichenfolge enthält, wie diese Version der URL:

`http://localhost:43097/TestRazorPart2.cshtml`? Show = true

Der Test selbst prüft die QueryString-Eigenschaft des Request-Objekts. Wenn die Abfrage Zeichenfolge ein Element mit dem Namen Show enthält und dieses Element auf true festgelegt ist, wird der If-Block ausgeführt, und die ShowMessage-Variable wird auf true festgelegt.

Hier gibt es einen Trick, wie Sie sehen können. Wie der Name besagt, ist die Abfrage Zeichenfolge eine Zeichenfolge. Sie können jedoch nur auf true und false testen, wenn der Wert, den Sie testen, ein boolescher Wert (true/false) ist. Bevor Sie den Wert der Variablen anzeigen in der Abfrage Zeichenfolge testen können, müssen Sie ihn in einen booleschen Wert konvertieren. Das ist die Methode der asbool-Methode – Sie nimmt eine Zeichenfolge als Eingabe an und konvertiert sie in einen booleschen Wert. Wenn die Zeichenfolge "true" lautet, konvertiert die asbool-Methode diesen Wert eindeutig in "true". Wenn der Wert der Zeichenfolge etwas anderes ist, gibt asbool false zurück.

> [!TIP] 
> 
> **Datentypen und AS ()-Methoden**
> 
> Wir haben bisher nur gesagt, dass Sie beim Erstellen einer Variablen das Schlüsselwort var verwenden. Das ist jedoch nicht die ganze Story. Zum Bearbeiten von Werten – zum Hinzufügen von Zahlen oder Verketten von Zeichen folgen oder zum Vergleichen von Datumsangaben oder zum Testen von true C# /false – muss mit einer entsprechenden internen Darstellung des Werts arbeiten. C#kann *in der Regel* ermitteln, was diese Darstellung sein sollte (d. h. der *Typ* der Daten), je nachdem, was Sie mit den Werten machen. Dies ist jedoch nicht möglich. Wenn dies nicht der Wert ist, müssen Sie dabei helfen, C# explizit anzugeben, wie die Daten darstellen sollen. Die asbool-Methode bewirkt, dass der C# –-Wert besagt, dass der Zeichen folgen Wert "true" oder "false" als boolescher Wert behandelt werden soll. Ähnliche Methoden sind auch vorhanden, um Zeichen folgen als andere Typen darzustellen, wie z. b. asint (als ganze Zahl behandeln), asdatetime (als Datum/Uhrzeit behandeln), asfloat (als Gleit Komma Zahl behandeln) usw. Wenn Sie diese as ()-Methoden verwenden, C# wenn den Zeichen folgen Wert nicht wie angefordert darstellen kann, wird ein Fehler angezeigt.

Entfernen Sie im Markup der Seite dieses Element, oder kommentieren Sie es aus (Dies wird als auskommentiert angezeigt):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Fügen Sie den folgenden Text hinzu, und fügen Sie den folgenden Text hinzu:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

Wenn die ShowMessage-Variable den Wert true hat, wird im if-Test ein &lt;p-&gt; Element mit dem Wert der Nachrichten Variablen gerentet.

### <a name="summary-of-your-conditional-logic"></a>Zusammenfassung Ihrer bedingten Logik

Wenn Sie nicht ganz sicher sind, was Sie gerade getan haben, finden Sie hier eine Zusammenfassung.

- Die Nachrichten Variable wird mit einer Standard Zeichenfolge initialisiert ("Dies ist das erste Mal...").
- Wenn die Seiten Anforderung das Ergebnis eines Submit (Post) ist, wird der Wert der Meldung in "jetzt, was Sie übermittelt haben..." geändert.
- Die ShowMessage-Variable wird mit false initialisiert.
- Wenn die Abfrage Zeichenfolge? Show = true enthält, wird die ShowMessage-Variable auf true festgelegt.
- Wenn ShowMessage im Markup true ist, wird ein &lt;p-&gt; Element gerendert, das den Wert von Message anzeigt. (Wenn ShowMessage den Wert false hat, wird an diesem Punkt im Markup nichts gerendert.)
- Wenn es sich bei der Anforderung um eine Post-Anforderung handelt, wird im Markup ein &lt;p-&gt; Element gerendert, das das Datum und die Uhrzeit anzeigt.

Führen Sie die Seite aus. Es gibt keine Meldung, da "ShowMessage" den Wert "false" hat, sodass der if (ShowMessage)-Test im Markup "false" zurückgibt.

Klicken Sie auf **Submit**(Senden). Das Datum und die Uhrzeit werden angezeigt, aber es wird immer noch keine Meldung angezeigt.

Wechseln Sie in Ihrem Browser zum Feld URL, und fügen Sie am Ende der URL Folgendes hinzu:? Show = true, und drücken Sie dann die EINGABETASTE.

![Seite "Test Razor 2" im Browser mit Abfrage Zeichenfolge](intro-to-web-pages-programming/_static/image5.png)

Die Seite wird erneut angezeigt. (Da Sie die URL geändert haben, handelt es sich hierbei um eine neue Anforderung, nicht um eine übermitteln.) Klicken Sie erneut auf **senden** . Die Meldung wird erneut angezeigt, wie Datum und Uhrzeit.

![Seite "Test Razor 2" nach der Übermittlung, wenn eine Abfrage Zeichenfolge vorhanden ist](intro-to-web-pages-programming/_static/image6.png)

Ändern Sie in der URL? Show = true in? Show = false, und drücken Sie die EINGABETASTE. Senden Sie die Seite erneut. Die Seite geht zurück zum Start – keine Nachricht.

Wie bereits erwähnt, ist die Logik dieses Beispiels ein wenig erfunden. Wenn jedoch auf vielen ihrer Seiten Hochfahren wird, wird mindestens eines der Formulare verwendet, die Sie hier gesehen haben.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Installieren eines Hilfsobjekts (Anzeigen eines Gravatar-Bilds)

Einige Aufgaben, die häufig auf Webseiten ausgeführt werden sollen, erfordern viel Code oder erfordern zusätzliches wissen. Beispiele: Anzeigen eines Diagramms für Daten Einfügen einer Facebook-Schaltfläche "like" auf einer Seite Senden von e-Mails von Ihrer Website Zuschneiden oder Ändern der Größe von Bildern Verwenden von PayPal für Ihre Website. Um dies zu vereinfachen, können *Sie mit ASP.net Web Pages Hilfsprogramme verwenden.* Hilfsprogramme sind Komponenten, die Sie für einen-Standort installieren und mit denen Sie typische Aufgaben durchführen können, indem Sie nur einige wenige Zeilen von Razor-Code verwenden.

In ASP.net Web Pages sind einige hilfshilfen integriert. Viele Hilfsprogramme sind jedoch in Paketen (Add-Ins) verfügbar, die mit dem nuget-Paket-Manager bereitgestellt werden. Mit nuget können Sie ein Paket für die Installation auswählen. Anschließend werden alle Details der Installation behandelt.

In diesem Teil des Tutorials installieren Sie ein Hilfsobjekt, mit dem Sie ein Gravatar-Image ("Global erkanntes Avatar") anzeigen können. Sie lernen zwei Dinge kennen. Eine solche Vorgehensweise ist das Suchen und Installieren eines Hilfsobjekts. Sie erfahren auch, wie ein Hilfsobjekt das Ausführen von etwas vereinfacht, das Sie anderweitig tun müssen, indem Sie viel Code verwenden, den Sie selbst schreiben müssten.

Sie können Ihren eigenen Gravatar auf der gravatar-Website bei [http://www.gravatar.com/](http://www.gravatar.com/)registrieren, aber es ist nicht zwingend notwendig, ein Gravatar-Konto zu erstellen, um diesen Teil des Tutorials auszuführen.

Klicken Sie in webmatrix auf die Schaltfläche **nuget** .

![Dialogfeld "nuget-Katalog" in webmatrix](intro-to-web-pages-programming/_static/image7.png)

Der nuget-Paket-Manager wird gestartet, und die verfügbaren Pakete werden angezeigt. (Nicht alle Pakete sind Hilfsprogramme. einige fügen Funktionen zu webmatrix hinzu, einige sind zusätzliche Vorlagen usw.) Möglicherweise erhalten Sie eine Fehlermeldung zur Versions Inkompatibilität. Sie können diese Fehlermeldung ignorieren, indem Sie auf **OK** klicken und mit diesem Tutorial fortfahren.

![Dialogfeld "nuget-Katalog" in webmatrix](intro-to-web-pages-programming/_static/image8.png)

Geben Sie im Suchfeld "ASP.net Hilfsprogramme" ein. In nuget werden die Pakete angezeigt, die mit den Suchbegriffen identisch sind.

![Nuget-Katalog in webmatrix mit Paketen](intro-to-web-pages-programming/_static/image9.png)

Die ASP.net-webhilfsbibliothek enthält Code, um viele gängige Aufgaben zu vereinfachen, einschließlich der Verwendung von Gravatar-Bildern. Wählen Sie das Paket **ASP.net-webhilfsprogramme** aus, und klicken Sie auf **Installieren** , um das Installationsprogramm zu starten. Wählen Sie **Ja** aus, wenn Sie gefragt werden, ob Sie das Paket installieren möchten, und akzeptieren Sie die Bedingungen, um die Installation abzuschließen.

Das ist alles. Nuget lädt alle Komponenten herunter und installiert Sie, einschließlich aller zusätzlichen Komponenten, die möglicherweise erforderlich sind (*Abhängigkeiten*).

Wenn Sie aus irgendeinem Grund ein Hilfsobjekt deinstallieren müssen, ist der Prozess sehr ähnlich. Klicken Sie auf die Schaltfläche **nuget** , klicken Sie auf die Registerkarte **installiert** , und wählen Sie das zu deinstallierende Paket aus.

## <a name="using-a-helper-in-a-page"></a>Verwenden eines Hilfsobjekts in einer Seite

Nun verwenden Sie das Hilfsprogramm, das Sie soeben installiert haben. Der Prozess zum Hinzufügen eines Hilfsprogramms zu einer Seite ist für die meisten Hilfsprogramme ähnlich.

Erstellen Sie in webmatrix eine Seite, und nennen Sie Sie *gravatartest. cshml*. (Sie erstellen eine spezielle Seite, um das Hilfsprogramm zu testen, aber Sie können Hilfsprogramme auf jeder Seite der Website verwenden.)

Fügen Sie innerhalb des &lt;Body&gt;-Elements ein &lt;div&gt;-Element hinzu. Geben Sie im &lt;div&gt;-Element Folgendes ein:

[https://login.microsoftonline.com/consumers/](@Gravatar).

Das @-Zeichen ist dasselbe Zeichen, das Sie zum Markieren von Razor-Code verwendet haben. **Gravatar** ist das Hilfsobjekt, mit dem Sie arbeiten.

Sobald Sie den Zeitraum (.) eingeben, zeigt webmatrix eine Liste der *Methoden* (Funktionen) an, die das Gravatar-Hilfsprogramm zur Verfügung stellt:

![IntelliSense-Dropdown Liste für Gravatar-Hilfsobjekt](intro-to-web-pages-programming/_static/image10.png)

Diese Funktion wird als *IntelliSense*bezeichnet. Sie hilft Ihnen beim Programmieren, indem Kontext geeignete Optionen bereitgestellt werden. IntelliSense funktioniert mit HTML, CSS, ASP.NET-Code, JavaScript und anderen Sprachen, die in webmatrix unterstützt werden. Es ist ein weiteres Feature, das die Entwicklung von Webseiten in webmatrix vereinfacht.

Drücken Sie auf der Tastatur die Taste G, und Sie sehen, dass IntelliSense die getHTML-Methode findet. Drücken Sie die Tab-Taste. IntelliSense fügt die ausgewählte Methode (getHTML) für Sie ein. Geben Sie eine öffnende Klammer ein, und beachten Sie, dass die schließende Klammer automatisch hinzugefügt wird. Geben Sie Ihre e-Mail-Adresse in Anführungszeichen zwischen den beiden Klammern ein. Wenn Sie über ein Gravatar-Konto verfügen, wird das Profilbild zurückgegeben. Wenn Sie nicht über ein Gravatar-Konto verfügen, wird ein Standard Image zurückgegeben. Wenn Sie fertig sind, sieht die Zeile wie folgt aus:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Sehen Sie sich nun die Seite in einem Browser an. Abhängig davon, ob Sie über ein Gravatar-Konto verfügen, wird entweder Ihr Bild oder das Standardbild angezeigt.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![Standardbild](intro-to-web-pages-programming/_static/image12.png)

Um eine Vorstellung davon zu erhalten, was das Hilfsobjekt für Sie macht, zeigen Sie die Quelle der Seite im Browser an. Zusammen mit dem HTML-Code, den Sie auf der Seite hatten, sehen Sie ein Image-Element, das einen Bezeichner enthält. Dies ist der Code, den das Hilfsprogramm an der Stelle an der Stelle gerendert hat, an der @Gravatar.GetHtml. Das Hilfsprogramm hat die von Ihnen bereitgestellten Informationen verarbeitet und den Code generiert, der direkt mit Gravatar kommuniziert, um das richtige Image für das angegebene Konto zu erhalten.

Die getHTML-Methode ermöglicht auch das Anpassen des Bilds, indem es andere Parameter bereitstellt. Der folgende Code zeigt, wie Sie ein Bild mit einer Breite und Höhe von 40 Pixel anfordern, und verwendet ein angegebenes Standard Image mit dem Namen **wavatar** , wenn das angegebene Konto nicht vorhanden ist.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Dieser Code erzeugt etwa das folgende Ergebnis (das Standardbild wird nach dem Zufallsprinzip variieren).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Nächste nächste

Um dieses Tutorial kurz zu halten, mussten wir uns nur auf einige Grundlagen konzentrieren. Natürlich gibt es noch *viel* mehr zu Razor und C#. Weitere Informationen finden Sie in diesen Tutorials. Wenn Sie mehr über die Programmier Aspekte von Razor erfahren möchten, finden Sie C# hier eine ausführlichere Einführung: [Einführung in die ASP.net-Webprogrammierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890).

Im nächsten Tutorial finden Sie eine Einführung in die Arbeit mit einer-Datenbank. In diesem Tutorial beginnen Sie mit dem Erstellen der Beispielanwendung, mit der Sie Ihre bevorzugten Filme auflisten können.

## <a name="complete-listing-for-testrazor-page"></a>Vervollständigen der Liste für die testrazor-Seite

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Vervollständigen der Liste für die TestRazorPart2-Seite

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Vervollständigen der Auflistung für die gravatartest-Seite

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in die ASP.net-Webprogrammierung mit der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Twitter-Hilfsprogramm](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Zurück](getting-started.md)
> [Weiter](displaying-data.md)
