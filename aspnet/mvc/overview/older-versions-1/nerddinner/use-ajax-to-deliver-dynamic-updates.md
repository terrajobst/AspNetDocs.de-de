---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Verwenden von AJAX zum übermitteln dynamischer Updates | Microsoft-Dokumentation
author: microsoft
description: Schritt 10 implementiert die Unterstützung für angemeldete Benutzer, um Ihr Interesse an der Teilnahme an einem Dinner mit einem AJAX-basierten Ansatz zu übernehmen, der in das Dinner-Detail integriert ist...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 3edc02fec546609505b5e085440fa684abe7acd0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486309"
---
# <a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="338c4-103">Bereitstellen von dynamischen Updates mithilfe von AJAX</span><span class="sxs-lookup"><span data-stu-id="338c4-103">Use AJAX to Deliver Dynamic Updates</span></span>

<span data-ttu-id="338c4-104">von [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="338c4-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="338c4-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="338c4-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="338c4-106">Dies ist Schritt 10 des kostenlosen ["nerddinner"](introducing-the-nerddinner-tutorial.md) -Lernprogramms, in dem erläutert wird, wie eine kleine, aber komplette Webanwendung mit ASP.NET MVC 1 erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="338c4-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="338c4-107">Schritt 10 implementiert die Unterstützung für angemeldete Benutzer, um Ihr Interesse an der Teilnahme an einem Dinner mit einem AJAX-basierten Ansatz zu übernehmen, der auf der Seite Dinner Details integriert ist.</span><span class="sxs-lookup"><span data-stu-id="338c4-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="338c4-108">Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.</span><span class="sxs-lookup"><span data-stu-id="338c4-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="338c4-109">Nerddinner Step 10: AJAX-Aktivierung von RSVPs akzeptiert</span><span class="sxs-lookup"><span data-stu-id="338c4-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="338c4-110">Nun implementieren wir die Unterstützung für angemeldete Benutzer, um Ihr Interesse an der Teilnahme an Dinner zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="338c4-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="338c4-111">Wir aktivieren dies mithilfe eines AJAX-basierten Ansatzes, der in die Dinner Details-Seite integriert ist.</span><span class="sxs-lookup"><span data-stu-id="338c4-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="338c4-112">Angeben, ob der Benutzer RSVP</span><span class="sxs-lookup"><span data-stu-id="338c4-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="338c4-113">Benutzer können die URL */Dinners/Details/[ID*] besuchen, um Details zu einem bestimmten Dinner anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="338c4-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="338c4-114">Die "Details ()"-Aktionsmethode wird wie folgt implementiert:</span><span class="sxs-lookup"><span data-stu-id="338c4-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="338c4-115">Der erste Schritt bei der Implementierung der RSVP-Unterstützung besteht darin, dem Dinner-Objekt (innerhalb der zuvor erstellten partiellen Dinner.cs partiellen Klasse) eine "isuserregistrierte (username)"-Hilfsmethode hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="338c4-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="338c4-116">Diese Hilfsmethode gibt "true" oder "false" zurück, je nachdem, ob der Benutzer zurzeit ein RSVP für das Dinner hat:</span><span class="sxs-lookup"><span data-stu-id="338c4-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="338c4-117">Wir können dann der Ansicht "Details. aspx" den folgenden Code hinzufügen, um eine entsprechende Meldung anzuzeigen, die angibt, ob der Benutzer für das Ereignis registriert ist oder nicht:</span><span class="sxs-lookup"><span data-stu-id="338c4-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="338c4-118">Und wenn ein Benutzer nun ein Dinner besucht, wird die Meldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="338c4-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="338c4-119">Und wenn Sie ein Dinner besuchen, werden Sie nicht für Sie registriert. die folgende Meldung wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="338c4-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="338c4-120">Implementieren der Register Action-Methode</span><span class="sxs-lookup"><span data-stu-id="338c4-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="338c4-121">Nun fügen wir die Funktionalität hinzu, die erforderlich ist, um Benutzern die RSVP für ein Dinner auf der Seite "Details" zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="338c4-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="338c4-122">Um dies zu implementieren, erstellen Sie eine neue Klasse "rsvpcontroller", indem Sie mit der rechten Maustaste auf das Verzeichnis "\controllers" klicken und den Menübefehl Add-&gt;Controller auswählen.</span><span class="sxs-lookup"><span data-stu-id="338c4-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="338c4-123">Wir implementieren in der neuen rsvpcontroller-Klasse eine "Register"-Aktionsmethode, die eine ID für ein Dinner als Argument annimmt, das entsprechende Dinner-Objekt abruft, prüft, ob der angemeldete Benutzer aktuell in der Liste der Benutzer enthalten ist, die sich registriert haben, und wenn Fügt kein RSVP-Objekt für Sie hinzu:</span><span class="sxs-lookup"><span data-stu-id="338c4-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="338c4-124">Beachten Sie, dass eine einfache Zeichenfolge als Ausgabe der Aktionsmethode zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="338c4-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="338c4-125">Wir könnten diese Nachricht in eine Ansichts Vorlage eingebettet haben – aber da Sie so klein ist, verwenden wir einfach die Content ()-Hilfsmethode für die Controller-Basisklasse und geben eine Zeichen folgen Nachricht wie oben zurück.</span><span class="sxs-lookup"><span data-stu-id="338c4-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="338c4-126">Aufrufen der rsvpforevent-Aktionsmethode mithilfe von AJAX</span><span class="sxs-lookup"><span data-stu-id="338c4-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="338c4-127">Wir verwenden AJAX zum Aufrufen der Register Action-Methode aus der Detailansicht.</span><span class="sxs-lookup"><span data-stu-id="338c4-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="338c4-128">Diese Implementierung ist ziemlich einfach.</span><span class="sxs-lookup"><span data-stu-id="338c4-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="338c4-129">Zuerst fügen wir zwei Skript Bibliotheks Verweise hinzu:</span><span class="sxs-lookup"><span data-stu-id="338c4-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="338c4-130">Die erste Bibliothek verweist auf die Client seitige Core ASP.NET AJAX-Skript Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="338c4-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="338c4-131">Diese Datei ist ungefähr rund um die Uhr groß (komprimiert) und enthält die Client seitigen Kernfunktionen von AJAX.</span><span class="sxs-lookup"><span data-stu-id="338c4-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="338c4-132">Die zweite Bibliothek enthält Hilfsprogrammfunktionen, die in die integrierten AJAX-Hilfsmethoden von ASP.NET MVC integriert werden (die wir in Kürze verwenden werden).</span><span class="sxs-lookup"><span data-stu-id="338c4-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="338c4-133">Wir können dann den zuvor hinzugefügten Ansichts Vorlagen Code aktualisieren, sodass anstelle der Meldung "Sie sind nicht für dieses Ereignis registriert" einen Link zu erstellen, der bei einem Pushvorgang einen AJAX-Aufruf ausführt, der unsere rsvpforevent-Aktionsmethode auf unserem RSVP-Controller aufruft. und RSVPs:</span><span class="sxs-lookup"><span data-stu-id="338c4-133">We can then update the view template code we added earlier so that instead of outputting a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="338c4-134">Die oben verwendete AJAX. Action Link ()-Hilfsmethode ist in ASP.NET MVC integriert und ähnelt der "HTML. Action Link ()"-Hilfsmethode, mit der Ausnahme, dass anstelle einer Standard Navigation ein AJAX-Aufrufe an die Aktionsmethode erfolgt, wenn auf den Link geklickt wird.</span><span class="sxs-lookup"><span data-stu-id="338c4-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="338c4-135">Oben rufen wir die "Register"-Aktionsmethode auf dem "RSVP"-Controller auf und übergeben die dinnerid als "ID"-Parameter an ihn.</span><span class="sxs-lookup"><span data-stu-id="338c4-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="338c4-136">Der endgültige Parameter "ajaxoptions", den wir übergeben, gibt an, dass der von der Aktionsmethode zurückgegebene Inhalt übernommen und das HTML-&lt;div&gt;-Element auf der Seite aktualisiert werden soll, deren ID "rsvpmsg" lautet.</span><span class="sxs-lookup"><span data-stu-id="338c4-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="338c4-137">Wenn ein Benutzer nun zu einem Dinner sucht, ist er noch nicht registriert, und es wird ein Link zur RSVP dafür angezeigt:</span><span class="sxs-lookup"><span data-stu-id="338c4-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="338c4-138">Wenn Sie auf den Link "RSVP für dieses Ereignis" klicken, wird ein AJAX-Rückruf der Register Action-Methode auf dem RSVP-Controller ausgeführt, und wenn Sie abgeschlossen ist, wird eine aktualisierte Meldung wie die folgende angezeigt:</span><span class="sxs-lookup"><span data-stu-id="338c4-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="338c4-139">Die Netzwerkbandbreite und der Datenverkehr bei der Erstellung dieses AJAX-Aufrufes ist wirklich einfach.</span><span class="sxs-lookup"><span data-stu-id="338c4-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="338c4-140">Wenn der Benutzer auf den Link "RSVP für dieses Ereignis" klickt, wird eine kleine HTTP Post-Netzwerk Anforderung an die */Dinners/Register/1* -URL gesendet, die wie unten gezeigt aussieht:</span><span class="sxs-lookup"><span data-stu-id="338c4-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="338c4-141">Die Antwort von der Register Aktionsmethode ist einfach:</span><span class="sxs-lookup"><span data-stu-id="338c4-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="338c4-142">Dieser Lightweight-Rückruf ist schnell und funktioniert sogar über ein langsames Netzwerk.</span><span class="sxs-lookup"><span data-stu-id="338c4-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="338c4-143">Hinzufügen einer jQuery-Animation</span><span class="sxs-lookup"><span data-stu-id="338c4-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="338c4-144">Die von uns implementierte AJAX-Funktionalität funktioniert gut und schnell.</span><span class="sxs-lookup"><span data-stu-id="338c4-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="338c4-145">Manchmal kann es so schnell vorkommen, dass ein Benutzer möglicherweise nicht bemerkt, dass der RSVP-Link durch neuen Text ersetzt wurde.</span><span class="sxs-lookup"><span data-stu-id="338c4-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="338c4-146">Um das Ergebnis etwas deutlicher zu machen, können wir eine einfache Animation hinzufügen, um auf die Aktualisierungs Nachricht aufmerksam zu machen.</span><span class="sxs-lookup"><span data-stu-id="338c4-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="338c4-147">Die standardmäßige ASP.NET-MVC-Projektvorlage enthält jQuery – eine ausgezeichnete (und sehr beliebte) Open Source-JavaScript-Bibliothek, die auch von Microsoft unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="338c4-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="338c4-148">jQuery bietet eine Reihe von Features, einschließlich einer netten HTML-DOM-Auswahl-und-Effekte-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="338c4-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="338c4-149">Zur Verwendung von jQuery fügen wir zuerst einen Skript Verweis hinzu.</span><span class="sxs-lookup"><span data-stu-id="338c4-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="338c4-150">Da wir jQuery innerhalb verschiedener Orte innerhalb unserer Website verwenden werden, fügen wir den Skript Verweis innerhalb unserer Website-Masterseiten Datei hinzu, damit Sie von allen Seiten verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="338c4-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="338c4-151">*Tipp: Stellen Sie sicher, dass Sie den JavaScript IntelliSense-Hotfix für VS 2008 SP1 installiert haben, der eine umfassendere IntelliSense-Unterstützung für JavaScript-Dateien (einschließlich jQuery) ermöglicht. Sie können Sie von: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="338c4-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="338c4-152">Mit jQuery geschriebener Code verwendet häufig eine globale "$ ()"-JavaScript-Methode, die ein oder mehrere HTML-Elemente mithilfe eines CSS-Selektor abruft.</span><span class="sxs-lookup"><span data-stu-id="338c4-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="338c4-153">Beispielsweise wählt *$ ("#rsvpmsg")* alle HTML-Elemente mit der ID rsvpmsg aus, während *$ (". Something")* alle Elemente mit dem CSS-Klassennamen "etwas" auswählen würde.</span><span class="sxs-lookup"><span data-stu-id="338c4-153">For example, *$("#rsvpmsg")* selects any HTML element with the id of rsvpmsg, while *$(".something")* would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="338c4-154">Sie können auch erweiterte Abfragen wie "alle aktivierten Options Felder zurückgeben" mit einer Auswahl Abfrage schreiben, z. b.: *$ ("input [@type= Radio] [@checked]")* .</span><span class="sxs-lookup"><span data-stu-id="338c4-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: *$("input[@type=radio][@checked]")*.</span></span>

<span data-ttu-id="338c4-155">Nachdem Sie Elemente ausgewählt haben, können Sie Methoden für diese Methoden zum Ausführen von Aktionen (z. b. ausblenden) aufzurufen: *$ ("#rsvpmsg"). Hide ();*</span><span class="sxs-lookup"><span data-stu-id="338c4-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="338c4-156">In unserem RSVP-Szenario definieren wir eine einfache JavaScript-Funktion mit dem Namen "animatersvpmessage", die den &lt;div-&gt; "rsvpmsg" auswählt und die Größe des Text Inhalts animiert.</span><span class="sxs-lookup"><span data-stu-id="338c4-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="338c4-157">Der folgende Code startet den Text "Small" und bewirkt dann, dass er sich über einen Zeitraum von 400 Millisekunden vergrößert:</span><span class="sxs-lookup"><span data-stu-id="338c4-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="338c4-158">Anschließend können Sie diese JavaScript-Funktion nach erfolgreichem Abschluss des AJAX-Aufrufs verbinden, indem Sie Ihren Namen an unsere AJAX. Action Link ()-Hilfsmethode übergeben (über die "onSuccess"-Ereignis Eigenschaft "ajaxoptions"):</span><span class="sxs-lookup"><span data-stu-id="338c4-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="338c4-159">Wenn Sie nun auf den Link "RSVP für diesen Ereignis" klicken und der AJAX-Befehl erfolgreich abgeschlossen wird, wird die zurück gesendete Inhalts Nachricht animiert und vergrößert:</span><span class="sxs-lookup"><span data-stu-id="338c4-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="338c4-160">Zusätzlich zur Bereitstellung eines onSuccess-Ereignisses macht das ajaxoptions-Objekt OnBegin-, OnFailure-und OnComplete-Ereignisse verfügbar, die Sie behandeln können (zusammen mit einer Reihe von anderen Eigenschaften und nützlichen Optionen).</span><span class="sxs-lookup"><span data-stu-id="338c4-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="338c4-161">Bereinigen: umgestalten einer RSVP-Teilansicht</span><span class="sxs-lookup"><span data-stu-id="338c4-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="338c4-162">Die Vorlage für die Detailansicht beginnt mit einem gewissen Zeitraum, und die Überstunden machen es etwas schwerer zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="338c4-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="338c4-163">Um die Lesbarkeit des Codes zu verbessern, erstellen Sie zunächst eine Teilansicht – rsvpstatus. ascx –, die den gesamten RSVP-Ansichts Code für unsere Detailseite Kapseln.</span><span class="sxs-lookup"><span data-stu-id="338c4-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="338c4-164">Klicken Sie hierzu mit der rechten Maustaste auf den Ordner "\views\dinners", und wählen Sie dann den Menübefehl Add-&gt;View aus.</span><span class="sxs-lookup"><span data-stu-id="338c4-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="338c4-165">Wir haben ein Dinner-Objekt als stark typisiertes ViewModel.</span><span class="sxs-lookup"><span data-stu-id="338c4-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="338c4-166">Anschließend können wir den RSVP-Inhalt aus der Ansicht "Details. aspx" Kopieren und einfügen.</span><span class="sxs-lookup"><span data-stu-id="338c4-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="338c4-167">Anschließend erstellen wir eine weitere Teilansicht – editanddelta etelinert. ascx, die den Code für den Link zum Bearbeiten und Löschen der Verknüpfung kapselt.</span><span class="sxs-lookup"><span data-stu-id="338c4-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="338c4-168">Außerdem wird ein Dinner-Objekt als stark typisiertes ViewModel übernommen, und die Bearbeitungs-und Lösch Logik wird aus der Ansicht "Details. aspx" kopiert und eingefügt.</span><span class="sxs-lookup"><span data-stu-id="338c4-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="338c4-169">Die Vorlage für die Detailansicht kann dann einfach zwei HTML. renderpartial ()-Methodenaufrufe im unteren Bereich einschließen:</span><span class="sxs-lookup"><span data-stu-id="338c4-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="338c4-170">Dadurch wird der Code sauberer zu lesen und zu warten.</span><span class="sxs-lookup"><span data-stu-id="338c4-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="338c4-171">Nächster Schritt</span><span class="sxs-lookup"><span data-stu-id="338c4-171">Next Step</span></span>

<span data-ttu-id="338c4-172">Sehen wir uns nun an, wie wir Ajax noch weiter verwenden und der Anwendung interaktive zuordnungsunterstützung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="338c4-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="338c4-173">[Zurück](secure-applications-using-authentication-and-authorization.md)
> [Weiter](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="338c4-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
