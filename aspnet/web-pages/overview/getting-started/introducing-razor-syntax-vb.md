---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Einführung in ASP.net-Webprogrammierung mit der Razor-Syntax (Visual Basic) | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieser Anhang enthält eine Übersicht über die Programmierung mit ASP.net Web Pages in Visual Basic mithilfe der Razor-Syntax.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 2be57655b8c9b76b94e1d9a7ae5fbee27545a0a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422661"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Einführung in ASP.net-Webprogrammierung mithilfe der Razor-Syntax (Visual Basic)

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel erhalten Sie einen Überblick über die Programmierung mit ASP.net Web Pages mithilfe der Razor-Syntax und Visual Basic. ASP.net ist die Technologie von Microsoft zum Ausführen dynamischer Webseiten auf Webservern.
> 
> **Lernen Sie**Folgendes:
> 
> - Die ersten 8 Programmiertipps für den Einstieg in die Programmierung ASP.net Web Pages mithilfe Razor-Syntax.
> - Grundlegende Programmier Konzepte, die Sie benötigen.
> - Was ist ASP.net Servercode und der Razor-Syntax.
>   
> 
> ## <a name="software-versions"></a>Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 3
>   
> 
> Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.

In den meisten Beispielen wird ASP.net Web Pages mit Razor-Syntax C#Verwendung von verwendet. Die Razor-Syntax unterstützt jedoch auch Visual Basic. Um eine ASP.NET-Webseite in Visual Basic zu programmieren, erstellen Sie eine Webseite mit der Dateinamenerweiterung " *. vbhtml* ", und fügen Sie dann Visual Basic Code hinzu. In diesem Artikel erhalten Sie einen Überblick über die Verwendung der Visual Basic Sprache und Syntax zum Erstellen von ASP.NET-Webseiten.

> [!NOTE]
> Die Standard Website Vorlagen für Microsoft webmatrix (**Bäckerei**, **Fotogalerie**und **Starter Site**usw.) sind in C# und Visual Basic Versionen verfügbar. Sie können die Visual Basic Vorlagen von als nuget-Pakete installieren. Website Vorlagen werden im Stamm Ordner Ihrer Website in einem Ordner namens *Microsoft-Vorlagen*installiert.

## <a name="the-top-8-programming-tips"></a>Die Top 8-Programmiertipps

Dieser Abschnitt enthält einige Tipps, die Sie unbedingt kennen müssen, wenn Sie mit dem Schreiben von ASP.net-Servercode mit dem Razor-Syntax beginnen.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Sie fügen Code zu einer Seite mit dem @-Zeichen hinzu.

Das `@` Zeichen startet Inline Ausdrücke, Blöcke mit einer einzelnen Anweisung und Blöcke mit mehreren Anweisungen:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Das in einem Browser angezeigte Ergebnis:

