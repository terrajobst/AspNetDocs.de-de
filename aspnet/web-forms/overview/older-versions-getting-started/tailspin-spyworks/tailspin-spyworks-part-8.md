---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Teil 8: abschließende Seiten, Ausnahmebehandlung und Schlussfolgerung | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 8 fügt eine Kontaktseite, eine Seite und eine Ausnahme hinzu...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 707dc9d87ae324a7897c971a451e40bc54c96cb3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474339"
---
# <a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="205c7-104">Teil 8: abschließende Seiten, Ausnahmebehandlung und Schlussfolgerung</span><span class="sxs-lookup"><span data-stu-id="205c7-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>

<span data-ttu-id="205c7-105">von [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="205c7-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="205c7-106">Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="205c7-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="205c7-107">Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="205c7-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="205c7-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="205c7-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="205c7-109">Teil 8 fügt eine Kontaktseite, Informationen zur Seite und Ausnahmebehandlung hinzu.</span><span class="sxs-lookup"><span data-stu-id="205c7-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="205c7-110">Dies ist das Ende der Reihe.</span><span class="sxs-lookup"><span data-stu-id="205c7-110">This is the conclusion of the series.</span></span>

## <a id="_Toc260221680"></a><span data-ttu-id="205c7-111">Kontaktseite (e-Mail senden von ASP.net)</span><span class="sxs-lookup"><span data-stu-id="205c7-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="205c7-112">Erstellen Sie eine neue Seite mit dem Namen contactus. aspx.</span><span class="sxs-lookup"><span data-stu-id="205c7-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="205c7-113">Erstellen Sie mit dem Designer das folgende Formular, und verwenden Sie dabei eine besondere Notiz, um den toolkitscriptmanager und das Editor-Steuerelement aus dem AjaxControlToolkit einzubeziehen.</span><span class="sxs-lookup"><span data-stu-id="205c7-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxControlToolkit.</span></span> <span data-ttu-id="205c7-114">erforderlich.</span><span class="sxs-lookup"><span data-stu-id="205c7-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="205c7-115">Doppelklicken Sie auf die Schaltfläche "Senden", um einen Click-Ereignishandler in der Code Behind-Datei zu generieren, und implementieren Sie eine Methode, um die Kontaktinformationen als e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="205c7-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="205c7-116">Dieser Code erfordert, dass Ihre Web. config-Datei einen Eintrag im Konfigurations Abschnitt enthält, der den SMTP-Server angibt, der zum Senden von e-Mails verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="205c7-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a><span data-ttu-id="205c7-117">Info-Seite</span><span class="sxs-lookup"><span data-stu-id="205c7-117">About Page</span></span>

<span data-ttu-id="205c7-118">Erstellen Sie eine Seite mit dem Namen "aboutus. aspx", und fügen Sie den gewünschten Inhalt hinzu.</span><span class="sxs-lookup"><span data-stu-id="205c7-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a><span data-ttu-id="205c7-119">Globaler Ausnahme Handler</span><span class="sxs-lookup"><span data-stu-id="205c7-119">Global Exception Handler</span></span>

<span data-ttu-id="205c7-120">Schließlich haben wir in der gesamten Anwendung Ausnahmen ausgelöst, und es gibt unvorhersehbare Umstände, die ebenfalls zu unbehandelten Ausnahmen in unserer Webanwendung führen.</span><span class="sxs-lookup"><span data-stu-id="205c7-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="205c7-121">Wir möchten niemals, dass eine nicht behandelte Ausnahme für den Besucher einer Website angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="205c7-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="205c7-122">Abgesehen davon, dass es sich nicht um eine schreckliche Benutzer Darstellung handelt, kann es auch zu Sicherheitsproblemen kommen.</span><span class="sxs-lookup"><span data-stu-id="205c7-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="205c7-123">Um dieses Problem zu beheben, wird ein globaler Ausnahmehandler implementiert.</span><span class="sxs-lookup"><span data-stu-id="205c7-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="205c7-124">Öffnen Sie hierzu die Datei Global. asax, und notieren Sie sich den folgenden vorab generierten Ereignishandler.</span><span class="sxs-lookup"><span data-stu-id="205c7-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="205c7-125">Fügen Sie Code hinzu, um den Anwendungs\_Fehlerhandler wie folgt zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="205c7-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="205c7-126">Fügen Sie der Projekt Mappe dann eine Seite mit dem Namen Error. aspx hinzu, und fügen Sie diesen Markup Ausschnitt hinzu.</span><span class="sxs-lookup"><span data-stu-id="205c7-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="205c7-127">Extrahieren Sie nun auf der Seite\_Load-Ereignishandler die Fehlermeldungen aus dem Anforderungs Objekt.</span><span class="sxs-lookup"><span data-stu-id="205c7-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a><span data-ttu-id="205c7-128">Abschluss</span><span class="sxs-lookup"><span data-stu-id="205c7-128">Conclusion</span></span>

<span data-ttu-id="205c7-129">Wir haben gesehen, dass ASP.net WebForms das Erstellen einer ausgereiften Website mit Datenbankzugriff, Mitgliedschaft, AJAX usw. erleichtert.</span><span class="sxs-lookup"><span data-stu-id="205c7-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="205c7-130">ziemlich schnell.</span><span class="sxs-lookup"><span data-stu-id="205c7-130">pretty quickly.</span></span>

<span data-ttu-id="205c7-131">Hoffentlich haben Sie in diesem Tutorial die Tools erhalten, die Sie benötigen, um mit dem Aufbau eigener ASP.net WebForms-Anwendungen zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="205c7-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="205c7-132">Previous</span><span class="sxs-lookup"><span data-stu-id="205c7-132">Previous</span></span>](tailspin-spyworks-part-7.md)
