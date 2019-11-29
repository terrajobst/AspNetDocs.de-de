---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Bekämpfung von Bots (VB) | Microsoft-Dokumentation
author: wenz
description: Automatisierte Bots Verputzen Webprotokolle und andere Websites mit Spam und senden Kommentar Formulare ohne Benutzerinteraktion. Das NOBOT-Steuerelement im ASP.NET AJAX-con...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: a8ca71b96cb84c97b1a60ae6a3d1a129cd1b0b10
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606387"
---
# <a name="fighting-bots-vb"></a><span data-ttu-id="cbafd-104">Abwehren von Bots (VB)</span><span class="sxs-lookup"><span data-stu-id="cbafd-104">Fighting Bots (VB)</span></span>

<span data-ttu-id="cbafd-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="cbafd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="cbafd-106">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="cbafd-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span></span>

> <span data-ttu-id="cbafd-107">Automatisierte Bots Verputzen Webprotokolle und andere Websites mit Spam und senden Kommentar Formulare ohne Benutzerinteraktion.</span><span class="sxs-lookup"><span data-stu-id="cbafd-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="cbafd-108">Das NOBOT-Steuerelement im ASP.NET AJAX Control Toolkit kann Ihnen helfen, diese Bots zu bekämpfen.</span><span class="sxs-lookup"><span data-stu-id="cbafd-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="overview"></a><span data-ttu-id="cbafd-109">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="cbafd-109">Overview</span></span>

<span data-ttu-id="cbafd-110">Automatisierte Bots Verputzen Webprotokolle und andere Websites mit Spam und senden Kommentar Formulare ohne Benutzerinteraktion.</span><span class="sxs-lookup"><span data-stu-id="cbafd-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="cbafd-111">Das NOBOT-Steuerelement im ASP.NET AJAX Control Toolkit kann Ihnen helfen, diese Bots zu bekämpfen.</span><span class="sxs-lookup"><span data-stu-id="cbafd-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="cbafd-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="cbafd-112">Steps</span></span>

<span data-ttu-id="cbafd-113">Ein gängiger Ansatz zur Bekämpfung von Bots ist die Verwendung von Captchas vollständig automatisierten öffentlichen Tests, um Computer und Menschen voneinander zu informieren.</span><span class="sxs-lookup"><span data-stu-id="cbafd-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="cbafd-114">Ein Turing-Test war ursprünglich ein Test, bei dem jemand entscheiden musste, ob es sich bei einem Kommunikationspartner um einen Menschen oder einen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="cbafd-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="cbafd-115">Im Web besteht eine CAPTCHA in der Regel aus einem Bild mit einigen verzerrten Buchstaben.</span><span class="sxs-lookup"><span data-stu-id="cbafd-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="cbafd-116">Die Idee ist, dass nur ein Mensch die Buchstaben auf dem Bild lesen kann, während die OCR-Algorithmen fehlschlagen.</span><span class="sxs-lookup"><span data-stu-id="cbafd-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="cbafd-117">Dieser Ansatz bietet mehrere vor-und Nachteile, aber eine Erörterung dieses Ansatzes geht über den Rahmen dieses Tutorials hinaus.</span><span class="sxs-lookup"><span data-stu-id="cbafd-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="cbafd-118">Es gibt jedoch ein Steuerelement im ASP.NET AJAX Control Toolkit, das einen ähnlichen Ansatz bietet: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="cbafd-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="cbafd-119">Sie ist einfacher zu überwinden als eine CAPTCHA, aber Sie ist sehr einfach zu verwenden und eignet sich hervorragend für Websites wie Blogs, bei denen es sich um einen Erfolg handelt, wenn die meisten Spam Versuche besiegt werden, was das `NoBot`-Steuerelement tun kann.</span><span class="sxs-lookup"><span data-stu-id="cbafd-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="cbafd-120">`NoBot` fängt das Postback des aktuellen ASP.net-Webformulars ab, wenn mindestens eine dieser Bedingungen erfüllt ist:</span><span class="sxs-lookup"><span data-stu-id="cbafd-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="cbafd-121">Der Browser kann ein JavaScript-Rätsel nicht lösen (z.b. wenn JavaScript deaktiviert ist).</span><span class="sxs-lookup"><span data-stu-id="cbafd-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="cbafd-122">Der Benutzer hat das Formular schnell übermittelt.</span><span class="sxs-lookup"><span data-stu-id="cbafd-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="cbafd-123">Die Client-IP-Adresse hat das Formular zu häufig in einem bestimmten Zeitraum übermittelt.</span><span class="sxs-lookup"><span data-stu-id="cbafd-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="cbafd-124">Um diese Bedingungen zu überprüfen, benötigt das `NoBot` Steuerelement diese Attribute (alle optional):</span><span class="sxs-lookup"><span data-stu-id="cbafd-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="cbafd-125">`ResponseMinimumDelaySeconds` minimale Anzahl von Sekunden zwischen Postbacks</span><span class="sxs-lookup"><span data-stu-id="cbafd-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="cbafd-126">`CutoffWindowSeconds` Länge des Zeitintervalls, in dem Postbacks von einer IP Measures sind</span><span class="sxs-lookup"><span data-stu-id="cbafd-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="cbafd-127">`CutoffMaximumInstances` maximale Anzahl von Sekunden pro Zeitintervall</span><span class="sxs-lookup"><span data-stu-id="cbafd-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="cbafd-128">Das folgende Markup erfordert, dass mindestens zwei Sekunden zwischen Postbacks Vergehen und dass nur fünf Postbacks oder weniger innerhalb eines Zeitraums von 30 Sekunden vorhanden sind:</span><span class="sxs-lookup"><span data-stu-id="cbafd-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

