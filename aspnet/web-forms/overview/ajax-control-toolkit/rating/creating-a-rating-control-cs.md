---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Erstellen eines Bewertungs Steuer ElementsC#() | Microsoft-Dokumentation
author: wenz
description: Viele Websites, von e-Commerce bis hin zu Communitysites, bieten ihren Benutzern die Bewertung von Artikeln oder Elementen. Dies erfordert in der Regel einen gewissen Codierungsaufwand, aber wir haben die...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: c0bf793406e378321f54f0460031c526a0b41a02
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496161"
---
# <a name="creating-a-rating-control-c"></a><span data-ttu-id="5e636-104">Erstellen eines Bewertungssteuerelements (C#)</span><span class="sxs-lookup"><span data-stu-id="5e636-104">Creating a Rating Control (C#)</span></span>

<span data-ttu-id="5e636-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5e636-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5e636-106">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5e636-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span></span>

> <span data-ttu-id="5e636-107">Viele Websites, von e-Commerce bis hin zu Communitysites, bieten ihren Benutzern die Bewertung von Artikeln oder Elementen.</span><span class="sxs-lookup"><span data-stu-id="5e636-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="5e636-108">Dies erfordert in der Regel einen gewissen Codierungsaufwand, aber wir haben das steuerungstooltoolkit zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="5e636-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="overview"></a><span data-ttu-id="5e636-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="5e636-109">Overview</span></span>

<span data-ttu-id="5e636-110">Viele Websites, von e-Commerce bis hin zu Communitysites, bieten ihren Benutzern die Bewertung von Artikeln oder Elementen.</span><span class="sxs-lookup"><span data-stu-id="5e636-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="5e636-111">Dies erfordert in der Regel einen gewissen Codierungsaufwand, aber wir haben das steuerungstooltoolkit zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="5e636-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="5e636-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="5e636-112">Steps</span></span>

<span data-ttu-id="5e636-113">Zuerst benötigen Sie mindestens zwei Arten von Bildern: eine für ein ausgefülltes Bewertungs Element und eine für ein leeres Bewertungs Element.</span><span class="sxs-lookup"><span data-stu-id="5e636-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="5e636-114">Ein Bewertungs Element ist normalerweise ein Stern oder ein Smiley.</span><span class="sxs-lookup"><span data-stu-id="5e636-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="5e636-115">Für dieses Szenario finden Sie die drei Dateien "Smiley. png" und "Empty. png" und "Smiley-done. png" im Rahmen der Quell Code Downloads für dieses Tutorial.</span><span class="sxs-lookup"><span data-stu-id="5e636-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="5e636-116">Erstellen Sie dann eine neue ASP.NET-Datei, und beginnen Sie damit, ihr ein `ScriptManager`-Steuerelement hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="5e636-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

<span data-ttu-id="5e636-117">Fügen Sie dann das `Rating`-Steuerelement aus dem ASP.NET AJAX Control Toolkit hinzu.</span><span class="sxs-lookup"><span data-stu-id="5e636-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="5e636-118">Für dieses Beispiel müssen die folgenden Attribute festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="5e636-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="5e636-119">`CurrentRating` die zu verwendende anfängliche Bewertung</span><span class="sxs-lookup"><span data-stu-id="5e636-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="5e636-120">maximale Bewertung `MaxRating`</span><span class="sxs-lookup"><span data-stu-id="5e636-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="5e636-121">`EmptyStarCssClass` Sie die zu verwendende CSS-Klasse, wenn ein Bewertungs Element (Star) leer ist.</span><span class="sxs-lookup"><span data-stu-id="5e636-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="5e636-122">`FilledStarCssClass` der CSS-Klasse, die verwendet werden soll, wenn ein Bewertungs Element (Star) ausgefüllt wird</span><span class="sxs-lookup"><span data-stu-id="5e636-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="5e636-123">`StarCssClass` der CSS-Klasse, die für einen sichtbaren stat verwendet werden soll</span><span class="sxs-lookup"><span data-stu-id="5e636-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="5e636-124">`WaitingStarCssClass` Sie die zu verwendende CSS-Klasse, während eine Sterne-Bewertung an den Server zurückgesendet wird.</span><span class="sxs-lookup"><span data-stu-id="5e636-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="5e636-125">Und hier ist das Markup, das ein Bewertungs Steuerelement mit fünf Elementen (Smileys) erstellt, von denen keine anfänglich ausgefüllt wird:</span><span class="sxs-lookup"><span data-stu-id="5e636-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

<span data-ttu-id="5e636-126">Die drei CSS-Klassen, auf die verwiesen wird, müssen jetzt die entsprechenden Bilddateien anzeigen, was mit CSS leicht zu tun ist:</span><span class="sxs-lookup"><span data-stu-id="5e636-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

<span data-ttu-id="5e636-127">Stellen Sie sicher, dass Sie die Breite und Höhe der drei Bilder angeben. andernfalls kann die Anzeige ein wenig nach oben erscheinen.</span><span class="sxs-lookup"><span data-stu-id="5e636-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="5e636-128">Schließlich sollte das Ergebnis der Bewertung dem Benutzer angezeigt werden (oder, zumindest in einer Datenbank gespeichert).</span><span class="sxs-lookup"><span data-stu-id="5e636-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="5e636-129">Fügen Sie daher eine Bezeichnung für die Ausgabe einer Textnachricht und eine Schaltfläche "Senden" hinzu, um das Bewertungsformular an den Server zurückzusenden:</span><span class="sxs-lookup"><span data-stu-id="5e636-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

<span data-ttu-id="5e636-130">Greifen Sie im serverseitigen Code über den `ID` auf das Bewertungs Steuerelement zu, und greifen Sie dann auf seine `CurrentRating`-Eigenschaft zu. Dies ist die Anzahl der ausgewählten Bewertungs Elemente, in unserem Beispiel ein Wert zwischen 0 und 5.</span><span class="sxs-lookup"><span data-stu-id="5e636-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

<span data-ttu-id="5e636-131">Speichern Sie die Seite, und laden Sie Sie in Ihren Browser.</span><span class="sxs-lookup"><span data-stu-id="5e636-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="5e636-132">Wenn Sie mit dem Mauszeiger auf die (anfänglich leeren) Bewertungs Elemente zeigen, tritt ein JavaScript-Effekt auf: die Bewertung ändert sich.</span><span class="sxs-lookup"><span data-stu-id="5e636-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="5e636-133">Wenn Sie auf den Sternen Satz klicken, wird die aktuelle Bewertung beibehalten.</span><span class="sxs-lookup"><span data-stu-id="5e636-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="5e636-134">Wenn Sie das Formular übermitteln, gibt der serverseitige Code die ausgewählte Bewertung aus.</span><span class="sxs-lookup"><span data-stu-id="5e636-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>

<span data-ttu-id="5e636-135">[![Erstellen eines Bewertungssystems mit minimalem Code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5e636-135">[![Creating a rating system with minimal code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="5e636-136">Erstellen eines Bewertungssystems mit minimalem Code ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-rating-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5e636-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5e636-137">Weiter</span><span class="sxs-lookup"><span data-stu-id="5e636-137">Next</span></span>](creating-a-rating-control-vb.md)
