---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Zulassen von nur bestimmten Zeichen in einem Textfeld (VB) | Microsoft-Dokumentation
author: wenz
description: ASP.net-Validierungs Steuerelemente können sicherstellen, dass nur bestimmte Zeichen in Benutzereingaben zulässig sind. Dies hindert Benutzer jedoch immer noch nicht daran, ungültige Eingaben einzugeben...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: 895708ebecc30c5f35e6ecd0349604bb777cbd93
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74573942"
---
# <a name="allowing-only-certain-characters-in-a-text-box-vb"></a><span data-ttu-id="c80f7-104">Zulassen von nur bestimmten Zeichen in einem Textfeld (VB)</span><span class="sxs-lookup"><span data-stu-id="c80f7-104">Allowing Only Certain Characters in a Text Box (VB)</span></span>

<span data-ttu-id="c80f7-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c80f7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c80f7-106">[Code herunterladen](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c80f7-106">[Download Code](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span></span>

> <span data-ttu-id="c80f7-107">ASP.net-Validierungs Steuerelemente können sicherstellen, dass nur bestimmte Zeichen in Benutzereingaben zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="c80f7-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="c80f7-108">Dadurch wird jedoch immer noch nicht verhindert, dass Benutzer ungültige Zeichen eingeben und das Formular übermitteln.</span><span class="sxs-lookup"><span data-stu-id="c80f7-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="overview"></a><span data-ttu-id="c80f7-109">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="c80f7-109">Overview</span></span>

<span data-ttu-id="c80f7-110">ASP.net-Validierungs Steuerelemente können sicherstellen, dass nur bestimmte Zeichen in Benutzereingaben zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="c80f7-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="c80f7-111">Dadurch wird jedoch immer noch nicht verhindert, dass Benutzer ungültige Zeichen eingeben und das Formular übermitteln.</span><span class="sxs-lookup"><span data-stu-id="c80f7-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="c80f7-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="c80f7-112">Steps</span></span>

<span data-ttu-id="c80f7-113">Das ASP.NET AJAX Control Toolkit enthält das `FilteredTextBox` Steuerelement, das ein Textfeld erweitert.</span><span class="sxs-lookup"><span data-stu-id="c80f7-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="c80f7-114">Nach der Aktivierung kann nur ein bestimmter Satz von Zeichen in das Feld eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="c80f7-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="c80f7-115">Damit dies funktioniert, benötigen wir zuerst das ASP.NET AJAX-`ScriptManager`, das die JavaScript-Bibliotheken lädt, die auch vom ASP.NET AJAX Control Toolkit verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="c80f7-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

<span data-ttu-id="c80f7-116">Anschließend benötigen wir ein Textfeld:</span><span class="sxs-lookup"><span data-stu-id="c80f7-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

<span data-ttu-id="c80f7-117">Zum Schluss übernimmt das `FilteredTextBoxExtender`-Steuerelement die Einschränkung der Zeichen, die der Benutzer eingeben darf.</span><span class="sxs-lookup"><span data-stu-id="c80f7-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="c80f7-118">Legen Sie zunächst das `TargetControlID`-Attribut auf die `ID` des `TextBox`-Steuer Elements fest.</span><span class="sxs-lookup"><span data-stu-id="c80f7-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="c80f7-119">Wählen Sie dann einen der verfügbaren `FilterType` Werte aus:</span><span class="sxs-lookup"><span data-stu-id="c80f7-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="c80f7-120">Standard `Custom`; Sie müssen eine Liste gültiger Zeichen angeben.</span><span class="sxs-lookup"><span data-stu-id="c80f7-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="c80f7-121">nur `LowercaseLetters` Kleinbuchstaben</span><span class="sxs-lookup"><span data-stu-id="c80f7-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="c80f7-122">nur `Numbers` Ziffern</span><span class="sxs-lookup"><span data-stu-id="c80f7-122">`Numbers` digits only</span></span>
- <span data-ttu-id="c80f7-123">nur `UppercaseLetters` Großbuchstaben</span><span class="sxs-lookup"><span data-stu-id="c80f7-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="c80f7-124">Wenn die `Custom FilterType` verwendet wird, muss die `ValidChars`-Eigenschaft festgelegt werden, und es muss eine Liste von Zeichen bereitgestellt werden, die eingegeben werden können.</span><span class="sxs-lookup"><span data-stu-id="c80f7-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="c80f7-125">Übrigens: Wenn Sie versuchen, Text in das Textfeld einzufügen, werden alle ungültigen Zeichen entfernt.</span><span class="sxs-lookup"><span data-stu-id="c80f7-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="c80f7-126">Hier sehen Sie das Markup für das `FilteredTextBoxExtender` Steuerelement, das nur Ziffern zulässt (etwas, das auch mit `FilterType="Numbers"`möglich wäre):</span><span class="sxs-lookup"><span data-stu-id="c80f7-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

<span data-ttu-id="c80f7-127">Führen Sie die Seite aus, und versuchen Sie, einen Buchstaben einzugeben, wenn JavaScript aktiviert ist. Ziffern werden jedoch auf der Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c80f7-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="c80f7-128">Beachten Sie jedoch, dass der von `FilteredTextBox` bereitgestellte Schutz nicht Aufzählungs sicher ist: Wenn JavaScript aktiviert ist, können alle Daten in das Textfeld eingegeben werden, sodass Sie zusätzliche Validierungs Mittel, d. h. ASP, verwenden müssen. Die Validierungs Steuerelemente des Netzes.</span><span class="sxs-lookup"><span data-stu-id="c80f7-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>

<span data-ttu-id="c80f7-129">[Es können ![nur Ziffern eingegeben werden.](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c80f7-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span></span>

<span data-ttu-id="c80f7-130">Es können nur Ziffern eingegeben werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="c80f7-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c80f7-131">Vorheriges</span><span class="sxs-lookup"><span data-stu-id="c80f7-131">Previous</span></span>](allowing-only-certain-characters-in-a-text-box-cs.md)
