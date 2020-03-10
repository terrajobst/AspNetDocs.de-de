---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Erstellen und Verwenden eines Hilfsobjekts in einer ASP.net Web Pages-(Razor-) Website | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Artikel wird beschrieben, wie ein Hilfsprogramm auf einer ASP.net Web Pages-Website (Razor) erstellt wird. Ein Hilfsprogramm ist eine wiederverwendbare Komponente, die Code und Markup für die Leistung enthält...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 380663951094c9fc7d5f0601e30995fa073a204b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454305"
---
# <a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="b0dab-104">Erstellen und Verwenden eines Hilfsobjekts in einer ASP.net Web Pages-(Razor-) Site</span><span class="sxs-lookup"><span data-stu-id="b0dab-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="b0dab-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b0dab-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b0dab-106">In diesem Artikel wird beschrieben, wie ein Hilfsprogramm auf einer ASP.net Web Pages-Website (Razor) erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="b0dab-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="b0dab-107">Ein Hilfsprogramm ist eine wiederverwendbare Komponente, die Code und Markup enthält, *um eine Aufgabe* auszuführen, die vielleicht mühsam oder komplex ist.</span><span class="sxs-lookup"><span data-stu-id="b0dab-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="b0dab-108">**Lernen Sie Folgendes:**</span><span class="sxs-lookup"><span data-stu-id="b0dab-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="b0dab-109">Erstellen und Verwenden eines einfachen Hilfsprogramms.</span><span class="sxs-lookup"><span data-stu-id="b0dab-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="b0dab-110">Dies sind die ASP.NET-Funktionen, die im Artikel eingeführt wurden:</span><span class="sxs-lookup"><span data-stu-id="b0dab-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="b0dab-111">Die `@helper`-Syntax.</span><span class="sxs-lookup"><span data-stu-id="b0dab-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b0dab-112">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="b0dab-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b0dab-113">ASP.net Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="b0dab-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="b0dab-114">Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="b0dab-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="overview-of-helpers"></a><span data-ttu-id="b0dab-115">Übersicht über Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="b0dab-115">Overview of Helpers</span></span>

