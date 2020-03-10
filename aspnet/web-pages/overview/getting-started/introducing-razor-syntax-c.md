---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Einführung in ASP.net-Webprogrammierung mit der Razor-C#Syntax () | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Kapitel enthält eine Übersicht über die Programmierung mit ASP.net Web Pages mithilfe des Razor-Syntax. ASP.net ist die Technologie von Microsoft zum Ausführen von dynamischem Web-PA...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: c2f420bb7c2f7d2e31654c20fb9ec7497a30a9f7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521205"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Einführung in ASP.net-Webprogrammierung mit der Razor-C#Syntax ()

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel erhalten Sie einen Überblick über die Programmierung mit ASP.net Web Pages mithilfe des Razor-Syntax. ASP.net ist die Technologie von Microsoft zum Ausführen dynamischer Webseiten auf Webservern. Dieser Artikel konzentriert sich auf die C# Verwendung der Programmiersprache.
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

## <a name="the-top-8-programming-tips"></a>Die Top 8-Programmiertipps

Dieser Abschnitt enthält einige Tipps, die Sie unbedingt kennen müssen, wenn Sie mit dem Schreiben von ASP.net-Servercode mit dem Razor-Syntax beginnen.

> [!NOTE]
> Der Razor-Syntax basiert auf der C# Programmiersprache, und das ist die Sprache, die am häufigsten mit ASP.net Web Pages verwendet wird. Der Razor-Syntax unterstützt jedoch auch die Visual Basic Sprache und alles, was Sie sehen können, auch in Visual Basic. Weitere Informationen finden Sie im Anhang [Visual Basic Sprache und Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).

Weitere Details zu den meisten dieser Programmiertechniken finden Sie später in diesem Artikel.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Sie fügen Code zu einer Seite mit dem @-Zeichen hinzu.

Das `@` Zeichen startet Inline Ausdrücke, einzelne Anweisungsblöcke und Blöcke mit mehreren Anweisungen:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Diese Anweisungen sehen wie folgt aus, wenn die Seite in einem Browser ausgeführt wird:

