---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Erstellen eines numerischen auf-/ab--Steuerelements mit einemC#Webdienst-Back-End () | Microsoft-Dokumentation
author: wenz
description: Anstatt einem Benutzer einen Wert in ein Kontrollkästchen einzugeben, kann es sein, dass ein numerisches auf-/ab--Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) mehr als c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: 816a840b9e93b95a049c3a4cb792e9deeab28983
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509013"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a><span data-ttu-id="4a6fa-103">Erstellen einen numerischen UpAndDown-Steuerelements mit einem Webdienst-Back-End (C#)</span><span class="sxs-lookup"><span data-stu-id="4a6fa-103">Creating a Numeric Up/Down Control with a Web Service Backend (C#)</span></span>

<span data-ttu-id="4a6fa-104">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4a6fa-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4a6fa-105">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4a6fa-105">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span></span>

> <span data-ttu-id="4a6fa-106">Anstatt einem Benutzer einen Wert in ein Kontrollkästchen einzugeben, kann ein numerisches auf-/ab-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) als komfortabler angesehen werden.</span><span class="sxs-lookup"><span data-stu-id="4a6fa-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="4a6fa-107">Standardmäßig erhöht oder verringert das NumericUpDown-Steuerelement einen Wert immer um 1, aber ein Webdienst erweist sich als mehr Flexibilität.</span><span class="sxs-lookup"><span data-stu-id="4a6fa-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="overview"></a><span data-ttu-id="4a6fa-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="4a6fa-108">Overview</span></span>

<span data-ttu-id="4a6fa-109">Anstatt einem Benutzer einen Wert in ein Kontrollkästchen einzugeben, kann ein numerisches auf-/ab-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) als komfortabler angesehen werden.</span><span class="sxs-lookup"><span data-stu-id="4a6fa-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="4a6fa-110">Standardmäßig erhöht oder verringert das `NumericUpDown` Steuerelement einen Wert immer um 1, aber ein Webdienst erweist sich als mehr Flexibilität.</span><span class="sxs-lookup"><span data-stu-id="4a6fa-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="4a6fa-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="4a6fa-111">Steps</span></span>

<span data-ttu-id="4a6fa-112">Das ASP.NET AJAX-Steuerelement-Toolkit enthält den `NumericUpDown` Extender, der automatisch zwei Schaltflächen zu einem Textfeld hinzufügt: eine zum Erhöhen des Werts, eine zum verringern.</span><span class="sxs-lookup"><span data-stu-id="4a6fa-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="4a6fa-113">Das-Steuerelement unterstützt jedoch auch einen Webdienst-oder Seiten Methodenaufrufe.</span><span class="sxs-lookup"><span data-stu-id="4a6fa-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="4a6fa-114">Wenn Sie auf die Schaltfläche nach oben oder nach unten klicken, stellt der JavaScript-Code eine Verbindung mit dem Webserver her und führt dort eine Methode aus.</span><span class="sxs-lookup"><span data-stu-id="4a6fa-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="4a6fa-115">Die Methoden Signatur ist die folgende:</span><span class="sxs-lookup"><span data-stu-id="4a6fa-115">The method signature is the following one:</span></span>

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

<span data-ttu-id="4a6fa-116">Das `current`-Argument ist der aktuelle Wert im Textfeld. Das `tag`-Attribut sind zusätzliche Kontext Daten, die als Eigenschaft des `NumericUpDown` Extender festgelegt werden können (Dies ist jedoch nicht erforderlich).</span><span class="sxs-lookup"><span data-stu-id="4a6fa-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="4a6fa-117">In diesem Beispiel darf das numerische auf-/ab--Steuerelement nur Werte zulassen, die die beiden folgenden Werte haben: 1, 2, 4, 8, 16, 32, 64 usw.</span><span class="sxs-lookup"><span data-stu-id="4a6fa-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="4a6fa-118">Daher muss die Methode, die ausgeführt wird, wenn der Benutzer den Wert erhöhen möchte, den alten Wert verdoppeln. die andere Methode muss den Wert durch zwei dividieren.</span><span class="sxs-lookup"><span data-stu-id="4a6fa-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="4a6fa-119">Hier ist der komplette Webdienst:</span><span class="sxs-lookup"><span data-stu-id="4a6fa-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

<span data-ttu-id="4a6fa-120">Erstellen Sie abschließend eine neue ASP.NET-Seite.</span><span class="sxs-lookup"><span data-stu-id="4a6fa-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="4a6fa-121">Wie üblich benötigen Sie ein `ScriptManager`-Steuerelement, ein `TextBox`-Steuerelement und ein `NumericUpDownExtender`-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="4a6fa-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="4a6fa-122">Für Letzteres müssen Sie die Webdienst Informationen bereitstellen:</span><span class="sxs-lookup"><span data-stu-id="4a6fa-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="4a6fa-123">`ServiceDownMethod` Name der Down-Webmethode oder Seiten Methode</span><span class="sxs-lookup"><span data-stu-id="4a6fa-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="4a6fa-124">`ServiceDownPath` Pfad zum Webdienst mit der Down-Dienst Methode. weglassen, wenn Sie eine Seiten Methode verwenden</span><span class="sxs-lookup"><span data-stu-id="4a6fa-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="4a6fa-125">`ServiceUpMethod` Name der up-Webmethode oder-Seiten Methode</span><span class="sxs-lookup"><span data-stu-id="4a6fa-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="4a6fa-126">`ServiceUpPath` Pfad zum Webdienst mit der up-Dienst Methode. weglassen, wenn Sie eine Seiten Methode verwenden</span><span class="sxs-lookup"><span data-stu-id="4a6fa-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="4a6fa-127">Hier ist das komplette Markup für die Seite:</span><span class="sxs-lookup"><span data-stu-id="4a6fa-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

<span data-ttu-id="4a6fa-128">Wenn Sie die Seite ausführen, beachten Sie, dass sich der Wert im Textfeld immer verdoppelt, wenn Sie auf die obere Schaltfläche klicken, und wird halbiert, wenn Sie auf die untere Schaltfläche klicken.</span><span class="sxs-lookup"><span data-stu-id="4a6fa-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>

<span data-ttu-id="4a6fa-129">[![werden nur Zahlen angezeigt, die eine Potenz von 2 haben.](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4a6fa-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)</span></span>

<span data-ttu-id="4a6fa-130">Nur Zahlen, die eine Potenz von 2 sind, werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="4a6fa-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4a6fa-131">Weiter</span><span class="sxs-lookup"><span data-stu-id="4a6fa-131">Next</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
