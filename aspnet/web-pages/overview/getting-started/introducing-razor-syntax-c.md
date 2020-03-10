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
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="5f462-104">Einführung in ASP.net-Webprogrammierung mit der Razor-C#Syntax ()</span><span class="sxs-lookup"><span data-stu-id="5f462-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>

<span data-ttu-id="5f462-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5f462-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5f462-106">In diesem Artikel erhalten Sie einen Überblick über die Programmierung mit ASP.net Web Pages mithilfe des Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="5f462-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="5f462-107">ASP.net ist die Technologie von Microsoft zum Ausführen dynamischer Webseiten auf Webservern.</span><span class="sxs-lookup"><span data-stu-id="5f462-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="5f462-108">Dieser Artikel konzentriert sich auf die C# Verwendung der Programmiersprache.</span><span class="sxs-lookup"><span data-stu-id="5f462-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="5f462-109">**Lernen Sie**Folgendes:</span><span class="sxs-lookup"><span data-stu-id="5f462-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="5f462-110">Die ersten 8 Programmiertipps für den Einstieg in die Programmierung ASP.net Web Pages mithilfe Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="5f462-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="5f462-111">Grundlegende Programmier Konzepte, die Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="5f462-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="5f462-112">Was ist ASP.net Servercode und der Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="5f462-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="5f462-113">Software Versionen</span><span class="sxs-lookup"><span data-stu-id="5f462-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="5f462-114">ASP.net Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5f462-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5f462-115">Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="5f462-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="the-top-8-programming-tips"></a><span data-ttu-id="5f462-116">Die Top 8-Programmiertipps</span><span class="sxs-lookup"><span data-stu-id="5f462-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="5f462-117">Dieser Abschnitt enthält einige Tipps, die Sie unbedingt kennen müssen, wenn Sie mit dem Schreiben von ASP.net-Servercode mit dem Razor-Syntax beginnen.</span><span class="sxs-lookup"><span data-stu-id="5f462-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="5f462-118">Der Razor-Syntax basiert auf der C# Programmiersprache, und das ist die Sprache, die am häufigsten mit ASP.net Web Pages verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="5f462-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="5f462-119">Der Razor-Syntax unterstützt jedoch auch die Visual Basic Sprache und alles, was Sie sehen können, auch in Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="5f462-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="5f462-120">Weitere Informationen finden Sie im Anhang [Visual Basic Sprache und Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span><span class="sxs-lookup"><span data-stu-id="5f462-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>

<span data-ttu-id="5f462-121">Weitere Details zu den meisten dieser Programmiertechniken finden Sie später in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="5f462-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="5f462-122">1. Sie fügen Code zu einer Seite mit dem @-Zeichen hinzu.</span><span class="sxs-lookup"><span data-stu-id="5f462-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="5f462-123">Das `@` Zeichen startet Inline Ausdrücke, einzelne Anweisungsblöcke und Blöcke mit mehreren Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="5f462-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="5f462-124">Diese Anweisungen sehen wie folgt aus, wenn die Seite in einem Browser ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="5f462-124">This is what these statements look like when the page runs in a browser:</span></span>

