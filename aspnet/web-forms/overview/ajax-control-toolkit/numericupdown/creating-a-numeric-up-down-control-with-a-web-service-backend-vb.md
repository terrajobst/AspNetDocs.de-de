---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Erstellen eines numerischen Steuer Elements nach oben/unten mit einem Webdienst-Back-End (VB) | Microsoft-Dokumentation
author: wenz
description: Anstatt einem Benutzer einen Wert in ein Kontrollkästchen einzugeben, kann es sein, dass ein numerisches auf-/ab--Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) mehr als c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 2bf6e1b27180589d39e308de62b5be1f47fa8fe2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606366"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a><span data-ttu-id="95d88-103">Erstellen einen numerischen UpAndDown-Steuerelements mit einem Webdienst-Back-End (VB)</span><span class="sxs-lookup"><span data-stu-id="95d88-103">Creating a Numeric Up/Down Control with a Web Service Backend (VB)</span></span>

<span data-ttu-id="95d88-104">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="95d88-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="95d88-105">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="95d88-105">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span></span>

> <span data-ttu-id="95d88-106">Anstatt einem Benutzer einen Wert in ein Kontrollkästchen einzugeben, kann ein numerisches auf-/ab-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) als komfortabler angesehen werden.</span><span class="sxs-lookup"><span data-stu-id="95d88-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="95d88-107">Standardmäßig erhöht oder verringert das NumericUpDown-Steuerelement einen Wert immer um 1, aber ein Webdienst erweist sich als mehr Flexibilität.</span><span class="sxs-lookup"><span data-stu-id="95d88-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="overview"></a><span data-ttu-id="95d88-108">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="95d88-108">Overview</span></span>

<span data-ttu-id="95d88-109">Anstatt einem Benutzer einen Wert in ein Kontrollkästchen einzugeben, kann ein numerisches auf-/ab-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) als komfortabler angesehen werden.</span><span class="sxs-lookup"><span data-stu-id="95d88-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="95d88-110">Standardmäßig erhöht oder verringert das `NumericUpDown` Steuerelement einen Wert immer um 1, aber ein Webdienst erweist sich als mehr Flexibilität.</span><span class="sxs-lookup"><span data-stu-id="95d88-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="95d88-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="95d88-111">Steps</span></span>

<span data-ttu-id="95d88-112">Das ASP.NET AJAX-Steuerelement-Toolkit enthält den `NumericUpDown` Extender, der automatisch zwei Schaltflächen zu einem Textfeld hinzufügt: eine zum Erhöhen des Werts, eine zum verringern.</span><span class="sxs-lookup"><span data-stu-id="95d88-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="95d88-113">Das-Steuerelement unterstützt jedoch auch einen Webdienst-oder Seiten Methodenaufrufe.</span><span class="sxs-lookup"><span data-stu-id="95d88-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="95d88-114">Wenn Sie auf die Schaltfläche nach oben oder nach unten klicken, stellt der JavaScript-Code eine Verbindung mit dem Webserver her und führt dort eine Methode aus.</span><span class="sxs-lookup"><span data-stu-id="95d88-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="95d88-115">Die Methoden Signatur ist die folgende:</span><span class="sxs-lookup"><span data-stu-id="95d88-115">The method signature is the following one:</span></span>

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

<span data-ttu-id="95d88-116">Das `current`-Argument ist der aktuelle Wert im Textfeld. Das `tag`-Attribut sind zusätzliche Kontext Daten, die als Eigenschaft des `NumericUpDown` Extender festgelegt werden können (Dies ist jedoch nicht erforderlich).</span><span class="sxs-lookup"><span data-stu-id="95d88-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="95d88-117">In diesem Beispiel darf das numerische auf-/ab--Steuerelement nur Werte zulassen, die die beiden folgenden Werte haben: 1, 2, 4, 8, 16, 32, 64 usw.</span><span class="sxs-lookup"><span data-stu-id="95d88-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="95d88-118">Daher muss die Methode, die ausgeführt wird, wenn der Benutzer den Wert erhöhen möchte, den alten Wert verdoppeln. die andere Methode muss den Wert durch zwei dividieren.</span><span class="sxs-lookup"><span data-stu-id="95d88-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="95d88-119">Hier ist der komplette Webdienst:</span><span class="sxs-lookup"><span data-stu-id="95d88-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

<span data-ttu-id="95d88-120">Erstellen Sie abschließend eine neue ASP.NET-Seite.</span><span class="sxs-lookup"><span data-stu-id="95d88-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="95d88-121">Wie üblich benötigen Sie ein `ScriptManager`-Steuerelement, ein `TextBox`-Steuerelement und ein `NumericUpDownExtender`-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="95d88-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="95d88-122">Für Letzteres müssen Sie die Webdienst Informationen bereitstellen:</span><span class="sxs-lookup"><span data-stu-id="95d88-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="95d88-123">`ServiceDownMethod` Name der Down-Webmethode oder Seiten Methode</span><span class="sxs-lookup"><span data-stu-id="95d88-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="95d88-124">`ServiceDownPath` Pfad zum Webdienst mit der Down-Dienst Methode. weglassen, wenn Sie eine Seiten Methode verwenden</span><span class="sxs-lookup"><span data-stu-id="95d88-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="95d88-125">`ServiceUpMethod` Name der up-Webmethode oder-Seiten Methode</span><span class="sxs-lookup"><span data-stu-id="95d88-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="95d88-126">`ServiceUpPath` Pfad zum Webdienst mit der up-Dienst Methode. weglassen, wenn Sie eine Seiten Methode verwenden</span><span class="sxs-lookup"><span data-stu-id="95d88-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="95d88-127">Hier ist das komplette Markup für die Seite:</span><span class="sxs-lookup"><span data-stu-id="95d88-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

<span data-ttu-id="95d88-128">Wenn Sie die Seite ausführen, beachten Sie, dass sich der Wert im Textfeld immer verdoppelt, wenn Sie auf die obere Schaltfläche klicken, und wird halbiert, wenn Sie auf die untere Schaltfläche klicken.</span><span class="sxs-lookup"><span data-stu-id="95d88-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>

<span data-ttu-id="95d88-129">[![werden nur Zahlen angezeigt, die eine Potenz von 2 haben.](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="95d88-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span></span>

<span data-ttu-id="95d88-130">Nur Zahlen, die eine Potenz von 2 sind, werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="95d88-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="95d88-131">Vorheriges</span><span class="sxs-lookup"><span data-stu-id="95d88-131">Previous</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
