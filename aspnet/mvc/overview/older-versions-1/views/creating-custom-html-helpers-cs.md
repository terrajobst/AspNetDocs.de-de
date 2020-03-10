---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Erstellen von benutzerdefinierten HTMLC#-Hilfsprogrammen () | Microsoft-Dokumentation
author: microsoft
description: Dieses Tutorial soll Ihnen zeigen, wie Sie benutzerdefinierte HTML-Hilfsprogramme erstellen können, die Sie in ihren MVC-Ansichten verwenden können. Durch die Nutzung von HTML-Hilfsprogrammen...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 264ff9850bad397826b45649d52fbfefafc53a01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78485787"
---
# <a name="creating-custom-html-helpers-c"></a><span data-ttu-id="4cbfa-104">Erstellen von benutzerdefinierten HTML-Hilfsprogrammen (C#)</span><span class="sxs-lookup"><span data-stu-id="4cbfa-104">Creating Custom HTML Helpers (C#)</span></span>

<span data-ttu-id="4cbfa-105">von [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="4cbfa-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="4cbfa-106">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="4cbfa-106">Download PDF</span></span>](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="4cbfa-107">Dieses Tutorial soll Ihnen zeigen, wie Sie benutzerdefinierte HTML-Hilfsprogramme erstellen können, die Sie in ihren MVC-Ansichten verwenden können.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="4cbfa-108">Durch die Nutzung von HTML-Hilfsprogrammen können Sie die Menge an mühsamer Typisierung von HTML-Tags verringern, die Sie ausführen müssen, um eine HTML-Standardseite zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="4cbfa-109">Dieses Tutorial soll Ihnen zeigen, wie Sie benutzerdefinierte HTML-Hilfsprogramme erstellen können, die Sie in ihren MVC-Ansichten verwenden können.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="4cbfa-110">Durch die Nutzung von HTML-Hilfsprogrammen können Sie die Menge an mühsamer Typisierung von HTML-Tags verringern, die Sie ausführen müssen, um eine HTML-Standardseite zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="4cbfa-111">Im ersten Teil dieses Tutorials beschreibe ich einige der vorhandenen HTML-Hilfsprogramme, die im ASP.NET MVC-Framework enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="4cbfa-112">Als nächstes werden zwei Methoden zum Erstellen von benutzerdefinierten HTML-Hilfsprogrammen beschrieben: Ich erkläre, wie benutzerdefinierte HTML-Hilfsprogramme erstellt werden, indem eine statische Methode erstellt und eine Erweiterungsmethode erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="4cbfa-113">Informationen zu HTML-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="4cbfa-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="4cbfa-114">Ein HTML-Hilfsprogramm ist nur eine Methode, die eine Zeichenfolge zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="4cbfa-115">Die Zeichenfolge kann jeden gewünschten Inhaltstyp darstellen.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="4cbfa-116">Sie können z. b. HTML-Hilfsprogramme verwenden, um Standard-HTML-Tags wie HTML-`<input>` und `<img>` Tags zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="4cbfa-117">Sie können auch HTML-Hilfsprogramme verwenden, um komplexere Inhalte, z. b. eine Registerkarte oder eine HTML-Tabelle mit Datenbankdaten, zu Rendering</span><span class="sxs-lookup"><span data-stu-id="4cbfa-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="4cbfa-118">Das ASP.NET-MVC-Framework umfasst die folgenden Standard-HTML-Hilfsprogramme (Dies ist keine komplette Liste):</span><span class="sxs-lookup"><span data-stu-id="4cbfa-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="4cbfa-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="4cbfa-119">Html.ActionLink()</span></span>
- <span data-ttu-id="4cbfa-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="4cbfa-120">Html.BeginForm()</span></span>
- <span data-ttu-id="4cbfa-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="4cbfa-121">Html.CheckBox()</span></span>
- <span data-ttu-id="4cbfa-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="4cbfa-122">Html.DropDownList()</span></span>
- <span data-ttu-id="4cbfa-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="4cbfa-123">Html.EndForm()</span></span>
- <span data-ttu-id="4cbfa-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="4cbfa-124">Html.Hidden()</span></span>
- <span data-ttu-id="4cbfa-125">HTML. ListBox ()</span><span class="sxs-lookup"><span data-stu-id="4cbfa-125">Html.ListBox()</span></span>
- <span data-ttu-id="4cbfa-126">HTML. Password ()</span><span class="sxs-lookup"><span data-stu-id="4cbfa-126">Html.Password()</span></span>
- <span data-ttu-id="4cbfa-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="4cbfa-127">Html.RadioButton()</span></span>
- <span data-ttu-id="4cbfa-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="4cbfa-128">Html.TextArea()</span></span>
- <span data-ttu-id="4cbfa-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="4cbfa-129">Html.TextBox()</span></span>

<span data-ttu-id="4cbfa-130">Nehmen Sie beispielsweise das Formular in der Liste 1.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="4cbfa-131">Dieses Formular wird mithilfe von zwei der Standard-HTML-Hilfsprogramme gerendert (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="4cbfa-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="4cbfa-132">Dieses Formular verwendet die `Html.BeginForm()`-und `Html.TextBox()`-Hilfsmethoden, um ein einfaches HTML-Formular zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>

<span data-ttu-id="4cbfa-133">[mit HTML-Hilfsprogrammen gerenderte ![Seite](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4cbfa-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="4cbfa-134">**Abbildung 01**: Seite mit HTML-Hilfsprogrammen gerendert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-custom-html-helpers-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4cbfa-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>

<span data-ttu-id="4cbfa-135">**Codebeispiel 1 – `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="4cbfa-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="4cbfa-136">Die Hilfsmethode HTML. BeginForm () wird verwendet, um die öffnenden und schließenden HTML-`<form>` Tags zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="4cbfa-137">Beachten Sie, dass die `Html.BeginForm()`-Methode in einer using-Anweisung aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="4cbfa-138">Die using-Anweisung stellt sicher, dass das `<form>`-Tag am Ende des using-Blocks geschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="4cbfa-139">Wenn Sie es vorziehen, anstatt einen Using-Block zu erstellen, können Sie die HTML. Endform ()-Hilfsmethode zum Schließen des `<form>`-Tags aufruft.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="4cbfa-140">Verwenden Sie den jeweiligen Ansatz, um eine öffnende und schließende `<form>` Kennung zu erstellen, die für Sie intuitiver erscheint.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="4cbfa-141">Die `Html.TextBox()`-Hilfsmethoden werden in der Liste 1 zum Rendering von HTML-`<input>` Tags verwendet.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="4cbfa-142">Wenn Sie die Option Quelle anzeigen in Ihrem Browser auswählen, wird die HTML-Quelle in der Liste 2 angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="4cbfa-143">Beachten Sie, dass die Quelle Standard-HTML-Tags enthält.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4cbfa-144">Beachten Sie, dass das `Html.TextBox()`-HTML-Hilfsprogramm mit `<%= %>` Tags anstelle `<% %>` Tags gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="4cbfa-145">Wenn Sie das Gleichheitszeichen nicht einschließen, wird nichts im Browser gerendert.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="4cbfa-146">Das ASP.NET-MVC-Framework enthält eine kleine Gruppe von Hilfsprogrammen.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="4cbfa-147">Höchstwahrscheinlich müssen Sie das MVC-Framework mit benutzerdefinierten HTML-Hilfsprogrammen erweitern.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="4cbfa-148">Im restlichen Teil dieses Tutorials erlernen Sie zwei Methoden zum Erstellen von benutzerdefinierten HTML-Hilfsprogrammen.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="4cbfa-149">**Codebeispiel 2 – `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="4cbfa-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="4cbfa-150">Erstellen von HTML-Hilfsprogrammen mit statischen Methoden</span><span class="sxs-lookup"><span data-stu-id="4cbfa-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="4cbfa-151">Die einfachste Möglichkeit, ein neues HTML-Hilfsprogramm zu erstellen, ist das Erstellen einer statischen Methode, die eine Zeichenfolge zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="4cbfa-152">Stellen Sie sich beispielsweise vor, dass Sie ein neues HTML-Hilfsprogramm erstellen, das einen HTML-`<label>` Tag rendert.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="4cbfa-153">Sie können die-Klasse in "Listing 2" verwenden, um eine `<label>` zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="4cbfa-154">**Codebeispiel 2 – `Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="4cbfa-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="4cbfa-155">In der Liste 2 gibt es keine besonderen Informationen über die Klasse.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="4cbfa-156">Die `Label()`-Methode gibt einfach eine Zeichenfolge zurück.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="4cbfa-157">Die geänderte Index Sicht in der Liste 3 verwendet die `LabelHelper`, um HTML-`<label>` Tags zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="4cbfa-158">Beachten Sie, dass die Sicht eine `<%@ imports %>` Direktive enthält, die den `Application1.Helpers`-Namespace importiert.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="4cbfa-159">**Codebeispiel 2 – `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="4cbfa-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="4cbfa-160">Erstellen von HTML-Hilfsprogrammen mit Erweiterungs Methoden</span><span class="sxs-lookup"><span data-stu-id="4cbfa-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="4cbfa-161">Wenn Sie HTML-Hilfsprogramme erstellen möchten, die genau wie die standardmäßigen HTML-Hilfsprogramme im ASP.NET MVC-Framework funktionieren, müssen Sie Erweiterungs Methoden erstellen.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="4cbfa-162">Erweiterungs Methoden ermöglichen es Ihnen, einer vorhandenen Klasse neue Methoden hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="4cbfa-163">Beim Erstellen einer HTML-Hilfsmethode fügen Sie der htmlhelper-Klasse, die durch die HTML-Eigenschaft einer Ansicht dargestellt wird, neue Methoden hinzu.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="4cbfa-164">Die-Klasse in der Liste 3 fügt der `HtmlHelper` Klasse mit dem Namen `Label()`eine Erweiterungsmethode hinzu.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="4cbfa-165">Es gibt einige Dinge, die Sie über diese Klasse bemerken sollten.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="4cbfa-166">Beachten Sie zunächst, dass die-Klasse eine statische Klasse ist.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="4cbfa-167">Sie müssen eine Erweiterungsmethode mit einer statischen Klasse definieren.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="4cbfa-168">Beachten Sie, dass dem ersten Parameter der `Label()`-Methode das Schlüsselwort `this`vorangestellt wird.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="4cbfa-169">Der erste Parameter einer Erweiterungsmethode gibt die Klasse an, die von der Erweiterungsmethode erweitert wird.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="4cbfa-170">**Codebeispiel 3 – `Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="4cbfa-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="4cbfa-171">Nachdem Sie eine Erweiterungsmethode erstellt und die Anwendung erfolgreich erstellt haben, wird die Erweiterungsmethode in Visual Studio IntelliSense wie alle anderen Methoden einer Klasse angezeigt (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="4cbfa-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="4cbfa-172">Der einzige Unterschied besteht darin, dass Erweiterungs Methoden mit einem speziellen Symbol nebeneinander angezeigt werden (Symbol eines abwärts Pfeils).</span><span class="sxs-lookup"><span data-stu-id="4cbfa-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>

<span data-ttu-id="4cbfa-173">[![mithilfe der HTML. Label ()-Erweiterungsmethode](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="4cbfa-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="4cbfa-174">**Abbildung 02**: Verwenden der HTML. Label ()-Erweiterungsmethode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-custom-html-helpers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4cbfa-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>

<span data-ttu-id="4cbfa-175">Die geänderte Index Sicht in der Liste 4 verwendet die HTML. Label ()-Erweiterungsmethode, um alle `<label>` Tags zu rendern.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="4cbfa-176">**Codebeispiel 4 – `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="4cbfa-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="4cbfa-177">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="4cbfa-177">Summary</span></span>

<span data-ttu-id="4cbfa-178">In diesem Tutorial haben Sie zwei Methoden zum Erstellen von benutzerdefinierten HTML-Hilfsprogrammen kennengelernt.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="4cbfa-179">Zuerst haben Sie gelernt, wie Sie ein Benutzer `Label()` definiertes HTML-Hilfsprogramm erstellen, indem Sie eine statische Methode erstellen, die eine Zeichenfolge zurückgibt</span><span class="sxs-lookup"><span data-stu-id="4cbfa-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="4cbfa-180">Als nächstes haben Sie erfahren, wie Sie eine benutzerdefinierte `Label()` HTML-Hilfsmethode erstellen, indem Sie eine Erweiterungsmethode für die `HtmlHelper`-Klasse erstellen.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="4cbfa-181">In diesem Tutorial habe ich mich auf das Entwickeln einer extrem einfachen HTML-Hilfsmethode konzentriert.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="4cbfa-182">Beachten Sie, dass ein HTML-Hilfsprogramm so kompliziert sein kann, wie Sie möchten.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="4cbfa-183">Sie können HTML-Hilfsprogramme erstellen, die umfangreichen Inhalt, z. b. Struktur Ansichten, Menüs oder Tabellen von Datenbankdaten, darstellen.</span><span class="sxs-lookup"><span data-stu-id="4cbfa-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4cbfa-184">[Zurück](asp-net-mvc-views-overview-cs.md)
> [Weiter](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="4cbfa-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>