![Razor-Bild1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="5f462-126">**HTML-Codierung**</span><span class="sxs-lookup"><span data-stu-id="5f462-126">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="5f462-127">Wenn Sie Inhalt in einer Seite mit dem `@` Zeichen anzeigen, wie in den vorherigen Beispielen, codiert ASP.net die Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="5f462-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="5f462-128">Dies ersetzt reservierte HTML-Zeichen (z. b. `<` und `>` und `&`) durch Codes, die es ermöglichen, dass die Zeichen als Zeichen in einer Webseite angezeigt werden, anstatt als HTML-Tags oder-Entitäten interpretiert zu werden.</span><span class="sxs-lookup"><span data-stu-id="5f462-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="5f462-129">Ohne HTML-Codierung wird die Ausgabe des Server Codes möglicherweise nicht ordnungsgemäß angezeigt und kann eine Seite für Sicherheitsrisiken verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="5f462-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="5f462-130">Wenn das Ziel darin besteht, HTML-Markup auszugeben, das Tags als Markup rendert (z. b. `<p></p>` für einen Absatz oder `<em></em>`, um Text hervorzuheben), lesen Sie den Abschnitt [Kombinieren von Text, Markup und Code in Code Blöcken](#BM_CombiningTextMarkupAndCode) weiter unten in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="5f462-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="5f462-131">Weitere Informationen zur HTML-Codierung finden Sie unter [Arbeiten mit Formularen](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="5f462-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>

### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="5f462-132">2. Sie schließen Code Blöcke in geschweiften Klammern ein.</span><span class="sxs-lookup"><span data-stu-id="5f462-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="5f462-133">Ein *Codeblock* enthält mindestens eine Code Anweisung und ist in geschweifte Klammern eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="5f462-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="5f462-134">Das in einem Browser angezeigte Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="5f462-134">The result displayed in a browser:</span></span>

![Razor-Bild2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="5f462-136">3. innerhalb eines-Blocks beenden Sie jede Code Anweisung mit einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="5f462-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="5f462-137">In einem Codeblock muss jede Complete Code-Anweisung mit einem Semikolon enden.</span><span class="sxs-lookup"><span data-stu-id="5f462-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="5f462-138">Inline Ausdrücke enden nicht mit einem Semikolon.</span><span class="sxs-lookup"><span data-stu-id="5f462-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="5f462-139">4. Sie verwenden Variablen zum Speichern von Werten.</span><span class="sxs-lookup"><span data-stu-id="5f462-139">4. You use variables to store values</span></span>

<span data-ttu-id="5f462-140">Sie können Werte in einer *Variablen*speichern, einschließlich Zeichen folgen, Zahlen und Datumsangaben usw. Sie erstellen eine neue Variable mit dem `var`-Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="5f462-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="5f462-141">Sie können Variablen Werte direkt in eine Seite einfügen, indem Sie `@`verwenden.</span><span class="sxs-lookup"><span data-stu-id="5f462-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="5f462-142">Das in einem Browser angezeigte Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="5f462-142">The result displayed in a browser:</span></span>

![Razor-Bild3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="5f462-144">5. Sie schließen Literalzeichenfolgen-Werte in doppelte Anführungszeichen ein.</span><span class="sxs-lookup"><span data-stu-id="5f462-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="5f462-145">Eine *Zeichen* Folge ist eine Sequenz von Zeichen, die als Text behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="5f462-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="5f462-146">Um eine Zeichenfolge anzugeben, müssen Sie Sie in doppelte Anführungszeichen einschließen:</span><span class="sxs-lookup"><span data-stu-id="5f462-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="5f462-147">Wenn die anzuzeigende Zeichenfolge einen umgekehrten Schrägstrich (`\`) oder doppelte Anführungszeichen (`"`) enthält, verwenden Sie einen *ausführlichen Zeichenfolgenliteralwert* , der dem `@`-Operator vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="5f462-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="5f462-148">(In C#hat das Zeichen \ eine besondere Bedeutung, es sei denn, Sie verwenden einen ausführlichen Zeichenfolgenliterals.</span><span class="sxs-lookup"><span data-stu-id="5f462-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="5f462-149">Um doppelte Anführungszeichen einzubetten, verwenden Sie einen ausführlichen Zeichenfolgenliteralwert und wiederholen die Anführungszeichen</span><span class="sxs-lookup"><span data-stu-id="5f462-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="5f462-150">Im folgenden finden Sie das Ergebnis der Verwendung beider Beispiele in einer Seite:</span><span class="sxs-lookup"><span data-stu-id="5f462-150">Here's the result of using both of these examples in a page:</span></span>

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="5f462-152">Beachten Sie, dass das `@` Zeichen verwendet wird, um ausführliche Zeichen folgen Literale in C# und zu kennzeichnen, um Code in ASP.NET Seiten zu markieren.</span><span class="sxs-lookup"><span data-stu-id="5f462-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>

### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="5f462-153">6. bei Code wird Groß-/Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="5f462-153">6. Code is case sensitive</span></span>

<span data-ttu-id="5f462-154">In C#wird bei Schlüsselwörtern (z. b. `var`, `true`und `if`) und Variablennamen die Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="5f462-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="5f462-155">In den folgenden Codezeilen werden zwei verschiedene Variablen erstellt: `lastName` und `LastName.`</span><span class="sxs-lookup"><span data-stu-id="5f462-155">The following lines of code create two different variables, `lastName` and `LastName.`</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="5f462-156">Wenn Sie eine Variable als `var lastName = "Smith";` deklarieren und versuchen, auf die Variable auf der Seite als `@LastName`zu verweisen, erhalten Sie den Wert `"Jones"` anstelle von `"Smith"`.</span><span class="sxs-lookup"><span data-stu-id="5f462-156">If you declare a variable as `var lastName = "Smith";` and try to reference that variable in your page as `@LastName`, you would get the value `"Jones"` instead of `"Smith"`.</span></span>

> [!NOTE]
> <span data-ttu-id="5f462-157">In Visual Basic wird die Groß-/Kleinschreibung *nicht* beachtet.</span><span class="sxs-lookup"><span data-stu-id="5f462-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>

### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="5f462-158">7. ein Großteil ihrer Codierung umfasst Objekte</span><span class="sxs-lookup"><span data-stu-id="5f462-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="5f462-159">Ein- *Objekt* stellt eine Aufgabe dar, mit &#8212; der Sie eine Seite, ein Textfeld, eine Datei, ein Bild, eine Webanforderung, eine e-Mail-Nachricht, einen Kundendaten Satz (Datenbankzeile) usw. programmieren können. Objekte verfügen über Eigenschaften, die ihre Merkmale beschreiben, und Sie können ein &#8212; Textfeld-`Text` Objekt lesen oder ändern (unter anderem), ein Request-Objekt verfügt über eine `Url`-Eigenschaft, eine e-Mail-Nachricht über eine `From`-Eigenschaft, und ein Customer-Objekt verfügt über eine `FirstName`-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="5f462-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="5f462-160">-Objekte verfügen auch über Methoden, die die &quot;Verben&quot; können, die Sie ausführen können.</span><span class="sxs-lookup"><span data-stu-id="5f462-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="5f462-161">Beispiele hierfür sind die `Save`-Methode eines File-Objekts, die `Rotate`-Methode eines Bildobjekts und die `Send`-Methode eines e-Mail-Objekts.</span><span class="sxs-lookup"><span data-stu-id="5f462-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="5f462-162">Häufig arbeiten Sie mit dem `Request` Objekt, das Ihnen Informationen wie die Werte von Textfeldern (Formularfelder) auf der Seite, den Typ des Browsers, der die Anforderung durchgeführt hat, die URL der Seite, die Benutzeridentität usw. enthält. Im folgenden Beispiel wird gezeigt, wie Sie auf Eigenschaften des `Request` Objekts zugreifen und wie Sie die `MapPath`-Methode des `Request`-Objekts aufrufen, das Ihnen den absoluten Pfad der Seite auf dem Server liefert:</span><span class="sxs-lookup"><span data-stu-id="5f462-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="5f462-163">Das in einem Browser angezeigte Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="5f462-163">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="5f462-165">8. Sie können Code schreiben, der Entscheidungen trifft.</span><span class="sxs-lookup"><span data-stu-id="5f462-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="5f462-166">Ein wichtiges Feature von dynamischen Webseiten ist, dass Sie anhand der Bedingungen ermitteln können, was zu tun ist.</span><span class="sxs-lookup"><span data-stu-id="5f462-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="5f462-167">Die gängigste Methode hierfür ist die `if`-Anweisung (und optional `else`-Anweisung).</span><span class="sxs-lookup"><span data-stu-id="5f462-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="5f462-168">Die-Anweisung `if(IsPost)` ist eine Kurzform zum Schreiben von `if(IsPost == true)`.</span><span class="sxs-lookup"><span data-stu-id="5f462-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="5f462-169">Zusammen mit `if`-Anweisungen gibt es eine Vielzahl von Möglichkeiten, Bedingungen zu testen, Code Blöcke zu wiederholen usw., die weiter unten in diesem Artikel beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="5f462-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="5f462-170">Das in einem Browser angezeigte Ergebnis (nach dem Klicken auf " **senden**"):</span><span class="sxs-lookup"><span data-stu-id="5f462-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="5f462-172">HTTP Get-und Post-Methoden und die ispost-Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="5f462-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="5f462-173">Das Protokoll, das für Webseiten (http) verwendet wird, unterstützt eine sehr begrenzte Anzahl von Methoden (Verben), die verwendet werden, um Anforderungen an den Server zu senden.</span><span class="sxs-lookup"><span data-stu-id="5f462-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="5f462-174">Die beiden häufigsten sind Get, das zum Lesen einer Seite verwendet wird, und Post, der zum Senden einer Seite verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="5f462-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="5f462-175">Wenn ein Benutzer zum ersten Mal eine Seite anfordert, wird die Seite mithilfe von Get angefordert.</span><span class="sxs-lookup"><span data-stu-id="5f462-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="5f462-176">Wenn der Benutzer ein Formular ausfüllt und dann auf die Schaltfläche Senden klickt, sendet der Browser eine Post-Anforderung an den Server.</span><span class="sxs-lookup"><span data-stu-id="5f462-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="5f462-177">Bei der Webprogrammierung ist es häufig hilfreich zu wissen, ob eine Seite als Get-oder Post-Vorgang angefordert wird, damit Sie wissen, wie die Seite verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="5f462-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="5f462-178">In ASP.net Web Pages können Sie die `IsPost`-Eigenschaft verwenden, um festzustellen, ob eine Anforderung ein Get-oder Post-Eintrag ist.</span><span class="sxs-lookup"><span data-stu-id="5f462-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="5f462-179">Wenn es sich bei der Anforderung um einen Post-Vorgang handelt, wird die `IsPost`-Eigenschaft "true" zurückgegeben. Sie können z. b. die Werte von Textfeldern in einem Formular lesen.</span><span class="sxs-lookup"><span data-stu-id="5f462-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="5f462-180">Viele Beispiele zeigen Ihnen, wie Sie die Seite je nach dem Wert `IsPost`anders verarbeiten können.</span><span class="sxs-lookup"><span data-stu-id="5f462-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>

## <a name="a-simple-code-example"></a><span data-ttu-id="5f462-181">Einfaches Code Beispiel</span><span class="sxs-lookup"><span data-stu-id="5f462-181">A Simple Code Example</span></span>

<span data-ttu-id="5f462-182">In diesem Verfahren wird gezeigt, wie Sie eine Seite erstellen, die grundlegende Programmiertechniken veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="5f462-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="5f462-183">In diesem Beispiel erstellen Sie eine Seite, mit der Benutzer zwei Zahlen eingeben können. Anschließend werden Sie hinzugefügt, und das Ergebnis wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5f462-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="5f462-184">Erstellen Sie im Editor eine neue Datei, und nennen Sie Sie " *AddNumbers. cshtml*".</span><span class="sxs-lookup"><span data-stu-id="5f462-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="5f462-185">Kopieren Sie den folgenden Code und das Markup in die Seite, und ersetzen Sie alles, was bereits auf der Seite ist.</span><span class="sxs-lookup"><span data-stu-id="5f462-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="5f462-186">Beachten Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="5f462-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="5f462-187">Mit dem `@` Zeichen wird der erste Codeblock auf der Seite gestartet, und vor der `totalMessage` Variablen, die am unteren Rand der Seite eingebettet ist.</span><span class="sxs-lookup"><span data-stu-id="5f462-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="5f462-188">Der Block am oberen Rand der Seite wird in geschweifte Klammern eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="5f462-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="5f462-189">Im Block am oberen Ende werden alle Zeilen mit einem Semikolon enden.</span><span class="sxs-lookup"><span data-stu-id="5f462-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="5f462-190">In den Variablen `total`, `num1`, `num2`und `totalMessage` werden mehrere Zahlen und eine Zeichenfolge gespeichert.</span><span class="sxs-lookup"><span data-stu-id="5f462-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="5f462-191">Der Literalzeichenfolgen-Wert, der der `totalMessage` Variablen zugewiesen ist, ist in doppelten Anführungszeichen.</span><span class="sxs-lookup"><span data-stu-id="5f462-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="5f462-192">Da im Code die Groß-/Kleinschreibung beachtet wird, muss der Name, wenn die `totalMessage` Variable im unteren Bereich der Seite verwendet wird, genau mit der Variablen oben übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="5f462-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="5f462-193">Der Ausdruck `num1.AsInt() + num2.AsInt()` zeigt, wie Sie mit Objekten und Methoden arbeiten.</span><span class="sxs-lookup"><span data-stu-id="5f462-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="5f462-194">Die `AsInt`-Methode für jede Variable konvertiert die von einem Benutzer eingegebene Zeichenfolge in eine Zahl (eine ganze Zahl), sodass Sie arithmetische Operationen ausführen können.</span><span class="sxs-lookup"><span data-stu-id="5f462-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="5f462-195">Das `<form>`-Tag enthält ein `method="post"` Attribut.</span><span class="sxs-lookup"><span data-stu-id="5f462-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="5f462-196">Dies gibt an, dass die Seite mithilfe der HTTP Post-Methode an den Server gesendet wird, wenn der Benutzer auf **Hinzufügen**klickt.</span><span class="sxs-lookup"><span data-stu-id="5f462-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="5f462-197">Wenn die Seite übermittelt wird, wird der `if(IsPost)` Test als true ausgewertet, und der bedingte Code wird ausgeführt, und das Ergebnis des Hinzufügens der Zahlen wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5f462-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="5f462-198">Speichern Sie die Seite, und führen Sie Sie in einem Browser aus.</span><span class="sxs-lookup"><span data-stu-id="5f462-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="5f462-199">(Stellen Sie sicher, dass die Seite im Arbeitsbereich " **Dateien** " ausgewählt ist, bevor Sie Sie ausführen.) Geben Sie zwei ganze Zahlen ein, und klicken Sie dann auf **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="5f462-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="5f462-201">Grundlegende Programmier Konzepte</span><span class="sxs-lookup"><span data-stu-id="5f462-201">Basic Programming Concepts</span></span>

<span data-ttu-id="5f462-202">Dieser Artikel bietet eine Übersicht über die ASP.net-Webprogrammierung.</span><span class="sxs-lookup"><span data-stu-id="5f462-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="5f462-203">Es handelt sich nicht um eine umfassende Prüfung, sondern nur eine kurze Tour durch die Programmier Konzepte, die Sie am häufigsten verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="5f462-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="5f462-204">Auch hier wird fast alles behandelt, was Sie für den Einstieg in ASP.net Web Pages benötigen.</span><span class="sxs-lookup"><span data-stu-id="5f462-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="5f462-205">Aber zuerst ein wenig technischer Hintergrund.</span><span class="sxs-lookup"><span data-stu-id="5f462-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="5f462-206">Die Razor-Syntax, der Server Code und ASP.net</span><span class="sxs-lookup"><span data-stu-id="5f462-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="5f462-207">Razor-Syntax ist eine einfache Programmier Syntax zum Einbetten von Server basiertem Code in einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="5f462-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="5f462-208">Auf einer Webseite, die die Razor-Syntax verwendet, gibt es zwei Arten von Inhalten: Client Inhalt und Servercode.</span><span class="sxs-lookup"><span data-stu-id="5f462-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="5f462-209">Client Inhalt ist das, was Sie in Webseiten verwenden: HTML-Markup (Elemente), Formatinformationen wie CSS, vielleicht einige Client Skripts wie JavaScript und nur-Text.</span><span class="sxs-lookup"><span data-stu-id="5f462-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="5f462-210">Mit Razor-Syntax können Sie diesem Client Inhalt Servercode hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="5f462-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="5f462-211">Wenn auf der Seite Servercode vorhanden ist, führt der Server den Code zuerst aus, bevor die Seite an den Browser gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="5f462-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="5f462-212">Durch die Ausführung auf dem Server kann der Code Aufgaben ausführen, die für die Verwendung von Client Inhalten allein sehr viel komplexer sein können, z. b. den Zugriff auf serverbasierte Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="5f462-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="5f462-213">Der wichtigste Punkt ist, dass der Servercode Client &#8212; Inhalte dynamisch erstellen kann, um HTML-Markup oder anderen Inhalt dynamisch zu generieren und ihn dann zusammen mit jedem statischen HTML-Code, den die Seite enthält, an den Browser zu senden.</span><span class="sxs-lookup"><span data-stu-id="5f462-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="5f462-214">Aus Sicht des Browsers unterscheidet sich der Client Inhalt, der vom Servercode generiert wird, nicht von anderen Client Inhalten.</span><span class="sxs-lookup"><span data-stu-id="5f462-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="5f462-215">Wie Sie bereits gesehen haben, ist der erforderliche Servercode recht einfach.</span><span class="sxs-lookup"><span data-stu-id="5f462-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="5f462-216">ASP.NET Webseiten, die die Razor-Syntax enthalten, haben eine spezielle Dateierweiterung ( *. cshtml* oder *. vbhtml*).</span><span class="sxs-lookup"><span data-stu-id="5f462-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="5f462-217">Der Server erkennt diese Erweiterungen, führt den Code aus, der mit Razor-Syntax gekennzeichnet ist, und sendet die Seite dann an den Browser.</span><span class="sxs-lookup"><span data-stu-id="5f462-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="5f462-218">Wo ist ASP.net passt?</span><span class="sxs-lookup"><span data-stu-id="5f462-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="5f462-219">Razor-Syntax basiert auf einer Technologie von Microsoft namens ASP.net, die wiederum auf dem Microsoft .NET Framework basiert.</span><span class="sxs-lookup"><span data-stu-id="5f462-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="5f462-220">The.NET Framework ist ein großes, umfassendes Programmier Framework von Microsoft für die Entwicklung von praktisch jeder Art von Computeranwendung.</span><span class="sxs-lookup"><span data-stu-id="5f462-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="5f462-221">ASP.net ist der Teil der .NET Framework, die speziell für das Erstellen von Webanwendungen entworfen wurde.</span><span class="sxs-lookup"><span data-stu-id="5f462-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="5f462-222">Entwickler haben ASP.NET verwendet, um viele der größten und weltweit höchsten Websites zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5f462-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="5f462-223">(Jedes Mal, wenn die Dateinamenerweiterung " *. aspx* " als Teil der URL einer Website angezeigt wird, wissen Sie, dass die Website mit ASP.NET geschrieben wurde.)</span><span class="sxs-lookup"><span data-stu-id="5f462-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="5f462-224">Der Razor-Syntax bietet Ihnen die gesamte Leistungsfähigkeit von ASP.net, verwendet jedoch eine vereinfachte Syntax, die einfacher zu erlernen ist, wenn Sie ein Anfänger sind und Sie produktiver machen, wenn Sie ein Experte sind.</span><span class="sxs-lookup"><span data-stu-id="5f462-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="5f462-225">Obwohl diese Syntax einfach zu verwenden ist, bedeutet Ihre Familienbeziehung zu ASP.net und der .NET Framework, dass Sie, wenn Ihre Websites komplexer werden, die Leistungsfähigkeit der größeren Frameworks haben, die Ihnen zur Verfügung stehen.</span><span class="sxs-lookup"><span data-stu-id="5f462-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> <span data-ttu-id="5f462-227">**Klassen und Instanzen**</span><span class="sxs-lookup"><span data-stu-id="5f462-227">**Classes and Instances**</span></span>
> 
> <span data-ttu-id="5f462-228">ASP.net-Servercode verwendet-Objekte, die wiederum auf der Idee von Klassen basieren.</span><span class="sxs-lookup"><span data-stu-id="5f462-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="5f462-229">Bei der-Klasse handelt es sich um die Definition oder Vorlage für ein Objekt.</span><span class="sxs-lookup"><span data-stu-id="5f462-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="5f462-230">Beispielsweise kann eine Anwendung eine `Customer` Klasse enthalten, die die Eigenschaften und Methoden definiert, die ein beliebiges Kunden Objekt benötigt.</span><span class="sxs-lookup"><span data-stu-id="5f462-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="5f462-231">Wenn die Anwendung mit den tatsächlichen Kundeninformationen arbeiten muss, erstellt Sie eine Instanz von (oder *instanziiert*) ein Customer-Objekt.</span><span class="sxs-lookup"><span data-stu-id="5f462-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="5f462-232">Bei jedem einzelnen Kunden handelt es sich um eine separate Instanz der `Customer`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="5f462-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="5f462-233">Jede Instanz unterstützt dieselben Eigenschaften und Methoden, aber die Eigenschaftswerte für jede Instanz unterscheiden sich in der Regel, da jedes Kunden Objekt eindeutig ist.</span><span class="sxs-lookup"><span data-stu-id="5f462-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="5f462-234">In einem Kunden Objekt kann die `LastName`-Eigenschaft "Smith" lauten. in einem anderen Customer-Objekt kann die `LastName`-Eigenschaft "Jones" lauten.</span><span class="sxs-lookup"><span data-stu-id="5f462-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="5f462-235">Auf ähnliche Weise ist jede einzelne Webseite in Ihrer Site ein `Page` Objekt, das eine Instanz der `Page`-Klasse ist.</span><span class="sxs-lookup"><span data-stu-id="5f462-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="5f462-236">Eine Schaltfläche auf der Seite ist ein `Button` Objekt, das eine Instanz der `Button`-Klasse ist usw.</span><span class="sxs-lookup"><span data-stu-id="5f462-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="5f462-237">Jede Instanz verfügt über eigene Merkmale, aber Sie basieren auf den Angaben, die in der Klassendefinition des Objekts angegeben sind.</span><span class="sxs-lookup"><span data-stu-id="5f462-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>

## <a name="basic-syntax"></a><span data-ttu-id="5f462-238">Grundlegende Syntax</span><span class="sxs-lookup"><span data-stu-id="5f462-238">Basic Syntax</span></span>

<span data-ttu-id="5f462-239">Früher haben Sie ein einfaches Beispiel für das Erstellen einer ASP.net Web Pages Seite und die Vorgehensweise zum Hinzufügen von Servercode zu HTML-Markup gesehen.</span><span class="sxs-lookup"><span data-stu-id="5f462-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="5f462-240">Hier lernen Sie die Grundlagen zum Schreiben von ASP.net-Servercode mithilfe &#8212; der Razor-Syntax der Programmiersprachen Regeln kennen.</span><span class="sxs-lookup"><span data-stu-id="5f462-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="5f462-241">Wenn Sie Erfahrung mit der Programmierung haben (insbesondere, wenn Sie C, C++, C#, Visual Basic oder JavaScript verwendet haben), werden viele der hier gelesenen Elemente Ihnen vertraut sein.</span><span class="sxs-lookup"><span data-stu-id="5f462-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="5f462-242">Sie müssen sich wahrscheinlich nur mit dem Hinzufügen von Servercode zu Markup in *. cshtml* -Dateien vertraut machen.</span><span class="sxs-lookup"><span data-stu-id="5f462-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="5f462-243">Kombinieren von Text, Markup und Code in Code Blöcken</span><span class="sxs-lookup"><span data-stu-id="5f462-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="5f462-244">In Servercode Blöcken möchten Sie häufig Text oder Markup (oder beides) an die Seite ausgeben.</span><span class="sxs-lookup"><span data-stu-id="5f462-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="5f462-245">Wenn ein Server Codeblock Text enthält, der nicht Code ist und stattdessen unverändert gerendert werden soll, muss ASP.net in der Lage sein, diesen Text vom Code zu unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="5f462-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="5f462-246">Hierfür gibt es mehrere Möglichkeiten.</span><span class="sxs-lookup"><span data-stu-id="5f462-246">There are several ways to do this.</span></span>

- <span data-ttu-id="5f462-247">Schließen Sie den Text in ein HTML-Element wie `<p></p>` oder `<em></em>`ein:</span><span class="sxs-lookup"><span data-stu-id="5f462-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="5f462-248">Das HTML-Element kann Text, zusätzliche HTML-Elemente und Servercode Ausdrücke enthalten.</span><span class="sxs-lookup"><span data-stu-id="5f462-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="5f462-249">Wenn ASP.NET das öffnende HTML-Tag sieht (z. b. `<p>`), werden alle Elemente, einschließlich des Elements und seines Inhalts, unverändert im Browser gerendert, und die Servercode Ausdrücke werden entsprechend aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="5f462-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="5f462-250">Verwenden Sie den `@:`-Operator oder das `<text>`-Element.</span><span class="sxs-lookup"><span data-stu-id="5f462-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="5f462-251">Der `@:` gibt eine einzelne Zeile mit Inhalt aus, die nur-Text oder nicht übereinstimmende HTML-Tags enthält Das `<text>` Element schließt mehrere Zeilen ein, die ausgegeben werden sollen.</span><span class="sxs-lookup"><span data-stu-id="5f462-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="5f462-252">Diese Optionen sind hilfreich, wenn Sie ein HTML-Element nicht als Teil der Ausgabe renderingrensten möchten.</span><span class="sxs-lookup"><span data-stu-id="5f462-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="5f462-253">Wenn Sie mehrere Textzeilen oder nicht übereinstimmende HTML-Tags ausgeben möchten, können Sie jede Zeile mit `@:`versehen, oder Sie können die Zeile in ein `<text>` Element einschließen.</span><span class="sxs-lookup"><span data-stu-id="5f462-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="5f462-254">Wie der `@:`-Operator werden`<text>` Tags von ASP.NET verwendet, um Textinhalte zu identifizieren und nie in der Seiten Ausgabe gerendert zu werden.</span><span class="sxs-lookup"><span data-stu-id="5f462-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="5f462-255">Im ersten Beispiel wird das vorherige Beispiel wiederholt, aber es wird ein einzelnes Paar `<text>` Tags verwendet, um den zu Rendering enden Text einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="5f462-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="5f462-256">Im zweiten Beispiel schließen die Tags "`<text>`" und "`</text>`" drei Zeilen ein, die alle über einen nicht enthaltenen Text und nicht übereinstimmende HTML-Tags (`<br />`) zusammen mit Servercode und übereinstimmenden HTML-Tags verfügen.</span><span class="sxs-lookup"><span data-stu-id="5f462-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="5f462-257">Auch hier kann jede Zeile einzeln mit dem `@:`-Operator vorangestellt werden. Beides funktioniert.</span><span class="sxs-lookup"><span data-stu-id="5f462-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5f462-258">Wenn Sie Text wie in diesem Abschnitt &#8212; gezeigt mit einem HTML--Element ausgeben, wird der `@:`-Operator oder &#8212; das `<text>` Element ASP.net die Ausgabe nicht in HTML-codiert.</span><span class="sxs-lookup"><span data-stu-id="5f462-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="5f462-259">(Wie bereits erwähnt, codiert ASP.net die Ausgabe von Servercode Ausdrücken und Servercode Blöcken, denen `@`vorangestellt ist, mit Ausnahme der in diesem Abschnitt notierten Sonderfälle.)</span><span class="sxs-lookup"><span data-stu-id="5f462-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="5f462-260">Whitespace</span><span class="sxs-lookup"><span data-stu-id="5f462-260">Whitespace</span></span>

<span data-ttu-id="5f462-261">Zusätzliche Leerzeichen in einer-Anweisung (und außerhalb eines Zeichenfolgenliterals) haben keine Auswirkung auf die Anweisung:</span><span class="sxs-lookup"><span data-stu-id="5f462-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="5f462-262">Ein Zeilenumbruch in einer-Anweisung hat keine Auswirkung auf die-Anweisung, und Sie können-Anweisungen zur besseren Lesbarkeit umschließen.</span><span class="sxs-lookup"><span data-stu-id="5f462-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="5f462-263">Die folgenden Anweisungen sind identisch:</span><span class="sxs-lookup"><span data-stu-id="5f462-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="5f462-264">Sie können jedoch keine Linie in der Mitte eines Zeichenfolgenliterals umschließen.</span><span class="sxs-lookup"><span data-stu-id="5f462-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="5f462-265">Das folgende Beispiel funktioniert nicht:</span><span class="sxs-lookup"><span data-stu-id="5f462-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="5f462-266">Um eine lange Zeichenfolge zu kombinieren, die mit mehreren Zeilen wie dem obigen Code umbrochen wird, gibt es zwei Optionen.</span><span class="sxs-lookup"><span data-stu-id="5f462-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="5f462-267">Sie können den Verkettungs Operator (`+`) verwenden, den Sie später in diesem Artikel sehen werden.</span><span class="sxs-lookup"><span data-stu-id="5f462-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="5f462-268">Sie können auch das `@` Zeichen verwenden, um ein wörtlich zeichenfolgenliteralformat zu erstellen, wie zuvor in diesem Artikel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="5f462-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="5f462-269">Sie können ausführliche Zeichen folgen Literale Zeilen übergreifend unterbrechen:</span><span class="sxs-lookup"><span data-stu-id="5f462-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="5f462-270">Kommentare zu Code (und Markup)</span><span class="sxs-lookup"><span data-stu-id="5f462-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="5f462-271">Mit Kommentaren können Sie Notizen für sich selbst oder für andere Personen hinterlassen.</span><span class="sxs-lookup"><span data-stu-id="5f462-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="5f462-272">Sie ermöglichen es Ihnen außerdem, einen Code Abschnitt oder Markup, den Sie nicht ausführen möchten, zu deaktivieren (*auszukommentieren*), auf der Seite zu speichern.</span><span class="sxs-lookup"><span data-stu-id="5f462-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="5f462-273">Es gibt verschiedene Kommentar Syntax für Razor-Code und für HTML-Markup.</span><span class="sxs-lookup"><span data-stu-id="5f462-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="5f462-274">Wie bei sämtlichen Razor-Code werden Razor-Kommentare auf dem Server verarbeitet (und dann entfernt), bevor die Seite an den Browser gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="5f462-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="5f462-275">Daher können Sie mit der Razor-Kommentar Syntax Kommentare in den Code einfügen (oder sogar in das Markup), den Sie beim Bearbeiten der Datei anzeigen können, die aber auch nicht in der Seitenquelle angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="5f462-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="5f462-276">Für ASP.net Razor-Kommentare starten Sie den Kommentar mit `@*` und beenden ihn mit `*@`.</span><span class="sxs-lookup"><span data-stu-id="5f462-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="5f462-277">Der Kommentar kann sich in einer Zeile oder in mehreren Zeilen befinden:</span><span class="sxs-lookup"><span data-stu-id="5f462-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="5f462-278">Hier ist ein Kommentar innerhalb eines Code Blocks:</span><span class="sxs-lookup"><span data-stu-id="5f462-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="5f462-279">Hier ist derselbe Codeblock, in dem die Codezeile auskommentiert ist, sodass Sie nicht ausgeführt werden kann:</span><span class="sxs-lookup"><span data-stu-id="5f462-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="5f462-280">In einem Codeblock können Sie als Alternative zur Verwendung der Razor-Kommentar Syntax die Kommentar Syntax der verwendeten Programmiersprache verwenden, z. b. C#:</span><span class="sxs-lookup"><span data-stu-id="5f462-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="5f462-281">In C#werden einzeilige Kommentare `//` Zeichen vorangestellt, und mehrzeilige Kommentare beginnen mit `/*` und enden mit `*/`.</span><span class="sxs-lookup"><span data-stu-id="5f462-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="5f462-282">(Wie bei Razor-Kommentaren C# werden Kommentare nicht im Browser gerendert.)</span><span class="sxs-lookup"><span data-stu-id="5f462-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="5f462-283">Für Markup können Sie, wie Sie wahrscheinlich wissen, einen HTML-Kommentar erstellen:</span><span class="sxs-lookup"><span data-stu-id="5f462-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="5f462-284">HTML-Kommentare beginnen mit `<!--` Zeichen und enden mit `-->`.</span><span class="sxs-lookup"><span data-stu-id="5f462-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="5f462-285">Sie können HTML-Kommentare verwenden, um nicht nur Text zu umschließen, sondern auch ein beliebiges HTML-Markup, das Sie auf der Seite behalten möchten, aber nicht Rendering möchten.</span><span class="sxs-lookup"><span data-stu-id="5f462-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="5f462-286">Dieser HTML-Kommentar blendet den gesamten Inhalt der Tags und den darin enthaltenen Text aus:</span><span class="sxs-lookup"><span data-stu-id="5f462-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="5f462-287">Im Gegensatz zu Razor-Kommentaren *werden* HTML-Kommentare auf der Seite gerendert, und der Benutzer kann diese anzeigen, indem er die Seitenquelle anzeigen kann.</span><span class="sxs-lookup"><span data-stu-id="5f462-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="5f462-288">Razor hat Einschränkungen für die Blöcke von C#.</span><span class="sxs-lookup"><span data-stu-id="5f462-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="5f462-289">Weitere Informationen finden Sie [unter C# benannte Variablen und geschiebte Blöcke generieren](http://aspnetwebstack.codeplex.com/workitem/1914) von fehlerhafter Code.</span><span class="sxs-lookup"><span data-stu-id="5f462-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="5f462-290">Variables</span><span class="sxs-lookup"><span data-stu-id="5f462-290">Variables</span></span>

<span data-ttu-id="5f462-291">Eine Variable ist ein benanntes Objekt, das Sie zum Speichern von Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="5f462-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="5f462-292">Sie können Variablen einen beliebigen Namen benennen, aber der Name muss mit einem alphabetischen Zeichen beginnen und darf keine Leerzeichen oder reservierten Zeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="5f462-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="5f462-293">Variablen und Datentypen</span><span class="sxs-lookup"><span data-stu-id="5f462-293">Variables and Data Types</span></span>

<span data-ttu-id="5f462-294">Eine Variable kann einen bestimmten Datentyp aufweisen, der angibt, welche Art von Daten in der Variablen gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="5f462-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="5f462-295">Sie können Zeichen folgen Variablen haben, die Zeichen folgen Werte (wie &quot;Hello World&quot;) speichern, ganzzahlige Variablen, die ganzzahlige Werte (z. b. 3 oder 79) speichern, und Datums Variablen, die Datumswerte in einer Vielzahl von Formaten (z. b. 4/12/2012 oder März 2009) speichern.</span><span class="sxs-lookup"><span data-stu-id="5f462-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="5f462-296">Und es gibt viele weitere Datentypen, die Sie verwenden können.</span><span class="sxs-lookup"><span data-stu-id="5f462-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="5f462-297">Sie müssen jedoch im Allgemeinen keinen Typ für eine Variable angeben.</span><span class="sxs-lookup"><span data-stu-id="5f462-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="5f462-298">In den meisten Fällen kann ASP.NET den Typ basierend auf der Verwendung der Daten in der Variablen ermitteln.</span><span class="sxs-lookup"><span data-stu-id="5f462-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="5f462-299">(Gelegentlich müssen Sie einen Typ angeben. es werden Beispiele angezeigt, wenn dies zutrifft.)</span><span class="sxs-lookup"><span data-stu-id="5f462-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="5f462-300">Sie deklarieren eine Variable mit dem `var`-Schlüsselwort (wenn Sie keinen Typ angeben möchten) oder mit dem Namen des Typs:</span><span class="sxs-lookup"><span data-stu-id="5f462-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="5f462-301">Das folgende Beispiel zeigt einige typische Verwendungen von Variablen in einer Webseite:</span><span class="sxs-lookup"><span data-stu-id="5f462-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="5f462-302">Wenn Sie die vorherigen Beispiele in einer Seite kombinieren, wird dies in einem Browser angezeigt:</span><span class="sxs-lookup"><span data-stu-id="5f462-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="5f462-304">Wandeln und Testen von Datentypen</span><span class="sxs-lookup"><span data-stu-id="5f462-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="5f462-305">Obwohl ASP.net normalerweise einen Datentyp automatisch ermitteln kann, kann dies manchmal nicht der Fall sein.</span><span class="sxs-lookup"><span data-stu-id="5f462-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="5f462-306">Daher müssen Sie möglicherweise ASP.net durch eine explizite Konvertierung unterstützen.</span><span class="sxs-lookup"><span data-stu-id="5f462-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="5f462-307">Auch wenn Sie keine Typen konvertieren müssen, ist es manchmal hilfreich, zu testen, um festzustellen, mit welchem Datentyp Sie möglicherweise arbeiten.</span><span class="sxs-lookup"><span data-stu-id="5f462-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="5f462-308">Der häufigste Fall ist, dass Sie eine Zeichenfolge in einen anderen Typ konvertieren müssen, z. b. in eine ganze Zahl oder ein Datum.</span><span class="sxs-lookup"><span data-stu-id="5f462-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="5f462-309">Das folgende Beispiel zeigt einen typischen Fall, in dem Sie eine Zeichenfolge in eine Zahl konvertieren müssen.</span><span class="sxs-lookup"><span data-stu-id="5f462-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="5f462-310">Als Regel werden Benutzereingaben als Zeichen folgen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5f462-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="5f462-311">Selbst wenn Sie Benutzer zur Eingabe einer Zahl aufgefordert haben und Sie eine Ziffer eingegeben haben, werden die Daten im Zeichen folgen Format angezeigt, wenn die Benutzereingaben übermittelt werden und Sie Sie im Code lesen.</span><span class="sxs-lookup"><span data-stu-id="5f462-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="5f462-312">Daher müssen Sie die Zeichenfolge in eine Zahl konvertieren.</span><span class="sxs-lookup"><span data-stu-id="5f462-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="5f462-313">Wenn Sie in diesem Beispiel versuchen, arithmetische Werte für die Werte auszuführen, ohne Sie zu wandeln, wird die folgende Fehlermeldung ausgegeben, da ASP.net nicht zwei Zeichen folgen hinzufügen kann:</span><span class="sxs-lookup"><span data-stu-id="5f462-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

<span data-ttu-id="5f462-314">*Der Typ "String" kann nicht implizit in "int" konvertiert werden.*</span><span class="sxs-lookup"><span data-stu-id="5f462-314">*Cannot implicitly convert type 'string' to 'int'.*</span></span>

<span data-ttu-id="5f462-315">Um die Werte in ganze Zahlen zu konvertieren, wird die `AsInt`-Methode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="5f462-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="5f462-316">Wenn die Konvertierung erfolgreich ist, können Sie die Zahlen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="5f462-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="5f462-317">In der folgenden Tabelle werden einige allgemeine Konvertierungs-und Testmethoden für Variablen aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="5f462-317">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="5f462-318"><strong>Methode</strong></span><span class="sxs-lookup"><span data-stu-id="5f462-318"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="5f462-319"><strong>Beschreibung</strong></span><span class="sxs-lookup"><span data-stu-id="5f462-319"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="5f462-320"><strong>Beispiel</strong></span><span class="sxs-lookup"><span data-stu-id="5f462-320"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="5f462-321">Konvertiert eine Zeichenfolge, die eine ganze Zahl (z. b. "593") darstellt, in eine ganze Zahl.</span><span class="sxs-lookup"><span data-stu-id="5f462-321">Converts a string that represents a whole number (like "593") to an integer.</span></span>
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
    <span data-ttu-id="5f462-322">Konvertiert eine Zeichenfolge wie &quot;true&quot; oder &quot;false-&quot; in einen booleschen Typ.</span><span class="sxs-lookup"><span data-stu-id="5f462-322">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
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
    <span data-ttu-id="5f462-323">Konvertiert eine Zeichenfolge mit einem Dezimalwert wie &quot;1,3&quot; oder &quot;7,439&quot; in eine Gleit Komma Zahl.</span><span class="sxs-lookup"><span data-stu-id="5f462-323">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
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
    <span data-ttu-id="5f462-324">Konvertiert eine Zeichenfolge mit einem Dezimalwert wie &quot;1,3&quot; oder &quot;7,439&quot; in eine Dezimalzahl.</span><span class="sxs-lookup"><span data-stu-id="5f462-324">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="5f462-325">(In ASP.net ist eine Dezimalzahl präziser als eine Gleit Komma Zahl.)</span><span class="sxs-lookup"><span data-stu-id="5f462-325">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
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
    <span data-ttu-id="5f462-326">Konvertiert eine Zeichenfolge, die einen Datums-und Uhrzeitwert darstellt, in den ASP.net-`DateTime` Typs.</span><span class="sxs-lookup"><span data-stu-id="5f462-326">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
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
    <span data-ttu-id="5f462-327">Konvertiert einen beliebigen anderen Datentyp in eine Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="5f462-327">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="5f462-328">Operatoren</span><span class="sxs-lookup"><span data-stu-id="5f462-328">Operators</span></span>

<span data-ttu-id="5f462-329">Ein Operator ist ein Schlüsselwort oder Zeichen, das ASP.net die Art des Befehls angibt, der in einem Ausdruck durchgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="5f462-329">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="5f462-330">Die C# Sprache (und die Razor-Syntax, die darauf basiert) unterstützt viele Operatoren, aber Sie müssen nur einige für den Einstieg erkennen.</span><span class="sxs-lookup"><span data-stu-id="5f462-330">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="5f462-331">In der folgenden Tabelle werden die gängigsten Operatoren zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="5f462-331">The following table summarizes the most common operators.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="5f462-332"><strong>Operator</strong></span><span class="sxs-lookup"><span data-stu-id="5f462-332"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="5f462-333"><strong>Beschreibung</strong></span><span class="sxs-lookup"><span data-stu-id="5f462-333"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="5f462-334"><strong>Beispiele</strong></span><span class="sxs-lookup"><span data-stu-id="5f462-334"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="5f462-335">`+` `-` `*` `/`</span><span class="sxs-lookup"><span data-stu-id="5f462-335">`+` `-` `*` `/`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="5f462-336">Mathematische Operatoren, die in numerischen Ausdrücken verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5f462-336">Math operators used in numerical expressions.</span></span>
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
    <span data-ttu-id="5f462-337">Zuweisung.</span><span class="sxs-lookup"><span data-stu-id="5f462-337">Assignment.</span></span> <span data-ttu-id="5f462-338">Weist dem-Objekt auf der linken Seite den Wert auf der rechten Seite einer-Anweisung zu.</span><span class="sxs-lookup"><span data-stu-id="5f462-338">Assigns the value on the right side of a statement to the object on the left side.</span></span>
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
    <span data-ttu-id="5f462-339">Gleichheit.</span><span class="sxs-lookup"><span data-stu-id="5f462-339">Equality.</span></span> <span data-ttu-id="5f462-340">Gibt `true` zurück, wenn die Werte gleich sind.</span><span class="sxs-lookup"><span data-stu-id="5f462-340">Returns `true` if the values are equal.</span></span> <span data-ttu-id="5f462-341">(Beachten Sie den Unterschied zwischen dem `=`-Operator und dem `==`-Operator.)</span><span class="sxs-lookup"><span data-stu-id="5f462-341">(Notice the distinction between the `=` operator and the `==` operator.)</span></span>
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
    <span data-ttu-id="5f462-342">Ungleichheit.</span><span class="sxs-lookup"><span data-stu-id="5f462-342">Inequality.</span></span> <span data-ttu-id="5f462-343">Gibt `true` zurück, wenn die Werte nicht gleich sind.</span><span class="sxs-lookup"><span data-stu-id="5f462-343">Returns `true` if the values are not equal.</span></span>
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
    <span data-ttu-id="5f462-344">"Kleiner als", "größer als", "kleiner als oder gleich", "größer als" oder "gleich".</span><span class="sxs-lookup"><span data-stu-id="5f462-344">Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.</span></span>
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
    <span data-ttu-id="5f462-345">Verkettung, die verwendet wird, um Zeichen folgen zu verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="5f462-345">Concatenation, which is used to join strings.</span></span> <span data-ttu-id="5f462-346">ASP.net kennt den Unterschied zwischen diesem Operator und dem Additions Operator auf der Grundlage des Datentyps des Ausdrucks.</span><span class="sxs-lookup"><span data-stu-id="5f462-346">ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="5f462-347">`+=` `-=`</span><span class="sxs-lookup"><span data-stu-id="5f462-347">`+=` `-=`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="5f462-348">Die Inkrement-und Dekrementoperatoren, die 1 (bzw.) von einer Variablen addieren bzw. daraus ziehen.</span><span class="sxs-lookup"><span data-stu-id="5f462-348">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
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
    <span data-ttu-id="5f462-349">Gewinn.</span><span class="sxs-lookup"><span data-stu-id="5f462-349">Dot.</span></span> <span data-ttu-id="5f462-350">Wird verwendet, um Objekte und deren Eigenschaften und Methoden zu unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="5f462-350">Used to distinguish objects and their properties and methods.</span></span>
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
    <span data-ttu-id="5f462-351">Klammern.</span><span class="sxs-lookup"><span data-stu-id="5f462-351">Parentheses.</span></span> <span data-ttu-id="5f462-352">Wird verwendet, um Ausdrücke zu gruppieren und um Parameter an Methoden zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="5f462-352">Used to group expressions and to pass parameters to methods.</span></span>
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
    <span data-ttu-id="5f462-353">Angegeben.</span><span class="sxs-lookup"><span data-stu-id="5f462-353">Brackets.</span></span> <span data-ttu-id="5f462-354">Wird für den Zugriff auf Werte in Arrays oder Auflistungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="5f462-354">Used for accessing values in arrays or collections.</span></span>
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
    <span data-ttu-id="5f462-355">Hätten.</span><span class="sxs-lookup"><span data-stu-id="5f462-355">Not.</span></span> <span data-ttu-id="5f462-356">Kehrt einen `true` Wert in `false` um und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="5f462-356">Reverses a `true` value to `false` and vice versa.</span></span> <span data-ttu-id="5f462-357">Wird in der Regel als Kurzform zum Testen auf `false` verwendet (d. h. für nicht `true`).</span><span class="sxs-lookup"><span data-stu-id="5f462-357">Typically used as a shorthand way to test for `false` (that is, for not `true`).</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="5f462-358">`&&` `||`</span><span class="sxs-lookup"><span data-stu-id="5f462-358">`&&` `||`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="5f462-359">Logisches and und or, die zum Verknüpfen von Bedingungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5f462-359">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="5f462-360">Arbeiten mit Datei-und Ordner Pfaden im Code</span><span class="sxs-lookup"><span data-stu-id="5f462-360">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="5f462-361">Häufig arbeiten Sie mit Datei-und Ordner Pfaden in Ihrem Code.</span><span class="sxs-lookup"><span data-stu-id="5f462-361">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="5f462-362">Im folgenden finden Sie ein Beispiel für eine physische Ordnerstruktur für eine Website, wie Sie auf dem Entwicklungs Computer angezeigt werden kann:</span><span class="sxs-lookup"><span data-stu-id="5f462-362">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="5f462-363">Im folgenden finden Sie einige wichtige Details zu URLs und Pfaden:</span><span class="sxs-lookup"><span data-stu-id="5f462-363">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="5f462-364">Eine URL beginnt entweder mit einem Domänen Namen (`http://www.example.com`) oder einem Servernamen (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="5f462-364">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="5f462-365">Eine URL entspricht einem physischen Pfad auf einem Host Computer.</span><span class="sxs-lookup"><span data-stu-id="5f462-365">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="5f462-366">Beispielsweise können `http://myserver` dem Ordner *c:\websites\meinewebsite* auf dem Server entsprechen.</span><span class="sxs-lookup"><span data-stu-id="5f462-366">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="5f462-367">Ein virtueller Pfad ist eine kurzzeile, um Pfade im Code darzustellen, ohne den vollständigen Pfad angeben zu müssen.</span><span class="sxs-lookup"><span data-stu-id="5f462-367">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="5f462-368">Sie enthält den Teil einer URL, der auf den Domänen-oder Servernamen folgt.</span><span class="sxs-lookup"><span data-stu-id="5f462-368">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="5f462-369">Wenn Sie virtuelle Pfade verwenden, können Sie Ihren Code in eine andere Domäne oder einen anderen Server verschieben, ohne die Pfade aktualisieren zu müssen.</span><span class="sxs-lookup"><span data-stu-id="5f462-369">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="5f462-370">Hier finden Sie ein Beispiel, das Ihnen hilft, die Unterschiede zu verstehen:</span><span class="sxs-lookup"><span data-stu-id="5f462-370">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="5f462-371">Complete URL</span><span class="sxs-lookup"><span data-stu-id="5f462-371">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="5f462-372">Servername</span><span class="sxs-lookup"><span data-stu-id="5f462-372">Server name</span></span> | <span data-ttu-id="5f462-373">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="5f462-373">*mycompanyserver*</span></span> |
| <span data-ttu-id="5f462-374">Virtueller Pfad</span><span class="sxs-lookup"><span data-stu-id="5f462-374">Virtual path</span></span> | <span data-ttu-id="5f462-375">*/HumanResources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="5f462-375">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="5f462-376">Physischer Pfad</span><span class="sxs-lookup"><span data-stu-id="5f462-376">Physical path</span></span> | <span data-ttu-id="5f462-377">*C:\meinewebsites\humanresources\companypolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="5f462-377">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="5f462-378">Das virtuelle Stammverzeichnis ist/, genauso wie der Stamm des Laufwerks C: \.</span><span class="sxs-lookup"><span data-stu-id="5f462-378">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="5f462-379">(Pfade für virtuelle Ordner verwenden immer Schrägstriche.) Der virtuelle Pfad eines Ordners muss nicht denselben Namen wie der physische Ordner aufweisen. Dabei kann es sich um einen Alias handeln.</span><span class="sxs-lookup"><span data-stu-id="5f462-379">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="5f462-380">(Auf Produktionsservern entspricht der virtuelle Pfad selten einem exakten physischen Pfad.)</span><span class="sxs-lookup"><span data-stu-id="5f462-380">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="5f462-381">Beim Arbeiten mit Dateien und Ordnern im Code müssen Sie manchmal auf den physischen Pfad und manchmal auf einen virtuellen Pfad verweisen, je nachdem, mit welchen Objekten Sie arbeiten.</span><span class="sxs-lookup"><span data-stu-id="5f462-381">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="5f462-382">ASP.net bietet Ihnen diese Tools zum Arbeiten mit Datei-und Ordner Pfaden im Code: die `Server.MapPath`-Methode und der `~`-Operator und `Href`-Methode.</span><span class="sxs-lookup"><span data-stu-id="5f462-382">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="5f462-383">Umstellen von virtuellen in physische Pfade: die Server. MapPath-Methode</span><span class="sxs-lookup"><span data-stu-id="5f462-383">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="5f462-384">Die `Server.MapPath`-Methode konvertiert einen virtuellen Pfad (z. b. */default.cshtml*) in einen absoluten physischen Pfad (z. b. *c:\websites\mywebsitefolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="5f462-384">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="5f462-385">Sie verwenden diese Methode, wenn Sie einen kompletten physischen Pfad benötigen.</span><span class="sxs-lookup"><span data-stu-id="5f462-385">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="5f462-386">Ein typisches Beispiel ist das Lesen oder Schreiben einer Textdatei oder Bilddatei auf dem Webserver.</span><span class="sxs-lookup"><span data-stu-id="5f462-386">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="5f462-387">Der absolute physische Pfad der Website auf dem Server einer Host Website ist in der Regel nicht bekannt. Daher kann diese Methode den Pfad, den Sie kennen – den virtuellen Pfad –, in den entsprechenden Pfad auf dem Server konvertieren.</span><span class="sxs-lookup"><span data-stu-id="5f462-387">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="5f462-388">Sie übergeben den virtuellen Pfad an eine Datei oder einen Ordner an die-Methode und geben den physischen Pfad zurück:</span><span class="sxs-lookup"><span data-stu-id="5f462-388">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="5f462-389">Verweisen auf das virtuelle Stammverzeichnis: der ~-Operator und die href-Methode</span><span class="sxs-lookup"><span data-stu-id="5f462-389">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="5f462-390">In einer *cshtml* -oder *vbhtml* -Datei können Sie mithilfe des `~`-Operators auf den virtuellen Stammpfad verweisen.</span><span class="sxs-lookup"><span data-stu-id="5f462-390">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="5f462-391">Dies ist sehr praktisch, da Sie Seiten auf einer Website verschieben können und alle Links, die Sie auf anderen Seiten enthalten, nicht beschädigt werden.</span><span class="sxs-lookup"><span data-stu-id="5f462-391">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="5f462-392">Es ist auch praktisch, wenn Sie Ihre Website jemals an einen anderen Speicherort verschieben.</span><span class="sxs-lookup"><span data-stu-id="5f462-392">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="5f462-393">Im Folgenden finden Sie einige Beispiele:</span><span class="sxs-lookup"><span data-stu-id="5f462-393">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="5f462-394">Wenn die Website `http://myserver/myapp`ist, so behandelt ASP.net diese Pfade, wenn die Seite ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="5f462-394">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="5f462-395">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="5f462-395">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="5f462-396">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="5f462-396">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="5f462-397">(Diese Pfade werden nicht als Werte der Variablen angezeigt, aber ASP.NET behandelt die Pfade so, als wären Sie so.)</span><span class="sxs-lookup"><span data-stu-id="5f462-397">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="5f462-398">Sie können den `~`-Operator sowohl im Servercode (wie oben) als auch im Markup wie folgt verwenden:</span><span class="sxs-lookup"><span data-stu-id="5f462-398">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="5f462-399">In Markup verwenden Sie den `~`-Operator, um Pfade zu Ressourcen wie Bilddateien, anderen Webseiten und CSS-Dateien zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5f462-399">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="5f462-400">Wenn die Seite ausgeführt wird, durchsucht ASP.net die Seite (sowohl Code als auch Markup) und löst alle `~` Verweise auf den entsprechenden Pfad auf.</span><span class="sxs-lookup"><span data-stu-id="5f462-400">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="5f462-401">Bedingte Logik und Schleifen</span><span class="sxs-lookup"><span data-stu-id="5f462-401">Conditional Logic and Loops</span></span>

<span data-ttu-id="5f462-402">ASP.net-Servercode ermöglicht das Ausführen von Aufgaben auf der Grundlage von Bedingungen und das Schreiben von Code, der-Anweisungen eine bestimmte Anzahl von Vorkommen wiederholt (d. h. Code, der eine Schleife ausführt).</span><span class="sxs-lookup"><span data-stu-id="5f462-402">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="5f462-403">Testbedingungen</span><span class="sxs-lookup"><span data-stu-id="5f462-403">Testing Conditions</span></span>

<span data-ttu-id="5f462-404">Um eine einfache Bedingung zu testen, verwenden Sie die `if`-Anweisung, die basierend auf einem von Ihnen angegebenen Test true oder false zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="5f462-404">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="5f462-405">Das `if`-Schlüsselwort startet einen-Block.</span><span class="sxs-lookup"><span data-stu-id="5f462-405">The `if` keyword starts a block.</span></span> <span data-ttu-id="5f462-406">Der tatsächliche Test (Bedingung) wird in Klammern gesetzt und gibt true oder false zurück.</span><span class="sxs-lookup"><span data-stu-id="5f462-406">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="5f462-407">Die Anweisungen, die ausgeführt werden, wenn der Test true ist, werden in geschweifte Klammern eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="5f462-407">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="5f462-408">Eine `if`-Anweisung kann einen `else`-Block enthalten, der Anweisungen angibt, die ausgeführt werden sollen, wenn die Bedingung false ist:</span><span class="sxs-lookup"><span data-stu-id="5f462-408">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="5f462-409">Sie können mit einem `else if`-Block mehrere Bedingungen hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="5f462-409">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="5f462-410">Wenn in diesem Beispiel die erste Bedingung im If-Block nicht true ist, wird die `else if` Bedingung geprüft.</span><span class="sxs-lookup"><span data-stu-id="5f462-410">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="5f462-411">Wenn diese Bedingung erfüllt ist, werden die Anweisungen im `else if`-Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="5f462-411">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="5f462-412">Wenn keine der Bedingungen erfüllt ist, werden die Anweisungen im `else`-Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="5f462-412">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="5f462-413">Sie können eine beliebige Anzahl von else-if-Blöcken hinzufügen und dann mit einem `else`-Block als &quot;alle anderen&quot; Bedingung schließen.</span><span class="sxs-lookup"><span data-stu-id="5f462-413">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="5f462-414">Um eine große Anzahl von Bedingungen zu testen, verwenden Sie einen `switch`-Block:</span><span class="sxs-lookup"><span data-stu-id="5f462-414">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="5f462-415">Der zu testende Wert ist in Klammern (im Beispiel die `weekday` Variable).</span><span class="sxs-lookup"><span data-stu-id="5f462-415">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="5f462-416">Jeder einzelne Test verwendet eine `case`-Anweisung, die mit einem Doppelpunkt endet (:).</span><span class="sxs-lookup"><span data-stu-id="5f462-416">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="5f462-417">Wenn der Wert einer `case`-Anweisung mit dem Testwert übereinstimmt, wird der Code in diesem Fall Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="5f462-417">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="5f462-418">Sie schließen jede Case-Anweisung mit einer `break`-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="5f462-418">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="5f462-419">(Wenn Sie vergessen, Unterbrechung in jedem `case` Block einzuschließen, wird der Code der nächsten `case` Anweisung ebenfalls ausgeführt.) Ein `switch` Block verfügt häufig über eine `default`-Anweisung als letzter Fall für eine &quot;alles andere&quot; Option, die ausgeführt wird, wenn keiner der anderen Fälle true ist.</span><span class="sxs-lookup"><span data-stu-id="5f462-419">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="5f462-420">Das Ergebnis der letzten beiden bedingten Blöcke, die in einem Browser angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="5f462-420">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="5f462-422">Schleifen Code</span><span class="sxs-lookup"><span data-stu-id="5f462-422">Looping Code</span></span>

<span data-ttu-id="5f462-423">Häufig müssen Sie dieselben Anweisungen wiederholt ausführen.</span><span class="sxs-lookup"><span data-stu-id="5f462-423">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="5f462-424">Dies geschieht durch Schleifen.</span><span class="sxs-lookup"><span data-stu-id="5f462-424">You do this by looping.</span></span> <span data-ttu-id="5f462-425">Beispielsweise führen Sie häufig die gleichen Anweisungen für jedes Element in einer Datensammlung aus.</span><span class="sxs-lookup"><span data-stu-id="5f462-425">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="5f462-426">Wenn Sie genau wissen, wie oft Sie eine Schleife durchführen möchten, können Sie eine `for` Schleife verwenden.</span><span class="sxs-lookup"><span data-stu-id="5f462-426">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="5f462-427">Diese Art von Schleife ist besonders nützlich, wenn Sie nach oben oder unten gezählt werden:</span><span class="sxs-lookup"><span data-stu-id="5f462-427">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="5f462-428">Die Schleife beginnt mit dem `for`-Schlüsselwort, gefolgt von drei Anweisungen in Klammern, die jeweils mit einem Semikolon beendet werden.</span><span class="sxs-lookup"><span data-stu-id="5f462-428">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="5f462-429">Innerhalb der Klammern erstellt die erste Anweisung (`var i=10;`) einen Counter und initialisiert sie mit 10.</span><span class="sxs-lookup"><span data-stu-id="5f462-429">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="5f462-430">Sie müssen den Counter nicht benennen, `i` &#8212; Sie beliebige Variablen verwenden können.</span><span class="sxs-lookup"><span data-stu-id="5f462-430">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="5f462-431">Wenn die `for`-Schleife ausgeführt wird, wird der-Wert automatisch inkrementiert.</span><span class="sxs-lookup"><span data-stu-id="5f462-431">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="5f462-432">Mit der zweiten Anweisung (`i < 21;`) wird die Bedingung für die gewünschte Anzahl festgelegt.</span><span class="sxs-lookup"><span data-stu-id="5f462-432">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="5f462-433">In diesem Fall möchten Sie, dass der Wert maximal 20 beträgt (d. h., der Wert ist kleiner als 21).</span><span class="sxs-lookup"><span data-stu-id="5f462-433">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="5f462-434">Die dritte Anweisung (`i++`) verwendet einen Inkrementoperator, der lediglich angibt, dass der-Wert bei jedem Ausführen der Schleife 1 hinzugefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="5f462-434">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="5f462-435">Innerhalb der geschweiften Klammern befindet sich der Code, der für jede Iterations Schleife ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="5f462-435">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="5f462-436">Das Markup erstellt jedes Mal einen neuen Absatz (`<p>`-Element) und fügt der Ausgabe eine Zeile hinzu, in der der Wert `i` (der Leistungs Bestellungs Wert) angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="5f462-436">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="5f462-437">Wenn Sie diese Seite ausführen, erstellt das Beispiel 11 Zeilen, in denen die Ausgabe angezeigt wird, wobei der Text in jeder Zeile die Element Nummer angibt.</span><span class="sxs-lookup"><span data-stu-id="5f462-437">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="5f462-439">Wenn Sie mit einer Sammlung oder einem Array arbeiten, verwenden Sie häufig eine `foreach` Schleife.</span><span class="sxs-lookup"><span data-stu-id="5f462-439">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="5f462-440">Eine Sammlung ist eine Gruppe von ähnlichen Objekten, und mit der `foreach`-Schleife können Sie für jedes Element in der Auflistung eine Aufgabe ausführen.</span><span class="sxs-lookup"><span data-stu-id="5f462-440">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="5f462-441">Diese Art von Schleife eignet sich für Auflistungen, da Sie im Gegensatz zu einer `for` Schleife den Wert nicht erhöhen oder eine Beschränkung festlegen müssen.</span><span class="sxs-lookup"><span data-stu-id="5f462-441">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="5f462-442">Stattdessen durchläuft der `foreach` Schleifen Code einfach die Auflistung, bis er abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="5f462-442">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="5f462-443">Der folgende Code gibt beispielsweise die Elemente in der `Request.ServerVariables`-Auflistung zurück, bei der es sich um ein Objekt handelt, das Informationen über den Webserver enthält.</span><span class="sxs-lookup"><span data-stu-id="5f462-443">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="5f462-444">Er verwendet eine `foreac` h-Schleife, um den Namen der einzelnen Elemente anzuzeigen, indem er ein neues `<li>` Element in einer Liste mit HTML-Auflistungs Zeichen erstellt.</span><span class="sxs-lookup"><span data-stu-id="5f462-444">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="5f462-445">Auf das `foreach` Schlüsselwort folgen Klammern, in denen Sie eine Variable deklarieren, die ein einzelnes Element in der Auflistung darstellt (im Beispiel `var item`), gefolgt vom `in` Schlüsselwort, gefolgt von der Auflistung, die Sie durchlaufen möchten.</span><span class="sxs-lookup"><span data-stu-id="5f462-445">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="5f462-446">Im Text der `foreach` Schleife können Sie mithilfe der Variablen, die Sie zuvor deklariert haben, auf das aktuelle Element zugreifen.</span><span class="sxs-lookup"><span data-stu-id="5f462-446">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="5f462-448">Verwenden Sie die `while`-Anweisung, um eine allgemeinere Schleife zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="5f462-448">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="5f462-449">Eine `while` Schleife beginnt mit dem Schlüsselwort "`while`", gefolgt von Klammern, in denen Sie angeben, wie lange die Schleife fortgesetzt wird (hier, solange `countNum` kleiner als 50 ist), und dann den Block, der wiederholt werden soll.</span><span class="sxs-lookup"><span data-stu-id="5f462-449">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="5f462-450">Schleifen werden in der Regel in einer Variablen oder einem Objekt, das für das zählen verwendet wird, Inkrement (zu) oder Dekrement (subtrahieren</span><span class="sxs-lookup"><span data-stu-id="5f462-450">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="5f462-451">Im Beispiel fügt der `+=`-Operator bei jeder Ausführung der Schleife 1 `countNum` hinzu.</span><span class="sxs-lookup"><span data-stu-id="5f462-451">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="5f462-452">(Um eine Variable in einer Schleife, die Sie anzählt, herabzusetzen, verwenden Sie den Dekrementoperator `-=`).</span><span class="sxs-lookup"><span data-stu-id="5f462-452">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="5f462-453">Objekte und Auflistungen</span><span class="sxs-lookup"><span data-stu-id="5f462-453">Objects and Collections</span></span>

<span data-ttu-id="5f462-454">Fast alles auf einer ASP.NET-Website ist ein Objekt, das auch die Webseite selbst enthält.</span><span class="sxs-lookup"><span data-stu-id="5f462-454">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="5f462-455">In diesem Abschnitt werden einige wichtige Objekte erläutert, mit denen Sie häufig in Ihrem Code arbeiten werden.</span><span class="sxs-lookup"><span data-stu-id="5f462-455">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="5f462-456">Seiten Objekte</span><span class="sxs-lookup"><span data-stu-id="5f462-456">Page Objects</span></span>

<span data-ttu-id="5f462-457">Das grundlegendste Objekt in ASP.net ist die Seite.</span><span class="sxs-lookup"><span data-stu-id="5f462-457">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="5f462-458">Sie können auf die Eigenschaften des Seiten Objekts direkt ohne qualifizierendes Objekt zugreifen.</span><span class="sxs-lookup"><span data-stu-id="5f462-458">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="5f462-459">Der folgende Code Ruft den Dateipfad der Seite ab, wobei das `Request` Objekt der Seite verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="5f462-459">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="5f462-460">Um das Festlegen von Eigenschaften und Methoden für das aktuelle Seiten Objekt zu vereinfachen, können Sie optional das Schlüsselwort `this` verwenden, um das Seiten Objekt in Ihrem Code darzustellen.</span><span class="sxs-lookup"><span data-stu-id="5f462-460">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="5f462-461">Im folgenden finden Sie das vorherige Codebeispiel, in dem `this` zur Darstellung der Seite hinzugefügt wurde:</span><span class="sxs-lookup"><span data-stu-id="5f462-461">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="5f462-462">Sie können die Eigenschaften des `Page`-Objekts verwenden, um zahlreiche Informationen zu erhalten, z. b.:</span><span class="sxs-lookup"><span data-stu-id="5f462-462">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="5f462-463">[https://login.microsoftonline.com/consumers/](`Request`).</span><span class="sxs-lookup"><span data-stu-id="5f462-463">`Request`.</span></span> <span data-ttu-id="5f462-464">Wie Sie bereits gesehen haben, handelt es sich hierbei um eine Sammlung von Informationen über die aktuelle Anforderung, einschließlich des Typs des Browsers, der die Anforderung gestellt hat, der URL der Seite, der Benutzeridentität usw.</span><span class="sxs-lookup"><span data-stu-id="5f462-464">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="5f462-465">[https://login.microsoftonline.com/consumers/](`Response`).</span><span class="sxs-lookup"><span data-stu-id="5f462-465">`Response`.</span></span> <span data-ttu-id="5f462-466">Dabei handelt es sich um eine Sammlung von Informationen über die Antwort (Seite), die an den Browser gesendet werden, wenn die Ausführung des Server Codes abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="5f462-466">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="5f462-467">Beispielsweise können Sie diese Eigenschaft verwenden, um Informationen in die Antwort zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="5f462-467">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="5f462-468">Sammlungsobjekte (Arrays und Wörterbücher)</span><span class="sxs-lookup"><span data-stu-id="5f462-468">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="5f462-469">Eine *Sammlung* ist eine Gruppe von Objekten desselben Typs, z. b. eine Auflistung von `Customer` Objekten aus einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="5f462-469">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="5f462-470">ASP.NET enthält viele integrierte Auflistungen, wie z. b. die `Request.Files` Auflistung.</span><span class="sxs-lookup"><span data-stu-id="5f462-470">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="5f462-471">Häufig arbeiten Sie mit Daten in Auflistungen.</span><span class="sxs-lookup"><span data-stu-id="5f462-471">You'll often work with data in collections.</span></span> <span data-ttu-id="5f462-472">Zwei gängige Auflistungs Typen sind das- *Array* und das- *Wörterbuch*.</span><span class="sxs-lookup"><span data-stu-id="5f462-472">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="5f462-473">Ein Array ist nützlich, wenn Sie eine Sammlung ähnlicher Elemente speichern möchten, aber keine separate Variable zum Speichern der einzelnen Elemente erstellen möchten:</span><span class="sxs-lookup"><span data-stu-id="5f462-473">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="5f462-474">Mit Arrays deklarieren Sie einen bestimmten Datentyp, z. b. `string`, `int`oder `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="5f462-474">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="5f462-475">Um anzugeben, dass die Variable ein Array enthalten kann, fügen Sie der Deklaration eckige Klammern hinzu (z. b. `string[]` oder `int[]`).</span><span class="sxs-lookup"><span data-stu-id="5f462-475">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="5f462-476">Sie können auf Elemente in einem Array zugreifen, indem Sie Ihre Position (Index) oder die `foreach`-Anweisung verwenden.</span><span class="sxs-lookup"><span data-stu-id="5f462-476">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="5f462-477">Array Indizes sind NULL basiert &#8212; , d. h., das erste Element befindet sich an Position 0, das zweite Element an Position 1 usw.</span><span class="sxs-lookup"><span data-stu-id="5f462-477">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="5f462-478">Sie können die Anzahl der Elemente in einem Array ermitteln, indem Sie die `Length`-Eigenschaft erhalten.</span><span class="sxs-lookup"><span data-stu-id="5f462-478">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="5f462-479">Um die Position eines bestimmten Elements im Array (zum Durchsuchen des Arrays) zu erhalten, verwenden Sie die `Array.IndexOf`-Methode.</span><span class="sxs-lookup"><span data-stu-id="5f462-479">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="5f462-480">Sie können auch den Inhalt eines Arrays umkehren (die `Array.Reverse`-Methode) oder den Inhalt (die `Array.Sort`-Methode) sortieren.</span><span class="sxs-lookup"><span data-stu-id="5f462-480">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="5f462-481">Die Ausgabe des Zeichen folgen Array Codes, der in einem Browser angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="5f462-481">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="5f462-483">Ein Wörterbuch ist eine Auflistung von Schlüssel-Wert-Paaren, bei der Sie den Schlüssel (oder Namen) bereitstellen, um den entsprechenden Wert festzulegen oder abzurufen:</span><span class="sxs-lookup"><span data-stu-id="5f462-483">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="5f462-484">Zum Erstellen eines Wörterbuchs verwenden Sie das `new`-Schlüsselwort, um anzugeben, dass Sie ein neues Dictionary-Objekt erstellen.</span><span class="sxs-lookup"><span data-stu-id="5f462-484">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="5f462-485">Mit dem `var`-Schlüsselwort können Sie einer Variablen ein Wörterbuch zuweisen.</span><span class="sxs-lookup"><span data-stu-id="5f462-485">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="5f462-486">Die Datentypen der Elemente im Wörterbuch werden mithilfe von spitzen Klammern (`< >`) angegeben.</span><span class="sxs-lookup"><span data-stu-id="5f462-486">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="5f462-487">Am Ende der Deklaration müssen Sie ein paar Klammern hinzufügen, da es sich hierbei tatsächlich um eine Methode handelt, die ein neues Wörterbuch erstellt.</span><span class="sxs-lookup"><span data-stu-id="5f462-487">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="5f462-488">Um dem Wörterbuch Elemente hinzuzufügen, können Sie die `Add`-Methode der Wörterbuch Variablen (in diesem Fall`myScores`) und dann einen Schlüssel und einen Wert angeben.</span><span class="sxs-lookup"><span data-stu-id="5f462-488">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="5f462-489">Alternativ können Sie eckige Klammern verwenden, um den Schlüssel anzugeben und eine einfache Zuweisung durchzuführen, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="5f462-489">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="5f462-490">Um einen Wert aus dem Wörterbuch zu erhalten, geben Sie den Schlüssel in eckige Klammern an:</span><span class="sxs-lookup"><span data-stu-id="5f462-490">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="5f462-491">Aufrufen von Methoden mit Parametern</span><span class="sxs-lookup"><span data-stu-id="5f462-491">Calling Methods with Parameters</span></span>

<span data-ttu-id="5f462-492">Wie Sie zuvor in diesem Artikel gelesen haben, können die Objekte, mit denen Sie programmieren, über Methoden verfügen.</span><span class="sxs-lookup"><span data-stu-id="5f462-492">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="5f462-493">Beispielsweise kann ein `Database` Objekt über eine `Database.Connect`-Methode verfügen.</span><span class="sxs-lookup"><span data-stu-id="5f462-493">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="5f462-494">Viele Methoden verfügen auch über einen oder mehrere Parameter.</span><span class="sxs-lookup"><span data-stu-id="5f462-494">Many methods also have one or more parameters.</span></span> <span data-ttu-id="5f462-495">Ein *Parameter* ist ein Wert, den Sie an eine-Methode übergeben, um die-Methode zum Abschließen der Aufgabe zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="5f462-495">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="5f462-496">Betrachten Sie z. b. eine Deklaration für die `Request.MapPath`-Methode, die drei Parameter annimmt:</span><span class="sxs-lookup"><span data-stu-id="5f462-496">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="5f462-497">(Die Zeile wurde umschließt, um Sie besser lesbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="5f462-497">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="5f462-498">Denken Sie daran, dass Sie Zeilenumbrüche fast an jeder Stelle platzieren können, außer in Zeichen folgen, die in Anführungszeichen eingeschlossen sind.)</span><span class="sxs-lookup"><span data-stu-id="5f462-498">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="5f462-499">Diese Methode gibt den physischen Pfad auf dem Server zurück, der einem angegebenen virtuellen Pfad entspricht.</span><span class="sxs-lookup"><span data-stu-id="5f462-499">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="5f462-500">Die drei Parameter für die-Methode sind `virtualPath`, `baseVirtualDir`und `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="5f462-500">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="5f462-501">(Beachten Sie, dass in der-Deklaration die Parameter mit den Datentypen der Daten aufgelistet werden, die Sie akzeptieren.) Wenn Sie diese Methode aufgerufen haben, müssen Sie Werte für alle drei Parameter angeben.</span><span class="sxs-lookup"><span data-stu-id="5f462-501">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="5f462-502">Der Razor-Syntax bietet Ihnen zwei Optionen zum Übergeben von Parametern an eine Methode: *Positions Parameter* und *benannte Parameter*.</span><span class="sxs-lookup"><span data-stu-id="5f462-502">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="5f462-503">Um eine Methode mit Positions Parametern aufzurufen, übergeben Sie die Parameter in einer strengen Reihenfolge, die in der Methoden Deklaration angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="5f462-503">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="5f462-504">(In der Regel wissen Sie diese Bestellung, indem Sie die Dokumentation für die-Methode lesen.) Sie müssen die Reihenfolge befolgen, und Sie können die Parameter &#8212; bei Bedarf nicht überspringen. Sie übergeben eine leere Zeichenfolge (`""`) oder `null` für einen positionellen Parameter, für den Sie keinen Wert besitzen.</span><span class="sxs-lookup"><span data-stu-id="5f462-504">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="5f462-505">Im folgenden Beispiel wird davon ausgegangen, dass Sie über einen Ordner namens *Scripts* auf Ihrer Website verfügen.</span><span class="sxs-lookup"><span data-stu-id="5f462-505">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="5f462-506">Der Code Ruft die `Request.MapPath`-Methode auf und übergibt Werte für die drei Parameter in der richtigen Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="5f462-506">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="5f462-507">Daraufhin wird der resultierende zugeordnete Pfad angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5f462-507">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="5f462-508">Wenn eine Methode über viele Parameter verfügt, können Sie Ihren Code mit benannten Parametern besser lesbar halten.</span><span class="sxs-lookup"><span data-stu-id="5f462-508">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="5f462-509">Um eine Methode mit benannten Parametern aufzurufen, geben Sie den Parameternamen gefolgt von einem Doppelpunkt (:) und dann den Wert an.</span><span class="sxs-lookup"><span data-stu-id="5f462-509">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="5f462-510">Der Vorteil von benannten Parametern besteht darin, dass Sie Sie in beliebiger Reihenfolge übergeben können.</span><span class="sxs-lookup"><span data-stu-id="5f462-510">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="5f462-511">(Ein Nachteil ist, dass der Methoden Aufrufwert nicht so kompakt ist.)</span><span class="sxs-lookup"><span data-stu-id="5f462-511">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="5f462-512">Im folgenden Beispiel wird dieselbe Methode wie oben aufgerufen, es werden jedoch benannte Parameter verwendet, um die Werte bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="5f462-512">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="5f462-513">Wie Sie sehen können, werden die Parameter in einer anderen Reihenfolge übermittelt.</span><span class="sxs-lookup"><span data-stu-id="5f462-513">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="5f462-514">Wenn Sie jedoch das vorherige Beispiel und dieses Beispiel ausführen, wird derselbe Wert zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="5f462-514">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="5f462-515">Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="5f462-515">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="5f462-516">Try-catch-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="5f462-516">Try-Catch Statements</span></span>

<span data-ttu-id="5f462-517">Sie verfügen häufig über Anweisungen in Ihrem Code, die aus Gründen außerhalb ihrer Kontrolle fehlschlagen können.</span><span class="sxs-lookup"><span data-stu-id="5f462-517">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="5f462-518">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="5f462-518">For example:</span></span>

- <span data-ttu-id="5f462-519">Wenn Ihr Code versucht, eine Datei zu erstellen oder darauf zuzugreifen, können alle möglichen Fehler auftreten.</span><span class="sxs-lookup"><span data-stu-id="5f462-519">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="5f462-520">Die gewünschte Datei ist möglicherweise nicht vorhanden, Sie ist möglicherweise gesperrt, der Code verfügt möglicherweise nicht über Berechtigungen usw.</span><span class="sxs-lookup"><span data-stu-id="5f462-520">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="5f462-521">Wenn Ihr Code versucht, Datensätze in einer Datenbank zu aktualisieren, können auch Berechtigungsprobleme auftreten, die Verbindung mit der Datenbank kann gelöscht werden, die zu speichernden Daten sind möglicherweise ungültig usw.</span><span class="sxs-lookup"><span data-stu-id="5f462-521">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="5f462-522">In Programmier begriffen werden diese Situationen als *Ausnahmen*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="5f462-522">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="5f462-523">Wenn im Code eine Ausnahme auftritt, wird eine Fehlermeldung generiert (ausgelöst), die für Benutzer am besten geeignet ist:</span><span class="sxs-lookup"><span data-stu-id="5f462-523">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="5f462-525">In Situationen, in denen in Ihrem Code Ausnahmen auftreten können, und um Fehlermeldungen dieses Typs zu vermeiden, können Sie `try/catch`-Anweisungen verwenden.</span><span class="sxs-lookup"><span data-stu-id="5f462-525">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="5f462-526">In der `try`-Anweisung führen Sie den Code aus, den Sie überprüfen.</span><span class="sxs-lookup"><span data-stu-id="5f462-526">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="5f462-527">In mindestens einer `catch`-Anweisung können Sie nach bestimmten Fehlern (bestimmte Typen von Ausnahmen) suchen, die möglicherweise aufgetreten sind.</span><span class="sxs-lookup"><span data-stu-id="5f462-527">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="5f462-528">Sie können so viele `catch` Anweisungen einschließen, wie Sie nach Fehlern suchen müssen, die Sie erwarten.</span><span class="sxs-lookup"><span data-stu-id="5f462-528">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="5f462-529">Es wird empfohlen, die Verwendung der `Response.Redirect`-Methode in `try/catch` Anweisungen zu vermeiden, da dies eine Ausnahme in der Seite verursachen kann.</span><span class="sxs-lookup"><span data-stu-id="5f462-529">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>

<span data-ttu-id="5f462-530">Das folgende Beispiel zeigt eine Seite, die bei der ersten Anforderung eine Textdatei erstellt und dann eine Schaltfläche anzeigt, mit der der Benutzer die Datei öffnen kann.</span><span class="sxs-lookup"><span data-stu-id="5f462-530">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="5f462-531">Im Beispiel wird absichtlich ein ungültiger Dateiname verwendet, damit eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="5f462-531">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="5f462-532">Der Code enthält `catch` Anweisungen für zwei mögliche Ausnahmen: `FileNotFoundException`, bei einem ungültigen Dateinamen und `DirectoryNotFoundException`. Dies tritt auf, wenn ASP.NET den Ordner nicht selbst finden kann.</span><span class="sxs-lookup"><span data-stu-id="5f462-532">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="5f462-533">(Sie können die Auskommentierung einer Anweisung im Beispiel aufheben, um zu sehen, wie Sie ausgeführt wird, wenn alles ordnungsgemäß funktioniert.)</span><span class="sxs-lookup"><span data-stu-id="5f462-533">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="5f462-534">Wenn der Code die Ausnahme nicht behandelt hat, sehen Sie eine Fehlerseite wie der vorherige Screenshot.</span><span class="sxs-lookup"><span data-stu-id="5f462-534">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="5f462-535">Mit dem `try/catch` Abschnitt wird jedoch verhindert, dass der Benutzer diese Fehlertypen erkennen kann.</span><span class="sxs-lookup"><span data-stu-id="5f462-535">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="5f462-536">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="5f462-536">Additional Resources</span></span>

<span data-ttu-id="5f462-537">**Programmieren mit Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="5f462-537">**Programming with Visual Basic**</span></span>

[<span data-ttu-id="5f462-538">Anhang: Visual Basic Sprache und Syntax</span><span class="sxs-lookup"><span data-stu-id="5f462-538">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)

<span data-ttu-id="5f462-539">**Referenzdokumentation**</span><span class="sxs-lookup"><span data-stu-id="5f462-539">**Reference Documentation**</span></span>

[<span data-ttu-id="5f462-540">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5f462-540">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="5f462-541">C#Kurse</span><span class="sxs-lookup"><span data-stu-id="5f462-541">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