<span data-ttu-id="b0dab-116">Wenn Sie dieselben Aufgaben auf verschiedenen Seiten auf Ihrer Website ausführen müssen, können Sie ein Hilfsprogramm verwenden.</span><span class="sxs-lookup"><span data-stu-id="b0dab-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="b0dab-117">ASP.net Web Pages umfasst eine Reihe von Hilfsprogrammen, die Sie herunterladen und installieren können.</span><span class="sxs-lookup"><span data-stu-id="b0dab-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="b0dab-118">(Eine Liste der integrierten Hilfsprogramme in ASP.net Web Pages finden Sie in der Kurzübersicht über die [ASP.NET-API](https://go.microsoft.com/fwlink/?LinkId=202907).) Wenn keines der vorhandenen Hilfsprogramme Ihren Anforderungen entspricht, können Sie ein eigenes Hilfsprogramm erstellen.</span><span class="sxs-lookup"><span data-stu-id="b0dab-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="b0dab-119">Mit einem-Hilfsprogramm können Sie einen gemeinsamen Codeblock über mehrere Seiten hinweg verwenden.</span><span class="sxs-lookup"><span data-stu-id="b0dab-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="b0dab-120">Angenommen, Sie möchten auf Ihrer Seite häufig ein Notiz Element erstellen, das von den normalen Absätzen getrennt ist.</span><span class="sxs-lookup"><span data-stu-id="b0dab-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="b0dab-121">Der Hinweis wird möglicherweise als `<div>` Element erstellt, das als Feld mit einem Rahmen formatiert ist.</span><span class="sxs-lookup"><span data-stu-id="b0dab-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="b0dab-122">Anstatt der Seite jedes Mal, wenn Sie eine Notiz anzeigen möchten, dasselbe Markup hinzuzufügen, können Sie das Markup als Hilfsobjekt verpacken.</span><span class="sxs-lookup"><span data-stu-id="b0dab-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="b0dab-123">Anschließend können Sie den Hinweis in eine einzelne Codezeile einfügen, wo Sie ihn benötigen.</span><span class="sxs-lookup"><span data-stu-id="b0dab-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="b0dab-124">Wenn Sie ein Hilfsobjekt verwenden, wird der Code in jeder Ihrer Seiten einfacher und leichter lesbar.</span><span class="sxs-lookup"><span data-stu-id="b0dab-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="b0dab-125">Außerdem ist es einfacher, Ihre Website zu verwalten, denn wenn Sie das Aussehen der Notizen ändern müssen, können Sie das Markup an einem Ort ändern.</span><span class="sxs-lookup"><span data-stu-id="b0dab-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="b0dab-126">Erstellen eines Hilfsprogramms</span><span class="sxs-lookup"><span data-stu-id="b0dab-126">Creating a Helper</span></span>

<span data-ttu-id="b0dab-127">In diesem Verfahren wird gezeigt, wie Sie das Hilfsprogramm erstellen, das den Hinweis erstellt, wie soeben beschrieben.</span><span class="sxs-lookup"><span data-stu-id="b0dab-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="b0dab-128">Dies ist ein einfaches Beispiel, aber das benutzerdefinierte Hilfsprogramm kann beliebigen Markup-und ASP.NET-Code enthalten, den Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="b0dab-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="b0dab-129">Erstellen Sie im Stamm Ordner der Website einen Ordner namens *App\_Code*.</span><span class="sxs-lookup"><span data-stu-id="b0dab-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="b0dab-130">Dies ist ein reservierter Ordnername in ASP.net, in dem Sie Code für Komponenten wie Hilfsprogramme einfügen können.</span><span class="sxs-lookup"><span data-stu-id="b0dab-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="b0dab-131">Erstellen Sie in der *App\_-Code* Ordners eine neue *cshtml* -Datei, und nennen Sie Sie *myhilfsprogramme. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b0dab-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="b0dab-132">Ersetzen Sie den vorhandenen Inhalt durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b0dab-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="b0dab-133">Der Code verwendet die `@helper` Syntax, um ein neues Hilfsprogramm mit dem Namen `MakeNote`zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="b0dab-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="b0dab-134">Mit diesem speziellen Hilfsprogramm können Sie einen Parameter mit dem Namen `content` übergeben, der eine Kombination aus Text und Markup enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="b0dab-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="b0dab-135">Das Hilfsprogramm fügt die Zeichenfolge mithilfe der `@content` Variable in den Hinweis Text ein.</span><span class="sxs-lookup"><span data-stu-id="b0dab-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="b0dab-136">Beachten Sie, dass die Datei " *myhelper. cshtml*" heißt, das Hilfsprogramm aber `MakeNote`heißt.</span><span class="sxs-lookup"><span data-stu-id="b0dab-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="b0dab-137">Sie können mehrere benutzerdefinierte Hilfsprogramme in eine einzelne Datei einfügen.</span><span class="sxs-lookup"><span data-stu-id="b0dab-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="b0dab-138">Speichern und schließen Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="b0dab-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="b0dab-139">Verwenden des Hilfsprogramms in einer Seite</span><span class="sxs-lookup"><span data-stu-id="b0dab-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="b0dab-140">Erstellen Sie im Stamm Ordner eine neue leere Datei mit dem Namen " *TestHelper. cshtml*".</span><span class="sxs-lookup"><span data-stu-id="b0dab-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="b0dab-141">Fügen Sie folgenden Code in die Datei ein:</span><span class="sxs-lookup"><span data-stu-id="b0dab-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="b0dab-142">Um das von Ihnen erstellte Hilfsprogramm aufzurufen, verwenden Sie `@` gefolgt von dem Dateinamen, in dem das Hilfsprogramm ist, einem Punkt und dem Namen des Hilfsprogramms.</span><span class="sxs-lookup"><span data-stu-id="b0dab-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="b0dab-143">(Wenn Sie über mehrere Ordner in der *App\_-Code* Ordner verfügen, können Sie die-Syntax `@FolderName.FileName.HelperName` verwenden, um das Hilfsprogramm in einer beliebigen geschachtelten Ordnerebene aufzurufen.)</span><span class="sxs-lookup"><span data-stu-id="b0dab-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="b0dab-144">Der Text, den Sie in Anführungszeichen innerhalb der Klammern einfügen, ist der Text, der vom Hilfsprogramm als Teil des Notiz Teils der Webseite angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="b0dab-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="b0dab-145">Speichern Sie die Seite, und führen Sie Sie in einem Browser aus.</span><span class="sxs-lookup"><span data-stu-id="b0dab-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="b0dab-146">Das Hilfsprogramm generiert das Notiz Element, in dem Sie das Hilfsobjekt aufgerufen haben: zwischen den beiden Absätzen.</span><span class="sxs-lookup"><span data-stu-id="b0dab-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Ein Screenshot, der die Seite im Browser anzeigt und zeigt, wie das Hilfsprogramm Markup generiert hat, das ein Feld um den angegebenen Text einfügt.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="b0dab-148">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="b0dab-148">Additional Resources</span></span>

<span data-ttu-id="b0dab-149">[Horizontales Menü als Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341)-Hilfsobjekt.</span><span class="sxs-lookup"><span data-stu-id="b0dab-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="b0dab-150">In diesem Blogbeitrag von Mike Papst wird gezeigt, wie Sie ein horizontales Menü als Hilfsobjekt mit Markup, CSS und Code erstellen.</span><span class="sxs-lookup"><span data-stu-id="b0dab-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="b0dab-151">[Nutzen von HTML5 in ASP.net Web Pages-Hilfsprogramme für webmatrix und ASP.net MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0dab-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="b0dab-152">Dieser Blogeintrag von Sam Abraham zeigt ein Hilfsobjekt, das ein HTML5-`Canvas` Element rendert.</span><span class="sxs-lookup"><span data-stu-id="b0dab-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="b0dab-153">[Der Unterschied zwischen @Helpers und @Functions in webmatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="b0dab-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="b0dab-154">Dieser Blogeintrag von Mike Brind beschreibt `@helper` Syntax und `@function` Syntax und wann Sie verwendet werden sollten.</span><span class="sxs-lookup"><span data-stu-id="b0dab-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