<span data-ttu-id="cbafd-129">Stellen Sie dann wie üblich sicher, dass die `ScriptManager` auf der Seite enthalten ist, damit die ASP.NET AJAX-Bibliothek geladen und das steuerungstooltoolkit verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="cbafd-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

<span data-ttu-id="cbafd-130">Da die meisten der Überprüfungen `NoBot` auf der Serverseite ausgeführt werden, müssen Sie das Ergebnis dieser Überprüfungen überprüfen.</span><span class="sxs-lookup"><span data-stu-id="cbafd-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="cbafd-131">Dies kann durch Aufrufen der `IsValid()`-Methode von `NoBot`erfolgen.</span><span class="sxs-lookup"><span data-stu-id="cbafd-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="cbafd-132">Es verfügt über ein Argument (als `out` Parameter/`ByRef` Parameter), das vom Typ `NoBotState`ist.</span><span class="sxs-lookup"><span data-stu-id="cbafd-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="cbafd-133">Die Zeichen folgen Darstellung enthält den Grund, in dem die Überprüfung fehlschlägt, und `Valid` andernfalls.</span><span class="sxs-lookup"><span data-stu-id="cbafd-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="cbafd-134">Der folgende Code gibt eine Nachricht gemäß `NoBot`Ergebnis aus:</span><span class="sxs-lookup"><span data-stu-id="cbafd-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

<span data-ttu-id="cbafd-135">Zum Schluss benötigen Sie ein zu über mittelendes Formular und ein Bezeichnungs Element zum Ausgeben der Nachricht.</span><span class="sxs-lookup"><span data-stu-id="cbafd-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

<span data-ttu-id="cbafd-136">Wenn Sie dieses Skript ausführen und JavaScript deaktivieren oder das Formular innerhalb der ersten zwei Sekunden übermitteln oder das Formular sieben Mal innerhalb von dreißig Sekunden übermitteln, erhalten Sie eine Fehlermeldung.</span><span class="sxs-lookup"><span data-stu-id="cbafd-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="cbafd-137">Verwenden Sie dieses Steuerelement jedoch klug, da für nur etwa 90-95% der Benutzer JavaScript aktiviert ist. aus diesem Grund können 5-10% der Benutzer `NoBot`Tests nicht ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cbafd-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>

<span data-ttu-id="cbafd-138">[![diese Fehlermeldung wurde möglicherweise von einem bot verursacht.](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cbafd-138">[![This error message could have been caused by a bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span></span>

<span data-ttu-id="cbafd-139">Diese Fehlermeldung wurde möglicherweise von einem bot verursacht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](fighting-bots-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="cbafd-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="cbafd-140">Vorheriges</span><span class="sxs-lookup"><span data-stu-id="cbafd-140">Previous</span></span>](fighting-bots-cs.md)
