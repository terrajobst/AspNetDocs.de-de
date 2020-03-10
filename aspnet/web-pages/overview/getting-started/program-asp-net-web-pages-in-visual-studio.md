---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programmieren von ASP.net Web Pages (Razor) mithilfe von Visual Studio | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Anhang wird erläutert, wie Sie Visual Studio 2010 oder Visual Web Developer 2010 Express verwenden können, um ASP.net Web Pages mit dem Razor-Syntax zu programmieren.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 1a76098779d05912bf7bdf2de5fdce024770752c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78514293"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="d6eb1-103">Programmieren von ASP.net Web Pages (Razor) mithilfe von Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6eb1-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>

<span data-ttu-id="d6eb1-104">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d6eb1-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d6eb1-105">In diesem Artikel wird erläutert, wie Sie Visual Studio oder Visual Web Developer Express verwenden können, um ASP.net Web Pages Websites (Razor) zu programmieren.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
>
> <span data-ttu-id="d6eb1-106">Sie lernen Folgendes</span><span class="sxs-lookup"><span data-stu-id="d6eb1-106">What you'll learn</span></span>
>
> - <span data-ttu-id="d6eb1-107">Was Sie benötigen, um mit ASP.net Web Pages in Ihrer Version von Visual Studio zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="d6eb1-108">Vorgehensweise beim Hinzufügen von Unterstützung für ASP.net Web Pages zu Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="d6eb1-109">Verwenden von Funktionen in Visual Studio für die Arbeit mit ASP.net Razor Pages, einschließlich IntelliSense und dem Debugger.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d6eb1-110">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="d6eb1-110">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="d6eb1-111">ASP.net Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d6eb1-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="d6eb1-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d6eb1-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="d6eb1-113">Webmatrix 3</span><span class="sxs-lookup"><span data-stu-id="d6eb1-113">WebMatrix 3</span></span>
>
>
> <span data-ttu-id="d6eb1-114">Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2, Visual Studio 2012, Visual Studio 2010 und webmatrix 2.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>

<span data-ttu-id="d6eb1-115">Sie können ASP.NET-Webseiten mit Razor-Syntax mithilfe von webmatrix oder vielen anderen Code-Editoren programmieren.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="d6eb1-116">Sie können auch Microsoft Visual Studio verwenden, bei dem es sich um eine integrierte Entwicklungsumgebung (Integrated Development Environment, IDE) mit vollem Funktionsumfang handelt, die eine Reihe leistungsfähiger Tools zum Erstellen vieler Anwendungs Typen (nicht nur Websites) bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="d6eb1-117">Um mit ASP.net Razor Pages Arbeiten zu können, können Sie [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)verwenden.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-117">To work with ASP.NET Razor pages, you can use [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span>

<span data-ttu-id="d6eb1-118">Zwei besonders nützliche Features, die Visual Studio für die Programmierung mit ASP.net Razor-Webseiten bietet:</span><span class="sxs-lookup"><span data-stu-id="d6eb1-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="d6eb1-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-119">*IntelliSense*.</span></span> <span data-ttu-id="d6eb1-120">Das IntelliSense-Feature, das in Visual Studio integriert ist, ist umfassender als IntelliSense in webmatrix.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="d6eb1-121">*Debugger*.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-121">*Debugger*.</span></span> <span data-ttu-id="d6eb1-122">Mit dem Debugger können Sie die Problembehandlung für den Code durchführen, indem Sie ein Programm während der Ausführung beenden, Variablen untersuchen und den Codezeilen Weise durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="d6eb1-123">Verwenden von Visual Studio mit verschiedenen Versionen von ASP.net Web Pages</span><span class="sxs-lookup"><span data-stu-id="d6eb1-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="d6eb1-124">Um ASP.net-Web-Apps in Visual Studio 2017 zu entwickeln, installieren Sie die Arbeitsauslastung **ASP.net und Webentwicklung** .</span><span class="sxs-lookup"><span data-stu-id="d6eb1-124">To develop ASP.NET web apps in Visual Studio 2017, install the **ASP.NET and web development** workload.</span></span>

