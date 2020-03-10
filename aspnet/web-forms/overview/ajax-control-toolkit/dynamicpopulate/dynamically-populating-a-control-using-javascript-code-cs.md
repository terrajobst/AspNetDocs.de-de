---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Dynamisches Auffüllen eines Steuer Elements mithilfe von JavaScript-C#Code () | Microsoft-Dokumentation
author: wenz
description: Das DynamicPopulate-Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert in ein Ziel Steuerelement unter t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 24dc358427dec3ffcba16d00041c9a2db657e7e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430503"
---
# <a name="dynamically-populating-a-control-using-javascript-code-c"></a><span data-ttu-id="c6384-103">Dynamisches Auffüllen eines Steuerelements über den JavaScript-Code (C#)</span><span class="sxs-lookup"><span data-stu-id="c6384-103">Dynamically Populating a Control Using JavaScript Code (C#)</span></span>

<span data-ttu-id="c6384-104">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c6384-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c6384-105">[Code herunterladen](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c6384-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span></span>

> <span data-ttu-id="c6384-106">Das DynamicPopulate-Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert in ein Ziel Steuerelement auf der Seite, ohne eine Seiten Aktualisierung durchführen zu müssen.</span><span class="sxs-lookup"><span data-stu-id="c6384-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="c6384-107">Es ist auch möglich, die Auffüllung mithilfe von benutzerdefiniertem Client seitigem JavaScript-Code zu initiieren.</span><span class="sxs-lookup"><span data-stu-id="c6384-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="c6384-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="c6384-108">Overview</span></span>

<span data-ttu-id="c6384-109">Das `DynamicPopulate`-Steuerelement im ASP.NET AJAX Control Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert ohne Seiten Aktualisierung in ein Ziel Steuerelement auf der Seite aus.</span><span class="sxs-lookup"><span data-stu-id="c6384-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="c6384-110">Es ist auch möglich, die Auffüllung mithilfe von benutzerdefiniertem Client seitigem JavaScript-Code zu initiieren.</span><span class="sxs-lookup"><span data-stu-id="c6384-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="c6384-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="c6384-111">Steps</span></span>

<span data-ttu-id="c6384-112">Zunächst benötigen Sie einen ASP.NET-Webdienst, der die Methode implementiert, die vom `DynamicPopulateExtender` Steuerelement aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="c6384-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="c6384-113">Der Webdienst implementiert die-Methode `getDate()`, die ein Argument vom Typ "String" erwartet, das als `contextKey`bezeichnet wird, da das `DynamicPopulate`-Steuerelement einen Teil der Kontextinformationen mit jedem Webdienst Aufruf sendet.</span><span class="sxs-lookup"><span data-stu-id="c6384-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="c6384-114">Im folgenden finden Sie den Code (File `DynamicPopulate.cs.asmx`), der das aktuelle Datum in einem von drei Formaten abruft:</span><span class="sxs-lookup"><span data-stu-id="c6384-114">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

<span data-ttu-id="c6384-115">Erstellen Sie im nächsten Schritt eine neue ASP.NET-Website, und beginnen Sie mit dem ASP.NET AJAX ScriptManager-Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="c6384-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

<span data-ttu-id="c6384-116">Fügen Sie dann ein Label-Steuerelement hinzu (beispielsweise mithilfe des HTML-Steuer Elements desselben Namens oder des `<asp:Label />` Web-Steuer Elements), das später das Ergebnis des Webdienst Aufrufes anzeigt.</span><span class="sxs-lookup"><span data-stu-id="c6384-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

<span data-ttu-id="c6384-117">Fügen Sie als nächstes ein `DynamicPopulateExtender` Steuerelement hinzu, und geben Sie Webdienst Informationen, das Ziel Steuerelement, aber nicht den Namen des Steuer Elements an, das die Auffüllung auslöst. Dies wird später mithilfe von benutzerdefiniertem JavaScript durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="c6384-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

<span data-ttu-id="c6384-118">Nun zum JavaScript-Teil.</span><span class="sxs-lookup"><span data-stu-id="c6384-118">Now to the JavaScript part.</span></span> <span data-ttu-id="c6384-119">Die `$find()` Funktion, die von der ASP.NET AJAX-Bibliothek definiert wird, gibt einen Verweis auf serverseitige Objekte des ASP.NET AJAX Control Toolkit zurück, z. b. `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="c6384-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="c6384-120">In der aktuellen Datei gibt `$find("dpe")` einen Verweis auf den `DynamicPopulateExtender` Steuerelement auf der Seite zurück.</span><span class="sxs-lookup"><span data-stu-id="c6384-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="c6384-121">Sie macht eine Methode namens `populate()` verfügbar, die den dynamischen auffüllungs Prozess auslöst.</span><span class="sxs-lookup"><span data-stu-id="c6384-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="c6384-122">Die `populate()`-Methode erfordert ein Argument: den Kontext Schlüssel, der als Argument für die `getDate()` Webmethode fungiert.</span><span class="sxs-lookup"><span data-stu-id="c6384-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="c6384-123">Beispielsweise wird `$find("dpe").populate("format1")` die Bezeichnung mit dem aktuellen Datum im Format month-day-year auffüllen.</span><span class="sxs-lookup"><span data-stu-id="c6384-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="c6384-124">Damit das Beispiel etwas flexibler wird, kann der Benutzer jetzt zwischen mehreren Datumsformaten wählen.</span><span class="sxs-lookup"><span data-stu-id="c6384-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="c6384-125">Für jeden dieser Elemente wird ein Optionsfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c6384-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="c6384-126">Sobald der Benutzer auf ein Optionsfeld klickt, füllt JavaScript-Code die Bezeichnung dynamisch mit dem ausgewählten Datumsformat auf.</span><span class="sxs-lookup"><span data-stu-id="c6384-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="c6384-127">Dies sind die folgenden Options Felder:</span><span class="sxs-lookup"><span data-stu-id="c6384-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

<span data-ttu-id="c6384-128">Beachten Sie, dass sich der JavaScript-`this.value` Ausdruck im Kontext eines Options Felds auf den Wert der aktuellen Schaltfläche bezieht, bei der es sich um genau dieselben Informationen handelt, mit denen die `getDate()` Methode funktionieren kann.</span><span class="sxs-lookup"><span data-stu-id="c6384-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>

<span data-ttu-id="c6384-129">[![ein Klick auf die Schaltfläche ruft das Datum vom Server im angegebenen Format ab.](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c6384-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="c6384-130">Wenn Sie auf die Schaltfläche klicken, wird das Datum vom Server im angegebenen Format abgerufen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="c6384-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c6384-131">[Zurück](dynamically-populating-a-control-cs.md)
> [Weiter](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span><span class="sxs-lookup"><span data-stu-id="c6384-131">[Previous](dynamically-populating-a-control-cs.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span></span>
