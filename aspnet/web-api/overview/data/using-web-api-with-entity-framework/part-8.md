---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Element Details anzeigen | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f48c5ad73ceb9a4873fbbb621b518553a498966
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449001"
---
# <a name="display-item-details"></a><span data-ttu-id="13eef-102">Anzeigen von Elementdetails</span><span class="sxs-lookup"><span data-stu-id="13eef-102">Display Item Details</span></span>

<span data-ttu-id="13eef-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="13eef-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="13eef-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="13eef-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="13eef-105">In diesem Abschnitt fügen Sie die Möglichkeit zum Anzeigen von Details zu jedem Buch hinzu.</span><span class="sxs-lookup"><span data-stu-id="13eef-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="13eef-106">Fügen Sie in "App. js" dem Ansichts Modell folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="13eef-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="13eef-107">Fügen Sie in views/Home/Index. cshtml dem Link Details ein Daten Bindungs Element hinzu:</span><span class="sxs-lookup"><span data-stu-id="13eef-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="13eef-108">Dadurch wird der Click-Handler für die &lt;ein&gt;-Element an die `getBookDetail`-Funktion im Ansichts Modell gebunden.</span><span class="sxs-lookup"><span data-stu-id="13eef-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="13eef-109">Ersetzen Sie in der gleichen Datei die folgende Markierung:</span><span class="sxs-lookup"><span data-stu-id="13eef-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="13eef-110">durch diesen:</span><span class="sxs-lookup"><span data-stu-id="13eef-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="13eef-111">Dieses Markup erstellt eine Tabelle, die an die Eigenschaften der `detail` Observable im Ansichts Modell gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="13eef-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="13eef-112">Mit der Syntax "&lt;!--ko-&gt;&quot; können Sie eine Knockout-Bindung außerhalb eines DOM-Elements einschließen.</span><span class="sxs-lookup"><span data-stu-id="13eef-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="13eef-113">In diesem Fall bewirkt die `if` Bindung, dass dieser Abschnitt des Markups nur angezeigt wird, wenn `details` nicht NULL ist.</span><span class="sxs-lookup"><span data-stu-id="13eef-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="13eef-114">Wenn Sie nun die app ausführen und auf eine der &quot;Details&quot; Links klicken, werden die Buch Details in der App angezeigt.</span><span class="sxs-lookup"><span data-stu-id="13eef-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="13eef-115">[Zurück](part-7.md)
> [Weiter](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="13eef-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
