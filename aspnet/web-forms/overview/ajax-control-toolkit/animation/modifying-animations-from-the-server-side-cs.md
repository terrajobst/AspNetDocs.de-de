---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Ändern von Animationen von der Server SeiteC#() | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animationen können auch...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 0594efea9598a6c2461a72f789b5bd5f8ece23da
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78483891"
---
# <a name="modifying-animations-from-the-server-side-c"></a><span data-ttu-id="c3e43-104">Ändern von Animationen von Server Seite (C#)</span><span class="sxs-lookup"><span data-stu-id="c3e43-104">Modifying Animations From The Server Side (C#)</span></span>

<span data-ttu-id="c3e43-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c3e43-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c3e43-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c3e43-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span></span>

> <span data-ttu-id="c3e43-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="c3e43-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c3e43-108">Die Animationen können auch auf Serverseite geändert werden.</span><span class="sxs-lookup"><span data-stu-id="c3e43-108">The animations may also be changed on the server-side</span></span>

## <a name="overview"></a><span data-ttu-id="c3e43-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="c3e43-109">Overview</span></span>

<span data-ttu-id="c3e43-110">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="c3e43-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c3e43-111">Die Animationen können auch auf Serverseite geändert werden.</span><span class="sxs-lookup"><span data-stu-id="c3e43-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="c3e43-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="c3e43-112">Steps</span></span>

<span data-ttu-id="c3e43-113">Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:</span><span class="sxs-lookup"><span data-stu-id="c3e43-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

<span data-ttu-id="c3e43-114">Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="c3e43-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

<span data-ttu-id="c3e43-115">Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:</span><span class="sxs-lookup"><span data-stu-id="c3e43-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

<span data-ttu-id="c3e43-116">Der Rest des Codes wird auf Serverseite ausgeführt und verwendet kein Markup. Stattdessen wird Code verwendet, um das `AnimationExtender` Steuerelement zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="c3e43-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

<span data-ttu-id="c3e43-117">Das steuerungstooltoolkit bietet derzeit jedoch keinen API-Zugriff zum Erstellen der einzelnen Animationen.</span><span class="sxs-lookup"><span data-stu-id="c3e43-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="c3e43-118">Es ist jedoch möglich, die Animation-Eigenschaft des `AnimationExtender`auf eine Zeichenfolge festzulegen, die das XML-Markup enthält, das beim deklarativen Zuweisen der Animationen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c3e43-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="c3e43-119">Um den XML-Code zu erstellen, der das `<Animations>`-Element nicht enthalten darf, können Sie die XML-Unterstützung des .NET Framework verwenden oder, wie im folgenden Code, einfach die Zeichenfolge angeben:</span><span class="sxs-lookup"><span data-stu-id="c3e43-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

<span data-ttu-id="c3e43-120">Fügen Sie abschließend der aktuellen Seite im `<form runat="server">`-Element das `AnimationExtender`-Steuerelement hinzu, und stellen Sie sicher, dass die Animation eingeschlossen ist und ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="c3e43-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]

<span data-ttu-id="c3e43-121">[![die Animation mit Server seitigem C#/VB-Code erstellt wird](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c3e43-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span></span>

<span data-ttu-id="c3e43-122">Die Animation wird mithilfe serverseitiger C#/VB-Code erstellt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](modifying-animations-from-the-server-side-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="c3e43-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c3e43-123">[Zurück](triggering-an-animation-in-another-control-cs.md)
> [Weiter](executing-animations-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="c3e43-123">[Previous](triggering-an-animation-in-another-control-cs.md)
[Next](executing-animations-using-client-side-code-cs.md)</span></span>