![Razor-Bild1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **HTML-Codierung**
> 
> Wenn Sie Inhalt in einer Seite mit dem `@` Zeichen anzeigen, wie in den vorherigen Beispielen, codiert ASP.net die Ausgabe. Dies ersetzt reservierte HTML-Zeichen (z. b. `<` und `>` und `&`) durch Codes, die es ermöglichen, dass die Zeichen als Zeichen in einer Webseite angezeigt werden, anstatt als HTML-Tags oder-Entitäten interpretiert zu werden. Ohne HTML-Codierung wird die Ausgabe des Server Codes möglicherweise nicht ordnungsgemäß angezeigt und kann eine Seite für Sicherheitsrisiken verfügbar machen.
> 
> Wenn das Ziel darin besteht, HTML-Markup auszugeben, das Tags als Markup rendert (z. b. `<p></p>` für einen Absatz oder `<em></em>`, um Text hervorzuheben), lesen Sie den Abschnitt [Kombinieren von Text, Markup und Code in Code Blöcken](#BM_CombiningTextMarkupAndCode) weiter unten in diesem Artikel.
> 
> Weitere Informationen zur HTML-Codierung finden Sie unter [Arbeiten mit Formularen](https://go.microsoft.com/fwlink/?LinkId=202892).

### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Sie schließen Code Blöcke in geschweiften Klammern ein.

Ein *Codeblock* enthält mindestens eine Code Anweisung und ist in geschweifte Klammern eingeschlossen.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Das in einem Browser angezeigte Ergebnis:

![Razor-Bild2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. innerhalb eines-Blocks beenden Sie jede Code Anweisung mit einem Semikolon.

In einem Codeblock muss jede Complete Code-Anweisung mit einem Semikolon enden. Inline Ausdrücke enden nicht mit einem Semikolon.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. Sie verwenden Variablen zum Speichern von Werten.

Sie können Werte in einer *Variablen*speichern, einschließlich Zeichen folgen, Zahlen und Datumsangaben usw. Sie erstellen eine neue Variable mit dem `var`-Schlüsselwort. Sie können Variablen Werte direkt in eine Seite einfügen, indem Sie `@`verwenden.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Das in einem Browser angezeigte Ergebnis:

![Razor-Bild3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Sie schließen Literalzeichenfolgen-Werte in doppelte Anführungszeichen ein.

Eine *Zeichen* Folge ist eine Sequenz von Zeichen, die als Text behandelt werden. Um eine Zeichenfolge anzugeben, müssen Sie Sie in doppelte Anführungszeichen einschließen:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Wenn die anzuzeigende Zeichenfolge einen umgekehrten Schrägstrich (`\`) oder doppelte Anführungszeichen (`"`) enthält, verwenden Sie einen *ausführlichen Zeichenfolgenliteralwert* , der dem `@`-Operator vorangestellt ist. (In C#hat das Zeichen \ eine besondere Bedeutung, es sei denn, Sie verwenden einen ausführlichen Zeichenfolgenliterals.

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Um doppelte Anführungszeichen einzubetten, verwenden Sie einen ausführlichen Zeichenfolgenliteralwert und wiederholen die Anführungszeichen

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Im folgenden finden Sie das Ergebnis der Verwendung beider Beispiele in einer Seite:

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Beachten Sie, dass das `@` Zeichen verwendet wird, um ausführliche Zeichen folgen Literale in C# und zu kennzeichnen, um Code in ASP.NET Seiten zu markieren.

### <a name="6-code-is-case-sensitive"></a>6. bei Code wird Groß-/Kleinschreibung

In C#wird bei Schlüsselwörtern (z. b. `var`, `true`und `if`) und Variablennamen die Groß-/Kleinschreibung beachtet. In den folgenden Codezeilen werden zwei verschiedene Variablen erstellt: `lastName` und `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Wenn Sie eine Variable als `var lastName = "Smith";` deklarieren und versuchen, auf die Variable auf der Seite als `@LastName`zu verweisen, erhalten Sie den Wert `"Jones"` anstelle von `"Smith"`.

> [!NOTE]
> In Visual Basic wird die Groß-/Kleinschreibung *nicht* beachtet.

### <a name="7-much-of-your-coding-involves-objects"></a>7. ein Großteil ihrer Codierung umfasst Objekte

Ein- *Objekt* stellt eine Aufgabe dar, mit &#8212; der Sie eine Seite, ein Textfeld, eine Datei, ein Bild, eine Webanforderung, eine e-Mail-Nachricht, einen Kundendaten Satz (Datenbankzeile) usw. programmieren können. Objekte verfügen über Eigenschaften, die ihre Merkmale beschreiben, und Sie können ein &#8212; Textfeld-`Text` Objekt lesen oder ändern (unter anderem), ein Request-Objekt verfügt über eine `Url`-Eigenschaft, eine e-Mail-Nachricht über eine `From`-Eigenschaft, und ein Customer-Objekt verfügt über eine `FirstName`-Eigenschaft. -Objekte verfügen auch über Methoden, die die &quot;Verben&quot; können, die Sie ausführen können. Beispiele hierfür sind die `Save`-Methode eines File-Objekts, die `Rotate`-Methode eines Bildobjekts und die `Send`-Methode eines e-Mail-Objekts.

Häufig arbeiten Sie mit dem `Request` Objekt, das Ihnen Informationen wie die Werte von Textfeldern (Formularfelder) auf der Seite, den Typ des Browsers, der die Anforderung durchgeführt hat, die URL der Seite, die Benutzeridentität usw. enthält. Im folgenden Beispiel wird gezeigt, wie Sie auf Eigenschaften des `Request` Objekts zugreifen und wie Sie die `MapPath`-Methode des `Request`-Objekts aufrufen, das Ihnen den absoluten Pfad der Seite auf dem Server liefert:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Das in einem Browser angezeigte Ergebnis:

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Sie können Code schreiben, der Entscheidungen trifft.

Ein wichtiges Feature von dynamischen Webseiten ist, dass Sie anhand der Bedingungen ermitteln können, was zu tun ist. Die gängigste Methode hierfür ist die `if`-Anweisung (und optional `else`-Anweisung).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

Die-Anweisung `if(IsPost)` ist eine Kurzform zum Schreiben von `if(IsPost == true)`. Zusammen mit `if`-Anweisungen gibt es eine Vielzahl von Möglichkeiten, Bedingungen zu testen, Code Blöcke zu wiederholen usw., die weiter unten in diesem Artikel beschrieben werden.

Das in einem Browser angezeigte Ergebnis (nach dem Klicken auf " **senden**"):

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP Get-und Post-Methoden und die ispost-Eigenschaft
> 
> Das Protokoll, das für Webseiten (http) verwendet wird, unterstützt eine sehr begrenzte Anzahl von Methoden (Verben), die verwendet werden, um Anforderungen an den Server zu senden. Die beiden häufigsten sind Get, das zum Lesen einer Seite verwendet wird, und Post, der zum Senden einer Seite verwendet wird. Wenn ein Benutzer zum ersten Mal eine Seite anfordert, wird die Seite mithilfe von Get angefordert. Wenn der Benutzer ein Formular ausfüllt und dann auf die Schaltfläche Senden klickt, sendet der Browser eine Post-Anforderung an den Server.
> 
> Bei der Webprogrammierung ist es häufig hilfreich zu wissen, ob eine Seite als Get-oder Post-Vorgang angefordert wird, damit Sie wissen, wie die Seite verarbeitet wird. In ASP.net Web Pages können Sie die `IsPost`-Eigenschaft verwenden, um festzustellen, ob eine Anforderung ein Get-oder Post-Eintrag ist. Wenn es sich bei der Anforderung um einen Post-Vorgang handelt, wird die `IsPost`-Eigenschaft "true" zurückgegeben. Sie können z. b. die Werte von Textfeldern in einem Formular lesen. Viele Beispiele zeigen Ihnen, wie Sie die Seite je nach dem Wert `IsPost`anders verarbeiten können.

## <a name="a-simple-code-example"></a>Einfaches Code Beispiel

In diesem Verfahren wird gezeigt, wie Sie eine Seite erstellen, die grundlegende Programmiertechniken veranschaulicht. In diesem Beispiel erstellen Sie eine Seite, mit der Benutzer zwei Zahlen eingeben können. Anschließend werden Sie hinzugefügt, und das Ergebnis wird angezeigt.

1. Erstellen Sie im Editor eine neue Datei, und nennen Sie Sie " *AddNumbers. cshtml*".
2. Kopieren Sie den folgenden Code und das Markup in die Seite, und ersetzen Sie alles, was bereits auf der Seite ist.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Beachten Sie Folgendes:

    - Mit dem `@` Zeichen wird der erste Codeblock auf der Seite gestartet, und vor der `totalMessage` Variablen, die am unteren Rand der Seite eingebettet ist.
    - Der Block am oberen Rand der Seite wird in geschweifte Klammern eingeschlossen.
    - Im Block am oberen Ende werden alle Zeilen mit einem Semikolon enden.
    - In den Variablen `total`, `num1`, `num2`und `totalMessage` werden mehrere Zahlen und eine Zeichenfolge gespeichert.
    - Der Literalzeichenfolgen-Wert, der der `totalMessage` Variablen zugewiesen ist, ist in doppelten Anführungszeichen.
    - Da im Code die Groß-/Kleinschreibung beachtet wird, muss der Name, wenn die `totalMessage` Variable im unteren Bereich der Seite verwendet wird, genau mit der Variablen oben übereinstimmen.
    - Der Ausdruck `num1.AsInt() + num2.AsInt()` zeigt, wie Sie mit Objekten und Methoden arbeiten. Die `AsInt`-Methode für jede Variable konvertiert die von einem Benutzer eingegebene Zeichenfolge in eine Zahl (eine ganze Zahl), sodass Sie arithmetische Operationen ausführen können.
    - Das `<form>`-Tag enthält ein `method="post"` Attribut. Dies gibt an, dass die Seite mithilfe der HTTP Post-Methode an den Server gesendet wird, wenn der Benutzer auf **Hinzufügen**klickt. Wenn die Seite übermittelt wird, wird der `if(IsPost)` Test als true ausgewertet, und der bedingte Code wird ausgeführt, und das Ergebnis des Hinzufügens der Zahlen wird angezeigt.
3. Speichern Sie die Seite, und führen Sie Sie in einem Browser aus. (Stellen Sie sicher, dass die Seite im Arbeitsbereich " **Dateien** " ausgewählt ist, bevor Sie Sie ausführen.) Geben Sie zwei ganze Zahlen ein, und klicken Sie dann auf **Hinzufügen** . 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Grundlegende Programmier Konzepte

Dieser Artikel bietet eine Übersicht über die ASP.net-Webprogrammierung. Es handelt sich nicht um eine umfassende Prüfung, sondern nur eine kurze Tour durch die Programmier Konzepte, die Sie am häufigsten verwenden werden. Auch hier wird fast alles behandelt, was Sie für den Einstieg in ASP.net Web Pages benötigen.

Aber zuerst ein wenig technischer Hintergrund.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Die Razor-Syntax, der Server Code und ASP.net

Razor-Syntax ist eine einfache Programmier Syntax zum Einbetten von Server basiertem Code in einer Webseite. Auf einer Webseite, die die Razor-Syntax verwendet, gibt es zwei Arten von Inhalten: Client Inhalt und Servercode. Client Inhalt ist das, was Sie in Webseiten verwenden: HTML-Markup (Elemente), Formatinformationen wie CSS, vielleicht einige Client Skripts wie JavaScript und nur-Text.

Mit Razor-Syntax können Sie diesem Client Inhalt Servercode hinzufügen. Wenn auf der Seite Servercode vorhanden ist, führt der Server den Code zuerst aus, bevor die Seite an den Browser gesendet wird. Durch die Ausführung auf dem Server kann der Code Aufgaben ausführen, die für die Verwendung von Client Inhalten allein sehr viel komplexer sein können, z. b. den Zugriff auf serverbasierte Datenbanken. Der wichtigste Punkt ist, dass der Servercode Client &#8212; Inhalte dynamisch erstellen kann, um HTML-Markup oder anderen Inhalt dynamisch zu generieren und ihn dann zusammen mit jedem statischen HTML-Code, den die Seite enthält, an den Browser zu senden. Aus Sicht des Browsers unterscheidet sich der Client Inhalt, der vom Servercode generiert wird, nicht von anderen Client Inhalten. Wie Sie bereits gesehen haben, ist der erforderliche Servercode recht einfach.

ASP.NET Webseiten, die die Razor-Syntax enthalten, haben eine spezielle Dateierweiterung ( *. cshtml* oder *. vbhtml*). Der Server erkennt diese Erweiterungen, führt den Code aus, der mit Razor-Syntax gekennzeichnet ist, und sendet die Seite dann an den Browser.

### <a name="where-does-aspnet-fit-in"></a>Wo ist ASP.net passt?

Razor-Syntax basiert auf einer Technologie von Microsoft namens ASP.net, die wiederum auf dem Microsoft .NET Framework basiert. The.NET Framework ist ein großes, umfassendes Programmier Framework von Microsoft für die Entwicklung von praktisch jeder Art von Computeranwendung. ASP.net ist der Teil der .NET Framework, die speziell für das Erstellen von Webanwendungen entworfen wurde. Entwickler haben ASP.NET verwendet, um viele der größten und weltweit höchsten Websites zu erstellen. (Jedes Mal, wenn die Dateinamenerweiterung " *. aspx* " als Teil der URL einer Website angezeigt wird, wissen Sie, dass die Website mit ASP.NET geschrieben wurde.)

Der Razor-Syntax bietet Ihnen die gesamte Leistungsfähigkeit von ASP.net, verwendet jedoch eine vereinfachte Syntax, die einfacher zu erlernen ist, wenn Sie ein Anfänger sind und Sie produktiver machen, wenn Sie ein Experte sind. Obwohl diese Syntax einfach zu verwenden ist, bedeutet Ihre Familienbeziehung zu ASP.net und der .NET Framework, dass Sie, wenn Ihre Websites komplexer werden, die Leistungsfähigkeit der größeren Frameworks haben, die Ihnen zur Verfügung stehen.

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Klassen und Instanzen**
> 
> ASP.net-Servercode verwendet-Objekte, die wiederum auf der Idee von Klassen basieren. Bei der-Klasse handelt es sich um die Definition oder Vorlage für ein Objekt. Beispielsweise kann eine Anwendung eine `Customer` Klasse enthalten, die die Eigenschaften und Methoden definiert, die ein beliebiges Kunden Objekt benötigt.
> 
> Wenn die Anwendung mit den tatsächlichen Kundeninformationen arbeiten muss, erstellt Sie eine Instanz von (oder *instanziiert*) ein Customer-Objekt. Bei jedem einzelnen Kunden handelt es sich um eine separate Instanz der `Customer`-Klasse. Jede Instanz unterstützt dieselben Eigenschaften und Methoden, aber die Eigenschaftswerte für jede Instanz unterscheiden sich in der Regel, da jedes Kunden Objekt eindeutig ist. In einem Kunden Objekt kann die `LastName`-Eigenschaft "Smith" lauten. in einem anderen Customer-Objekt kann die `LastName`-Eigenschaft "Jones" lauten.
> 
> Auf ähnliche Weise ist jede einzelne Webseite in Ihrer Site ein `Page` Objekt, das eine Instanz der `Page`-Klasse ist. Eine Schaltfläche auf der Seite ist ein `Button` Objekt, das eine Instanz der `Button`-Klasse ist usw. Jede Instanz verfügt über eigene Merkmale, aber Sie basieren auf den Angaben, die in der Klassendefinition des Objekts angegeben sind.

## <a name="basic-syntax"></a>Grundlegende Syntax

Früher haben Sie ein einfaches Beispiel für das Erstellen einer ASP.net Web Pages Seite und die Vorgehensweise zum Hinzufügen von Servercode zu HTML-Markup gesehen. Hier lernen Sie die Grundlagen zum Schreiben von ASP.net-Servercode mithilfe &#8212; der Razor-Syntax der Programmiersprachen Regeln kennen.

Wenn Sie Erfahrung mit der Programmierung haben (insbesondere, wenn Sie C, C++, C#, Visual Basic oder JavaScript verwendet haben), werden viele der hier gelesenen Elemente Ihnen vertraut sein. Sie müssen sich wahrscheinlich nur mit dem Hinzufügen von Servercode zu Markup in *. cshtml* -Dateien vertraut machen.

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Kombinieren von Text, Markup und Code in Code Blöcken

In Servercode Blöcken möchten Sie häufig Text oder Markup (oder beides) an die Seite ausgeben. Wenn ein Server Codeblock Text enthält, der nicht Code ist und stattdessen unverändert gerendert werden soll, muss ASP.net in der Lage sein, diesen Text vom Code zu unterscheiden. Hierfür gibt es mehrere Möglichkeiten.

- Schließen Sie den Text in ein HTML-Element wie `<p></p>` oder `<em></em>`ein:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    Das HTML-Element kann Text, zusätzliche HTML-Elemente und Servercode Ausdrücke enthalten. Wenn ASP.NET das öffnende HTML-Tag sieht (z. b. `<p>`), werden alle Elemente, einschließlich des Elements und seines Inhalts, unverändert im Browser gerendert, und die Servercode Ausdrücke werden entsprechend aufgelöst.
- Verwenden Sie den `@:`-Operator oder das `<text>`-Element. Der `@:` gibt eine einzelne Zeile mit Inhalt aus, die nur-Text oder nicht übereinstimmende HTML-Tags enthält Das `<text>` Element schließt mehrere Zeilen ein, die ausgegeben werden sollen. Diese Optionen sind hilfreich, wenn Sie ein HTML-Element nicht als Teil der Ausgabe renderingrensten möchten.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Wenn Sie mehrere Textzeilen oder nicht übereinstimmende HTML-Tags ausgeben möchten, können Sie jede Zeile mit `@:`versehen, oder Sie können die Zeile in ein `<text>` Element einschließen. Wie der `@:`-Operator werden`<text>` Tags von ASP.NET verwendet, um Textinhalte zu identifizieren und nie in der Seiten Ausgabe gerendert zu werden.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    Im ersten Beispiel wird das vorherige Beispiel wiederholt, aber es wird ein einzelnes Paar `<text>` Tags verwendet, um den zu Rendering enden Text einzuschließen. Im zweiten Beispiel schließen die Tags "`<text>`" und "`</text>`" drei Zeilen ein, die alle über einen nicht enthaltenen Text und nicht übereinstimmende HTML-Tags (`<br />`) zusammen mit Servercode und übereinstimmenden HTML-Tags verfügen. Auch hier kann jede Zeile einzeln mit dem `@:`-Operator vorangestellt werden. Beides funktioniert.

    > [!NOTE]
    > Wenn Sie Text wie in diesem Abschnitt &#8212; gezeigt mit einem HTML--Element ausgeben, wird der `@:`-Operator oder &#8212; das `<text>` Element ASP.net die Ausgabe nicht in HTML-codiert. (Wie bereits erwähnt, codiert ASP.net die Ausgabe von Servercode Ausdrücken und Servercode Blöcken, denen `@`vorangestellt ist, mit Ausnahme der in diesem Abschnitt notierten Sonderfälle.)

### <a name="whitespace"></a>Whitespace

Zusätzliche Leerzeichen in einer-Anweisung (und außerhalb eines Zeichenfolgenliterals) haben keine Auswirkung auf die Anweisung:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Ein Zeilenumbruch in einer-Anweisung hat keine Auswirkung auf die-Anweisung, und Sie können-Anweisungen zur besseren Lesbarkeit umschließen. Die folgenden Anweisungen sind identisch:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Sie können jedoch keine Linie in der Mitte eines Zeichenfolgenliterals umschließen. Das folgende Beispiel funktioniert nicht:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Um eine lange Zeichenfolge zu kombinieren, die mit mehreren Zeilen wie dem obigen Code umbrochen wird, gibt es zwei Optionen. Sie können den Verkettungs Operator (`+`) verwenden, den Sie später in diesem Artikel sehen werden. Sie können auch das `@` Zeichen verwenden, um ein wörtlich zeichenfolgenliteralformat zu erstellen, wie zuvor in diesem Artikel gezeigt. Sie können ausführliche Zeichen folgen Literale Zeilen übergreifend unterbrechen:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Kommentare zu Code (und Markup)

Mit Kommentaren können Sie Notizen für sich selbst oder für andere Personen hinterlassen. Sie ermöglichen es Ihnen außerdem, einen Code Abschnitt oder Markup, den Sie nicht ausführen möchten, zu deaktivieren (*auszukommentieren*), auf der Seite zu speichern.

Es gibt verschiedene Kommentar Syntax für Razor-Code und für HTML-Markup. Wie bei sämtlichen Razor-Code werden Razor-Kommentare auf dem Server verarbeitet (und dann entfernt), bevor die Seite an den Browser gesendet wird. Daher können Sie mit der Razor-Kommentar Syntax Kommentare in den Code einfügen (oder sogar in das Markup), den Sie beim Bearbeiten der Datei anzeigen können, die aber auch nicht in der Seitenquelle angezeigt werden.

Für ASP.net Razor-Kommentare starten Sie den Kommentar mit `@*` und beenden ihn mit `*@`. Der Kommentar kann sich in einer Zeile oder in mehreren Zeilen befinden:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Hier ist ein Kommentar innerhalb eines Code Blocks:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Hier ist derselbe Codeblock, in dem die Codezeile auskommentiert ist, sodass Sie nicht ausgeführt werden kann:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

In einem Codeblock können Sie als Alternative zur Verwendung der Razor-Kommentar Syntax die Kommentar Syntax der verwendeten Programmiersprache verwenden, z. b. C#:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

In C#werden einzeilige Kommentare `//` Zeichen vorangestellt, und mehrzeilige Kommentare beginnen mit `/*` und enden mit `*/`. (Wie bei Razor-Kommentaren C# werden Kommentare nicht im Browser gerendert.)

Für Markup können Sie, wie Sie wahrscheinlich wissen, einen HTML-Kommentar erstellen:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

HTML-Kommentare beginnen mit `<!--` Zeichen und enden mit `-->`. Sie können HTML-Kommentare verwenden, um nicht nur Text zu umschließen, sondern auch ein beliebiges HTML-Markup, das Sie auf der Seite behalten möchten, aber nicht Rendering möchten. Dieser HTML-Kommentar blendet den gesamten Inhalt der Tags und den darin enthaltenen Text aus:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

Im Gegensatz zu Razor-Kommentaren *werden* HTML-Kommentare auf der Seite gerendert, und der Benutzer kann diese anzeigen, indem er die Seitenquelle anzeigen kann.

Razor hat Einschränkungen für die Blöcke von C#. Weitere Informationen finden Sie [unter C# benannte Variablen und geschiebte Blöcke generieren](http://aspnetwebstack.codeplex.com/workitem/1914) von fehlerhafter Code.

## <a name="variables"></a>Variables

Eine Variable ist ein benanntes Objekt, das Sie zum Speichern von Daten verwenden. Sie können Variablen einen beliebigen Namen benennen, aber der Name muss mit einem alphabetischen Zeichen beginnen und darf keine Leerzeichen oder reservierten Zeichen enthalten.

### <a name="variables-and-data-types"></a>Variablen und Datentypen

Eine Variable kann einen bestimmten Datentyp aufweisen, der angibt, welche Art von Daten in der Variablen gespeichert ist. Sie können Zeichen folgen Variablen haben, die Zeichen folgen Werte (wie &quot;Hello World&quot;) speichern, ganzzahlige Variablen, die ganzzahlige Werte (z. b. 3 oder 79) speichern, und Datums Variablen, die Datumswerte in einer Vielzahl von Formaten (z. b. 4/12/2012 oder März 2009) speichern. Und es gibt viele weitere Datentypen, die Sie verwenden können.

Sie müssen jedoch im Allgemeinen keinen Typ für eine Variable angeben. In den meisten Fällen kann ASP.NET den Typ basierend auf der Verwendung der Daten in der Variablen ermitteln. (Gelegentlich müssen Sie einen Typ angeben. es werden Beispiele angezeigt, wenn dies zutrifft.)

Sie deklarieren eine Variable mit dem `var`-Schlüsselwort (wenn Sie keinen Typ angeben möchten) oder mit dem Namen des Typs:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

Das folgende Beispiel zeigt einige typische Verwendungen von Variablen in einer Webseite:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Wenn Sie die vorherigen Beispiele in einer Seite kombinieren, wird dies in einem Browser angezeigt:

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Wandeln und Testen von Datentypen

Obwohl ASP.net normalerweise einen Datentyp automatisch ermitteln kann, kann dies manchmal nicht der Fall sein. Daher müssen Sie möglicherweise ASP.net durch eine explizite Konvertierung unterstützen. Auch wenn Sie keine Typen konvertieren müssen, ist es manchmal hilfreich, zu testen, um festzustellen, mit welchem Datentyp Sie möglicherweise arbeiten.

Der häufigste Fall ist, dass Sie eine Zeichenfolge in einen anderen Typ konvertieren müssen, z. b. in eine ganze Zahl oder ein Datum. Das folgende Beispiel zeigt einen typischen Fall, in dem Sie eine Zeichenfolge in eine Zahl konvertieren müssen.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Als Regel werden Benutzereingaben als Zeichen folgen angezeigt. Selbst wenn Sie Benutzer zur Eingabe einer Zahl aufgefordert haben und Sie eine Ziffer eingegeben haben, werden die Daten im Zeichen folgen Format angezeigt, wenn die Benutzereingaben übermittelt werden und Sie Sie im Code lesen. Daher müssen Sie die Zeichenfolge in eine Zahl konvertieren. Wenn Sie in diesem Beispiel versuchen, arithmetische Werte für die Werte auszuführen, ohne Sie zu wandeln, wird die folgende Fehlermeldung ausgegeben, da ASP.net nicht zwei Zeichen folgen hinzufügen kann:

*Der Typ "String" kann nicht implizit in "int" konvertiert werden.*

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
    Konvertiert eine Zeichenfolge, die eine ganze Zahl (z. b. "593") darstellt, in eine ganze Zahl.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
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
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Operatoren

Ein Operator ist ein Schlüsselwort oder Zeichen, das ASP.net die Art des Befehls angibt, der in einem Ausdruck durchgeführt werden soll. Die C# Sprache (und die Razor-Syntax, die darauf basiert) unterstützt viele Operatoren, aber Sie müssen nur einige für den Einstieg erkennen. In der folgenden Tabelle werden die gängigsten Operatoren zusammengefasst.

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
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
    Mathematische Operatoren, die in numerischen Ausdrücken verwendet werden.
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    Zuweisung. Weist dem-Objekt auf der linken Seite den Wert auf der rechten Seite einer-Anweisung zu.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    Gleichheit. Gibt `true` zurück, wenn die Werte gleich sind. (Beachten Sie den Unterschied zwischen dem `=`-Operator und dem `==`-Operator.)
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    Ungleichheit. Gibt `true` zurück, wenn die Werte nicht gleich sind.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    "Kleiner als", "größer als", "kleiner als oder gleich", "größer als" oder "gleich".
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    Verkettung, die verwendet wird, um Zeichen folgen zu verknüpfen. ASP.net kennt den Unterschied zwischen diesem Operator und dem Additions Operator auf der Grundlage des Datentyps des Ausdrucks.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
    Die Inkrement-und Dekrementoperatoren, die 1 (bzw.) von einer Variablen addieren bzw. daraus ziehen.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    Klammern. Wird verwendet, um Ausdrücke zu gruppieren und um Parameter an Methoden zu übergeben.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    Angegeben. Wird für den Zugriff auf Werte in Arrays oder Auflistungen verwendet.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    Hätten. Kehrt einen `true` Wert in `false` um und umgekehrt. Wird in der Regel als Kurzform zum Testen auf `false` verwendet (d. h. für nicht `true`).
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&&` `||`
    :::column-end:::
    :::column:::
    Logisches and und or, die zum Verknüpfen von Bedingungen verwendet werden.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
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

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Verweisen auf das virtuelle Stammverzeichnis: der ~-Operator und die href-Methode

In einer *cshtml* -oder *vbhtml* -Datei können Sie mithilfe des `~`-Operators auf den virtuellen Stammpfad verweisen. Dies ist sehr praktisch, da Sie Seiten auf einer Website verschieben können und alle Links, die Sie auf anderen Seiten enthalten, nicht beschädigt werden. Es ist auch praktisch, wenn Sie Ihre Website jemals an einen anderen Speicherort verschieben. Im Folgenden finden Sie einige Beispiele:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Wenn die Website `http://myserver/myapp`ist, so behandelt ASP.net diese Pfade, wenn die Seite ausgeführt wird:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Diese Pfade werden nicht als Werte der Variablen angezeigt, aber ASP.NET behandelt die Pfade so, als wären Sie so.)

Sie können den `~`-Operator sowohl im Servercode (wie oben) als auch im Markup wie folgt verwenden:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

In Markup verwenden Sie den `~`-Operator, um Pfade zu Ressourcen wie Bilddateien, anderen Webseiten und CSS-Dateien zu erstellen. Wenn die Seite ausgeführt wird, durchsucht ASP.net die Seite (sowohl Code als auch Markup) und löst alle `~` Verweise auf den entsprechenden Pfad auf.

## <a name="conditional-logic-and-loops"></a>Bedingte Logik und Schleifen

ASP.net-Servercode ermöglicht das Ausführen von Aufgaben auf der Grundlage von Bedingungen und das Schreiben von Code, der-Anweisungen eine bestimmte Anzahl von Vorkommen wiederholt (d. h. Code, der eine Schleife ausführt).

### <a name="testing-conditions"></a>Testbedingungen

Um eine einfache Bedingung zu testen, verwenden Sie die `if`-Anweisung, die basierend auf einem von Ihnen angegebenen Test true oder false zurückgibt:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

Das `if`-Schlüsselwort startet einen-Block. Der tatsächliche Test (Bedingung) wird in Klammern gesetzt und gibt true oder false zurück. Die Anweisungen, die ausgeführt werden, wenn der Test true ist, werden in geschweifte Klammern eingeschlossen. Eine `if`-Anweisung kann einen `else`-Block enthalten, der Anweisungen angibt, die ausgeführt werden sollen, wenn die Bedingung false ist:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Sie können mit einem `else if`-Block mehrere Bedingungen hinzufügen:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

Wenn in diesem Beispiel die erste Bedingung im If-Block nicht true ist, wird die `else if` Bedingung geprüft. Wenn diese Bedingung erfüllt ist, werden die Anweisungen im `else if`-Block ausgeführt. Wenn keine der Bedingungen erfüllt ist, werden die Anweisungen im `else`-Block ausgeführt. Sie können eine beliebige Anzahl von else-if-Blöcken hinzufügen und dann mit einem `else`-Block als &quot;alle anderen&quot; Bedingung schließen.

Um eine große Anzahl von Bedingungen zu testen, verwenden Sie einen `switch`-Block:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

Der zu testende Wert ist in Klammern (im Beispiel die `weekday` Variable). Jeder einzelne Test verwendet eine `case`-Anweisung, die mit einem Doppelpunkt endet (:). Wenn der Wert einer `case`-Anweisung mit dem Testwert übereinstimmt, wird der Code in diesem Fall Block ausgeführt. Sie schließen jede Case-Anweisung mit einer `break`-Anweisung. (Wenn Sie vergessen, Unterbrechung in jedem `case` Block einzuschließen, wird der Code der nächsten `case` Anweisung ebenfalls ausgeführt.) Ein `switch` Block verfügt häufig über eine `default`-Anweisung als letzter Fall für eine &quot;alles andere&quot; Option, die ausgeführt wird, wenn keiner der anderen Fälle true ist.

Das Ergebnis der letzten beiden bedingten Blöcke, die in einem Browser angezeigt werden:

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Schleifen Code

Häufig müssen Sie dieselben Anweisungen wiederholt ausführen. Dies geschieht durch Schleifen. Beispielsweise führen Sie häufig die gleichen Anweisungen für jedes Element in einer Datensammlung aus. Wenn Sie genau wissen, wie oft Sie eine Schleife durchführen möchten, können Sie eine `for` Schleife verwenden. Diese Art von Schleife ist besonders nützlich, wenn Sie nach oben oder unten gezählt werden:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

Die Schleife beginnt mit dem `for`-Schlüsselwort, gefolgt von drei Anweisungen in Klammern, die jeweils mit einem Semikolon beendet werden.

- Innerhalb der Klammern erstellt die erste Anweisung (`var i=10;`) einen Counter und initialisiert sie mit 10. Sie müssen den Counter nicht benennen, `i` &#8212; Sie beliebige Variablen verwenden können. Wenn die `for`-Schleife ausgeführt wird, wird der-Wert automatisch inkrementiert.
- Mit der zweiten Anweisung (`i < 21;`) wird die Bedingung für die gewünschte Anzahl festgelegt. In diesem Fall möchten Sie, dass der Wert maximal 20 beträgt (d. h., der Wert ist kleiner als 21).
- Die dritte Anweisung (`i++`) verwendet einen Inkrementoperator, der lediglich angibt, dass der-Wert bei jedem Ausführen der Schleife 1 hinzugefügt werden soll.

Innerhalb der geschweiften Klammern befindet sich der Code, der für jede Iterations Schleife ausgeführt wird. Das Markup erstellt jedes Mal einen neuen Absatz (`<p>`-Element) und fügt der Ausgabe eine Zeile hinzu, in der der Wert `i` (der Leistungs Bestellungs Wert) angezeigt wird. Wenn Sie diese Seite ausführen, erstellt das Beispiel 11 Zeilen, in denen die Ausgabe angezeigt wird, wobei der Text in jeder Zeile die Element Nummer angibt.

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

Wenn Sie mit einer Sammlung oder einem Array arbeiten, verwenden Sie häufig eine `foreach` Schleife. Eine Sammlung ist eine Gruppe von ähnlichen Objekten, und mit der `foreach`-Schleife können Sie für jedes Element in der Auflistung eine Aufgabe ausführen. Diese Art von Schleife eignet sich für Auflistungen, da Sie im Gegensatz zu einer `for` Schleife den Wert nicht erhöhen oder eine Beschränkung festlegen müssen. Stattdessen durchläuft der `foreach` Schleifen Code einfach die Auflistung, bis er abgeschlossen ist.

Der folgende Code gibt beispielsweise die Elemente in der `Request.ServerVariables`-Auflistung zurück, bei der es sich um ein Objekt handelt, das Informationen über den Webserver enthält. Er verwendet eine `foreac` h-Schleife, um den Namen der einzelnen Elemente anzuzeigen, indem er ein neues `<li>` Element in einer Liste mit HTML-Auflistungs Zeichen erstellt.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

Auf das `foreach` Schlüsselwort folgen Klammern, in denen Sie eine Variable deklarieren, die ein einzelnes Element in der Auflistung darstellt (im Beispiel `var item`), gefolgt vom `in` Schlüsselwort, gefolgt von der Auflistung, die Sie durchlaufen möchten. Im Text der `foreach` Schleife können Sie mithilfe der Variablen, die Sie zuvor deklariert haben, auf das aktuelle Element zugreifen.

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

Verwenden Sie die `while`-Anweisung, um eine allgemeinere Schleife zu erstellen:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

Eine `while` Schleife beginnt mit dem Schlüsselwort "`while`", gefolgt von Klammern, in denen Sie angeben, wie lange die Schleife fortgesetzt wird (hier, solange `countNum` kleiner als 50 ist), und dann den Block, der wiederholt werden soll. Schleifen werden in der Regel in einer Variablen oder einem Objekt, das für das zählen verwendet wird, Inkrement (zu) oder Dekrement (subtrahieren Im Beispiel fügt der `+=`-Operator bei jeder Ausführung der Schleife 1 `countNum` hinzu. (Um eine Variable in einer Schleife, die Sie anzählt, herabzusetzen, verwenden Sie den Dekrementoperator `-=`).

## <a name="objects-and-collections"></a>Objekte und Auflistungen

Fast alles auf einer ASP.NET-Website ist ein Objekt, das auch die Webseite selbst enthält. In diesem Abschnitt werden einige wichtige Objekte erläutert, mit denen Sie häufig in Ihrem Code arbeiten werden.

### <a name="page-objects"></a>Seiten Objekte

Das grundlegendste Objekt in ASP.net ist die Seite. Sie können auf die Eigenschaften des Seiten Objekts direkt ohne qualifizierendes Objekt zugreifen. Der folgende Code Ruft den Dateipfad der Seite ab, wobei das `Request` Objekt der Seite verwendet wird:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Um das Festlegen von Eigenschaften und Methoden für das aktuelle Seiten Objekt zu vereinfachen, können Sie optional das Schlüsselwort `this` verwenden, um das Seiten Objekt in Ihrem Code darzustellen. Im folgenden finden Sie das vorherige Codebeispiel, in dem `this` zur Darstellung der Seite hinzugefügt wurde:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Sie können die Eigenschaften des `Page`-Objekts verwenden, um zahlreiche Informationen zu erhalten, z. b.:

- [https://login.microsoftonline.com/consumers/](`Request`). Wie Sie bereits gesehen haben, handelt es sich hierbei um eine Sammlung von Informationen über die aktuelle Anforderung, einschließlich des Typs des Browsers, der die Anforderung gestellt hat, der URL der Seite, der Benutzeridentität usw.
- [https://login.microsoftonline.com/consumers/](`Response`). Dabei handelt es sich um eine Sammlung von Informationen über die Antwort (Seite), die an den Browser gesendet werden, wenn die Ausführung des Server Codes abgeschlossen ist. Beispielsweise können Sie diese Eigenschaft verwenden, um Informationen in die Antwort zu schreiben. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Sammlungsobjekte (Arrays und Wörterbücher)

Eine *Sammlung* ist eine Gruppe von Objekten desselben Typs, z. b. eine Auflistung von `Customer` Objekten aus einer Datenbank. ASP.NET enthält viele integrierte Auflistungen, wie z. b. die `Request.Files` Auflistung.

Häufig arbeiten Sie mit Daten in Auflistungen. Zwei gängige Auflistungs Typen sind das- *Array* und das- *Wörterbuch*. Ein Array ist nützlich, wenn Sie eine Sammlung ähnlicher Elemente speichern möchten, aber keine separate Variable zum Speichern der einzelnen Elemente erstellen möchten:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Mit Arrays deklarieren Sie einen bestimmten Datentyp, z. b. `string`, `int`oder `DateTime`. Um anzugeben, dass die Variable ein Array enthalten kann, fügen Sie der Deklaration eckige Klammern hinzu (z. b. `string[]` oder `int[]`). Sie können auf Elemente in einem Array zugreifen, indem Sie Ihre Position (Index) oder die `foreach`-Anweisung verwenden. Array Indizes sind NULL basiert &#8212; , d. h., das erste Element befindet sich an Position 0, das zweite Element an Position 1 usw.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Sie können die Anzahl der Elemente in einem Array ermitteln, indem Sie die `Length`-Eigenschaft erhalten. Um die Position eines bestimmten Elements im Array (zum Durchsuchen des Arrays) zu erhalten, verwenden Sie die `Array.IndexOf`-Methode. Sie können auch den Inhalt eines Arrays umkehren (die `Array.Reverse`-Methode) oder den Inhalt (die `Array.Sort`-Methode) sortieren.

Die Ausgabe des Zeichen folgen Array Codes, der in einem Browser angezeigt wird:

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Ein Wörterbuch ist eine Auflistung von Schlüssel-Wert-Paaren, bei der Sie den Schlüssel (oder Namen) bereitstellen, um den entsprechenden Wert festzulegen oder abzurufen:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Zum Erstellen eines Wörterbuchs verwenden Sie das `new`-Schlüsselwort, um anzugeben, dass Sie ein neues Dictionary-Objekt erstellen. Mit dem `var`-Schlüsselwort können Sie einer Variablen ein Wörterbuch zuweisen. Die Datentypen der Elemente im Wörterbuch werden mithilfe von spitzen Klammern (`< >`) angegeben. Am Ende der Deklaration müssen Sie ein paar Klammern hinzufügen, da es sich hierbei tatsächlich um eine Methode handelt, die ein neues Wörterbuch erstellt.

Um dem Wörterbuch Elemente hinzuzufügen, können Sie die `Add`-Methode der Wörterbuch Variablen (in diesem Fall`myScores`) und dann einen Schlüssel und einen Wert angeben. Alternativ können Sie eckige Klammern verwenden, um den Schlüssel anzugeben und eine einfache Zuweisung durchzuführen, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Um einen Wert aus dem Wörterbuch zu erhalten, geben Sie den Schlüssel in eckige Klammern an:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Aufrufen von Methoden mit Parametern

Wie Sie zuvor in diesem Artikel gelesen haben, können die Objekte, mit denen Sie programmieren, über Methoden verfügen. Beispielsweise kann ein `Database` Objekt über eine `Database.Connect`-Methode verfügen. Viele Methoden verfügen auch über einen oder mehrere Parameter. Ein *Parameter* ist ein Wert, den Sie an eine-Methode übergeben, um die-Methode zum Abschließen der Aufgabe zu aktivieren. Betrachten Sie z. b. eine Deklaration für die `Request.MapPath`-Methode, die drei Parameter annimmt:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(Die Zeile wurde umschließt, um Sie besser lesbar zu machen. Denken Sie daran, dass Sie Zeilenumbrüche fast an jeder Stelle platzieren können, außer in Zeichen folgen, die in Anführungszeichen eingeschlossen sind.)

Diese Methode gibt den physischen Pfad auf dem Server zurück, der einem angegebenen virtuellen Pfad entspricht. Die drei Parameter für die-Methode sind `virtualPath`, `baseVirtualDir`und `allowCrossAppMapping`. (Beachten Sie, dass in der-Deklaration die Parameter mit den Datentypen der Daten aufgelistet werden, die Sie akzeptieren.) Wenn Sie diese Methode aufgerufen haben, müssen Sie Werte für alle drei Parameter angeben.

Der Razor-Syntax bietet Ihnen zwei Optionen zum Übergeben von Parametern an eine Methode: *Positions Parameter* und *benannte Parameter*. Um eine Methode mit Positions Parametern aufzurufen, übergeben Sie die Parameter in einer strengen Reihenfolge, die in der Methoden Deklaration angegeben ist. (In der Regel wissen Sie diese Bestellung, indem Sie die Dokumentation für die-Methode lesen.) Sie müssen die Reihenfolge befolgen, und Sie können die Parameter &#8212; bei Bedarf nicht überspringen. Sie übergeben eine leere Zeichenfolge (`""`) oder `null` für einen positionellen Parameter, für den Sie keinen Wert besitzen.

Im folgenden Beispiel wird davon ausgegangen, dass Sie über einen Ordner namens *Scripts* auf Ihrer Website verfügen. Der Code Ruft die `Request.MapPath`-Methode auf und übergibt Werte für die drei Parameter in der richtigen Reihenfolge. Daraufhin wird der resultierende zugeordnete Pfad angezeigt.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Wenn eine Methode über viele Parameter verfügt, können Sie Ihren Code mit benannten Parametern besser lesbar halten. Um eine Methode mit benannten Parametern aufzurufen, geben Sie den Parameternamen gefolgt von einem Doppelpunkt (:) und dann den Wert an. Der Vorteil von benannten Parametern besteht darin, dass Sie Sie in beliebiger Reihenfolge übergeben können. (Ein Nachteil ist, dass der Methoden Aufrufwert nicht so kompakt ist.)

Im folgenden Beispiel wird dieselbe Methode wie oben aufgerufen, es werden jedoch benannte Parameter verwendet, um die Werte bereitzustellen:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Wie Sie sehen können, werden die Parameter in einer anderen Reihenfolge übermittelt. Wenn Sie jedoch das vorherige Beispiel und dieses Beispiel ausführen, wird derselbe Wert zurückgegeben.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Behandeln von Fehlern

### <a name="try-catch-statements"></a>Try-catch-Anweisungen

Sie verfügen häufig über Anweisungen in Ihrem Code, die aus Gründen außerhalb ihrer Kontrolle fehlschlagen können. Beispiel:

- Wenn Ihr Code versucht, eine Datei zu erstellen oder darauf zuzugreifen, können alle möglichen Fehler auftreten. Die gewünschte Datei ist möglicherweise nicht vorhanden, Sie ist möglicherweise gesperrt, der Code verfügt möglicherweise nicht über Berechtigungen usw.
- Wenn Ihr Code versucht, Datensätze in einer Datenbank zu aktualisieren, können auch Berechtigungsprobleme auftreten, die Verbindung mit der Datenbank kann gelöscht werden, die zu speichernden Daten sind möglicherweise ungültig usw.

In Programmier begriffen werden diese Situationen als *Ausnahmen*bezeichnet. Wenn im Code eine Ausnahme auftritt, wird eine Fehlermeldung generiert (ausgelöst), die für Benutzer am besten geeignet ist:

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

In Situationen, in denen in Ihrem Code Ausnahmen auftreten können, und um Fehlermeldungen dieses Typs zu vermeiden, können Sie `try/catch`-Anweisungen verwenden. In der `try`-Anweisung führen Sie den Code aus, den Sie überprüfen. In mindestens einer `catch`-Anweisung können Sie nach bestimmten Fehlern (bestimmte Typen von Ausnahmen) suchen, die möglicherweise aufgetreten sind. Sie können so viele `catch` Anweisungen einschließen, wie Sie nach Fehlern suchen müssen, die Sie erwarten.

> [!NOTE]
> Es wird empfohlen, die Verwendung der `Response.Redirect`-Methode in `try/catch` Anweisungen zu vermeiden, da dies eine Ausnahme in der Seite verursachen kann.

Das folgende Beispiel zeigt eine Seite, die bei der ersten Anforderung eine Textdatei erstellt und dann eine Schaltfläche anzeigt, mit der der Benutzer die Datei öffnen kann. Im Beispiel wird absichtlich ein ungültiger Dateiname verwendet, damit eine Ausnahme ausgelöst wird. Der Code enthält `catch` Anweisungen für zwei mögliche Ausnahmen: `FileNotFoundException`, bei einem ungültigen Dateinamen und `DirectoryNotFoundException`. Dies tritt auf, wenn ASP.NET den Ordner nicht selbst finden kann. (Sie können die Auskommentierung einer Anweisung im Beispiel aufheben, um zu sehen, wie Sie ausgeführt wird, wenn alles ordnungsgemäß funktioniert.)

Wenn der Code die Ausnahme nicht behandelt hat, sehen Sie eine Fehlerseite wie der vorherige Screenshot. Mit dem `try/catch` Abschnitt wird jedoch verhindert, dass der Benutzer diese Fehlertypen erkennen kann.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

**Programmieren mit Visual Basic**

[Anhang: Visual Basic Sprache und Syntax](https://go.microsoft.com/fwlink/?LinkId=202908)

**Referenzdokumentation**

[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[C#Kurse](https://msdn.microsoft.com/library/kx37x362.aspx)