<span data-ttu-id="d6eb1-125">Visual Studio 2012 und Visual Studio 2013 enthalten Unterstützung für ASP.net Web Pages.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-125">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="d6eb1-126">(Die Pakete, die zur Unterstützung von ASP.net Web Pages erforderlich sind, werden bei der Installation von Visual Studio installiert.)</span><span class="sxs-lookup"><span data-stu-id="d6eb1-126">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="d6eb1-127">Visual Studio 2010 schließt standardmäßig keine Unterstützung für ASP.net Web Pages ein.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-127">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="d6eb1-128">Wenn Sie ASP.net Web Pages mit Visual Studio 2010 verwenden möchten, müssen Sie das ASP.NET-MVC-Paket installieren.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-128">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="d6eb1-129">Um ASP.net Web Pages 2 zu erhalten, installieren Sie ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-129">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="d6eb1-130">In der folgenden Tabelle wird die Unterstützung für ASP.net Web Pages in verschiedenen Versionen von Visual Studio zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-130">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="d6eb1-131">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="d6eb1-131">Visual Studio 2010</span></span> | <span data-ttu-id="d6eb1-132">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="d6eb1-132">Visual Studio 2012</span></span> | <span data-ttu-id="d6eb1-133">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d6eb1-133">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="d6eb1-134">**ASP.net Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="d6eb1-134">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="d6eb1-135">Installieren von ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="d6eb1-135">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="d6eb1-136">Nahmen</span><span class="sxs-lookup"><span data-stu-id="d6eb1-136">(Included)</span></span> | <span data-ttu-id="d6eb1-137">Nahmen</span><span class="sxs-lookup"><span data-stu-id="d6eb1-137">(Included)</span></span> |
| <span data-ttu-id="d6eb1-138">**ASP.net Web Pages 3**</span><span class="sxs-lookup"><span data-stu-id="d6eb1-138">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="d6eb1-139">Aktualisieren auf ASP.net Web Pages 3 über nuget</span><span class="sxs-lookup"><span data-stu-id="d6eb1-139">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="d6eb1-140">Nahmen</span><span class="sxs-lookup"><span data-stu-id="d6eb1-140">(Included)</span></span> |