![Razor-Bild1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **HTML-Codierung**
> 
> Wenn Sie Inhalt in einer Seite mit dem `@` Zeichen anzeigen, wie in den vorherigen Beispielen, codiert ASP.net die Ausgabe. Dies ersetzt reservierte HTML-Zeichen (z. b. `<` und `>` und `&`) durch Codes, die es ermöglichen, dass die Zeichen als Zeichen in einer Webseite angezeigt werden, anstatt als HTML-Tags oder-Entitäten interpretiert zu werden. Ohne HTML-Codierung wird die Ausgabe des Server Codes möglicherweise nicht ordnungsgemäß angezeigt und kann eine Seite für Sicherheitsrisiken verfügbar machen.
> 
> Wenn das Ziel darin besteht, HTML-Markup auszugeben, das Tags als Markup rendert (z. b. `<p></p>` für einen Absatz oder `<em></em>`, um Text hervorzuheben), lesen Sie den Abschnitt [Kombinieren von Text, Markup und Code in Code Blöcken](#BM_CombiningTextMarkupAndCode) weiter unten in diesem Artikel.
> 
> Weitere Informationen zur HTML-Codierung finden Sie unter [Arbeiten mit HTML-Formularen auf ASP.net Web Pages Websites](https://go.microsoft.com/fwlink/?LinkId=202892).

### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Sie schließen Code Blöcke mit Code ein... Endcode

Ein Codeblock enthält mindestens eine Code Anweisung und ist in den Schlüsselwörtern `Code` und `End Code`eingeschlossen. Platzieren Sie das öffnende `Code`-Schlüsselwort direkt &#8212; nach dem `@` Zeichen, da zwischen Ihnen kein Leerraum vorhanden sein darf.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Das in einem Browser angezeigte Ergebnis:

![Razor-Bild2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. innerhalb eines-Blocks beenden Sie jede Code Anweisung mit einem Zeilenumbruch.

In einem Visual Basic Codeblock endet jede Anweisung mit einem Zeilenumbruch. (Später in diesem Artikel wird eine Möglichkeit angezeigt, eine lange Code Anweisung bei Bedarf in mehrere Zeilen zu umschließen.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. Sie verwenden Variablen zum Speichern von Werten.

Sie können Werte in einer *Variablen*speichern, einschließlich Zeichen folgen, Zahlen und Datumsangaben usw. Sie erstellen eine neue Variable mit dem `Dim`-Schlüsselwort. Sie können Variablen Werte direkt in eine Seite einfügen, indem Sie `@`verwenden.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Das in einem Browser angezeigte Ergebnis:

![Razor-Bild3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Sie schließen Literalzeichenfolgen-Werte in doppelte Anführungszeichen ein.

Eine *Zeichen* Folge ist eine Sequenz von Zeichen, die als Text behandelt werden. Um eine Zeichenfolge anzugeben, müssen Sie Sie in doppelte Anführungszeichen einschließen:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Wenn Sie doppelte Anführungszeichen in einen Zeichen folgen Wert einbetten möchten, fügen Sie zwei doppelte Anführungszeichen ein. Wenn das doppelte Anführungszeichen einmal in der Seiten Ausgabe angezeigt werden soll, geben Sie es als `""` innerhalb der Zeichenfolge in Anführungszeichen ein, und `""""` geben Sie das doppelte Anführungszeichen in der Zeichenfolge in Anführungszeichen ein, wenn es zweimal angezeigt werden soll.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Das in einem Browser angezeigte Ergebnis:

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Visual Basic Code berücksichtigt nicht die Groß-/Kleinschreibung

Beim Visual Basic Sprache wird die Groß-/Kleinschreibung nicht beachtet Programmier Schlüsselwörter (wie z. b. `Dim`, `If`und `True`) und Variablennamen (wie `myString`oder `subTotal`) können in jedem Fall geschrieben werden.

Die folgenden Codezeilen weisen der Variablen einen Wert `lastname` mit einem Kleinbuchstaben zu und geben dann den Variablen Wert mithilfe eines Großbuchstaben an die Seite aus.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Das in einem Browser angezeigte Ergebnis:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. ein Großteil ihrer Codierung umfasst das Arbeiten mit Objekten.

Ein-Objekt stellt eine Aufgabe dar, mit &#8212; der Sie eine Seite, ein Textfeld, eine Datei, ein Bild, eine Webanforderung, eine e-Mail-Nachricht, einen Kundendaten Satz (Datenbankzeile) usw. programmieren können. Objekte verfügen über Eigenschaften, die Ihre &#8212; Merkmale beschreiben: ein Textfeld-Objekt verfügt über eine `Text`-Eigenschaft, ein Request-Objekt verfügt über eine `Url`-Eigenschaft, eine e-Mail hat eine `From`-Eigenschaft, und ein Customer-Objekt verfügt über eine `FirstName`-Eigenschaft. -Objekte verfügen auch über Methoden, die die &quot;Verben&quot; können, die Sie ausführen können. Beispiele hierfür sind die `Save`-Methode eines File-Objekts, die `Rotate`-Methode eines Bildobjekts und die `Send`-Methode eines e-Mail-Objekts.

Häufig arbeiten Sie mit dem `Request` Objekt, das Ihnen Informationen wie die Werte der Formularfelder auf der Seite (Textfelder usw.), den Typ des Browsers, der die Anforderung erteilt hat, die URL der Seite, die Benutzeridentität usw. Dieses Beispiel zeigt, wie Sie auf Eigenschaften des `Request` Objekts zugreifen und wie Sie die `MapPath`-Methode des `Request`-Objekts aufrufen, das Ihnen den absoluten Pfad der Seite auf dem Server liefert:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Das in einem Browser angezeigte Ergebnis:

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Sie können Code schreiben, der Entscheidungen trifft.

Ein wichtiges Feature von dynamischen Webseiten ist, dass Sie anhand der Bedingungen ermitteln können, was zu tun ist. Die gängigste Methode hierfür ist die `If`-Anweisung (und optional `Else`-Anweisung).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

Die-Anweisung `If IsPost` ist eine Kurzform zum Schreiben von `If IsPost = True`. Zusammen mit `If`-Anweisungen gibt es eine Vielzahl von Möglichkeiten, Bedingungen zu testen, Code Blöcke zu wiederholen usw., die weiter unten in diesem Artikel beschrieben werden.

Das in einem Browser angezeigte Ergebnis (nach dem Klicken auf " **senden**"):

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP Get-und Post-Methoden und die ispost-Eigenschaft**
> 
> Das Protokoll, das für Webseiten (http) verwendet wird, unterstützt eine sehr begrenzte Anzahl von Methoden (&quot;Verben&quot;), die verwendet werden, um Anforderungen an den Server zu senden. Die beiden häufigsten sind Get, das zum Lesen einer Seite verwendet wird, und Post, der zum Senden einer Seite verwendet wird. Wenn ein Benutzer zum ersten Mal eine Seite anfordert, wird die Seite mithilfe von Get angefordert. Wenn der Benutzer ein Formular ausfüllt und dann auf **senden**klickt, sendet der Browser eine Post-Anforderung an den Server.
> 
> Bei der Webprogrammierung ist es häufig hilfreich zu wissen, ob eine Seite als Get-oder Post-Vorgang angefordert wird, damit Sie wissen, wie die Seite verarbeitet wird. In ASP.net Web Pages können Sie die `IsPost`-Eigenschaft verwenden, um festzustellen, ob eine Anforderung ein Get-oder Post-Eintrag ist. Wenn es sich bei der Anforderung um einen Post-Vorgang handelt, wird die `IsPost`-Eigenschaft "true" zurückgegeben. Sie können z. b. die Werte von Textfeldern in einem Formular lesen. Viele Beispiele zeigen Ihnen, wie Sie die Seite je nach dem Wert `IsPost`anders verarbeiten können.

## <a name="a-simple-code-example"></a>Einfaches Code Beispiel

In diesem Verfahren wird gezeigt, wie Sie eine Seite erstellen, die grundlegende Programmiertechniken veranschaulicht. In diesem Beispiel erstellen Sie eine Seite, mit der Benutzer zwei Zahlen eingeben können. Anschließend werden Sie hinzugefügt, und das Ergebnis wird angezeigt.

1. Erstellen Sie im Editor eine neue Datei, und nennen Sie Sie *AddNumbers. vbhtml*.
2. Kopieren Sie den folgenden Code und das Markup in die Seite, und ersetzen Sie alles, was bereits auf der Seite ist.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Beachten Sie Folgendes:

    - Mit dem `@` Zeichen wird der erste Codeblock auf der Seite gestartet, und vor der `totalMessage` Variablen, die im unteren Bereich eingebettet ist.
    - Der Block am oberen Rand der Seite wird in `Code...End Code`eingeschlossen.
    - In den Variablen `total`, `num1`, `num2`und `totalMessage` werden mehrere Zahlen und eine Zeichenfolge gespeichert.
    - Der Literalzeichenfolgen-Wert, der der `totalMessage` Variablen zugewiesen ist, ist in doppelten Anführungszeichen.
    - Da bei Visual Basic Code die Groß-/Kleinschreibung nicht beachtet wird, muss der Name, wenn die `totalMessage` Variable im unteren Bereich der Seite verwendet wird, nur mit der Schreibweise der Variablen Deklaration am oberen Rand der Seite identisch sein. Die Schreibweise spielt keine Rolle.
    - Der Ausdruck `num1.AsInt()` + `num2.AsInt()` zeigt, wie Sie mit Objekten und Methoden arbeiten. Die `AsInt`-Methode für jede Variable konvertiert die von einem Benutzer eingegebene Zeichenfolge in eine ganze Zahl (eine Ganzzahl), die hinzugefügt werden kann.
    - Das `<form>`-Tag enthält ein `method="post"` Attribut. Dies gibt an, dass die Seite mithilfe der HTTP Post-Methode an den Server gesendet wird, wenn der Benutzer auf **Hinzufügen**klickt. Wenn die Seite übermittelt wird, wird der Code `If IsPost` als true ausgewertet, und der bedingte Code wird ausgeführt, und das Ergebnis des Hinzufügens der Zahlen wird angezeigt.
3. Speichern Sie die Seite, und führen Sie Sie in einem Browser aus. (Stellen Sie sicher, dass die Seite im Arbeitsbereich " **Dateien** " ausgewählt ist, bevor Sie Sie ausführen.) Geben Sie zwei ganze Zahlen ein, und klicken Sie dann auf **Hinzufügen** .

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Visual Basic Sprache und Syntax

Früher haben Sie ein einfaches Beispiel für das Erstellen einer ASP.NET-Webseite und die Vorgehensweise zum Hinzufügen von Servercode zu HTML-Markup gesehen. Hier lernen Sie die Grundlagen der Verwendung von Visual Basic zum Schreiben von ASP.net-Servercode &#8212; mithilfe des Razor-Syntax der Programmiersprachen Regeln kennen.

Wenn Sie Erfahrung mit der Programmierung haben (insbesondere, wenn Sie C, C++, C#, Visual Basic oder JavaScript verwendet haben), werden viele der hier gelesenen Elemente Ihnen vertraut sein. Sie müssen sich wahrscheinlich nur damit vertraut machen, wie webmatrix-Code Markup in *vbhtml* -Dateien hinzugefügt wird.

### <a id="BM_CombiningTextMarkupAndCode"></a>Kombinieren von Text, Markup und Code in Code Blöcken

In Servercode Blöcken möchten Sie häufig Text und Markup an die Seite ausgeben. Wenn ein Server Codeblock Text enthält, der nicht Code ist und stattdessen unverändert gerendert werden soll, muss ASP.net in der Lage sein, diesen Text vom Code zu unterscheiden. Hierfür gibt es mehrere Möglichkeiten.

- Schließen Sie den Text in ein HTML-Block Element wie `<p></p>` oder `<em></em>`ein:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    Das HTML-Element kann Text, zusätzliche HTML-Elemente und Servercode Ausdrücke enthalten. Wenn ASP.NET das öffnende HTML-Tag sieht (z. b. `<p>`), rendert das Element und dessen Inhalt unverändert im Browser (und löst die Servercode Ausdrücke auf).

- Verwenden Sie den `@:`-Operator oder das `<text>`-Element. Der `@:` gibt eine einzelne Zeile mit Inhalt aus, die nur-Text oder nicht übereinstimmende HTML-Tags enthält Das `<text>` Element schließt mehrere Zeilen ein, die ausgegeben werden sollen. Diese Optionen sind hilfreich, wenn Sie ein HTML-Element nicht als Teil der Ausgabe renderingrensten möchten.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    Im folgenden Beispiel wird das vorherige Beispiel wiederholt, aber es wird ein einzelnes Paar `<text>` Tags verwendet, um den zu Rendering enden Text einzuschließen.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    Im folgenden Beispiel schließen die Tags "`<text>`" und "`</text>`" drei Zeilen ein, die alle über einen nicht enthaltenen Text und nicht übereinstimmende HTML-Tags (`<br />`) sowie Servercode und übereinstimmende HTML-Tags verfügen. Auch hier kann jede Zeile einzeln mit dem `@:`-Operator vorangestellt werden. Beides funktioniert.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Wenn Sie Text wie in diesem Abschnitt &#8212; gezeigt mit einem HTML--Element ausgeben, wird der `@:`-Operator oder &#8212; das `<text>` Element ASP.net die Ausgabe nicht in HTML-codiert. (Wie bereits erwähnt, codiert ASP.net die Ausgabe von Servercode Ausdrücken und Servercode Blöcken, denen `@`vorangestellt ist, mit Ausnahme der in diesem Abschnitt notierten Sonderfälle.)

### <a name="whitespace"></a>Whitespace

Zusätzliche Leerzeichen in einer-Anweisung (und außerhalb eines Zeichenfolgenliterals) haben keine Auswirkung auf die Anweisung:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Unterbrechen von langen Anweisungen in mehrere Zeilen

Sie können eine lange Code Anweisung in mehrere Zeilen unterbrechen, indem Sie nach jeder Codezeile den Unterstrich `_` (der in Visual Basic als *Fortsetzungs Zeichen*bezeichnet wird) verwenden. Fügen Sie am Ende der Zeile ein Leerzeichen und dann das Fortsetzungs Zeichen hinzu, um eine Anweisung in der nächsten Zeile zu unterbrechen. Setzen Sie die-Anweisung in der nächsten Zeile fort. Sie können-Anweisungen so viele Zeilen wie erforderlich umschließen, um die Lesbarkeit zu verbessern. Die folgenden Anweisungen sind identisch:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Sie können jedoch keine Linie in der Mitte eines Zeichenfolgenliterals umschließen. Das folgende Beispiel funktioniert nicht:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Um eine lange Zeichenfolge zu kombinieren, die mit mehreren Zeilen wie dem obigen Code umschlossen ist, müssen Sie den *Verkettungs Operator* (`&`) verwenden, den Sie später in diesem Artikel sehen werden.

### <a name="code-comments"></a>Code Kommentare

Mit Kommentaren können Sie Notizen für sich selbst oder für andere Personen hinterlassen. Razor-Syntax Kommentaren wird `@*` vorangestellt und mit `*@`enden.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

Innerhalb von Code Blöcken können Sie die Razor-Syntax Kommentare verwenden, oder Sie können normale Visual Basic Kommentarzeichen verwenden, wobei es sich um ein einfaches Anführungszeichen (`'`) handelt, das jeder Zeile vorangestellt ist.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Variables

Eine Variable ist ein benanntes Objekt, das Sie zum Speichern von Daten verwenden. Sie können Variablen einen beliebigen Namen benennen, aber der Name muss mit einem alphabetischen Zeichen beginnen und darf keine Leerzeichen oder reservierten Zeichen enthalten. Wie Sie bereits gesehen haben, ist die Groß-/Kleinschreibung der Buchstaben in einem Variablennamen in Visual Basic unerheblich.

### <a name="variables-and-data-types"></a>Variablen und Datentypen

Eine Variable kann einen bestimmten Datentyp aufweisen, der angibt, welche Art von Daten in der Variablen gespeichert ist. Sie können Zeichen folgen Variablen haben, die Zeichen folgen Werte (wie &quot;Hello World&quot;) speichern, ganzzahlige Variablen, die ganzzahlige Werte (z. b. 3 oder 79) speichern, und Datums Variablen, die Datumswerte in einer Vielzahl von Formaten (z. b. 4/12/2012 oder März 2009) speichern. Und es gibt viele weitere Datentypen, die Sie verwenden können.

Allerdings müssen Sie keinen Typ für eine Variable angeben. In den meisten Fällen kann ASP.NET den Typ basierend auf der Verwendung der Daten in der Variablen ermitteln. (Gelegentlich müssen Sie einen Typ angeben. es werden Beispiele angezeigt, wenn dies zutrifft.)

Verwenden Sie `Dim` plus den Variablennamen (z. `Dim myVar`.), um eine Variable ohne Angabe eines Typs zu deklarieren. Um eine Variable mit einem Typ zu deklarieren, verwenden Sie `Dim` plus den Variablennamen, gefolgt von `As` und dann den Typnamen (z.b. `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

Das folgende Beispiel zeigt einige Inline Ausdrücke, die die Variablen in einer Webseite verwenden.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Das in einem Browser angezeigte Ergebnis:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Wandeln und Testen von Datentypen

Obwohl ASP.net normalerweise einen Datentyp automatisch ermitteln kann, kann dies manchmal nicht der Fall sein. Daher müssen Sie möglicherweise ASP.net durch eine explizite Konvertierung unterstützen. Auch wenn Sie keine Typen konvertieren müssen, ist es manchmal hilfreich, zu testen, um festzustellen, mit welchem Datentyp Sie möglicherweise arbeiten.

Der häufigste Fall ist, dass Sie eine Zeichenfolge in einen anderen Typ konvertieren müssen, z. b. in eine ganze Zahl oder ein Datum. Das folgende Beispiel zeigt einen typischen Fall, in dem Sie eine Zeichenfolge in eine Zahl konvertieren müssen.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Als Regel werden Benutzereingaben als Zeichen folgen angezeigt. Auch wenn Sie den Benutzer aufgefordert haben, eine Zahl einzugeben, und auch wenn er eine Ziffer eingegeben hat, wenn die Benutzereingaben gesendet werden und Sie Sie im Code lesen, werden die Daten im Zeichen folgen Format angezeigt. Daher müssen Sie die Zeichenfolge in eine Zahl konvertieren. Wenn Sie in diesem Beispiel versuchen, arithmetische Werte für die Werte auszuführen, ohne Sie zu wandeln, wird die folgende Fehlermeldung ausgegeben, da ASP.net nicht zwei Zeichen folgen hinzufügen kann:

`Cannot implicitly convert type 'string' to 'int'.`

Um die Werte in ganze Zahlen zu konvertieren, wird die `AsInt`-Methode aufgerufen. Wenn die Konvertierung erfolgreich ist, können Sie die Zahlen hinzufügen.

In der folgenden Tabelle werden einige allgemeine Konvertierungs-und Testmethoden für Variablen aufgelistet.

:::row:::
    :::column:::
        <strong>Methode</strong>
    :::column-end:::
    :::column:::
        <strong>Beschreibung</strong>
    :::column-end:::
    :::column:::
        <strong>Beispiel</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        Konvertiert eine Zeichenfolge, die eine ganze Zahl darstellt (z. b. &quot;593&quot;), in eine ganze Zahl.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        Konvertiert eine Zeichenfolge wie &quot;true&quot; oder &quot;false-&quot; in einen booleschen Typ.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        Konvertiert eine Zeichenfolge mit einem Dezimalwert wie &quot;1,3&quot; oder &quot;7,439&quot; in eine Gleit Komma Zahl.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        Konvertiert eine Zeichenfolge mit einem Dezimalwert wie &quot;1,3&quot; oder &quot;7,439&quot; in eine Dezimalzahl. (In ASP.net ist eine Dezimalzahl präziser als eine Gleit Komma Zahl.)
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        Konvertiert eine Zeichenfolge, die einen Datums-und Uhrzeitwert darstellt, in den ASP.net-`DateTime` Typs.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        Konvertiert einen beliebigen anderen Datentyp in eine Zeichenfolge.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Operatoren

Ein Operator ist ein Schlüsselwort oder Zeichen, das ASP.net die Art des Befehls angibt, der in einem Ausdruck durchgeführt werden soll. Visual Basic unterstützt viele Operatoren, aber Sie müssen nur einige für den Einstieg in die Entwicklung von ASP.NET-Webseiten erkennen. In der folgenden Tabelle werden die gängigsten Operatoren zusammengefasst.

:::row:::
    :::column:::
        <strong>Operator</strong>
    :::column-end:::
    :::column:::
        <strong>Beschreibung</strong>
    :::column-end:::
    :::column:::
        <strong>Beispiele</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        Mathematische Operatoren, die in numerischen Ausdrücken verwendet werden.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Zuweisung und Gleichheit. Abhängig vom Kontext weist entweder den Wert auf der rechten Seite einer Anweisung dem-Objekt auf der linken Seite zu oder überprüft die Werte auf Gleichheit.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        Ungleichheit. Gibt `True` zurück, wenn die Werte nicht gleich sind.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Kleiner als, größer als, kleiner als oder gleich und größer als oder gleich.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        Verkettung, die verwendet wird, um Zeichen folgen zu verknüpfen.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        Die Inkrement-und Dekrementoperatoren, die 1 (bzw.) von einer Variablen addieren bzw. daraus ziehen.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        Gewinn. Wird verwendet, um Objekte und deren Eigenschaften und Methoden zu unterscheiden.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Klammern. Wird zum Gruppieren von Ausdrücken, zum Übergeben von Parametern an Methoden und zum Zugreifen auf Member von Arrays und Auflistungen verwendet.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        Hätten. Kehrt einen true-Wert in false um und umgekehrt. Wird in der Regel als Kurzform zum Testen auf `False` verwendet (d. h. für nicht `True`).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        Logisches and und or, die zum Verknüpfen von Bedingungen verwendet werden.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a>Arbeiten mit Datei-und Ordner Pfaden im Code

Häufig arbeiten Sie mit Datei-und Ordner Pfaden in Ihrem Code. Im folgenden finden Sie ein Beispiel für eine physische Ordnerstruktur für eine Website, wie Sie auf dem Entwicklungs Computer angezeigt werden kann:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Im folgenden finden Sie einige wichtige Details zu URLs und Pfaden:

- Eine URL beginnt entweder mit einem Domänen Namen (`http://www.example.com`) oder einem Servernamen (`http://localhost`, `http://mycomputer`).
- Eine URL entspricht einem physischen Pfad auf einem Host Computer. Beispielsweise können `http://myserver` dem Ordner *c:\websites\meinewebsite* auf dem Server entsprechen.
- Ein virtueller Pfad ist eine kurzzeile, um Pfade im Code darzustellen, ohne den vollständigen Pfad angeben zu müssen. Sie enthält den Teil einer URL, der auf den Domänen-oder Servernamen folgt. Wenn Sie virtuelle Pfade verwenden, können Sie Ihren Code in eine andere Domäne oder einen anderen Server verschieben, ohne die Pfade aktualisieren zu müssen.

Hier finden Sie ein Beispiel, das Ihnen hilft, die Unterschiede zu verstehen:

| Complete URL | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Servername | *mycompanyserver* |
| Virtueller Pfad | */HumanResources/CompanyPolicy.htm* |
| Physischer Pfad | *C:\meinewebsites\humanresources\companypolicy.htm* |

Das virtuelle Stammverzeichnis ist/, genauso wie der Stamm des Laufwerks C: \. (Pfade für virtuelle Ordner verwenden immer Schrägstriche.) Der virtuelle Pfad eines Ordners muss nicht denselben Namen wie der physische Ordner aufweisen. Dabei kann es sich um einen Alias handeln. (Auf Produktionsservern entspricht der virtuelle Pfad selten einem exakten physischen Pfad.)

Beim Arbeiten mit Dateien und Ordnern im Code müssen Sie manchmal auf den physischen Pfad und manchmal auf einen virtuellen Pfad verweisen, je nachdem, mit welchen Objekten Sie arbeiten. ASP.net bietet Ihnen diese Tools zum Arbeiten mit Datei-und Ordner Pfaden im Code: die `Server.MapPath`-Methode und der `~`-Operator und `Href`-Methode.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Umstellen von virtuellen in physische Pfade: die Server. MapPath-Methode

Die `Server.MapPath`-Methode konvertiert einen virtuellen Pfad (z. b. */default.cshtml*) in einen absoluten physischen Pfad (z. b. *c:\websites\mywebsitefolder\default.cshtml*). Sie verwenden diese Methode, wenn Sie einen kompletten physischen Pfad benötigen. Ein typisches Beispiel ist das Lesen oder Schreiben einer Textdatei oder Bilddatei auf dem Webserver.

Der absolute physische Pfad der Website auf dem Server einer Host Website ist in der Regel nicht bekannt. Daher kann diese Methode den Pfad, den Sie kennen – den virtuellen Pfad –, in den entsprechenden Pfad auf dem Server konvertieren. Sie übergeben den virtuellen Pfad an eine Datei oder einen Ordner an die-Methode und geben den physischen Pfad zurück:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Verweisen auf das virtuelle Stammverzeichnis: der ~-Operator und die href-Methode

In einer *cshtml* -oder *vbhtml* -Datei können Sie mithilfe des `~`-Operators auf den virtuellen Stammpfad verweisen. Dies ist sehr praktisch, da Sie Seiten auf einer Website verschieben können und alle Links, die Sie auf anderen Seiten enthalten, nicht beschädigt werden. Es ist auch praktisch, wenn Sie Ihre Website jemals an einen anderen Speicherort verschieben. Im Folgenden finden Sie einige Beispiele:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Wenn die Website `http://myserver/myapp`ist, so behandelt ASP.net diese Pfade, wenn die Seite ausgeführt wird:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Diese Pfade werden nicht als Werte der Variablen angezeigt, aber ASP.NET behandelt die Pfade so, als wären Sie so.)

Sie können den `~`-Operator sowohl im Servercode (wie oben) als auch im Markup wie folgt verwenden:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

In Markup verwenden Sie den `~`-Operator, um Pfade zu Ressourcen wie Bilddateien, anderen Webseiten und CSS-Dateien zu erstellen. Wenn die Seite ausgeführt wird, durchsucht ASP.net die Seite (sowohl Code als auch Markup) und löst alle `~` Verweise auf den entsprechenden Pfad auf.

## <a name="conditional-logic-and-loops"></a>Bedingte Logik und Schleifen

ASP.net-Servercode ermöglicht Ihnen das Ausführen von Aufgaben auf der Grundlage von Bedingungen und das Schreiben von Code, der-Anweisungen eine bestimmte Anzahl von Wiederholungen (Code, der eine Schleife ausführt) wiederholt.

### <a name="testing-conditions"></a>Testbedingungen

Um eine einfache Bedingung zu testen, verwenden Sie die `If...Then`-Anweisung, die basierend auf einem von Ihnen angegebenen Test `True` oder `False` zurückgibt:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

Das `If`-Schlüsselwort startet einen-Block. Der tatsächliche Test (Bedingung) folgt dem `If`-Schlüsselwort und gibt true oder false zurück. Die `If`-Anweisung endet mit `Then`. Die-Anweisungen, die ausgeführt werden, wenn der Test true ist, werden durch `If` und `End If`eingeschlossen. Eine `If`-Anweisung kann einen `Else`-Block enthalten, der Anweisungen angibt, die ausgeführt werden sollen, wenn die Bedingung false ist:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Wenn eine `If` Anweisung einen Codeblock startet, müssen Sie die normalen `Code...End Code` Anweisungen nicht verwenden, um die Blöcke einzuschließen. Sie können dem-Block einfach `@` hinzufügen, und er funktioniert. Diese Vorgehensweise funktioniert sowohl mit `If` als auch anderen Visual Basic Programmier Schlüsselwörtern, auf die Code Blöcke folgen, einschließlich `For`, `For Each`, `Do While`usw.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Sie können mehrere Bedingungen mithilfe eines oder mehrerer `ElseIf` Blöcke hinzufügen:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

Wenn in diesem Beispiel die erste Bedingung im `If`-Block nicht true ist, wird die `ElseIf` Bedingung geprüft. Wenn diese Bedingung erfüllt ist, werden die Anweisungen im `ElseIf`-Block ausgeführt. Wenn keine der Bedingungen erfüllt ist, werden die Anweisungen im `Else`-Block ausgeführt. Sie können beliebig viele `ElseIf` Blöcke hinzufügen und dann mit einem `Else`-Block als &quot;alles andere&quot; Bedingung schließen.

Um eine große Anzahl von Bedingungen zu testen, verwenden Sie einen `Select Case`-Block:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

Der zu überprüfende Wert ist in Klammern (im Beispiel die Wochentag-Variable). Jeder einzelne Test verwendet eine `Case`-Anweisung, die einen Wert auflistet. Wenn der Wert einer `Case`-Anweisung mit dem Testwert übereinstimmt, wird der Code in diesem `Case`-Block ausgeführt.

Das Ergebnis der letzten beiden bedingten Blöcke, die in einem Browser angezeigt werden:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Schleifen Code

Häufig müssen Sie dieselben Anweisungen wiederholt ausführen. Dies geschieht durch Schleifen. Beispielsweise führen Sie häufig die gleichen Anweisungen für jedes Element in einer Datensammlung aus. Wenn Sie genau wissen, wie oft Sie eine Schleife durchführen möchten, können Sie eine `For` Schleife verwenden. Diese Art von Schleife ist besonders nützlich, wenn Sie nach oben oder unten gezählt werden:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

Die Schleife beginnt mit dem `For`-Schlüsselwort, gefolgt von drei Elementen:

- Direkt nach der `For`-Anweisung deklarieren Sie eine Indikator Variable (Sie müssen keine `Dim`verwenden), und geben Sie dann den Bereich wie in `i = 10 to 20`an. Dies bedeutet, dass die Variable `i` bei 10 beginnt und fortgesetzt wird, bis 20 (einschließlich) erreicht wird.
- Zwischen der `For`-und `Next`-Anweisung ist der Inhalt des-Blocks. Dies kann eine oder mehrere Code Anweisungen enthalten, die mit jeder Schleife ausgeführt werden.
- Die `Next i`-Anweisung beendet die Schleife. Der Wert wird erhöht, und die nächste Iterations Schleife wird gestartet.

Die Codezeile zwischen dem `For` und `Next` Zeilen enthält den Code, der für jede Iterations Schleife ausgeführt wird. Das Markup erstellt jedes Mal einen neuen Absatz (`<p>` Element) und fügt der Ausgabe eine Zeile hinzu, in der der Wert i (The Counter) angezeigt wird. Wenn Sie diese Seite ausführen, erstellt das Beispiel 11 Zeilen, in denen die Ausgabe angezeigt wird, wobei der Text in jeder Zeile die Element Nummer angibt.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Wenn Sie mit einer Sammlung oder einem Array arbeiten, verwenden Sie häufig eine `For Each` Schleife. Eine Sammlung ist eine Gruppe von ähnlichen Objekten, und mit der `For Each`-Schleife können Sie für jedes Element in der Auflistung eine Aufgabe ausführen. Diese Art von Schleife eignet sich für Auflistungen, da Sie im Gegensatz zu einer `For` Schleife den Wert nicht erhöhen oder eine Beschränkung festlegen müssen. Stattdessen durchläuft der `For Each` Schleifen Code einfach die Auflistung, bis er abgeschlossen ist.

In diesem Beispiel werden die Elemente in der `Request.ServerVariables`-Auflistung zurückgegeben, die Informationen über Ihren Webserver enthält. Es wird eine `For Each` Schleife verwendet, um den Namen der einzelnen Elemente anzuzeigen, indem ein neues `<li>` Element in einer Liste mit HTML-Auflistungs Zeichen erstellt wird.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

Auf das `For Each` Schlüsselwort folgt eine Variable, die ein einzelnes Element in der Auflistung darstellt (im Beispiel `myItem`), gefolgt vom `In` Schlüsselwort, gefolgt von der Auflistung, die Sie durchlaufen möchten. Im Text der `For Each` Schleife können Sie mithilfe der Variablen, die Sie zuvor deklariert haben, auf das aktuelle Element zugreifen.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Verwenden Sie die `Do While`-Anweisung, um eine allgemeinere Schleife zu erstellen:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Diese Schleife beginnt mit dem `Do While`-Schlüsselwort, gefolgt von einer Bedingung, gefolgt vom Block, der wiederholt werden soll. Schleifen werden in der Regel in einer Variablen oder einem Objekt, das für das zählen verwendet wird, Inkrement (zu) oder Dekrement (subtrahieren Im Beispiel fügt der `+=`-Operator bei jeder Ausführung der Schleife 1 zum Wert einer Variablen hinzu. (Um eine Variable in einer Schleife, die Sie anzählt, herabzusetzen, verwenden Sie den Dekrementoperator `-=`.)

## <a name="objects-and-collections"></a>Objekte und Auflistungen

Fast alles auf einer ASP.NET-Website ist ein Objekt, das auch die Webseite selbst enthält. In diesem Abschnitt werden einige wichtige Objekte erläutert, mit denen Sie häufig in Ihrem Code arbeiten werden.

### <a name="page-objects"></a>Seiten Objekte

Das grundlegendste Objekt in ASP.net ist die Seite. Sie können auf die Eigenschaften des Seiten Objekts direkt ohne qualifizierendes Objekt zugreifen. Der folgende Code Ruft den Dateipfad der Seite ab, wobei das `Request` Objekt der Seite verwendet wird:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Sie können die Eigenschaften des `Page`-Objekts verwenden, um zahlreiche Informationen zu erhalten, z. b.:

- [https://login.microsoftonline.com/consumers/](`Request`). Wie Sie bereits gesehen haben, handelt es sich hierbei um eine Sammlung von Informationen über die aktuelle Anforderung, einschließlich des Typs des Browsers, der die Anforderung gestellt hat, der URL der Seite, der Benutzeridentität usw.
- [https://login.microsoftonline.com/consumers/](`Response`). Dabei handelt es sich um eine Sammlung von Informationen über die Antwort (Seite), die an den Browser gesendet werden, wenn die Ausführung des Server Codes abgeschlossen ist. Beispielsweise können Sie diese Eigenschaft verwenden, um Informationen in die Antwort zu schreiben.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Sammlungsobjekte (Arrays und Wörterbücher)

Eine Sammlung ist eine Gruppe von Objekten desselben Typs, z. b. eine Auflistung von `Customer` Objekten aus einer Datenbank. ASP.NET enthält viele integrierte Auflistungen, wie z. b. die `Request.Files` Auflistung.

Häufig arbeiten Sie mit Daten in Auflistungen. Zwei gängige Auflistungs Typen sind das- *Array* und das- *Wörterbuch*. Ein Array ist nützlich, wenn Sie eine Sammlung ähnlicher Elemente speichern möchten, aber keine separate Variable zum Speichern der einzelnen Elemente erstellen möchten:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Mit Arrays deklarieren Sie einen bestimmten Datentyp, z. b. `String`, `Integer`oder `DateTime`. Um anzugeben, dass die Variable ein Array enthalten kann, fügen Sie dem Variablennamen in der Deklaration Klammern hinzu (z. b. `Dim myVar() As String`). Sie können auf Elemente in einem Array zugreifen, indem Sie Ihre Position (Index) oder die `For Each`-Anweisung verwenden. Array Indizes sind NULL basiert &#8212; , d. h., das erste Element befindet sich an Position 0, das zweite Element an Position 1 usw.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Sie können die Anzahl der Elemente in einem Array ermitteln, indem Sie die `Length`-Eigenschaft erhalten. Um die Position eines bestimmten Elements im Array (d. h. zum Durchsuchen des Arrays) zu erhalten, verwenden Sie die `Array.IndexOf`-Methode. Sie können auch den Inhalt eines Arrays umkehren (die `Array.Reverse`-Methode) oder den Inhalt (die `Array.Sort`-Methode) sortieren.

Die Ausgabe des Zeichen folgen Array Codes, der in einem Browser angezeigt wird:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Ein Wörterbuch ist eine Auflistung von Schlüssel-Wert-Paaren, bei der Sie den Schlüssel (oder Namen) bereitstellen, um den entsprechenden Wert festzulegen oder abzurufen:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Zum Erstellen eines Wörterbuchs verwenden Sie das `New`-Schlüsselwort, um anzugeben, dass Sie ein neues `Dictionary` Objekt erstellen. Mit dem `Dim`-Schlüsselwort können Sie einer Variablen ein Wörterbuch zuweisen. Sie geben die Datentypen der Elemente im Wörterbuch mithilfe von Klammern (`( )`) an. Am Ende der Deklaration müssen Sie ein weiteres Paar Klammern hinzufügen, da es sich hierbei um eine Methode handelt, die ein neues Wörterbuch erstellt.

Um dem Wörterbuch Elemente hinzuzufügen, können Sie die `Add`-Methode der Wörterbuch Variablen (in diesem Fall`myScores`) und dann einen Schlüssel und einen Wert angeben. Alternativ können Sie Klammern verwenden, um den Schlüssel anzugeben und eine einfache Zuweisung durchzuführen, wie im folgenden Beispiel gezeigt:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Um einen Wert aus dem Wörterbuch zu erhalten, geben Sie den Schlüssel in Klammern an:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Aufrufen von Methoden mit Parametern

Wie Sie bereits zuvor in diesem Artikel gesehen haben, verfügen die Objekte, mit denen Sie programmieren, über Methoden. Beispielsweise kann ein `Database` Objekt über eine `Database.Connect`-Methode verfügen. Viele Methoden verfügen auch über einen oder mehrere Parameter. Ein *Parameter* ist ein Wert, den Sie an eine-Methode übergeben, um die-Methode zum Abschließen der Aufgabe zu aktivieren. Betrachten Sie z. b. eine Deklaration für die `Request.MapPath`-Methode, die drei Parameter annimmt:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Diese Methode gibt den physischen Pfad auf dem Server zurück, der einem angegebenen virtuellen Pfad entspricht. Die drei Parameter für die-Methode sind `virtualPath`, `baseVirtualDir`und `allowCrossAppMapping`. (Beachten Sie, dass in der-Deklaration die Parameter mit den Datentypen der Daten aufgelistet werden, die Sie akzeptieren.) Wenn Sie diese Methode aufgerufen haben, müssen Sie Werte für alle drei Parameter angeben.

Wenn Sie Visual Basic mit dem Razor-Syntax verwenden, haben Sie zwei Optionen zum Übergeben von Parametern an eine Methode: *Positions Parameter* oder *benannte Parameter*. Um eine Methode mit Positions Parametern aufzurufen, übergeben Sie die Parameter in einer strengen Reihenfolge, die in der Methoden Deklaration angegeben ist. (In der Regel wissen Sie diese Bestellung, indem Sie die Dokumentation für die-Methode lesen.) Sie müssen die Reihenfolge befolgen, und Sie können die Parameter &#8212; bei Bedarf nicht überspringen. Sie übergeben eine leere Zeichenfolge (`""`) oder NULL für einen positionellen Parameter, für den Sie keinen Wert haben.

Im folgenden Beispiel wird davon ausgegangen, dass Sie über einen Ordner namens *Scripts* auf Ihrer Website verfügen. Der Code Ruft die `Request.MapPath`-Methode auf und übergibt Werte für die drei Parameter in der richtigen Reihenfolge. Daraufhin wird der resultierende zugeordnete Pfad angezeigt.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Wenn für eine Methode viele Parameter vorhanden sind, können Sie Ihren Code bereinigen und besser lesbarer machen, indem Sie benannte Parameter verwenden. Um eine Methode mit benannten Parametern aufzurufen, geben Sie den Parameternamen gefolgt von `:=` an, und geben Sie dann den Wert an. Ein Vorteil von benannten Parametern ist, dass Sie Sie in beliebiger Reihenfolge hinzufügen können. (Ein Nachteil ist, dass der Methoden Aufrufwert nicht so kompakt ist.)

Im folgenden Beispiel wird dieselbe Methode wie oben aufgerufen, es werden jedoch benannte Parameter verwendet, um die Werte bereitzustellen:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Wie Sie sehen können, werden die Parameter in einer anderen Reihenfolge übermittelt. Wenn Sie jedoch das vorherige Beispiel und dieses Beispiel ausführen, wird derselbe Wert zurückgegeben.

## <a name="handling-errors"></a>Behandeln von Fehlern

### <a name="try-catch-statements"></a>Try-catch-Anweisungen

Sie verfügen häufig über Anweisungen in Ihrem Code, die aus Gründen außerhalb ihrer Kontrolle fehlschlagen können. Beispiel:

- Wenn Ihr Code versucht, eine Datei zu öffnen, zu erstellen, zu lesen oder zu schreiben, können alle möglichen Fehler auftreten. Die gewünschte Datei ist möglicherweise nicht vorhanden, Sie ist möglicherweise gesperrt, der Code verfügt möglicherweise nicht über Berechtigungen usw.
- Wenn Ihr Code versucht, Datensätze in einer Datenbank zu aktualisieren, können auch Berechtigungsprobleme auftreten, die Verbindung mit der Datenbank kann gelöscht werden, die zu speichernden Daten sind möglicherweise ungültig usw.

In Programmier begriffen werden diese Situationen als *Ausnahmen*bezeichnet. Wenn im Code eine Ausnahme auftritt, wird eine Fehlermeldung generiert (ausgelöst), die für Benutzer am besten geeignet ist.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

In Situationen, in denen in Ihrem Code Ausnahmen auftreten können, und um Fehlermeldungen dieses Typs zu vermeiden, können Sie `Try/Catch`-Anweisungen verwenden. In der `Try`-Anweisung führen Sie den Code aus, den Sie überprüfen. In mindestens einer `Catch`-Anweisung können Sie nach bestimmten Fehlern (bestimmte Typen von Ausnahmen) suchen, die möglicherweise aufgetreten sind. Sie können so viele `Catch` Anweisungen einschließen, wie Sie nach Fehlern suchen müssen, die Sie erwarten.

> [!NOTE]
> Es wird empfohlen, die Verwendung der `Response.Redirect`-Methode in `Try/Catch` Anweisungen zu vermeiden, da dies eine Ausnahme in der Seite verursachen kann.

Das folgende Beispiel zeigt eine Seite, die bei der ersten Anforderung eine Textdatei erstellt und dann eine Schaltfläche anzeigt, mit der der Benutzer die Datei öffnen kann. Im Beispiel wird absichtlich ein ungültiger Dateiname verwendet, damit eine Ausnahme ausgelöst wird. Der Code enthält `Catch` Anweisungen für zwei mögliche Ausnahmen: `FileNotFoundException`, bei einem ungültigen Dateinamen und `DirectoryNotFoundException`. Dies tritt auf, wenn ASP.NET den Ordner nicht selbst finden kann. (Sie können die Auskommentierung einer Anweisung im Beispiel aufheben, um zu sehen, wie Sie ausgeführt wird, wenn alles ordnungsgemäß funktioniert.)

Wenn der Code die Ausnahme nicht behandelt hat, sehen Sie eine Fehlerseite wie der vorherige Screenshot. Mit dem `Try/Catch` Abschnitt wird jedoch verhindert, dass der Benutzer diese Fehlertypen erkennen kann.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

### <a name="reference-documentation"></a>Referenzdokumentation

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Visual Basic-Sprache](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
