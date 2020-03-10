---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Verwenden von DynamicPopulate mit einem Benutzer Steuerelement und JavaScript (VB) | Microsoft-Dokumentation
author: wenz
description: Das DynamicPopulate-Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert in ein Ziel Steuerelement unter t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: ee5923ad6d8b101f689a0564aef8b1e0e00a7639
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497301"
---
# <a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a><span data-ttu-id="a836c-103">Verwenden von DynamicPopulate mit einem Benutzersteuerelement und JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="a836c-103">Using DynamicPopulate with a User Control And JavaScript (VB)</span></span>

<span data-ttu-id="a836c-104">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a836c-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a836c-105">[Code herunterladen](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a836c-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span></span>

> <span data-ttu-id="a836c-106">Das DynamicPopulate-Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert in ein Ziel Steuerelement auf der Seite, ohne eine Seiten Aktualisierung durchführen zu müssen.</span><span class="sxs-lookup"><span data-stu-id="a836c-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="a836c-107">Es ist auch möglich, die Auffüllung mithilfe von benutzerdefiniertem Client seitigem JavaScript-Code zu initiieren.</span><span class="sxs-lookup"><span data-stu-id="a836c-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="a836c-108">Es muss jedoch besonders sorgfältig vorgegangen werden, wenn sich der Extender in einem Benutzer Steuerelement befindet.</span><span class="sxs-lookup"><span data-stu-id="a836c-108">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="overview"></a><span data-ttu-id="a836c-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="a836c-109">Overview</span></span>

<span data-ttu-id="a836c-110">Das `DynamicPopulate`-Steuerelement im ASP.NET AJAX Control Toolkit Ruft einen Webdienst (oder eine Seiten Methode) auf und füllt den resultierenden Wert ohne Seiten Aktualisierung in ein Ziel Steuerelement auf der Seite aus.</span><span class="sxs-lookup"><span data-stu-id="a836c-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="a836c-111">Es ist auch möglich, die Auffüllung mithilfe von benutzerdefiniertem Client seitigem JavaScript-Code zu initiieren.</span><span class="sxs-lookup"><span data-stu-id="a836c-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="a836c-112">Es muss jedoch besonders sorgfältig vorgegangen werden, wenn sich der Extender in einem Benutzer Steuerelement befindet.</span><span class="sxs-lookup"><span data-stu-id="a836c-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="a836c-113">Schritte</span><span class="sxs-lookup"><span data-stu-id="a836c-113">Steps</span></span>

