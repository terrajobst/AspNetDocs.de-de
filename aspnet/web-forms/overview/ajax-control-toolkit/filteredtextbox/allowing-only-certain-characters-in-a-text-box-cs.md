---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Zulassen von nur bestimmten Zeichen in einem TextfeldC#() | Microsoft-Dokumentation
author: wenz
description: ASP.net-Validierungs Steuerelemente können sicherstellen, dass nur bestimmte Zeichen in Benutzereingaben zulässig sind. Dies hindert Benutzer jedoch immer noch nicht daran, ungültige Eingaben einzugeben...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: d1e367becd574e31d24fca8545f76b1ed3c4d85e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446355"
---
# <a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="1c9bc-104">Zulassen von nur bestimmten Zeichen in einem Textfeld (C#)</span><span class="sxs-lookup"><span data-stu-id="1c9bc-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>

<span data-ttu-id="1c9bc-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1c9bc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1c9bc-106">[Code herunterladen](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="1c9bc-106">[Download Code](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="1c9bc-107">ASP.net-Validierungs Steuerelemente können sicherstellen, dass nur bestimmte Zeichen in Benutzereingaben zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="1c9bc-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="1c9bc-108">Dadurch wird jedoch immer noch nicht verhindert, dass Benutzer ungültige Zeichen eingeben und das Formular übermitteln.</span><span class="sxs-lookup"><span data-stu-id="1c9bc-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="overview"></a><span data-ttu-id="1c9bc-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="1c9bc-109">Overview</span></span>

<span data-ttu-id="1c9bc-110">ASP.net-Validierungs Steuerelemente können sicherstellen, dass nur bestimmte Zeichen in Benutzereingaben zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="1c9bc-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="1c9bc-111">Dadurch wird jedoch immer noch nicht verhindert, dass Benutzer ungültige Zeichen eingeben und das Formular übermitteln.</span><span class="sxs-lookup"><span data-stu-id="1c9bc-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="1c9bc-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="1c9bc-112">Steps</span></span>

<span data-ttu-id="1c9bc-113">Das ASP.NET AJAX Control Toolkit enthält das `FilteredTextBox` Steuerelement, das ein Textfeld erweitert.</span><span class="sxs-lookup"><span data-stu-id="1c9bc-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="1c9bc-114">Nach der Aktivierung kann nur ein bestimmter Satz von Zeichen in das Feld eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="1c9bc-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="1c9bc-115">Damit dies funktioniert, benötigen wir zuerst das ASP.NET AJAX-`ScriptManager`, das die JavaScript-Bibliotheken lädt, die auch vom ASP.NET AJAX Control Toolkit verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="1c9bc-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="1c9bc-116">Anschließend benötigen wir ein Textfeld:</span><span class="sxs-lookup"><span data-stu-id="1c9bc-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="1c9bc-117">Zum Schluss übernimmt das `FilteredTextBoxExtender`-Steuerelement die Einschränkung der Zeichen, die der Benutzer eingeben darf.</span><span class="sxs-lookup"><span data-stu-id="1c9bc-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="1c9bc-118">Legen Sie zunächst das `TargetControlID`-Attribut auf die `ID` des `TextBox`-Steuer Elements fest.</span><span class="sxs-lookup"><span data-stu-id="1c9bc-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="1c9bc-119">Wählen Sie dann einen der verfügbaren `FilterType` Werte aus:</span><span class="sxs-lookup"><span data-stu-id="1c9bc-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="1c9bc-120">Standard `Custom`; Sie müssen eine Liste gültiger Zeichen angeben.</span><span class="sxs-lookup"><span data-stu-id="1c9bc-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="1c9bc-121">nur `LowercaseLetters` Kleinbuchstaben</span><span class="sxs-lookup"><span data-stu-id="1c9bc-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="1c9bc-122">nur `Numbers` Ziffern</span><span class="sxs-lookup"><span data-stu-id="1c9bc-122">`Numbers` digits only</span></span>
- <span data-ttu-id="1c9bc-123">nur `UppercaseLetters` Großbuchstaben</span><span class="sxs-lookup"><span data-stu-id="1c9bc-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="1c9bc-124">Wenn die `Custom FilterType` verwendet wird, muss die `ValidChars`-Eigenschaft festgelegt werden, und es muss eine Liste von Zeichen bereitgestellt werden, die eingegeben werden können.</span><span class="sxs-lookup"><span data-stu-id="1c9bc-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="1c9bc-125">Übrigens: Wenn Sie versuchen, Text in das Textfeld einzufügen, werden alle ungültigen Zeichen entfernt.</span><span class="sxs-lookup"><span data-stu-id="1c9bc-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="1c9bc-126">Hier sehen Sie das Markup für das `FilteredTextBoxExtender` Steuerelement, das nur Ziffern zulässt (etwas, das auch mit `FilterType="Numbers"`möglich wäre):</span><span class="sxs-lookup"><span data-stu-id="1c9bc-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="1c9bc-127">Führen Sie die Seite aus, und versuchen Sie, einen Buchstaben einzugeben, wenn JavaScript aktiviert ist. Ziffern werden jedoch auf der Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1c9bc-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="1c9bc-128">Beachten Sie jedoch, dass der von `FilteredTextBox` bereitgestellte Schutz nicht Aufzählungs sicher ist: Wenn JavaScript aktiviert ist, können alle Daten in das Textfeld eingegeben werden, sodass Sie zusätzliche Validierungs Mittel, d. h. ASP, verwenden müssen. Die Validierungs Steuerelemente des Netzes.</span><span class="sxs-lookup"><span data-stu-id="1c9bc-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>

<span data-ttu-id="1c9bc-129">[Es können ![nur Ziffern eingegeben werden.](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1c9bc-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="1c9bc-130">Es können nur Ziffern eingegeben werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="1c9bc-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1c9bc-131">Weiter</span><span class="sxs-lookup"><span data-stu-id="1c9bc-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
