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
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="34298-103">Einführung in ASP.net-Webprogrammierung mithilfe der Razor-Syntax (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="34298-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>

<span data-ttu-id="34298-104">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="34298-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="34298-105">In diesem Artikel erhalten Sie einen Überblick über die Programmierung mit ASP.net Web Pages mithilfe der Razor-Syntax und Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="34298-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="34298-106">ASP.net ist die Technologie von Microsoft zum Ausführen dynamischer Webseiten auf Webservern.</span><span class="sxs-lookup"><span data-stu-id="34298-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="34298-107">**Lernen Sie**Folgendes:</span><span class="sxs-lookup"><span data-stu-id="34298-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="34298-108">Die ersten 8 Programmiertipps für den Einstieg in die Programmierung ASP.net Web Pages mithilfe Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="34298-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="34298-109">Grundlegende Programmier Konzepte, die Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="34298-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="34298-110">Was ist ASP.net Servercode und der Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="34298-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="34298-111">Software Versionen</span><span class="sxs-lookup"><span data-stu-id="34298-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="34298-112">ASP.net Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="34298-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="34298-113">Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="34298-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="34298-114">In den meisten Beispielen wird ASP.net Web Pages mit Razor-Syntax C#Verwendung von verwendet.</span><span class="sxs-lookup"><span data-stu-id="34298-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="34298-115">Die Razor-Syntax unterstützt jedoch auch Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="34298-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="34298-116">Um eine ASP.NET-Webseite in Visual Basic zu programmieren, erstellen Sie eine Webseite mit der Dateinamenerweiterung " *. vbhtml* ", und fügen Sie dann Visual Basic Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="34298-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="34298-117">In diesem Artikel erhalten Sie einen Überblick über die Verwendung der Visual Basic Sprache und Syntax zum Erstellen von ASP.NET-Webseiten.</span><span class="sxs-lookup"><span data-stu-id="34298-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="34298-118">Die Standard Website Vorlagen für Microsoft webmatrix (**Bäckerei**, **Fotogalerie**und **Starter Site**usw.) sind in C# und Visual Basic Versionen verfügbar.</span><span class="sxs-lookup"><span data-stu-id="34298-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="34298-119">Sie können die Visual Basic Vorlagen von als nuget-Pakete installieren.</span><span class="sxs-lookup"><span data-stu-id="34298-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="34298-120">Website Vorlagen werden im Stamm Ordner Ihrer Website in einem Ordner namens *Microsoft-Vorlagen*installiert.</span><span class="sxs-lookup"><span data-stu-id="34298-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>

## <a name="the-top-8-programming-tips"></a><span data-ttu-id="34298-121">Die Top 8-Programmiertipps</span><span class="sxs-lookup"><span data-stu-id="34298-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="34298-122">Dieser Abschnitt enthält einige Tipps, die Sie unbedingt kennen müssen, wenn Sie mit dem Schreiben von ASP.net-Servercode mit dem Razor-Syntax beginnen.</span><span class="sxs-lookup"><span data-stu-id="34298-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="34298-123">1. Sie fügen Code zu einer Seite mit dem @-Zeichen hinzu.</span><span class="sxs-lookup"><span data-stu-id="34298-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="34298-124">Das `@` Zeichen startet Inline Ausdrücke, Blöcke mit einer einzelnen Anweisung und Blöcke mit mehreren Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="34298-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="34298-125">Das in einem Browser angezeigte Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="34298-125">The result displayed in a browser:</span></span>

![Razor-Bild1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="34298-127">**HTML-Codierung**</span><span class="sxs-lookup"><span data-stu-id="34298-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="34298-128">Wenn Sie Inhalt in einer Seite mit dem `@` Zeichen anzeigen, wie in den vorherigen Beispielen, codiert ASP.net die Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="34298-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="34298-129">Dies ersetzt reservierte HTML-Zeichen (z. b. `<` und `>` und `&`) durch Codes, die es ermöglichen, dass die Zeichen als Zeichen in einer Webseite angezeigt werden, anstatt als HTML-Tags oder-Entitäten interpretiert zu werden.</span><span class="sxs-lookup"><span data-stu-id="34298-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="34298-130">Ohne HTML-Codierung wird die Ausgabe des Server Codes möglicherweise nicht ordnungsgemäß angezeigt und kann eine Seite für Sicherheitsrisiken verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="34298-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="34298-131">Wenn das Ziel darin besteht, HTML-Markup auszugeben, das Tags als Markup rendert (z. b. `<p></p>` für einen Absatz oder `<em></em>`, um Text hervorzuheben), lesen Sie den Abschnitt [Kombinieren von Text, Markup und Code in Code Blöcken](#BM_CombiningTextMarkupAndCode) weiter unten in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="34298-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="34298-132">Weitere Informationen zur HTML-Codierung finden Sie unter [Arbeiten mit HTML-Formularen auf ASP.net Web Pages Websites](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="34298-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>

### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="34298-133">2. Sie schließen Code Blöcke mit Code ein... Endcode</span><span class="sxs-lookup"><span data-stu-id="34298-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="34298-134">Ein Codeblock enthält mindestens eine Code Anweisung und ist in den Schlüsselwörtern `Code` und `End Code`eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="34298-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="34298-135">Platzieren Sie das öffnende `Code`-Schlüsselwort direkt &#8212; nach dem `@` Zeichen, da zwischen Ihnen kein Leerraum vorhanden sein darf.</span><span class="sxs-lookup"><span data-stu-id="34298-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="34298-136">Das in einem Browser angezeigte Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="34298-136">The result displayed in a browser:</span></span>

![Razor-Bild2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="34298-138">3. innerhalb eines-Blocks beenden Sie jede Code Anweisung mit einem Zeilenumbruch.</span><span class="sxs-lookup"><span data-stu-id="34298-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="34298-139">In einem Visual Basic Codeblock endet jede Anweisung mit einem Zeilenumbruch.</span><span class="sxs-lookup"><span data-stu-id="34298-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="34298-140">(Später in diesem Artikel wird eine Möglichkeit angezeigt, eine lange Code Anweisung bei Bedarf in mehrere Zeilen zu umschließen.)</span><span class="sxs-lookup"><span data-stu-id="34298-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="34298-141">4. Sie verwenden Variablen zum Speichern von Werten.</span><span class="sxs-lookup"><span data-stu-id="34298-141">4. You use variables to store values</span></span>

<span data-ttu-id="34298-142">Sie können Werte in einer *Variablen*speichern, einschließlich Zeichen folgen, Zahlen und Datumsangaben usw. Sie erstellen eine neue Variable mit dem `Dim`-Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="34298-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="34298-143">Sie können Variablen Werte direkt in eine Seite einfügen, indem Sie `@`verwenden.</span><span class="sxs-lookup"><span data-stu-id="34298-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="34298-144">Das in einem Browser angezeigte Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="34298-144">The result displayed in a browser:</span></span>

![Razor-Bild3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="34298-146">5. Sie schließen Literalzeichenfolgen-Werte in doppelte Anführungszeichen ein.</span><span class="sxs-lookup"><span data-stu-id="34298-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="34298-147">Eine *Zeichen* Folge ist eine Sequenz von Zeichen, die als Text behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="34298-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="34298-148">Um eine Zeichenfolge anzugeben, müssen Sie Sie in doppelte Anführungszeichen einschließen:</span><span class="sxs-lookup"><span data-stu-id="34298-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="34298-149">Wenn Sie doppelte Anführungszeichen in einen Zeichen folgen Wert einbetten möchten, fügen Sie zwei doppelte Anführungszeichen ein.</span><span class="sxs-lookup"><span data-stu-id="34298-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="34298-150">Wenn das doppelte Anführungszeichen einmal in der Seiten Ausgabe angezeigt werden soll, geben Sie es als `""` innerhalb der Zeichenfolge in Anführungszeichen ein, und `""""` geben Sie das doppelte Anführungszeichen in der Zeichenfolge in Anführungszeichen ein, wenn es zweimal angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="34298-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="34298-151">Das in einem Browser angezeigte Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="34298-151">The result displayed in a browser:</span></span>

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="34298-153">6. Visual Basic Code berücksichtigt nicht die Groß-/Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="34298-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="34298-154">Beim Visual Basic Sprache wird die Groß-/Kleinschreibung nicht beachtet</span><span class="sxs-lookup"><span data-stu-id="34298-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="34298-155">Programmier Schlüsselwörter (wie z. b. `Dim`, `If`und `True`) und Variablennamen (wie `myString`oder `subTotal`) können in jedem Fall geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="34298-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="34298-156">Die folgenden Codezeilen weisen der Variablen einen Wert `lastname` mit einem Kleinbuchstaben zu und geben dann den Variablen Wert mithilfe eines Großbuchstaben an die Seite aus.</span><span class="sxs-lookup"><span data-stu-id="34298-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="34298-157">Das in einem Browser angezeigte Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="34298-157">The result displayed in a browser:</span></span>

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="34298-159">7. ein Großteil ihrer Codierung umfasst das Arbeiten mit Objekten.</span><span class="sxs-lookup"><span data-stu-id="34298-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="34298-160">Ein-Objekt stellt eine Aufgabe dar, mit &#8212; der Sie eine Seite, ein Textfeld, eine Datei, ein Bild, eine Webanforderung, eine e-Mail-Nachricht, einen Kundendaten Satz (Datenbankzeile) usw. programmieren können. Objekte verfügen über Eigenschaften, die Ihre &#8212; Merkmale beschreiben: ein Textfeld-Objekt verfügt über eine `Text`-Eigenschaft, ein Request-Objekt verfügt über eine `Url`-Eigenschaft, eine e-Mail hat eine `From`-Eigenschaft, und ein Customer-Objekt verfügt über eine `FirstName`-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="34298-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="34298-161">-Objekte verfügen auch über Methoden, die die &quot;Verben&quot; können, die Sie ausführen können.</span><span class="sxs-lookup"><span data-stu-id="34298-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="34298-162">Beispiele hierfür sind die `Save`-Methode eines File-Objekts, die `Rotate`-Methode eines Bildobjekts und die `Send`-Methode eines e-Mail-Objekts.</span><span class="sxs-lookup"><span data-stu-id="34298-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="34298-163">Häufig arbeiten Sie mit dem `Request` Objekt, das Ihnen Informationen wie die Werte der Formularfelder auf der Seite (Textfelder usw.), den Typ des Browsers, der die Anforderung erteilt hat, die URL der Seite, die Benutzeridentität usw. Dieses Beispiel zeigt, wie Sie auf Eigenschaften des `Request` Objekts zugreifen und wie Sie die `MapPath`-Methode des `Request`-Objekts aufrufen, das Ihnen den absoluten Pfad der Seite auf dem Server liefert:</span><span class="sxs-lookup"><span data-stu-id="34298-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="34298-164">Das in einem Browser angezeigte Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="34298-164">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="34298-166">8. Sie können Code schreiben, der Entscheidungen trifft.</span><span class="sxs-lookup"><span data-stu-id="34298-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="34298-167">Ein wichtiges Feature von dynamischen Webseiten ist, dass Sie anhand der Bedingungen ermitteln können, was zu tun ist.</span><span class="sxs-lookup"><span data-stu-id="34298-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="34298-168">Die gängigste Methode hierfür ist die `If`-Anweisung (und optional `Else`-Anweisung).</span><span class="sxs-lookup"><span data-stu-id="34298-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="34298-169">Die-Anweisung `If IsPost` ist eine Kurzform zum Schreiben von `If IsPost = True`.</span><span class="sxs-lookup"><span data-stu-id="34298-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="34298-170">Zusammen mit `If`-Anweisungen gibt es eine Vielzahl von Möglichkeiten, Bedingungen zu testen, Code Blöcke zu wiederholen usw., die weiter unten in diesem Artikel beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="34298-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="34298-171">Das in einem Browser angezeigte Ergebnis (nach dem Klicken auf " **senden**"):</span><span class="sxs-lookup"><span data-stu-id="34298-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="34298-173">**HTTP Get-und Post-Methoden und die ispost-Eigenschaft**</span><span class="sxs-lookup"><span data-stu-id="34298-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="34298-174">Das Protokoll, das für Webseiten (http) verwendet wird, unterstützt eine sehr begrenzte Anzahl von Methoden (&quot;Verben&quot;), die verwendet werden, um Anforderungen an den Server zu senden.</span><span class="sxs-lookup"><span data-stu-id="34298-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="34298-175">Die beiden häufigsten sind Get, das zum Lesen einer Seite verwendet wird, und Post, der zum Senden einer Seite verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="34298-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="34298-176">Wenn ein Benutzer zum ersten Mal eine Seite anfordert, wird die Seite mithilfe von Get angefordert.</span><span class="sxs-lookup"><span data-stu-id="34298-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="34298-177">Wenn der Benutzer ein Formular ausfüllt und dann auf **senden**klickt, sendet der Browser eine Post-Anforderung an den Server.</span><span class="sxs-lookup"><span data-stu-id="34298-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="34298-178">Bei der Webprogrammierung ist es häufig hilfreich zu wissen, ob eine Seite als Get-oder Post-Vorgang angefordert wird, damit Sie wissen, wie die Seite verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="34298-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="34298-179">In ASP.net Web Pages können Sie die `IsPost`-Eigenschaft verwenden, um festzustellen, ob eine Anforderung ein Get-oder Post-Eintrag ist.</span><span class="sxs-lookup"><span data-stu-id="34298-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="34298-180">Wenn es sich bei der Anforderung um einen Post-Vorgang handelt, wird die `IsPost`-Eigenschaft "true" zurückgegeben. Sie können z. b. die Werte von Textfeldern in einem Formular lesen.</span><span class="sxs-lookup"><span data-stu-id="34298-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="34298-181">Viele Beispiele zeigen Ihnen, wie Sie die Seite je nach dem Wert `IsPost`anders verarbeiten können.</span><span class="sxs-lookup"><span data-stu-id="34298-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>

## <a name="a-simple-code-example"></a><span data-ttu-id="34298-182">Einfaches Code Beispiel</span><span class="sxs-lookup"><span data-stu-id="34298-182">A Simple Code Example</span></span>

<span data-ttu-id="34298-183">In diesem Verfahren wird gezeigt, wie Sie eine Seite erstellen, die grundlegende Programmiertechniken veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="34298-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="34298-184">In diesem Beispiel erstellen Sie eine Seite, mit der Benutzer zwei Zahlen eingeben können. Anschließend werden Sie hinzugefügt, und das Ergebnis wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="34298-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="34298-185">Erstellen Sie im Editor eine neue Datei, und nennen Sie Sie *AddNumbers. vbhtml*.</span><span class="sxs-lookup"><span data-stu-id="34298-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="34298-186">Kopieren Sie den folgenden Code und das Markup in die Seite, und ersetzen Sie alles, was bereits auf der Seite ist.</span><span class="sxs-lookup"><span data-stu-id="34298-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="34298-187">Beachten Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="34298-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="34298-188">Mit dem `@` Zeichen wird der erste Codeblock auf der Seite gestartet, und vor der `totalMessage` Variablen, die im unteren Bereich eingebettet ist.</span><span class="sxs-lookup"><span data-stu-id="34298-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="34298-189">Der Block am oberen Rand der Seite wird in `Code...End Code`eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="34298-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="34298-190">In den Variablen `total`, `num1`, `num2`und `totalMessage` werden mehrere Zahlen und eine Zeichenfolge gespeichert.</span><span class="sxs-lookup"><span data-stu-id="34298-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="34298-191">Der Literalzeichenfolgen-Wert, der der `totalMessage` Variablen zugewiesen ist, ist in doppelten Anführungszeichen.</span><span class="sxs-lookup"><span data-stu-id="34298-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="34298-192">Da bei Visual Basic Code die Groß-/Kleinschreibung nicht beachtet wird, muss der Name, wenn die `totalMessage` Variable im unteren Bereich der Seite verwendet wird, nur mit der Schreibweise der Variablen Deklaration am oberen Rand der Seite identisch sein.</span><span class="sxs-lookup"><span data-stu-id="34298-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="34298-193">Die Schreibweise spielt keine Rolle.</span><span class="sxs-lookup"><span data-stu-id="34298-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="34298-194">Der Ausdruck `num1.AsInt()` + `num2.AsInt()` zeigt, wie Sie mit Objekten und Methoden arbeiten.</span><span class="sxs-lookup"><span data-stu-id="34298-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="34298-195">Die `AsInt`-Methode für jede Variable konvertiert die von einem Benutzer eingegebene Zeichenfolge in eine ganze Zahl (eine Ganzzahl), die hinzugefügt werden kann.</span><span class="sxs-lookup"><span data-stu-id="34298-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="34298-196">Das `<form>`-Tag enthält ein `method="post"` Attribut.</span><span class="sxs-lookup"><span data-stu-id="34298-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="34298-197">Dies gibt an, dass die Seite mithilfe der HTTP Post-Methode an den Server gesendet wird, wenn der Benutzer auf **Hinzufügen**klickt.</span><span class="sxs-lookup"><span data-stu-id="34298-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="34298-198">Wenn die Seite übermittelt wird, wird der Code `If IsPost` als true ausgewertet, und der bedingte Code wird ausgeführt, und das Ergebnis des Hinzufügens der Zahlen wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="34298-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="34298-199">Speichern Sie die Seite, und führen Sie Sie in einem Browser aus.</span><span class="sxs-lookup"><span data-stu-id="34298-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="34298-200">(Stellen Sie sicher, dass die Seite im Arbeitsbereich " **Dateien** " ausgewählt ist, bevor Sie Sie ausführen.) Geben Sie zwei ganze Zahlen ein, und klicken Sie dann auf **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="34298-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="34298-202">Visual Basic Sprache und Syntax</span><span class="sxs-lookup"><span data-stu-id="34298-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="34298-203">Früher haben Sie ein einfaches Beispiel für das Erstellen einer ASP.NET-Webseite und die Vorgehensweise zum Hinzufügen von Servercode zu HTML-Markup gesehen.</span><span class="sxs-lookup"><span data-stu-id="34298-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="34298-204">Hier lernen Sie die Grundlagen der Verwendung von Visual Basic zum Schreiben von ASP.net-Servercode &#8212; mithilfe des Razor-Syntax der Programmiersprachen Regeln kennen.</span><span class="sxs-lookup"><span data-stu-id="34298-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="34298-205">Wenn Sie Erfahrung mit der Programmierung haben (insbesondere, wenn Sie C, C++, C#, Visual Basic oder JavaScript verwendet haben), werden viele der hier gelesenen Elemente Ihnen vertraut sein.</span><span class="sxs-lookup"><span data-stu-id="34298-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="34298-206">Sie müssen sich wahrscheinlich nur damit vertraut machen, wie webmatrix-Code Markup in *vbhtml* -Dateien hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="34298-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a><span data-ttu-id="34298-207">Kombinieren von Text, Markup und Code in Code Blöcken</span><span class="sxs-lookup"><span data-stu-id="34298-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="34298-208">In Servercode Blöcken möchten Sie häufig Text und Markup an die Seite ausgeben.</span><span class="sxs-lookup"><span data-stu-id="34298-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="34298-209">Wenn ein Server Codeblock Text enthält, der nicht Code ist und stattdessen unverändert gerendert werden soll, muss ASP.net in der Lage sein, diesen Text vom Code zu unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="34298-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="34298-210">Hierfür gibt es mehrere Möglichkeiten.</span><span class="sxs-lookup"><span data-stu-id="34298-210">There are several ways to do this.</span></span>

- <span data-ttu-id="34298-211">Schließen Sie den Text in ein HTML-Block Element wie `<p></p>` oder `<em></em>`ein:</span><span class="sxs-lookup"><span data-stu-id="34298-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="34298-212">Das HTML-Element kann Text, zusätzliche HTML-Elemente und Servercode Ausdrücke enthalten.</span><span class="sxs-lookup"><span data-stu-id="34298-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="34298-213">Wenn ASP.NET das öffnende HTML-Tag sieht (z. b. `<p>`), rendert das Element und dessen Inhalt unverändert im Browser (und löst die Servercode Ausdrücke auf).</span><span class="sxs-lookup"><span data-stu-id="34298-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="34298-214">Verwenden Sie den `@:`-Operator oder das `<text>`-Element.</span><span class="sxs-lookup"><span data-stu-id="34298-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="34298-215">Der `@:` gibt eine einzelne Zeile mit Inhalt aus, die nur-Text oder nicht übereinstimmende HTML-Tags enthält Das `<text>` Element schließt mehrere Zeilen ein, die ausgegeben werden sollen.</span><span class="sxs-lookup"><span data-stu-id="34298-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="34298-216">Diese Optionen sind hilfreich, wenn Sie ein HTML-Element nicht als Teil der Ausgabe renderingrensten möchten.</span><span class="sxs-lookup"><span data-stu-id="34298-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="34298-217">Im folgenden Beispiel wird das vorherige Beispiel wiederholt, aber es wird ein einzelnes Paar `<text>` Tags verwendet, um den zu Rendering enden Text einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="34298-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="34298-218">Im folgenden Beispiel schließen die Tags "`<text>`" und "`</text>`" drei Zeilen ein, die alle über einen nicht enthaltenen Text und nicht übereinstimmende HTML-Tags (`<br />`) sowie Servercode und übereinstimmende HTML-Tags verfügen.</span><span class="sxs-lookup"><span data-stu-id="34298-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="34298-219">Auch hier kann jede Zeile einzeln mit dem `@:`-Operator vorangestellt werden. Beides funktioniert.</span><span class="sxs-lookup"><span data-stu-id="34298-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="34298-220">Wenn Sie Text wie in diesem Abschnitt &#8212; gezeigt mit einem HTML--Element ausgeben, wird der `@:`-Operator oder &#8212; das `<text>` Element ASP.net die Ausgabe nicht in HTML-codiert.</span><span class="sxs-lookup"><span data-stu-id="34298-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="34298-221">(Wie bereits erwähnt, codiert ASP.net die Ausgabe von Servercode Ausdrücken und Servercode Blöcken, denen `@`vorangestellt ist, mit Ausnahme der in diesem Abschnitt notierten Sonderfälle.)</span><span class="sxs-lookup"><span data-stu-id="34298-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="34298-222">Whitespace</span><span class="sxs-lookup"><span data-stu-id="34298-222">Whitespace</span></span>

<span data-ttu-id="34298-223">Zusätzliche Leerzeichen in einer-Anweisung (und außerhalb eines Zeichenfolgenliterals) haben keine Auswirkung auf die Anweisung:</span><span class="sxs-lookup"><span data-stu-id="34298-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="34298-224">Unterbrechen von langen Anweisungen in mehrere Zeilen</span><span class="sxs-lookup"><span data-stu-id="34298-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="34298-225">Sie können eine lange Code Anweisung in mehrere Zeilen unterbrechen, indem Sie nach jeder Codezeile den Unterstrich `_` (der in Visual Basic als *Fortsetzungs Zeichen*bezeichnet wird) verwenden.</span><span class="sxs-lookup"><span data-stu-id="34298-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="34298-226">Fügen Sie am Ende der Zeile ein Leerzeichen und dann das Fortsetzungs Zeichen hinzu, um eine Anweisung in der nächsten Zeile zu unterbrechen.</span><span class="sxs-lookup"><span data-stu-id="34298-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="34298-227">Setzen Sie die-Anweisung in der nächsten Zeile fort.</span><span class="sxs-lookup"><span data-stu-id="34298-227">Continue the statement on the next line.</span></span> <span data-ttu-id="34298-228">Sie können-Anweisungen so viele Zeilen wie erforderlich umschließen, um die Lesbarkeit zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="34298-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="34298-229">Die folgenden Anweisungen sind identisch:</span><span class="sxs-lookup"><span data-stu-id="34298-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="34298-230">Sie können jedoch keine Linie in der Mitte eines Zeichenfolgenliterals umschließen.</span><span class="sxs-lookup"><span data-stu-id="34298-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="34298-231">Das folgende Beispiel funktioniert nicht:</span><span class="sxs-lookup"><span data-stu-id="34298-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="34298-232">Um eine lange Zeichenfolge zu kombinieren, die mit mehreren Zeilen wie dem obigen Code umschlossen ist, müssen Sie den *Verkettungs Operator* (`&`) verwenden, den Sie später in diesem Artikel sehen werden.</span><span class="sxs-lookup"><span data-stu-id="34298-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="34298-233">Code Kommentare</span><span class="sxs-lookup"><span data-stu-id="34298-233">Code comments</span></span>

<span data-ttu-id="34298-234">Mit Kommentaren können Sie Notizen für sich selbst oder für andere Personen hinterlassen.</span><span class="sxs-lookup"><span data-stu-id="34298-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="34298-235">Razor-Syntax Kommentaren wird `@*` vorangestellt und mit `*@`enden.</span><span class="sxs-lookup"><span data-stu-id="34298-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="34298-236">Innerhalb von Code Blöcken können Sie die Razor-Syntax Kommentare verwenden, oder Sie können normale Visual Basic Kommentarzeichen verwenden, wobei es sich um ein einfaches Anführungszeichen (`'`) handelt, das jeder Zeile vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="34298-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="34298-237">Variables</span><span class="sxs-lookup"><span data-stu-id="34298-237">Variables</span></span>

<span data-ttu-id="34298-238">Eine Variable ist ein benanntes Objekt, das Sie zum Speichern von Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="34298-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="34298-239">Sie können Variablen einen beliebigen Namen benennen, aber der Name muss mit einem alphabetischen Zeichen beginnen und darf keine Leerzeichen oder reservierten Zeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="34298-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="34298-240">Wie Sie bereits gesehen haben, ist die Groß-/Kleinschreibung der Buchstaben in einem Variablennamen in Visual Basic unerheblich.</span><span class="sxs-lookup"><span data-stu-id="34298-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="34298-241">Variablen und Datentypen</span><span class="sxs-lookup"><span data-stu-id="34298-241">Variables and data types</span></span>

<span data-ttu-id="34298-242">Eine Variable kann einen bestimmten Datentyp aufweisen, der angibt, welche Art von Daten in der Variablen gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="34298-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="34298-243">Sie können Zeichen folgen Variablen haben, die Zeichen folgen Werte (wie &quot;Hello World&quot;) speichern, ganzzahlige Variablen, die ganzzahlige Werte (z. b. 3 oder 79) speichern, und Datums Variablen, die Datumswerte in einer Vielzahl von Formaten (z. b. 4/12/2012 oder März 2009) speichern.</span><span class="sxs-lookup"><span data-stu-id="34298-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="34298-244">Und es gibt viele weitere Datentypen, die Sie verwenden können.</span><span class="sxs-lookup"><span data-stu-id="34298-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="34298-245">Allerdings müssen Sie keinen Typ für eine Variable angeben.</span><span class="sxs-lookup"><span data-stu-id="34298-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="34298-246">In den meisten Fällen kann ASP.NET den Typ basierend auf der Verwendung der Daten in der Variablen ermitteln.</span><span class="sxs-lookup"><span data-stu-id="34298-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="34298-247">(Gelegentlich müssen Sie einen Typ angeben. es werden Beispiele angezeigt, wenn dies zutrifft.)</span><span class="sxs-lookup"><span data-stu-id="34298-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="34298-248">Verwenden Sie `Dim` plus den Variablennamen (z. `Dim myVar`.), um eine Variable ohne Angabe eines Typs zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="34298-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="34298-249">Um eine Variable mit einem Typ zu deklarieren, verwenden Sie `Dim` plus den Variablennamen, gefolgt von `As` und dann den Typnamen (z.b. `Dim myVar As String`).</span><span class="sxs-lookup"><span data-stu-id="34298-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="34298-250">Das folgende Beispiel zeigt einige Inline Ausdrücke, die die Variablen in einer Webseite verwenden.</span><span class="sxs-lookup"><span data-stu-id="34298-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="34298-251">Das in einem Browser angezeigte Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="34298-251">The result displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="34298-253">Wandeln und Testen von Datentypen</span><span class="sxs-lookup"><span data-stu-id="34298-253">Converting and testing data types</span></span>

<span data-ttu-id="34298-254">Obwohl ASP.net normalerweise einen Datentyp automatisch ermitteln kann, kann dies manchmal nicht der Fall sein.</span><span class="sxs-lookup"><span data-stu-id="34298-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="34298-255">Daher müssen Sie möglicherweise ASP.net durch eine explizite Konvertierung unterstützen.</span><span class="sxs-lookup"><span data-stu-id="34298-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="34298-256">Auch wenn Sie keine Typen konvertieren müssen, ist es manchmal hilfreich, zu testen, um festzustellen, mit welchem Datentyp Sie möglicherweise arbeiten.</span><span class="sxs-lookup"><span data-stu-id="34298-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="34298-257">Der häufigste Fall ist, dass Sie eine Zeichenfolge in einen anderen Typ konvertieren müssen, z. b. in eine ganze Zahl oder ein Datum.</span><span class="sxs-lookup"><span data-stu-id="34298-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="34298-258">Das folgende Beispiel zeigt einen typischen Fall, in dem Sie eine Zeichenfolge in eine Zahl konvertieren müssen.</span><span class="sxs-lookup"><span data-stu-id="34298-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="34298-259">Als Regel werden Benutzereingaben als Zeichen folgen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="34298-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="34298-260">Auch wenn Sie den Benutzer aufgefordert haben, eine Zahl einzugeben, und auch wenn er eine Ziffer eingegeben hat, wenn die Benutzereingaben gesendet werden und Sie Sie im Code lesen, werden die Daten im Zeichen folgen Format angezeigt.</span><span class="sxs-lookup"><span data-stu-id="34298-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="34298-261">Daher müssen Sie die Zeichenfolge in eine Zahl konvertieren.</span><span class="sxs-lookup"><span data-stu-id="34298-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="34298-262">Wenn Sie in diesem Beispiel versuchen, arithmetische Werte für die Werte auszuführen, ohne Sie zu wandeln, wird die folgende Fehlermeldung ausgegeben, da ASP.net nicht zwei Zeichen folgen hinzufügen kann:</span><span class="sxs-lookup"><span data-stu-id="34298-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="34298-263">Um die Werte in ganze Zahlen zu konvertieren, wird die `AsInt`-Methode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="34298-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="34298-264">Wenn die Konvertierung erfolgreich ist, können Sie die Zahlen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="34298-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="34298-265">In der folgenden Tabelle werden einige allgemeine Konvertierungs-und Testmethoden für Variablen aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="34298-265">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="34298-266"><strong>Methode</strong></span><span class="sxs-lookup"><span data-stu-id="34298-266"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="34298-267"><strong>Beschreibung</strong></span><span class="sxs-lookup"><span data-stu-id="34298-267"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="34298-268"><strong>Beispiel</strong></span><span class="sxs-lookup"><span data-stu-id="34298-268"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="34298-269">Konvertiert eine Zeichenfolge, die eine ganze Zahl darstellt (z. b. &quot;593&quot;), in eine ganze Zahl.</span><span class="sxs-lookup"><span data-stu-id="34298-269">Converts a string that represents a whole number (like &quot;593&quot;) to an integer.</span></span>
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
        <span data-ttu-id="34298-270">Konvertiert eine Zeichenfolge wie &quot;true&quot; oder &quot;false-&quot; in einen booleschen Typ.</span><span class="sxs-lookup"><span data-stu-id="34298-270">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
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
        <span data-ttu-id="34298-271">Konvertiert eine Zeichenfolge mit einem Dezimalwert wie &quot;1,3&quot; oder &quot;7,439&quot; in eine Gleit Komma Zahl.</span><span class="sxs-lookup"><span data-stu-id="34298-271">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
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
        <span data-ttu-id="34298-272">Konvertiert eine Zeichenfolge mit einem Dezimalwert wie &quot;1,3&quot; oder &quot;7,439&quot; in eine Dezimalzahl.</span><span class="sxs-lookup"><span data-stu-id="34298-272">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="34298-273">(In ASP.net ist eine Dezimalzahl präziser als eine Gleit Komma Zahl.)</span><span class="sxs-lookup"><span data-stu-id="34298-273">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
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
        <span data-ttu-id="34298-274">Konvertiert eine Zeichenfolge, die einen Datums-und Uhrzeitwert darstellt, in den ASP.net-`DateTime` Typs.</span><span class="sxs-lookup"><span data-stu-id="34298-274">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
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
        <span data-ttu-id="34298-275">Konvertiert einen beliebigen anderen Datentyp in eine Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="34298-275">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="34298-276">Operatoren</span><span class="sxs-lookup"><span data-stu-id="34298-276">Operators</span></span>

<span data-ttu-id="34298-277">Ein Operator ist ein Schlüsselwort oder Zeichen, das ASP.net die Art des Befehls angibt, der in einem Ausdruck durchgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="34298-277">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="34298-278">Visual Basic unterstützt viele Operatoren, aber Sie müssen nur einige für den Einstieg in die Entwicklung von ASP.NET-Webseiten erkennen.</span><span class="sxs-lookup"><span data-stu-id="34298-278">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="34298-279">In der folgenden Tabelle werden die gängigsten Operatoren zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="34298-279">The following table summarizes the most common operators.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="34298-280"><strong>Operator</strong></span><span class="sxs-lookup"><span data-stu-id="34298-280"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="34298-281"><strong>Beschreibung</strong></span><span class="sxs-lookup"><span data-stu-id="34298-281"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="34298-282"><strong>Beispiele</strong></span><span class="sxs-lookup"><span data-stu-id="34298-282"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        <span data-ttu-id="34298-283">Mathematische Operatoren, die in numerischen Ausdrücken verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="34298-283">Math operators used in numerical expressions.</span></span>
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
        <span data-ttu-id="34298-284">Zuweisung und Gleichheit.</span><span class="sxs-lookup"><span data-stu-id="34298-284">Assignment and equality.</span></span> <span data-ttu-id="34298-285">Abhängig vom Kontext weist entweder den Wert auf der rechten Seite einer Anweisung dem-Objekt auf der linken Seite zu oder überprüft die Werte auf Gleichheit.</span><span class="sxs-lookup"><span data-stu-id="34298-285">Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.</span></span>
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
        <span data-ttu-id="34298-286">Ungleichheit.</span><span class="sxs-lookup"><span data-stu-id="34298-286">Inequality.</span></span> <span data-ttu-id="34298-287">Gibt `True` zurück, wenn die Werte nicht gleich sind.</span><span class="sxs-lookup"><span data-stu-id="34298-287">Returns `True` if the values are not equal.</span></span>
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
        <span data-ttu-id="34298-288">Kleiner als, größer als, kleiner als oder gleich und größer als oder gleich.</span><span class="sxs-lookup"><span data-stu-id="34298-288">Less than, greater than, less than or equal, and greater than or equal.</span></span>
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
        <span data-ttu-id="34298-289">Verkettung, die verwendet wird, um Zeichen folgen zu verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="34298-289">Concatenation, which is used to join strings.</span></span>
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
        <span data-ttu-id="34298-290">Die Inkrement-und Dekrementoperatoren, die 1 (bzw.) von einer Variablen addieren bzw. daraus ziehen.</span><span class="sxs-lookup"><span data-stu-id="34298-290">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
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
        <span data-ttu-id="34298-291">Gewinn.</span><span class="sxs-lookup"><span data-stu-id="34298-291">Dot.</span></span> <span data-ttu-id="34298-292">Wird verwendet, um Objekte und deren Eigenschaften und Methoden zu unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="34298-292">Used to distinguish objects and their properties and methods.</span></span>
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
        <span data-ttu-id="34298-293">Klammern.</span><span class="sxs-lookup"><span data-stu-id="34298-293">Parentheses.</span></span> <span data-ttu-id="34298-294">Wird zum Gruppieren von Ausdrücken, zum Übergeben von Parametern an Methoden und zum Zugreifen auf Member von Arrays und Auflistungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="34298-294">Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.</span></span>
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
        <span data-ttu-id="34298-295">Hätten.</span><span class="sxs-lookup"><span data-stu-id="34298-295">Not.</span></span> <span data-ttu-id="34298-296">Kehrt einen true-Wert in false um und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="34298-296">Reverses a true value to false and vice versa.</span></span> <span data-ttu-id="34298-297">Wird in der Regel als Kurzform zum Testen auf `False` verwendet (d. h. für nicht `True`).</span><span class="sxs-lookup"><span data-stu-id="34298-297">Typically used as a shorthand way to test for `False` (that is, for not `True`).</span></span>
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
        <span data-ttu-id="34298-298">Logisches and und or, die zum Verknüpfen von Bedingungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="34298-298">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="34298-299">Arbeiten mit Datei-und Ordner Pfaden im Code</span><span class="sxs-lookup"><span data-stu-id="34298-299">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="34298-300">Häufig arbeiten Sie mit Datei-und Ordner Pfaden in Ihrem Code.</span><span class="sxs-lookup"><span data-stu-id="34298-300">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="34298-301">Im folgenden finden Sie ein Beispiel für eine physische Ordnerstruktur für eine Website, wie Sie auf dem Entwicklungs Computer angezeigt werden kann:</span><span class="sxs-lookup"><span data-stu-id="34298-301">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="34298-302">Im folgenden finden Sie einige wichtige Details zu URLs und Pfaden:</span><span class="sxs-lookup"><span data-stu-id="34298-302">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="34298-303">Eine URL beginnt entweder mit einem Domänen Namen (`http://www.example.com`) oder einem Servernamen (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="34298-303">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="34298-304">Eine URL entspricht einem physischen Pfad auf einem Host Computer.</span><span class="sxs-lookup"><span data-stu-id="34298-304">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="34298-305">Beispielsweise können `http://myserver` dem Ordner *c:\websites\meinewebsite* auf dem Server entsprechen.</span><span class="sxs-lookup"><span data-stu-id="34298-305">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="34298-306">Ein virtueller Pfad ist eine kurzzeile, um Pfade im Code darzustellen, ohne den vollständigen Pfad angeben zu müssen.</span><span class="sxs-lookup"><span data-stu-id="34298-306">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="34298-307">Sie enthält den Teil einer URL, der auf den Domänen-oder Servernamen folgt.</span><span class="sxs-lookup"><span data-stu-id="34298-307">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="34298-308">Wenn Sie virtuelle Pfade verwenden, können Sie Ihren Code in eine andere Domäne oder einen anderen Server verschieben, ohne die Pfade aktualisieren zu müssen.</span><span class="sxs-lookup"><span data-stu-id="34298-308">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="34298-309">Hier finden Sie ein Beispiel, das Ihnen hilft, die Unterschiede zu verstehen:</span><span class="sxs-lookup"><span data-stu-id="34298-309">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="34298-310">Complete URL</span><span class="sxs-lookup"><span data-stu-id="34298-310">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="34298-311">Servername</span><span class="sxs-lookup"><span data-stu-id="34298-311">Server name</span></span> | <span data-ttu-id="34298-312">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="34298-312">*mycompanyserver*</span></span> |
| <span data-ttu-id="34298-313">Virtueller Pfad</span><span class="sxs-lookup"><span data-stu-id="34298-313">Virtual path</span></span> | <span data-ttu-id="34298-314">*/HumanResources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="34298-314">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="34298-315">Physischer Pfad</span><span class="sxs-lookup"><span data-stu-id="34298-315">Physical path</span></span> | <span data-ttu-id="34298-316">*C:\meinewebsites\humanresources\companypolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="34298-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="34298-317">Das virtuelle Stammverzeichnis ist/, genauso wie der Stamm des Laufwerks C: \.</span><span class="sxs-lookup"><span data-stu-id="34298-317">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="34298-318">(Pfade für virtuelle Ordner verwenden immer Schrägstriche.) Der virtuelle Pfad eines Ordners muss nicht denselben Namen wie der physische Ordner aufweisen. Dabei kann es sich um einen Alias handeln.</span><span class="sxs-lookup"><span data-stu-id="34298-318">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="34298-319">(Auf Produktionsservern entspricht der virtuelle Pfad selten einem exakten physischen Pfad.)</span><span class="sxs-lookup"><span data-stu-id="34298-319">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="34298-320">Beim Arbeiten mit Dateien und Ordnern im Code müssen Sie manchmal auf den physischen Pfad und manchmal auf einen virtuellen Pfad verweisen, je nachdem, mit welchen Objekten Sie arbeiten.</span><span class="sxs-lookup"><span data-stu-id="34298-320">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="34298-321">ASP.net bietet Ihnen diese Tools zum Arbeiten mit Datei-und Ordner Pfaden im Code: die `Server.MapPath`-Methode und der `~`-Operator und `Href`-Methode.</span><span class="sxs-lookup"><span data-stu-id="34298-321">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="34298-322">Umstellen von virtuellen in physische Pfade: die Server. MapPath-Methode</span><span class="sxs-lookup"><span data-stu-id="34298-322">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="34298-323">Die `Server.MapPath`-Methode konvertiert einen virtuellen Pfad (z. b. */default.cshtml*) in einen absoluten physischen Pfad (z. b. *c:\websites\mywebsitefolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="34298-323">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="34298-324">Sie verwenden diese Methode, wenn Sie einen kompletten physischen Pfad benötigen.</span><span class="sxs-lookup"><span data-stu-id="34298-324">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="34298-325">Ein typisches Beispiel ist das Lesen oder Schreiben einer Textdatei oder Bilddatei auf dem Webserver.</span><span class="sxs-lookup"><span data-stu-id="34298-325">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="34298-326">Der absolute physische Pfad der Website auf dem Server einer Host Website ist in der Regel nicht bekannt. Daher kann diese Methode den Pfad, den Sie kennen – den virtuellen Pfad –, in den entsprechenden Pfad auf dem Server konvertieren.</span><span class="sxs-lookup"><span data-stu-id="34298-326">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="34298-327">Sie übergeben den virtuellen Pfad an eine Datei oder einen Ordner an die-Methode und geben den physischen Pfad zurück:</span><span class="sxs-lookup"><span data-stu-id="34298-327">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="34298-328">Verweisen auf das virtuelle Stammverzeichnis: der ~-Operator und die href-Methode</span><span class="sxs-lookup"><span data-stu-id="34298-328">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="34298-329">In einer *cshtml* -oder *vbhtml* -Datei können Sie mithilfe des `~`-Operators auf den virtuellen Stammpfad verweisen.</span><span class="sxs-lookup"><span data-stu-id="34298-329">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="34298-330">Dies ist sehr praktisch, da Sie Seiten auf einer Website verschieben können und alle Links, die Sie auf anderen Seiten enthalten, nicht beschädigt werden.</span><span class="sxs-lookup"><span data-stu-id="34298-330">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="34298-331">Es ist auch praktisch, wenn Sie Ihre Website jemals an einen anderen Speicherort verschieben.</span><span class="sxs-lookup"><span data-stu-id="34298-331">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="34298-332">Im Folgenden finden Sie einige Beispiele:</span><span class="sxs-lookup"><span data-stu-id="34298-332">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="34298-333">Wenn die Website `http://myserver/myapp`ist, so behandelt ASP.net diese Pfade, wenn die Seite ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="34298-333">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="34298-334">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="34298-334">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="34298-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="34298-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="34298-336">(Diese Pfade werden nicht als Werte der Variablen angezeigt, aber ASP.NET behandelt die Pfade so, als wären Sie so.)</span><span class="sxs-lookup"><span data-stu-id="34298-336">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="34298-337">Sie können den `~`-Operator sowohl im Servercode (wie oben) als auch im Markup wie folgt verwenden:</span><span class="sxs-lookup"><span data-stu-id="34298-337">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="34298-338">In Markup verwenden Sie den `~`-Operator, um Pfade zu Ressourcen wie Bilddateien, anderen Webseiten und CSS-Dateien zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="34298-338">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="34298-339">Wenn die Seite ausgeführt wird, durchsucht ASP.net die Seite (sowohl Code als auch Markup) und löst alle `~` Verweise auf den entsprechenden Pfad auf.</span><span class="sxs-lookup"><span data-stu-id="34298-339">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="34298-340">Bedingte Logik und Schleifen</span><span class="sxs-lookup"><span data-stu-id="34298-340">Conditional Logic and Loops</span></span>

<span data-ttu-id="34298-341">ASP.net-Servercode ermöglicht Ihnen das Ausführen von Aufgaben auf der Grundlage von Bedingungen und das Schreiben von Code, der-Anweisungen eine bestimmte Anzahl von Wiederholungen (Code, der eine Schleife ausführt) wiederholt.</span><span class="sxs-lookup"><span data-stu-id="34298-341">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="34298-342">Testbedingungen</span><span class="sxs-lookup"><span data-stu-id="34298-342">Testing conditions</span></span>

<span data-ttu-id="34298-343">Um eine einfache Bedingung zu testen, verwenden Sie die `If...Then`-Anweisung, die basierend auf einem von Ihnen angegebenen Test `True` oder `False` zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="34298-343">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="34298-344">Das `If`-Schlüsselwort startet einen-Block.</span><span class="sxs-lookup"><span data-stu-id="34298-344">The `If` keyword starts a block.</span></span> <span data-ttu-id="34298-345">Der tatsächliche Test (Bedingung) folgt dem `If`-Schlüsselwort und gibt true oder false zurück.</span><span class="sxs-lookup"><span data-stu-id="34298-345">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="34298-346">Die `If`-Anweisung endet mit `Then`.</span><span class="sxs-lookup"><span data-stu-id="34298-346">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="34298-347">Die-Anweisungen, die ausgeführt werden, wenn der Test true ist, werden durch `If` und `End If`eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="34298-347">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="34298-348">Eine `If`-Anweisung kann einen `Else`-Block enthalten, der Anweisungen angibt, die ausgeführt werden sollen, wenn die Bedingung false ist:</span><span class="sxs-lookup"><span data-stu-id="34298-348">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="34298-349">Wenn eine `If` Anweisung einen Codeblock startet, müssen Sie die normalen `Code...End Code` Anweisungen nicht verwenden, um die Blöcke einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="34298-349">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="34298-350">Sie können dem-Block einfach `@` hinzufügen, und er funktioniert.</span><span class="sxs-lookup"><span data-stu-id="34298-350">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="34298-351">Diese Vorgehensweise funktioniert sowohl mit `If` als auch anderen Visual Basic Programmier Schlüsselwörtern, auf die Code Blöcke folgen, einschließlich `For`, `For Each`, `Do While`usw.</span><span class="sxs-lookup"><span data-stu-id="34298-351">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="34298-352">Sie können mehrere Bedingungen mithilfe eines oder mehrerer `ElseIf` Blöcke hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="34298-352">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="34298-353">Wenn in diesem Beispiel die erste Bedingung im `If`-Block nicht true ist, wird die `ElseIf` Bedingung geprüft.</span><span class="sxs-lookup"><span data-stu-id="34298-353">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="34298-354">Wenn diese Bedingung erfüllt ist, werden die Anweisungen im `ElseIf`-Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="34298-354">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="34298-355">Wenn keine der Bedingungen erfüllt ist, werden die Anweisungen im `Else`-Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="34298-355">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="34298-356">Sie können beliebig viele `ElseIf` Blöcke hinzufügen und dann mit einem `Else`-Block als &quot;alles andere&quot; Bedingung schließen.</span><span class="sxs-lookup"><span data-stu-id="34298-356">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="34298-357">Um eine große Anzahl von Bedingungen zu testen, verwenden Sie einen `Select Case`-Block:</span><span class="sxs-lookup"><span data-stu-id="34298-357">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="34298-358">Der zu überprüfende Wert ist in Klammern (im Beispiel die Wochentag-Variable).</span><span class="sxs-lookup"><span data-stu-id="34298-358">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="34298-359">Jeder einzelne Test verwendet eine `Case`-Anweisung, die einen Wert auflistet.</span><span class="sxs-lookup"><span data-stu-id="34298-359">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="34298-360">Wenn der Wert einer `Case`-Anweisung mit dem Testwert übereinstimmt, wird der Code in diesem `Case`-Block ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="34298-360">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="34298-361">Das Ergebnis der letzten beiden bedingten Blöcke, die in einem Browser angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="34298-361">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="34298-363">Schleifen Code</span><span class="sxs-lookup"><span data-stu-id="34298-363">Looping code</span></span>

<span data-ttu-id="34298-364">Häufig müssen Sie dieselben Anweisungen wiederholt ausführen.</span><span class="sxs-lookup"><span data-stu-id="34298-364">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="34298-365">Dies geschieht durch Schleifen.</span><span class="sxs-lookup"><span data-stu-id="34298-365">You do this by looping.</span></span> <span data-ttu-id="34298-366">Beispielsweise führen Sie häufig die gleichen Anweisungen für jedes Element in einer Datensammlung aus.</span><span class="sxs-lookup"><span data-stu-id="34298-366">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="34298-367">Wenn Sie genau wissen, wie oft Sie eine Schleife durchführen möchten, können Sie eine `For` Schleife verwenden.</span><span class="sxs-lookup"><span data-stu-id="34298-367">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="34298-368">Diese Art von Schleife ist besonders nützlich, wenn Sie nach oben oder unten gezählt werden:</span><span class="sxs-lookup"><span data-stu-id="34298-368">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="34298-369">Die Schleife beginnt mit dem `For`-Schlüsselwort, gefolgt von drei Elementen:</span><span class="sxs-lookup"><span data-stu-id="34298-369">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="34298-370">Direkt nach der `For`-Anweisung deklarieren Sie eine Indikator Variable (Sie müssen keine `Dim`verwenden), und geben Sie dann den Bereich wie in `i = 10 to 20`an.</span><span class="sxs-lookup"><span data-stu-id="34298-370">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="34298-371">Dies bedeutet, dass die Variable `i` bei 10 beginnt und fortgesetzt wird, bis 20 (einschließlich) erreicht wird.</span><span class="sxs-lookup"><span data-stu-id="34298-371">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="34298-372">Zwischen der `For`-und `Next`-Anweisung ist der Inhalt des-Blocks.</span><span class="sxs-lookup"><span data-stu-id="34298-372">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="34298-373">Dies kann eine oder mehrere Code Anweisungen enthalten, die mit jeder Schleife ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="34298-373">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="34298-374">Die `Next i`-Anweisung beendet die Schleife.</span><span class="sxs-lookup"><span data-stu-id="34298-374">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="34298-375">Der Wert wird erhöht, und die nächste Iterations Schleife wird gestartet.</span><span class="sxs-lookup"><span data-stu-id="34298-375">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="34298-376">Die Codezeile zwischen dem `For` und `Next` Zeilen enthält den Code, der für jede Iterations Schleife ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="34298-376">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="34298-377">Das Markup erstellt jedes Mal einen neuen Absatz (`<p>` Element) und fügt der Ausgabe eine Zeile hinzu, in der der Wert i (The Counter) angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="34298-377">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="34298-378">Wenn Sie diese Seite ausführen, erstellt das Beispiel 11 Zeilen, in denen die Ausgabe angezeigt wird, wobei der Text in jeder Zeile die Element Nummer angibt.</span><span class="sxs-lookup"><span data-stu-id="34298-378">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="34298-380">Wenn Sie mit einer Sammlung oder einem Array arbeiten, verwenden Sie häufig eine `For Each` Schleife.</span><span class="sxs-lookup"><span data-stu-id="34298-380">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="34298-381">Eine Sammlung ist eine Gruppe von ähnlichen Objekten, und mit der `For Each`-Schleife können Sie für jedes Element in der Auflistung eine Aufgabe ausführen.</span><span class="sxs-lookup"><span data-stu-id="34298-381">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="34298-382">Diese Art von Schleife eignet sich für Auflistungen, da Sie im Gegensatz zu einer `For` Schleife den Wert nicht erhöhen oder eine Beschränkung festlegen müssen.</span><span class="sxs-lookup"><span data-stu-id="34298-382">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="34298-383">Stattdessen durchläuft der `For Each` Schleifen Code einfach die Auflistung, bis er abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="34298-383">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="34298-384">In diesem Beispiel werden die Elemente in der `Request.ServerVariables`-Auflistung zurückgegeben, die Informationen über Ihren Webserver enthält.</span><span class="sxs-lookup"><span data-stu-id="34298-384">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="34298-385">Es wird eine `For Each` Schleife verwendet, um den Namen der einzelnen Elemente anzuzeigen, indem ein neues `<li>` Element in einer Liste mit HTML-Auflistungs Zeichen erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="34298-385">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="34298-386">Auf das `For Each` Schlüsselwort folgt eine Variable, die ein einzelnes Element in der Auflistung darstellt (im Beispiel `myItem`), gefolgt vom `In` Schlüsselwort, gefolgt von der Auflistung, die Sie durchlaufen möchten.</span><span class="sxs-lookup"><span data-stu-id="34298-386">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="34298-387">Im Text der `For Each` Schleife können Sie mithilfe der Variablen, die Sie zuvor deklariert haben, auf das aktuelle Element zugreifen.</span><span class="sxs-lookup"><span data-stu-id="34298-387">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="34298-389">Verwenden Sie die `Do While`-Anweisung, um eine allgemeinere Schleife zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="34298-389">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="34298-390">Diese Schleife beginnt mit dem `Do While`-Schlüsselwort, gefolgt von einer Bedingung, gefolgt vom Block, der wiederholt werden soll.</span><span class="sxs-lookup"><span data-stu-id="34298-390">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="34298-391">Schleifen werden in der Regel in einer Variablen oder einem Objekt, das für das zählen verwendet wird, Inkrement (zu) oder Dekrement (subtrahieren</span><span class="sxs-lookup"><span data-stu-id="34298-391">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="34298-392">Im Beispiel fügt der `+=`-Operator bei jeder Ausführung der Schleife 1 zum Wert einer Variablen hinzu.</span><span class="sxs-lookup"><span data-stu-id="34298-392">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="34298-393">(Um eine Variable in einer Schleife, die Sie anzählt, herabzusetzen, verwenden Sie den Dekrementoperator `-=`.)</span><span class="sxs-lookup"><span data-stu-id="34298-393">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="34298-394">Objekte und Auflistungen</span><span class="sxs-lookup"><span data-stu-id="34298-394">Objects and Collections</span></span>

<span data-ttu-id="34298-395">Fast alles auf einer ASP.NET-Website ist ein Objekt, das auch die Webseite selbst enthält.</span><span class="sxs-lookup"><span data-stu-id="34298-395">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="34298-396">In diesem Abschnitt werden einige wichtige Objekte erläutert, mit denen Sie häufig in Ihrem Code arbeiten werden.</span><span class="sxs-lookup"><span data-stu-id="34298-396">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="34298-397">Seiten Objekte</span><span class="sxs-lookup"><span data-stu-id="34298-397">Page objects</span></span>

<span data-ttu-id="34298-398">Das grundlegendste Objekt in ASP.net ist die Seite.</span><span class="sxs-lookup"><span data-stu-id="34298-398">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="34298-399">Sie können auf die Eigenschaften des Seiten Objekts direkt ohne qualifizierendes Objekt zugreifen.</span><span class="sxs-lookup"><span data-stu-id="34298-399">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="34298-400">Der folgende Code Ruft den Dateipfad der Seite ab, wobei das `Request` Objekt der Seite verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="34298-400">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="34298-401">Sie können die Eigenschaften des `Page`-Objekts verwenden, um zahlreiche Informationen zu erhalten, z. b.:</span><span class="sxs-lookup"><span data-stu-id="34298-401">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="34298-402">[https://login.microsoftonline.com/consumers/](`Request`).</span><span class="sxs-lookup"><span data-stu-id="34298-402">`Request`.</span></span> <span data-ttu-id="34298-403">Wie Sie bereits gesehen haben, handelt es sich hierbei um eine Sammlung von Informationen über die aktuelle Anforderung, einschließlich des Typs des Browsers, der die Anforderung gestellt hat, der URL der Seite, der Benutzeridentität usw.</span><span class="sxs-lookup"><span data-stu-id="34298-403">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="34298-404">[https://login.microsoftonline.com/consumers/](`Response`).</span><span class="sxs-lookup"><span data-stu-id="34298-404">`Response`.</span></span> <span data-ttu-id="34298-405">Dabei handelt es sich um eine Sammlung von Informationen über die Antwort (Seite), die an den Browser gesendet werden, wenn die Ausführung des Server Codes abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="34298-405">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="34298-406">Beispielsweise können Sie diese Eigenschaft verwenden, um Informationen in die Antwort zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="34298-406">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="34298-407">Sammlungsobjekte (Arrays und Wörterbücher)</span><span class="sxs-lookup"><span data-stu-id="34298-407">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="34298-408">Eine Sammlung ist eine Gruppe von Objekten desselben Typs, z. b. eine Auflistung von `Customer` Objekten aus einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="34298-408">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="34298-409">ASP.NET enthält viele integrierte Auflistungen, wie z. b. die `Request.Files` Auflistung.</span><span class="sxs-lookup"><span data-stu-id="34298-409">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="34298-410">Häufig arbeiten Sie mit Daten in Auflistungen.</span><span class="sxs-lookup"><span data-stu-id="34298-410">You'll often work with data in collections.</span></span> <span data-ttu-id="34298-411">Zwei gängige Auflistungs Typen sind das- *Array* und das- *Wörterbuch*.</span><span class="sxs-lookup"><span data-stu-id="34298-411">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="34298-412">Ein Array ist nützlich, wenn Sie eine Sammlung ähnlicher Elemente speichern möchten, aber keine separate Variable zum Speichern der einzelnen Elemente erstellen möchten:</span><span class="sxs-lookup"><span data-stu-id="34298-412">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="34298-413">Mit Arrays deklarieren Sie einen bestimmten Datentyp, z. b. `String`, `Integer`oder `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="34298-413">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="34298-414">Um anzugeben, dass die Variable ein Array enthalten kann, fügen Sie dem Variablennamen in der Deklaration Klammern hinzu (z. b. `Dim myVar() As String`).</span><span class="sxs-lookup"><span data-stu-id="34298-414">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="34298-415">Sie können auf Elemente in einem Array zugreifen, indem Sie Ihre Position (Index) oder die `For Each`-Anweisung verwenden.</span><span class="sxs-lookup"><span data-stu-id="34298-415">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="34298-416">Array Indizes sind NULL basiert &#8212; , d. h., das erste Element befindet sich an Position 0, das zweite Element an Position 1 usw.</span><span class="sxs-lookup"><span data-stu-id="34298-416">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="34298-417">Sie können die Anzahl der Elemente in einem Array ermitteln, indem Sie die `Length`-Eigenschaft erhalten.</span><span class="sxs-lookup"><span data-stu-id="34298-417">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="34298-418">Um die Position eines bestimmten Elements im Array (d. h. zum Durchsuchen des Arrays) zu erhalten, verwenden Sie die `Array.IndexOf`-Methode.</span><span class="sxs-lookup"><span data-stu-id="34298-418">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="34298-419">Sie können auch den Inhalt eines Arrays umkehren (die `Array.Reverse`-Methode) oder den Inhalt (die `Array.Sort`-Methode) sortieren.</span><span class="sxs-lookup"><span data-stu-id="34298-419">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="34298-420">Die Ausgabe des Zeichen folgen Array Codes, der in einem Browser angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="34298-420">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="34298-422">Ein Wörterbuch ist eine Auflistung von Schlüssel-Wert-Paaren, bei der Sie den Schlüssel (oder Namen) bereitstellen, um den entsprechenden Wert festzulegen oder abzurufen:</span><span class="sxs-lookup"><span data-stu-id="34298-422">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="34298-423">Zum Erstellen eines Wörterbuchs verwenden Sie das `New`-Schlüsselwort, um anzugeben, dass Sie ein neues `Dictionary` Objekt erstellen.</span><span class="sxs-lookup"><span data-stu-id="34298-423">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="34298-424">Mit dem `Dim`-Schlüsselwort können Sie einer Variablen ein Wörterbuch zuweisen.</span><span class="sxs-lookup"><span data-stu-id="34298-424">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="34298-425">Sie geben die Datentypen der Elemente im Wörterbuch mithilfe von Klammern (`( )`) an.</span><span class="sxs-lookup"><span data-stu-id="34298-425">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="34298-426">Am Ende der Deklaration müssen Sie ein weiteres Paar Klammern hinzufügen, da es sich hierbei um eine Methode handelt, die ein neues Wörterbuch erstellt.</span><span class="sxs-lookup"><span data-stu-id="34298-426">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="34298-427">Um dem Wörterbuch Elemente hinzuzufügen, können Sie die `Add`-Methode der Wörterbuch Variablen (in diesem Fall`myScores`) und dann einen Schlüssel und einen Wert angeben.</span><span class="sxs-lookup"><span data-stu-id="34298-427">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="34298-428">Alternativ können Sie Klammern verwenden, um den Schlüssel anzugeben und eine einfache Zuweisung durchzuführen, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="34298-428">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="34298-429">Um einen Wert aus dem Wörterbuch zu erhalten, geben Sie den Schlüssel in Klammern an:</span><span class="sxs-lookup"><span data-stu-id="34298-429">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="34298-430">Aufrufen von Methoden mit Parametern</span><span class="sxs-lookup"><span data-stu-id="34298-430">Calling Methods with Parameters</span></span>

<span data-ttu-id="34298-431">Wie Sie bereits zuvor in diesem Artikel gesehen haben, verfügen die Objekte, mit denen Sie programmieren, über Methoden.</span><span class="sxs-lookup"><span data-stu-id="34298-431">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="34298-432">Beispielsweise kann ein `Database` Objekt über eine `Database.Connect`-Methode verfügen.</span><span class="sxs-lookup"><span data-stu-id="34298-432">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="34298-433">Viele Methoden verfügen auch über einen oder mehrere Parameter.</span><span class="sxs-lookup"><span data-stu-id="34298-433">Many methods also have one or more parameters.</span></span> <span data-ttu-id="34298-434">Ein *Parameter* ist ein Wert, den Sie an eine-Methode übergeben, um die-Methode zum Abschließen der Aufgabe zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="34298-434">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="34298-435">Betrachten Sie z. b. eine Deklaration für die `Request.MapPath`-Methode, die drei Parameter annimmt:</span><span class="sxs-lookup"><span data-stu-id="34298-435">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="34298-436">Diese Methode gibt den physischen Pfad auf dem Server zurück, der einem angegebenen virtuellen Pfad entspricht.</span><span class="sxs-lookup"><span data-stu-id="34298-436">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="34298-437">Die drei Parameter für die-Methode sind `virtualPath`, `baseVirtualDir`und `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="34298-437">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="34298-438">(Beachten Sie, dass in der-Deklaration die Parameter mit den Datentypen der Daten aufgelistet werden, die Sie akzeptieren.) Wenn Sie diese Methode aufgerufen haben, müssen Sie Werte für alle drei Parameter angeben.</span><span class="sxs-lookup"><span data-stu-id="34298-438">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="34298-439">Wenn Sie Visual Basic mit dem Razor-Syntax verwenden, haben Sie zwei Optionen zum Übergeben von Parametern an eine Methode: *Positions Parameter* oder *benannte Parameter*.</span><span class="sxs-lookup"><span data-stu-id="34298-439">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="34298-440">Um eine Methode mit Positions Parametern aufzurufen, übergeben Sie die Parameter in einer strengen Reihenfolge, die in der Methoden Deklaration angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="34298-440">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="34298-441">(In der Regel wissen Sie diese Bestellung, indem Sie die Dokumentation für die-Methode lesen.) Sie müssen die Reihenfolge befolgen, und Sie können die Parameter &#8212; bei Bedarf nicht überspringen. Sie übergeben eine leere Zeichenfolge (`""`) oder NULL für einen positionellen Parameter, für den Sie keinen Wert haben.</span><span class="sxs-lookup"><span data-stu-id="34298-441">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="34298-442">Im folgenden Beispiel wird davon ausgegangen, dass Sie über einen Ordner namens *Scripts* auf Ihrer Website verfügen.</span><span class="sxs-lookup"><span data-stu-id="34298-442">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="34298-443">Der Code Ruft die `Request.MapPath`-Methode auf und übergibt Werte für die drei Parameter in der richtigen Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="34298-443">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="34298-444">Daraufhin wird der resultierende zugeordnete Pfad angezeigt.</span><span class="sxs-lookup"><span data-stu-id="34298-444">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="34298-445">Wenn für eine Methode viele Parameter vorhanden sind, können Sie Ihren Code bereinigen und besser lesbarer machen, indem Sie benannte Parameter verwenden.</span><span class="sxs-lookup"><span data-stu-id="34298-445">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="34298-446">Um eine Methode mit benannten Parametern aufzurufen, geben Sie den Parameternamen gefolgt von `:=` an, und geben Sie dann den Wert an.</span><span class="sxs-lookup"><span data-stu-id="34298-446">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="34298-447">Ein Vorteil von benannten Parametern ist, dass Sie Sie in beliebiger Reihenfolge hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="34298-447">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="34298-448">(Ein Nachteil ist, dass der Methoden Aufrufwert nicht so kompakt ist.)</span><span class="sxs-lookup"><span data-stu-id="34298-448">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="34298-449">Im folgenden Beispiel wird dieselbe Methode wie oben aufgerufen, es werden jedoch benannte Parameter verwendet, um die Werte bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="34298-449">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="34298-450">Wie Sie sehen können, werden die Parameter in einer anderen Reihenfolge übermittelt.</span><span class="sxs-lookup"><span data-stu-id="34298-450">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="34298-451">Wenn Sie jedoch das vorherige Beispiel und dieses Beispiel ausführen, wird derselbe Wert zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="34298-451">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="34298-452">Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="34298-452">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="34298-453">Try-catch-Anweisungen</span><span class="sxs-lookup"><span data-stu-id="34298-453">Try-Catch statements</span></span>

<span data-ttu-id="34298-454">Sie verfügen häufig über Anweisungen in Ihrem Code, die aus Gründen außerhalb ihrer Kontrolle fehlschlagen können.</span><span class="sxs-lookup"><span data-stu-id="34298-454">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="34298-455">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="34298-455">For example:</span></span>

- <span data-ttu-id="34298-456">Wenn Ihr Code versucht, eine Datei zu öffnen, zu erstellen, zu lesen oder zu schreiben, können alle möglichen Fehler auftreten.</span><span class="sxs-lookup"><span data-stu-id="34298-456">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="34298-457">Die gewünschte Datei ist möglicherweise nicht vorhanden, Sie ist möglicherweise gesperrt, der Code verfügt möglicherweise nicht über Berechtigungen usw.</span><span class="sxs-lookup"><span data-stu-id="34298-457">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="34298-458">Wenn Ihr Code versucht, Datensätze in einer Datenbank zu aktualisieren, können auch Berechtigungsprobleme auftreten, die Verbindung mit der Datenbank kann gelöscht werden, die zu speichernden Daten sind möglicherweise ungültig usw.</span><span class="sxs-lookup"><span data-stu-id="34298-458">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="34298-459">In Programmier begriffen werden diese Situationen als *Ausnahmen*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="34298-459">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="34298-460">Wenn im Code eine Ausnahme auftritt, wird eine Fehlermeldung generiert (ausgelöst), die für Benutzer am besten geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="34298-460">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="34298-462">In Situationen, in denen in Ihrem Code Ausnahmen auftreten können, und um Fehlermeldungen dieses Typs zu vermeiden, können Sie `Try/Catch`-Anweisungen verwenden.</span><span class="sxs-lookup"><span data-stu-id="34298-462">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="34298-463">In der `Try`-Anweisung führen Sie den Code aus, den Sie überprüfen.</span><span class="sxs-lookup"><span data-stu-id="34298-463">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="34298-464">In mindestens einer `Catch`-Anweisung können Sie nach bestimmten Fehlern (bestimmte Typen von Ausnahmen) suchen, die möglicherweise aufgetreten sind.</span><span class="sxs-lookup"><span data-stu-id="34298-464">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="34298-465">Sie können so viele `Catch` Anweisungen einschließen, wie Sie nach Fehlern suchen müssen, die Sie erwarten.</span><span class="sxs-lookup"><span data-stu-id="34298-465">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="34298-466">Es wird empfohlen, die Verwendung der `Response.Redirect`-Methode in `Try/Catch` Anweisungen zu vermeiden, da dies eine Ausnahme in der Seite verursachen kann.</span><span class="sxs-lookup"><span data-stu-id="34298-466">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>

<span data-ttu-id="34298-467">Das folgende Beispiel zeigt eine Seite, die bei der ersten Anforderung eine Textdatei erstellt und dann eine Schaltfläche anzeigt, mit der der Benutzer die Datei öffnen kann.</span><span class="sxs-lookup"><span data-stu-id="34298-467">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="34298-468">Im Beispiel wird absichtlich ein ungültiger Dateiname verwendet, damit eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="34298-468">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="34298-469">Der Code enthält `Catch` Anweisungen für zwei mögliche Ausnahmen: `FileNotFoundException`, bei einem ungültigen Dateinamen und `DirectoryNotFoundException`. Dies tritt auf, wenn ASP.NET den Ordner nicht selbst finden kann.</span><span class="sxs-lookup"><span data-stu-id="34298-469">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="34298-470">(Sie können die Auskommentierung einer Anweisung im Beispiel aufheben, um zu sehen, wie Sie ausgeführt wird, wenn alles ordnungsgemäß funktioniert.)</span><span class="sxs-lookup"><span data-stu-id="34298-470">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="34298-471">Wenn der Code die Ausnahme nicht behandelt hat, sehen Sie eine Fehlerseite wie der vorherige Screenshot.</span><span class="sxs-lookup"><span data-stu-id="34298-471">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="34298-472">Mit dem `Try/Catch` Abschnitt wird jedoch verhindert, dass der Benutzer diese Fehlertypen erkennen kann.</span><span class="sxs-lookup"><span data-stu-id="34298-472">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="34298-473">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="34298-473">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="34298-474">Referenzdokumentation</span><span class="sxs-lookup"><span data-stu-id="34298-474">Reference Documentation</span></span>

- [<span data-ttu-id="34298-475">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="34298-475">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="34298-476">Visual Basic-Sprache</span><span class="sxs-lookup"><span data-stu-id="34298-476">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