<span data-ttu-id="a836c-114">Zunächst benötigen Sie einen ASP.NET-Webdienst, der die Methode implementiert, die vom `DynamicPopulateExtender` Steuerelement aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="a836c-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="a836c-115">Der Webdienst implementiert die-Methode `getDate()`, die ein Argument vom Typ "String" erwartet, das als `contextKey`bezeichnet wird, da das `DynamicPopulate`-Steuerelement einen Teil der Kontextinformationen mit jedem Webdienst Aufruf sendet.</span><span class="sxs-lookup"><span data-stu-id="a836c-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="a836c-116">Im folgenden finden Sie den Code (Dateien `DynamicPopulate.vb.asmx`), der das aktuelle Datum in einem von drei Formaten abruft:</span><span class="sxs-lookup"><span data-stu-id="a836c-116">Here is the code (files `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="a836c-117">Erstellen Sie im nächsten Schritt ein neues Benutzer Steuerelement (`.ascx` Datei), das in der ersten Zeile mit der folgenden Deklaration gekennzeichnet ist:</span><span class="sxs-lookup"><span data-stu-id="a836c-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="a836c-118">Ein &lt;`label`&gt;-Element wird verwendet, um die vom Server kommenden Daten anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="a836c-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

<span data-ttu-id="a836c-119">Außerdem verwenden wir in der Benutzer Steuerelement-Datei drei Options Felder, die jeweils eines der drei möglichen Datumsformate darstellen, die vom Webdienst unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="a836c-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="a836c-120">Wenn der Benutzer auf eine der Options Felder klickt, führt der Browser JavaScript-Code aus, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="a836c-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

<span data-ttu-id="a836c-121">Dieser Code greift auf die `DynamicPopulateExtender` zu (machen Sie sich noch keine Gedanken über die seltsame ID, diese wird später behandelt) und löst die dynamische Auffüllung mit Daten aus.</span><span class="sxs-lookup"><span data-stu-id="a836c-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="a836c-122">Im Kontext des aktuellen Options Felds verweist `this.value` auf seinen Wert, der `format1`, `format2` oder `format3` genau der von der Webmethode erwarteten Wert ist.</span><span class="sxs-lookup"><span data-stu-id="a836c-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="a836c-123">Das einzige, was im Benutzer Steuerelement fehlt, ist das `DynamicPopulateExtender` Steuerelement, mit dem die Options Felder mit dem Webdienst verknüpft werden.</span><span class="sxs-lookup"><span data-stu-id="a836c-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="a836c-124">Auch hier können Sie die seltsame ID beachten, die im Steuerelement verwendet wird: `mcd1$myDate` statt `myDate`.</span><span class="sxs-lookup"><span data-stu-id="a836c-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="a836c-125">Zuvor hat der JavaScript-Code, der für den Zugriff auf die `DynamicPopulateExtender` verwendet wurde, anstelle von `dpe1``mcd1_dpe1`. Diese Benennungs Strategie ist eine besondere Anforderung, wenn Sie `DynamicPopulateExtender` innerhalb eines Benutzer Steuer Elements verwenden.</span><span class="sxs-lookup"><span data-stu-id="a836c-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="a836c-126">Außerdem müssen Sie das Benutzer Steuerelement auf eine bestimmte Weise einbetten, damit es alles funktioniert.</span><span class="sxs-lookup"><span data-stu-id="a836c-126">Furthermore, you have to embed the user control in a specific way to make it all work.</span></span> <span data-ttu-id="a836c-127">Erstellen Sie eine neue ASP.NET-Seite, und registrieren Sie ein Tagpräfix für das Benutzer Steuerelement, das Sie soeben implementiert haben:</span><span class="sxs-lookup"><span data-stu-id="a836c-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

<span data-ttu-id="a836c-128">Fügen Sie dann das ASP.NET AJAX `ScriptManager`-Steuerelement auf der neuen Seite hinzu:</span><span class="sxs-lookup"><span data-stu-id="a836c-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

<span data-ttu-id="a836c-129">Fügen Sie abschließend der Seite das Benutzer Steuerelement hinzu.</span><span class="sxs-lookup"><span data-stu-id="a836c-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="a836c-130">Sie müssen lediglich das `ID` Attribut festlegen (und natürlich `runat="server"`), aber Sie müssen es auch auf einen bestimmten Namen festlegen: `mcd1`, da dies das Präfix ist, das innerhalb des Benutzer Steuer Elements verwendet wird, um über JavaScript darauf zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="a836c-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

<span data-ttu-id="a836c-131">Und das ist schon alles!</span><span class="sxs-lookup"><span data-stu-id="a836c-131">And that's it!</span></span> <span data-ttu-id="a836c-132">Die Seite verhält sich erwartungsgemäß: ein Benutzer klickt auf eines der Options Felder, das Steuerelement im Toolkit Ruft den Webdienst auf und zeigt das aktuelle Datum im gewünschten Format an.</span><span class="sxs-lookup"><span data-stu-id="a836c-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>

<span data-ttu-id="a836c-133">[![sich die Options Felder in einem Benutzer Steuerelement befinden.](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a836c-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="a836c-134">Die Options Felder befinden sich in einem Benutzer Steuerelement ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a836c-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a836c-135">Previous</span><span class="sxs-lookup"><span data-stu-id="a836c-135">Previous</span></span>](dynamically-populating-a-control-using-javascript-code-vb.md)
