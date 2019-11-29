---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: Dynamisches Steuern von Update Panel-Animationen (C#) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Für den Inhalt eines...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 183974564764aab9c0d8a4e577995f3c444bf2d3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606788"
---
# <a name="dynamically-controlling-updatepanel-animations-c"></a><span data-ttu-id="41227-104">Dynamisches Steuern von UpdatePanel-Animationen (C#)</span><span class="sxs-lookup"><span data-stu-id="41227-104">Dynamically Controlling UpdatePanel Animations (C#)</span></span>

<span data-ttu-id="41227-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="41227-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="41227-106">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="41227-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span></span>

> <span data-ttu-id="41227-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="41227-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="41227-108">Für den Inhalt eines Update Panel ist ein spezieller Extender vorhanden, der sich stark auf das Animations Framework stützt: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="41227-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="41227-109">Es kann auch zusammen mit Update Panel-Triggern verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="41227-109">It can also work together with UpdatePanel triggers.</span></span>

## <a name="overview"></a><span data-ttu-id="41227-110">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="41227-110">Overview</span></span>

<span data-ttu-id="41227-111">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="41227-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="41227-112">Für den Inhalt einer `UpdatePanel`ist ein spezieller Extender vorhanden, der sich stark auf das Animations Framework stützt: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="41227-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="41227-113">Sie kann auch mit `UpdatePanel` Triggern zusammenarbeiten.</span><span class="sxs-lookup"><span data-stu-id="41227-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="41227-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="41227-114">Steps</span></span>

<span data-ttu-id="41227-115">Der erste Schritt besteht darin, die `ScriptManager` auf der Seite einzuschließen, sodass die ASP.NET AJAX-Bibliothek geladen und das steuerungstooltoolkit verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="41227-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

<span data-ttu-id="41227-116">Die Animation in diesem Szenario wird auf eine Anzeige der aktuellen Zeit angewendet.</span><span class="sxs-lookup"><span data-stu-id="41227-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="41227-117">Diese Informationen können mit der `Page_Load()`-Methode in eine Bezeichnung geschrieben werden, oder (aus Gründen der Einfachheit) der folgende Inline Code wird verwendet:</span><span class="sxs-lookup"><span data-stu-id="41227-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

<span data-ttu-id="41227-118">Außerdem wird eine Schaltfläche zum Aktualisieren der Zeit erstellt:</span><span class="sxs-lookup"><span data-stu-id="41227-118">Also, a button to trigger updating the time is created:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

<span data-ttu-id="41227-119">Dieser Code wird dann in den `<ContentTemplate>` Abschnitt eines `UpdatePanel` Elements eingefügt.</span><span class="sxs-lookup"><span data-stu-id="41227-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="41227-120">Das `UpdateMode`-Attribut des Bereichs muss auf `"Conditional"`festgelegt werden, da nur Trigger den Inhalt des Panels aktualisieren können.</span><span class="sxs-lookup"><span data-stu-id="41227-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="41227-121">Im Abschnitt `<Triggers>` der `UpdatePanel`wird ein asynchroner Postback-ausgelöst, der mit dem `Click`-Ereignis der Schaltfläche verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="41227-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="41227-122">Wenn der Benutzer auf die Schaltfläche klickt, wird der `UpdatePanel` aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="41227-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="41227-123">Hier sehen Sie das Markup für das `UpdatePanel`-Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="41227-123">Here is the markup for the `UpdatePanel` control:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

<span data-ttu-id="41227-124">Zum Schluss muss der `UpdatePanelAnimationExtender` konfiguriert werden: Legen Sie das `TargetControlID`-Attribut auf die ID des Panels fest, und definieren Sie eine Animation innerhalb des Extenders.</span><span class="sxs-lookup"><span data-stu-id="41227-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="41227-125">Das Ausblenden in ist sinnvoll, wodurch eine schöne visuelle Betonung der aktualisierten Zeit entsteht.</span><span class="sxs-lookup"><span data-stu-id="41227-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="41227-126">Das Extendermarkup kann dann wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="41227-126">Your extender markup may then look like this:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

<span data-ttu-id="41227-127">Führen Sie die Datei im Browser aus.</span><span class="sxs-lookup"><span data-stu-id="41227-127">Run the file in the browser.</span></span> <span data-ttu-id="41227-128">Wenn Sie auf die Schaltfläche klicken, wird die aktuelle Uhrzeit im Panel angezeigt, während die Dauer von einer Sekunde immer ausgeblendet wird.</span><span class="sxs-lookup"><span data-stu-id="41227-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>

<span data-ttu-id="41227-129">[![die aktuelle Zeit in](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="41227-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span></span>

<span data-ttu-id="41227-130">Die aktuelle Zeit wird ausgeblendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="41227-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="41227-131">[Zurück](animating-an-updatepanel-control-cs.md)
> [Weiter](adding-animation-to-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="41227-131">[Previous](animating-an-updatepanel-control-cs.md)
[Next](adding-animation-to-a-control-vb.md)</span></span>
