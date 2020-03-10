---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Hinzufügen der Validierung zum Modell | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Erstellen Sie eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 9403be574324c34edf93bef1e0e4fd7ba68a3a9d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437277"
---
# <a name="adding-validation-to-the-model"></a><span data-ttu-id="d7248-104">Hinzufügen der Überprüfung zum Modell</span><span class="sxs-lookup"><span data-stu-id="d7248-104">Adding Validation to the Model</span></span>

<span data-ttu-id="d7248-105">von [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="d7248-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="d7248-106">Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="d7248-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="d7248-107">Sie erstellen eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.</span><span class="sxs-lookup"><span data-stu-id="d7248-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="d7248-108">Besuchen Sie das [ASP.NET MVC Learning Center](../../../index.md) , um weitere ASP.NET MVC-Tutorials und-Beispiele zu finden.</span><span class="sxs-lookup"><span data-stu-id="d7248-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="d7248-109">In diesem Abschnitt implementieren wir die erforderliche Unterstützung zum Aktivieren der Eingabevalidierung in unserer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="d7248-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="d7248-110">Wir stellen sicher, dass der Daten Bank Inhalt immer korrekt ist, und stellen hilfreiche Fehlermeldungen für Endbenutzer bereit, wenn Sie versuchen, die Film Daten einzugeben, die ungültig sind.</span><span class="sxs-lookup"><span data-stu-id="d7248-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="d7248-111">Wir beginnen mit dem Hinzufügen einer kleinen Validierungs Logik zur Movie-Klasse.</span><span class="sxs-lookup"><span data-stu-id="d7248-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="d7248-112">Klicken Sie mit der rechten Maustaste auf den Modell Ordner und wählen Sie Klasse hinzufügen</span><span class="sxs-lookup"><span data-stu-id="d7248-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="d7248-113">Benennen Sie Ihre Class Movie.</span><span class="sxs-lookup"><span data-stu-id="d7248-113">Name your class Movie.</span></span>

<span data-ttu-id="d7248-114">Als wir das Movie-Entitäts Modell zuvor erstellt haben, hat die IDE eine Movie-Klasse erstellt.</span><span class="sxs-lookup"><span data-stu-id="d7248-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="d7248-115">Tatsächlich kann sich ein Teil der Movie-Klasse in einer Datei befinden und Teil eines anderen sein.</span><span class="sxs-lookup"><span data-stu-id="d7248-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="d7248-116">Dies wird als partielle Klasse bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="d7248-116">This is called a Partial Class.</span></span> <span data-ttu-id="d7248-117">Wir erweitern die Movie-Klasse aus einer anderen Datei.</span><span class="sxs-lookup"><span data-stu-id="d7248-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="d7248-118">Wir erstellen eine partielle Movie-Klasse, die auf eine "Buddy-Klasse" zeigt, mit einigen Attributen, die Validierungs Hinweise an das System übergeben.</span><span class="sxs-lookup"><span data-stu-id="d7248-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="d7248-119">Wir markieren den Titel und den Preis als erforderlich und setzen auch darauf, dass sich der Preis innerhalb eines bestimmten Bereichs liegt.</span><span class="sxs-lookup"><span data-stu-id="d7248-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="d7248-120">Klicken Sie mit der rechten Maustaste auf den Ordner Modelle, und wählen Sie Klasse</span><span class="sxs-lookup"><span data-stu-id="d7248-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="d7248-121">Benennen Sie die Klasse Movie, und klicken Sie auf die Schaltfläche OK.</span><span class="sxs-lookup"><span data-stu-id="d7248-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="d7248-122">So sieht unsere partielle Movie-Klasse aus.</span><span class="sxs-lookup"><span data-stu-id="d7248-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="d7248-123">Führen Sie die Anwendung erneut aus, und versuchen Sie, einen Film mit einem Preis über 100 einzugeben.</span><span class="sxs-lookup"><span data-stu-id="d7248-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="d7248-124">Nachdem Sie das Formular übermittelt haben, erhalten Sie einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="d7248-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="d7248-125">Der Fehler wird auf Serverseite abgefangen und tritt auf, nachdem das Formular gepostet wurde.</span><span class="sxs-lookup"><span data-stu-id="d7248-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="d7248-126">Beachten Sie, dass die integrierten HTML-Hilfsprogramme von ASP.NET MVC intelligent genug waren, um die Fehlermeldung anzuzeigen und die Werte für uns innerhalb der Textfeld-Elemente beizubehalten:</span><span class="sxs-lookup"><span data-stu-id="d7248-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="d7248-127">[!["kreatemoviewithvalidation"](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d7248-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="d7248-128">Dies funktioniert hervorragend, aber es wäre schön, wenn wir den Benutzer sofort über die Clientseite informieren könnten, bevor der Server eingebunden wird.</span><span class="sxs-lookup"><span data-stu-id="d7248-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="d7248-129">Wir ermöglichen eine Client seitige Validierung mit JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d7248-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="d7248-130">Hinzufügen der Client seitigen Validierung</span><span class="sxs-lookup"><span data-stu-id="d7248-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="d7248-131">Da unsere Movie-Klasse bereits über einige Validierungs Attribute verfügt, müssen wir nur einige JavaScript-Dateien zur CREATE. aspx-Ansichts Vorlage hinzufügen und eine Codezeile hinzufügen, um die Client seitige Validierung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="d7248-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="d7248-132">Wechseln Sie in VWD in den Ordner views/Movie, und öffnen Sie Create. aspx.</span><span class="sxs-lookup"><span data-stu-id="d7248-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="d7248-133">Öffnen Sie den Ordner Scripts in der Projektmappen-Explorer, und ziehen Sie die folgenden drei Skripts in das &lt;Head&gt;-Tags.</span><span class="sxs-lookup"><span data-stu-id="d7248-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="d7248-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="d7248-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="d7248-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="d7248-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="d7248-136">Sie möchten, dass diese Skriptdateien in dieser Reihenfolge angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="d7248-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="d7248-137">Fügen Sie außerdem diese einzelne Zeile oberhalb von HTML. BeginForm hinzu:</span><span class="sxs-lookup"><span data-stu-id="d7248-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="d7248-138">Hier sehen Sie den Code, der in der IDE angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="d7248-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="d7248-139">[![Filme-Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d7248-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="d7248-140">Führen Sie die Anwendung aus, und klicken Sie auf/Movies/Create, und klicken Sie auf erstellen, ohne Daten einzugeben.</span><span class="sxs-lookup"><span data-stu-id="d7248-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="d7248-141">Die Fehlermeldungen werden sofort ohne den seitenflash angezeigt, den wir mit dem Senden von Daten an den Server verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="d7248-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="d7248-142">Dies liegt daran, dass ASP.NET MVC nun die Eingabe sowohl auf dem Client (mit JavaScript) als auch auf dem Server überprüft.</span><span class="sxs-lookup"><span data-stu-id="d7248-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="d7248-143">[![erstellen-Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d7248-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="d7248-144">Das sieht gut aus!</span><span class="sxs-lookup"><span data-stu-id="d7248-144">This is looking good!</span></span> <span data-ttu-id="d7248-145">Nun fügen wir der Datenbank eine zusätzliche Spalte hinzu.</span><span class="sxs-lookup"><span data-stu-id="d7248-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d7248-146">[Zurück](getting-started-with-mvc-part6.md)
> [Weiter](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="d7248-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
