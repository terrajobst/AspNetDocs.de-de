---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Dynamisches Auffüllen eines Steuer Elements (VB) | Microsoft-Dokumentation
author: wenz
description: Das DynamicPopulate-Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert in ein Ziel Steuerelement unter t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d11320f1f89bb69afe5f62751574079716124da0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599398"
---
# <a name="dynamically-populating-a-control-vb"></a><span data-ttu-id="02fb5-103">Dynamisches Auffüllen eines Steuerelements (VB)</span><span class="sxs-lookup"><span data-stu-id="02fb5-103">Dynamically Populating a Control (VB)</span></span>

<span data-ttu-id="02fb5-104">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="02fb5-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="02fb5-105">[Code herunterladen](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="02fb5-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span></span>

> <span data-ttu-id="02fb5-106">Das DynamicPopulate-Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert in ein Ziel Steuerelement auf der Seite, ohne eine Seiten Aktualisierung durchführen zu müssen.</span><span class="sxs-lookup"><span data-stu-id="02fb5-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>

## <a name="overview"></a><span data-ttu-id="02fb5-107">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="02fb5-107">Overview</span></span>

<span data-ttu-id="02fb5-108">Das `DynamicPopulate`-Steuerelement im ASP.NET AJAX Control Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert ohne Seiten Aktualisierung in ein Ziel Steuerelement auf der Seite aus.</span><span class="sxs-lookup"><span data-stu-id="02fb5-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="02fb5-109">In diesem Tutorial wird gezeigt, wie Sie diese Vorgehensweise einrichten.</span><span class="sxs-lookup"><span data-stu-id="02fb5-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="02fb5-110">Schritte</span><span class="sxs-lookup"><span data-stu-id="02fb5-110">Steps</span></span>

<span data-ttu-id="02fb5-111">Zunächst benötigen Sie einen ASP.NET-Webdienst, der die Methode implementiert, die von `DynamicPopulate`aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="02fb5-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="02fb5-112">Die Webdienst Klasse erfordert das `ScriptService`-Attribut, das in `Microsoft.Web.Script.Services`definiert ist; Andernfalls kann ASP.NET AJAX den Client seitigen JavaScript-Proxy für den Webdienst nicht erstellen, der wiederum von `DynamicPopulate`benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="02fb5-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="02fb5-113">Die Webmethode muss ein Argument vom Typ "String" mit dem Namen "`contextKey`" erwarten, da das `DynamicPopulate` Steuerelement einen Teil der Kontextinformationen mit jedem Webdienst Aufruf sendet.</span><span class="sxs-lookup"><span data-stu-id="02fb5-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="02fb5-114">Der folgende Webdienst gibt das aktuelle Datum in einem Format zurück, das durch das `contextKey`-Argument dargestellt wird:</span><span class="sxs-lookup"><span data-stu-id="02fb5-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="02fb5-115">Der Webdienst wird dann als `DynamicPopulate.vb.asmx`gespeichert.</span><span class="sxs-lookup"><span data-stu-id="02fb5-115">The web service is then saved as `DynamicPopulate.vb.asmx`.</span></span> <span data-ttu-id="02fb5-116">Alternativ könnten Sie die `getDate()`-Methode als Seiten Methode innerhalb der eigentlichen ASP.NET-Seite mit dem `DynamicPopulate`-Steuerelement implementieren.</span><span class="sxs-lookup"><span data-stu-id="02fb5-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="02fb5-117">Erstellen Sie im nächsten Schritt eine neue ASP.NET-Datei.</span><span class="sxs-lookup"><span data-stu-id="02fb5-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="02fb5-118">Wie immer besteht der erste Schritt darin, die `ScriptManager` auf der aktuellen Seite einzuschließen, um die ASP.NET AJAX-Bibliothek zu laden und das steuerungstooltoolkit funktionsfähig zu machen:</span><span class="sxs-lookup"><span data-stu-id="02fb5-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="02fb5-119">Fügen Sie dann ein Label-Steuerelement hinzu (beispielsweise mit dem HTML-Steuerelement mit dem gleichen Namen oder dem &lt;`asp:Label` /&gt; websteuer Element), das später das Ergebnis des Webdienst Aufrufes anzeigt.</span><span class="sxs-lookup"><span data-stu-id="02fb5-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

<span data-ttu-id="02fb5-120">Eine HTML-Schaltfläche (als HTML-Steuerelement, da kein Postback an den Server erforderlich ist) wird dann verwendet, um die dynamische Auffüllung zu starten:</span><span class="sxs-lookup"><span data-stu-id="02fb5-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="02fb5-121">Schließlich benötigen wir das `DynamicPopulateExtender`-Steuerelement, um es zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="02fb5-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="02fb5-122">Die folgenden Attribute werden festgelegt (außer den offensichtlichen, `ID` und `runat`=`"server"`):</span><span class="sxs-lookup"><span data-stu-id="02fb5-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="02fb5-123">`TargetControlID`, wo das Ergebnis aus dem Webdienst aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="02fb5-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="02fb5-124">`ServicePath` Pfad zum Webdienst (auslassen, wenn Sie eine Seiten Methode verwenden möchten)</span><span class="sxs-lookup"><span data-stu-id="02fb5-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="02fb5-125">`ServiceMethod` Name der Webmethode oder Seiten Methode</span><span class="sxs-lookup"><span data-stu-id="02fb5-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="02fb5-126">`ContextKey` Kontextinformationen, die an den Webdienst gesendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="02fb5-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="02fb5-127">`PopulateTriggerControlID`-Element, das den Webdienst aufruft.</span><span class="sxs-lookup"><span data-stu-id="02fb5-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="02fb5-128">`ClearContentsDuringUpdate`, ob das Target-Element während des Webdienst Aufrufes leer ist.</span><span class="sxs-lookup"><span data-stu-id="02fb5-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="02fb5-129">Wie Sie sehen können, benötigt das Steuerelement einige Informationen, aber es ist recht unkompliziert, alles zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="02fb5-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="02fb5-130">Im folgenden finden Sie das Markup für das `DynamicPopulateExtender`-Steuerelement im aktuellen Szenario:</span><span class="sxs-lookup"><span data-stu-id="02fb5-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="02fb5-131">Führen Sie die ASP.NET-Seite im Browser aus, und klicken Sie auf die Schaltfläche. das aktuelle Datum wird im Format Monat-Tag-Jahr angezeigt.</span><span class="sxs-lookup"><span data-stu-id="02fb5-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>

<span data-ttu-id="02fb5-132">[![ein Klick auf die Schaltfläche ruft das Datum vom Server ab.](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="02fb5-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="02fb5-133">Durch Klicken auf die Schaltfläche wird das Datum vom Server abgerufen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](dynamically-populating-a-control-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="02fb5-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="02fb5-134">[Zurück](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [Weiter](dynamically-populating-a-control-using-javascript-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="02fb5-134">[Previous](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
[Next](dynamically-populating-a-control-using-javascript-code-vb.md)</span></span>
