---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: Übersicht über ASP.NET MVC-Ansichten (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Was ist eine ASP.NET MVC-Ansicht, und wie unterscheidet Sie sich von einer HTML-Seite? In diesem Tutorial stellt Stephen Walther Sie in Ansichten vor und veranschaulicht, wie Sie es tun können...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: f02728ed248f29b09d654e509977ed43889cbb83
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435273"
---
# <a name="aspnet-mvc-views-overview-vb"></a><span data-ttu-id="be032-104">ASP.NET MVC-Ansichten – Übersicht (VB)</span><span class="sxs-lookup"><span data-stu-id="be032-104">ASP.NET MVC Views Overview (VB)</span></span>

<span data-ttu-id="be032-105">von [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="be032-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="be032-106">Was ist eine ASP.NET MVC-Ansicht, und wie unterscheidet Sie sich von einer HTML-Seite?</span><span class="sxs-lookup"><span data-stu-id="be032-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="be032-107">In diesem Tutorial stellt Stephen Walther Sie in Ansichten vor und veranschaulicht, wie Sie die Vorteile von Anzeigedaten und HTML-Hilfsprogrammen in einer Ansicht nutzen können.</span><span class="sxs-lookup"><span data-stu-id="be032-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>

<span data-ttu-id="be032-108">Der Zweck dieses Tutorials besteht darin, Ihnen eine kurze Einführung in ASP.NET MVC-Ansichten, Daten anzeigen und HTML-Hilfsprogramme zu bieten.</span><span class="sxs-lookup"><span data-stu-id="be032-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="be032-109">Am Ende dieses Tutorials sollten Sie wissen, wie neue Sichten erstellt, Daten von einem Controller an eine Ansicht übergeben werden und wie HTML-Hilfsprogramme verwendet werden, um Inhalte in einer Ansicht zu generieren.</span><span class="sxs-lookup"><span data-stu-id="be032-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="be032-110">Grundlegendes zu Sichten</span><span class="sxs-lookup"><span data-stu-id="be032-110">Understanding Views</span></span>

<span data-ttu-id="be032-111">Im Gegensatz zu ASP.net-oder Active Server Seiten schließt ASP.NET MVC nichts ein, das direkt einer Seite entspricht.</span><span class="sxs-lookup"><span data-stu-id="be032-111">Unlike ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="be032-112">In einer ASP.NET-MVC-Anwendung gibt es keine Seite auf dem Datenträger, die dem Pfad in der URL entspricht, die Sie in die Adressleiste Ihres Browsers eingeben.</span><span class="sxs-lookup"><span data-stu-id="be032-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="be032-113">Die nächstgelegene Seite einer Seite in einer ASP.NET MVC-Anwendung wird als *Ansicht*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="be032-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="be032-114">In einer ASP.NET-MVC-Anwendung werden eingehende Browser Anforderungen Controller Aktionen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="be032-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="be032-115">Eine Controller Aktion kann eine Ansicht zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="be032-115">A controller action might return a view.</span></span> <span data-ttu-id="be032-116">Eine Controller Aktion kann jedoch eine andere Art von Aktion ausführen, z. b. eine Umleitung an eine andere Controller Aktion.</span><span class="sxs-lookup"><span data-stu-id="be032-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="be032-117">In der Liste 1 ist ein einfacher Controller namens HomeController enthalten.</span><span class="sxs-lookup"><span data-stu-id="be032-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="be032-118">Der HomeController macht zwei Controller Aktionen namens Index () und Details () verfügbar.</span><span class="sxs-lookup"><span data-stu-id="be032-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="be032-119">**Codebeispiel 1: HomeController. vb**</span><span class="sxs-lookup"><span data-stu-id="be032-119">**Listing 1 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

<span data-ttu-id="be032-120">Sie können die erste Aktion, die Index ()-Aktion, aufrufen, indem Sie die folgende URL in die Adressleiste Ihres Browsers eingeben:</span><span class="sxs-lookup"><span data-stu-id="be032-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="be032-121">/Home/Index</span><span class="sxs-lookup"><span data-stu-id="be032-121">/Home/Index</span></span>

<span data-ttu-id="be032-122">Sie können die zweite Aktion, die Aktion "Details ()" aufrufen, indem Sie diese Adresse in Ihrem Browser eingeben:</span><span class="sxs-lookup"><span data-stu-id="be032-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="be032-123">/Home/Details</span><span class="sxs-lookup"><span data-stu-id="be032-123">/Home/Details</span></span>

<span data-ttu-id="be032-124">Die Index ()-Aktion gibt eine Ansicht zurück.</span><span class="sxs-lookup"><span data-stu-id="be032-124">The Index() action returns a view.</span></span> <span data-ttu-id="be032-125">Bei den meisten von Ihnen erstellten Aktionen werden Sichten zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="be032-125">Most actions that you create will return views.</span></span> <span data-ttu-id="be032-126">Eine Aktion kann jedoch andere Arten von Aktions Ergebnissen zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="be032-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="be032-127">Beispielsweise gibt die Aktion Details () ein redirecttoaction-Ergebnis zurück, das die eingehende Anforderung an die Index ()-Aktion umleitet.</span><span class="sxs-lookup"><span data-stu-id="be032-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="be032-128">Die Index ()-Aktion enthält die folgende einzelne Codezeile:</span><span class="sxs-lookup"><span data-stu-id="be032-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="be032-129">Anzeigen ()</span><span class="sxs-lookup"><span data-stu-id="be032-129">View()</span></span>

<span data-ttu-id="be032-130">Diese Codezeile gibt eine Ansicht zurück, die sich unter folgendem Pfad auf dem Webserver befinden muss:</span><span class="sxs-lookup"><span data-stu-id="be032-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="be032-131">\Views\home\index.aspx</span><span class="sxs-lookup"><span data-stu-id="be032-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="be032-132">Der Pfad zur Ansicht wird vom Namen des Controllers und vom Namen der Controller Aktion abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="be032-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="be032-133">Wenn Sie möchten, können Sie die Ansicht explizit angeben.</span><span class="sxs-lookup"><span data-stu-id="be032-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="be032-134">In der folgenden Codezeile wird eine Ansicht mit dem Namen Fred zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="be032-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="be032-135">Ansicht (Fred)</span><span class="sxs-lookup"><span data-stu-id="be032-135">View( Fred )</span></span>

<span data-ttu-id="be032-136">Wenn diese Codezeile ausgeführt wird, wird eine Ansicht von folgendem Pfad zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="be032-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="be032-137">\Views\home\fred.aspx</span><span class="sxs-lookup"><span data-stu-id="be032-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="be032-138">Wenn Sie Vorhaben, Komponententests für Ihre ASP.NET MVC-Anwendung zu erstellen, empfiehlt es sich, explizit über Ansichts Namen zu sprechen.</span><span class="sxs-lookup"><span data-stu-id="be032-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="be032-139">Auf diese Weise können Sie einen Komponenten Test erstellen, um zu überprüfen, ob die erwartete Ansicht von einer Controller Aktion zurückgegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="be032-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>

## <a name="adding-content-to-a-view"></a><span data-ttu-id="be032-140">Hinzufügen von Inhalt zu einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="be032-140">Adding Content to a View</span></span>

<span data-ttu-id="be032-141">Eine Sicht ist ein Standard-HTML-Dokument (X), das Skripts enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="be032-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="be032-142">Mithilfe von Skripts können Sie dynamische Inhalte zu einer Ansicht hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="be032-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="be032-143">In der Ansicht in der Liste 2 werden z. b. das aktuelle Datum und die Uhrzeit angezeigt.</span><span class="sxs-lookup"><span data-stu-id="be032-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="be032-144">**Codebeispiel 2: \views\home\index.aspx**</span><span class="sxs-lookup"><span data-stu-id="be032-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

<span data-ttu-id="be032-145">Beachten Sie, dass der Text der HTML-Seite in der Liste 2 das folgende Skript enthält:</span><span class="sxs-lookup"><span data-stu-id="be032-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="be032-146">&lt;% Response. Write (DateTime. Now)%&gt;</span><span class="sxs-lookup"><span data-stu-id="be032-146">&lt;% Response.Write(DateTime.Now)%&gt;</span></span>

<span data-ttu-id="be032-147">Sie verwenden die Skript Trennzeichen &lt;% und%&gt;, um den Anfang und das Ende eines Skripts zu markieren.</span><span class="sxs-lookup"><span data-stu-id="be032-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="be032-148">Dieses Skript ist in Visual Basic geschrieben.</span><span class="sxs-lookup"><span data-stu-id="be032-148">This script is written in Visual basic.</span></span> <span data-ttu-id="be032-149">Es zeigt das aktuelle Datum und die aktuelle Uhrzeit durch Aufrufen der Response. Write ()-Methode zum Rendering von Inhalten im Browser an.</span><span class="sxs-lookup"><span data-stu-id="be032-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="be032-150">Die Skript Trennzeichen &lt;% und%&gt; können verwendet werden, um eine oder mehrere-Anweisungen auszuführen.</span><span class="sxs-lookup"><span data-stu-id="be032-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="be032-151">Da Sie "Response. Write ()" so oft aufrufen, stellt Microsoft Ihnen eine Verknüpfung zum Aufrufen der Response. Write ()-Methode zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="be032-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="be032-152">Die Ansicht in der Liste 3 verwendet die Trennzeichen &lt;% = und%&gt; als Verknüpfung zum Aufrufen von Response. Write ().</span><span class="sxs-lookup"><span data-stu-id="be032-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="be032-153">**Codebeispiel 3: views\home\index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="be032-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

<span data-ttu-id="be032-154">Sie können eine beliebige .NET-Sprache verwenden, um dynamischen Inhalt in einer Ansicht zu generieren.</span><span class="sxs-lookup"><span data-stu-id="be032-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="be032-155">Normalerweise verwenden Sie entweder Visual Basic .net oder C# zum Schreiben von Controllern und Ansichten.</span><span class="sxs-lookup"><span data-stu-id="be032-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="be032-156">Verwenden von HTML-Hilfsprogrammen zum Generieren von Ansichts Inhalten</span><span class="sxs-lookup"><span data-stu-id="be032-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="be032-157">Um das Hinzufügen von Inhalt zu einer Ansicht zu vereinfachen, können Sie die Vorteile eines *HTML*-Hilfsobjekts nutzen.</span><span class="sxs-lookup"><span data-stu-id="be032-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="be032-158">Ein HTML-Hilfsprogramm ist in der Regel eine Methode, die eine Zeichenfolge generiert.</span><span class="sxs-lookup"><span data-stu-id="be032-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="be032-159">Sie können HTML-Hilfsprogramme verwenden, um HTML-Standardelemente wie Textfelder, Links, Dropdown Listen und Listenfelder zu generieren.</span><span class="sxs-lookup"><span data-stu-id="be032-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="be032-160">Die Ansicht in der Liste 4 nutzt beispielsweise drei HTML-Hilfsprogramme: die BeginForm ()-, TextBox-und Password ()-Hilfsprogramme, um ein Anmeldeformular zu generieren (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="be032-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="be032-161">**Codebeispiel 4: \views\home\login.aspx**</span><span class="sxs-lookup"><span data-stu-id="be032-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]

<span data-ttu-id="be032-162">[![des Dialog Felds "Neues Projekt"](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="be032-162">[![The New Project dialog box](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="be032-163">**Abbildung 01**: ein Standard Anmeldeformular ([Klicken Sie, um das Bild in voller Größe anzuzeigen](asp-net-mvc-views-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="be032-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-vb/_static/image2.png))</span></span>

<span data-ttu-id="be032-164">Alle HTML-Hilfsmethoden werden in der HTML-Eigenschaft der Ansicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="be032-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="be032-165">Beispielsweise können Sie ein Textfeld durch Aufrufen der HTML. TextBox ()-Methode Rendering.</span><span class="sxs-lookup"><span data-stu-id="be032-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="be032-166">Beachten Sie, dass Sie die Skript Trennzeichen &lt;% = und%&gt; verwenden, wenn Sie sowohl die HTML. TextBox ()-als auch die HTML. Password ()-Hilfsprogramme aufrufen.</span><span class="sxs-lookup"><span data-stu-id="be032-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="be032-167">Diese Hilfsprogramme geben einfach eine Zeichenfolge zurück.</span><span class="sxs-lookup"><span data-stu-id="be032-167">These helpers simply return a string.</span></span> <span data-ttu-id="be032-168">Sie müssen Response. Write () aufzurufen, um die Zeichenfolge im Browser zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="be032-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="be032-169">Die Verwendung von HTML-Hilfsmethoden ist optional.</span><span class="sxs-lookup"><span data-stu-id="be032-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="be032-170">Dadurch wird das Leben vereinfacht, indem die Menge an HTML und Skript reduziert wird, die Sie schreiben müssen.</span><span class="sxs-lookup"><span data-stu-id="be032-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="be032-171">Die Ansicht in Auflistung 5 rendert genau dasselbe Formular wie die Ansicht in der Ansicht 4, ohne HTML-Hilfsprogramme zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="be032-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="be032-172">**Codebeispiel 5: \views\home\login.aspx**</span><span class="sxs-lookup"><span data-stu-id="be032-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

<span data-ttu-id="be032-173">Sie haben auch die Möglichkeit, eigene HTML-Hilfsprogramme zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="be032-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="be032-174">Sie können z. b. eine GridView ()-Hilfsmethode erstellen, die einen Satz von Datenbankdaten Sätzen in einer HTML-Tabelle automatisch anzeigt.</span><span class="sxs-lookup"><span data-stu-id="be032-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="be032-175">Dieses Thema wird im Tutorial Erstellen von **benutzerdefinierten HTML**-Hilfsprogrammen erläutert.</span><span class="sxs-lookup"><span data-stu-id="be032-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="be032-176">Verwenden von Ansichts Daten zum Übergeben von Daten an eine Sicht</span><span class="sxs-lookup"><span data-stu-id="be032-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="be032-177">Sie verwenden Daten anzeigen, um Daten von einem Controller an eine Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="be032-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="be032-178">Betrachten Sie Daten wie ein Paket, das Sie über die e-Mail senden.</span><span class="sxs-lookup"><span data-stu-id="be032-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="be032-179">Alle Daten, die von einem Controller an eine Ansicht übermittelt werden, müssen mithilfe dieses Pakets gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="be032-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="be032-180">Der Controller in der Liste 6 fügt z. b. eine Meldung zum Anzeigen von Daten hinzu.</span><span class="sxs-lookup"><span data-stu-id="be032-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="be032-181">**Codebeispiel 6: ProductController. vb**</span><span class="sxs-lookup"><span data-stu-id="be032-181">**Listing 6 - ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

<span data-ttu-id="be032-182">Die Eigenschaft "ViewData" des Controllers stellt eine Auflistung von Name-Wert-Paaren dar.</span><span class="sxs-lookup"><span data-stu-id="be032-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="be032-183">In der Liste 6 fügt die Index ()-Methode der Ansichts Datensammlung mit dem Wert Hallo Welt! ein Element hinzu.</span><span class="sxs-lookup"><span data-stu-id="be032-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="be032-184">Wenn die Ansicht von der Index ()-Methode zurückgegeben wird, werden die Ansichts Daten automatisch an die Ansicht übermittelt.</span><span class="sxs-lookup"><span data-stu-id="be032-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="be032-185">Die Ansicht in der Liste 7 Ruft die Nachricht aus den Ansichts Daten ab und rendert die Nachricht im Browser.</span><span class="sxs-lookup"><span data-stu-id="be032-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="be032-186">**Codebeispiel 7: \views\product\index.aspx**</span><span class="sxs-lookup"><span data-stu-id="be032-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

<span data-ttu-id="be032-187">Beachten Sie, dass die-Sicht beim Rendern der Nachricht die HTML. Encode ()-HTML-Hilfsmethode nutzt.</span><span class="sxs-lookup"><span data-stu-id="be032-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="be032-188">Das HTML-Hilfsprogramm HTML. Encode () codiert Sonderzeichen, z. b. &lt; und &gt; in Zeichen, die auf einer Webseite sicher angezeigt werden können.</span><span class="sxs-lookup"><span data-stu-id="be032-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="be032-189">Wenn Sie Inhalte, die von einem Benutzer an eine Website übermittelt werden, wiederzugeben, sollten Sie den Inhalt codieren, um JavaScript Injection-Angriffe zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="be032-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="be032-190">(Da wir die Nachricht selbst in ProductController erstellt haben, müssen wir die Nachricht wirklich codieren.</span><span class="sxs-lookup"><span data-stu-id="be032-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="be032-191">Es ist jedoch eine gute Gewohnheit, immer die HTML. Encode ()-Methode aufzurufen, wenn Inhalte angezeigt werden, die aus Sicht Daten innerhalb einer Ansicht abgerufen werden.)</span><span class="sxs-lookup"><span data-stu-id="be032-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="be032-192">In der Liste 7 haben wir die Anzeigedaten genutzt, um eine einfache Zeichen folgen Nachricht von einem Controller an eine Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="be032-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="be032-193">Sie können auch Daten anzeigen verwenden, um andere Datentypen, z. b. eine Sammlung von Datenbankdaten Sätzen, von einem Controller an eine Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="be032-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="be032-194">Wenn Sie z. b. den Inhalt der Datenbanktabelle "Products" in einer Ansicht anzeigen möchten, übergeben Sie die Sammlung der Datenbankdaten Sätze in "Daten anzeigen".</span><span class="sxs-lookup"><span data-stu-id="be032-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="be032-195">Sie haben auch die Möglichkeit, stark typisierte Ansichts Daten von einem Controller an eine Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="be032-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="be032-196">Dieses Thema wird im Lernprogramm Grundlegendes zu **stark typisierten Ansichts Daten und Ansichten**beschrieben.</span><span class="sxs-lookup"><span data-stu-id="be032-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="be032-197">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="be032-197">Summary</span></span>

<span data-ttu-id="be032-198">Dieses Tutorial bot eine kurze Einführung in ASP.NET MVC-Ansichten, Ansichts Daten und HTML-Hilfsprogramme.</span><span class="sxs-lookup"><span data-stu-id="be032-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="be032-199">Im ersten Abschnitt haben Sie erfahren, wie Sie Ihrem Projekt neue Ansichten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="be032-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="be032-200">Sie haben gelernt, dass Sie dem richtigen Ordner eine Ansicht hinzufügen müssen, um Sie von einem bestimmten Controller aus aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="be032-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="be032-201">Als nächstes haben wir das Thema der HTML-Hilfsprogramme erläutert.</span><span class="sxs-lookup"><span data-stu-id="be032-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="be032-202">Sie haben gelernt, wie HTML-Hilfsprogramme Ihnen die einfache Generierung von HTML-Standard Inhalten ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="be032-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="be032-203">Schließlich haben Sie gelernt, wie Sie die Anzeige von Daten nutzen können, um Daten von einem Controller an eine Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="be032-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="be032-204">[Zurück](passing-data-to-view-master-pages-cs.md)
> [Weiter](creating-custom-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="be032-204">[Previous](passing-data-to-view-master-pages-cs.md)
[Next](creating-custom-html-helpers-vb.md)</span></span>