<span data-ttu-id="d6eb1-141">Informationen zum Arbeiten mit Visual Studio 2010 finden Sie [unter Installieren der Unterstützung für ASP.net Web Pages in Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="d6eb1-141">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="d6eb1-142">Starten von Visual Studio aus webmatrix</span><span class="sxs-lookup"><span data-stu-id="d6eb1-142">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="d6eb1-143">Wenn Sie ein Projekt in webmatrix gestartet haben und zu Visual Studio wechseln möchten, bietet webmatrix eine Schaltfläche, um das Projekt problemlos in Visual Studio zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-143">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="d6eb1-144">Sie müssen Visual Studio auf Ihrem Computer installiert haben, damit diese Schaltfläche aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-144">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="d6eb1-145">Die folgende Abbildung zeigt die Schaltfläche in webmatrix.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-145">The following image shows the button in WebMatrix.</span></span>

![Starten von Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="d6eb1-147">Wenn Sie auf die Schaltfläche klicken, wird das Projekt in Visual Studio geöffnet.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-147">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="d6eb1-148">Sie können ohne Probleme zwischen webmatrix und Visual Studio hin-und herwechseln.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-148">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="d6eb1-149">Sie werden benachrichtigt, wenn sich Dateien in der anderen Umgebung geändert haben und erneut geladen werden müssen, um die neuesten Änderungen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-149">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="d6eb1-150">Erstellen einer ASP.net Razor-Website in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6eb1-150">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="d6eb1-151">So erstellen Sie eine ASP.net Razor-Website in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d6eb1-151">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="d6eb1-152">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-152">Open Visual Studio.</span></span>
2. <span data-ttu-id="d6eb1-153">Klicken Sie im Menü **Datei** auf **neue Website**.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-153">In the **File** menu, click **New Web Site**.</span></span>

    ![neue Website erstellen](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="d6eb1-155">Wählen Sie im Dialogfeld **neue Website** die zu verwendende Sprache (Visual C# oder Visual Basic) aus.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-155">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="d6eb1-156">Wählen Sie die Vorlage **ASP.net Web Site (Razor)** aus.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-156">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![Razor-Site](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="d6eb1-158">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-158">Click **OK**.</span></span>

<span data-ttu-id="d6eb1-159">Ihr neues Projekt ist vorhanden und wird mit einigen Standard Webseiten aufgefüllt, um Ihnen den Einstieg zu erleichtern.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-159">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="d6eb1-160">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="d6eb1-160">Using IntelliSense</span></span>

<span data-ttu-id="d6eb1-161">Nachdem Sie eine Website erstellt haben, können Sie sehen, wie IntelliSense in Visual Studio funktioniert.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-161">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="d6eb1-162">Öffnen Sie auf der soeben erstellten Website die Seite *default. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="d6eb1-162">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="d6eb1-163">Geben Sie nach den `<h3>`-Tags auf der Seite `@ServerInfo.` (einschließlich Punkt) ein.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-163">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="d6eb1-164">Beachten Sie, wie IntelliSense die verfügbaren Methoden für das `ServerInfo`-Hilfsprogramm in einer Dropdown Liste anzeigt.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-164">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span>

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="d6eb1-166">Wählen Sie die `GetHtml` Methode aus der Liste aus, und drücken Sie dann die EINGABETASTE.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-166">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="d6eb1-167">IntelliSense füllt automatisch die-Methode aus.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-167">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="d6eb1-168">(Wie bei jeder Methode in C#müssen Sie `()` Zeichen nach der-Methode hinzufügen.) Der vollständige Code für die `GetHtml`-Methode sieht wie im folgenden Beispiel aus:</span><span class="sxs-lookup"><span data-stu-id="d6eb1-168">(As with any method in C#, you must add `()` characters after the method.) The completed code for the `GetHtml` method looks like the following example:</span></span>

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="d6eb1-169">Drücken Sie STRG + F5, um die Seite auszuführen.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="d6eb1-170">So sieht die Seite aus, wenn Sie in einem Browser angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="d6eb1-170">This is what the page looks like when displayed in a browser:</span></span>

    ![Standardseite im Browser](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="d6eb1-172">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="d6eb1-173">Verwenden des Debuggers</span><span class="sxs-lookup"><span data-stu-id="d6eb1-173">Using the Debugger</span></span>

1. <span data-ttu-id="d6eb1-174">Fügen Sie am oberen Rand der Seite *default. cshtml* nach der Zeile, die mit `Page.Title`beginnt, die folgende Codezeile hinzu:</span><span class="sxs-lookup"><span data-stu-id="d6eb1-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span>

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="d6eb1-175">Klicken Sie im grauen Rand des Editors links neben dem Code auf neben dieser neuen Zeile, um einen *Breakpoint*hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="d6eb1-176">Ein Haltepunkt ist ein Marker, der den Debugger anweist, die Ausführung des Programms an diesem Punkt anzuhalten, damit Sie sehen können, was passiert.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![Haltepunkt festlegen](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="d6eb1-178">Entfernen Sie den aufzurufenden `ServerInfo.GetHtml`-Methode, und fügen Sie an seiner Stelle einen aufzurufenden `@myTime` Variablen hinzu.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="d6eb1-179">Dieser Aufruf zeigt den aktuellen Uhrzeitwert an, der von der neuen Codezeile zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="d6eb1-180">Drücken Sie F5, um die Seite im Debugger auszuführen.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="d6eb1-181">Die Seite wird an dem Haltepunkt angehalten, den Sie festlegen.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="d6eb1-182">In der folgenden Abbildung wird gezeigt, wie die Seite im Editor mit dem Haltepunkt (gelb) aussieht.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span>

    ![Haltepunkt Debuggen](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="d6eb1-184">Klicken Sie in der Symbolleiste Debuggen auf die Schaltfläche Einzel **Schritt** (oder drücken Sie F11), um die nächste Codezeile auszuführen.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="d6eb1-185">Jedes Mal, wenn Sie auf diese Schaltfläche klicken, wird die Ausführung mit der nächsten Codezeile fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Schaltfläche Einzelschritt](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="d6eb1-187">Überprüfen Sie den Wert der `myTime` Variablen, indem Sie den Mauszeiger darüber bewegen oder indem **Sie die Werte** in den Fenstern "lokal" und " **Telefon Stapel** " überprüfen.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="d6eb1-188">Visual Studio zeigt den Wert der Variablen an.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-188">Visual Studio display the value of the variable.</span></span>

    ![Zeitwert anzeigen](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="d6eb1-190">Wenn Sie die Variable untersucht und den Code schrittweise durchlaufen haben, drücken Sie F5, um die Seite fortzusetzen, ohne in jeder Zeile anzuhalten.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="d6eb1-191">Nachdem Sie den gesamten Code schrittweise durchlaufen haben, zeigt der Browser die Seite an.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="d6eb1-192">Weitere Informationen zum Debugger und zum Debuggen von Code in Visual Studio finden Sie unter Exemplarische Vorgehensweise [: Debuggen von Webseiten in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="d6eb1-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="d6eb1-193">Verwenden von Razor in ASP.NET MVC-Projekten mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6eb1-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="d6eb1-194">Die Razor-Syntax wird auch intensiv in ASP.NET-MVC-Projekten verwendet.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="d6eb1-195">MVC ist eine leistungsstarke, auf Mustern basierende Methode zum Erstellen dynamischer Websites.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="d6eb1-196">Wenn die Verwaltung Ihrer ASP.net Web Pages Site schwierig wird, empfiehlt es sich, Sie in eine ASP.NET-MVC-Anwendung umzuwandeln.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="d6eb1-197">Ein Beispiel für das Erstellen einer MVC-Anwendung finden Sie unter [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="d6eb1-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="d6eb1-198">Installieren der Unterstützung für ASP.net Web Pages in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="d6eb1-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="d6eb1-199">In diesem Abschnitt wird gezeigt, wie Visual Web Developer Express 2010 und die ASP.net Web Pages Tools für Visual Studio installiert werden.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="d6eb1-200">Wenn Sie den Webplattform-Installer noch nicht installiert haben, laden Sie ihn unter der folgenden URL herunter:</span><span class="sxs-lookup"><span data-stu-id="d6eb1-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="d6eb1-201">Führen Sie den Webplattform-Installer aus.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="d6eb1-202">Klicken Sie auf die Registerkarte **Produkte** .</span><span class="sxs-lookup"><span data-stu-id="d6eb1-202">Click the **Products** tab.</span></span>

    ![Registerkarte für webpi-Produkte](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="d6eb1-204">Suchen Sie nach **ASP.NET MVC 4** (für ASP.net Web Pages 2), und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="d6eb1-205">Diese Produkte umfassen Visual Studio-Tools zum Entwickeln von ASP.net Razor-Websites.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Webpi-Installationsoptionen](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="d6eb1-207">Klicken Sie auf **Installieren** , um die Installation abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="d6eb1-207">Click **Install** to complete the installation.</span></span>
