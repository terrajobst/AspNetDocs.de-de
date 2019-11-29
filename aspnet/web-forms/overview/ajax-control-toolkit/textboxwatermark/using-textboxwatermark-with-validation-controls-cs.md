---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Verwenden von textboxwatermark mit Validierungs SteuerC#Elementen () | Microsoft-Dokumentation
author: wenz
description: Mit dem textboxwatermark-Steuerelement im AJAX Control Toolkit wird ein Textfeld erweitert, sodass ein Text im Feld angezeigt wird. Wenn ein Benutzer in das Feld klickt,
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: bc9498b1c5ba2f38b90706c9200ffa813a945fa9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74610863"
---
# <a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="fa5dc-104">Verwenden von TextBoxWatermark mit Validierungssteuerelementen (C#)</span><span class="sxs-lookup"><span data-stu-id="fa5dc-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>

<span data-ttu-id="fa5dc-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fa5dc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fa5dc-106">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fa5dc-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="fa5dc-107">Mit dem textboxwatermark-Steuerelement im AJAX Control Toolkit wird ein Textfeld erweitert, sodass ein Text im Feld angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="fa5dc-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="fa5dc-108">Wenn ein Benutzer auf das Feld klickt, wird es geleert.</span><span class="sxs-lookup"><span data-stu-id="fa5dc-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="fa5dc-109">Wenn der Benutzer das Kontrollkästchen verlässt, ohne Text einzugeben, wird der vorab gefüllte Text erneut angezeigt.</span><span class="sxs-lookup"><span data-stu-id="fa5dc-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="fa5dc-110">Dies kann mit ASP.net-Validierungs Steuerelementen auf derselben Seite kollidieren, diese Probleme können jedoch möglicherweise behoben werden.</span><span class="sxs-lookup"><span data-stu-id="fa5dc-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="overview"></a><span data-ttu-id="fa5dc-111">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="fa5dc-111">Overview</span></span>

<span data-ttu-id="fa5dc-112">Mit dem `TextBoxWatermark`-Steuerelement im AJAX Control Toolkit wird ein Textfeld erweitert, sodass ein Text im Feld angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="fa5dc-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="fa5dc-113">Wenn ein Benutzer auf das Feld klickt, wird es geleert.</span><span class="sxs-lookup"><span data-stu-id="fa5dc-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="fa5dc-114">Wenn der Benutzer das Kontrollkästchen verlässt, ohne Text einzugeben, wird der vorab gefüllte Text erneut angezeigt.</span><span class="sxs-lookup"><span data-stu-id="fa5dc-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="fa5dc-115">Dies kann mit ASP.net-Validierungs Steuerelementen auf derselben Seite kollidieren, diese Probleme können jedoch möglicherweise behoben werden.</span><span class="sxs-lookup"><span data-stu-id="fa5dc-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="fa5dc-116">Schritte</span><span class="sxs-lookup"><span data-stu-id="fa5dc-116">Steps</span></span>

<span data-ttu-id="fa5dc-117">Die grundlegende Einrichtung des Beispiels sieht folgendermaßen aus: ein `TextBox` Steuerelement wird mithilfe eines `TextBoxWatermarkExtender` Steuer Elements als Wasserzeichen gekennzeichnet.</span><span class="sxs-lookup"><span data-stu-id="fa5dc-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="fa5dc-118">Eine Schaltfläche löst ein Postback aus und wird später verwendet, um die Validierungs Steuerelemente auf der Seite zu lösen.</span><span class="sxs-lookup"><span data-stu-id="fa5dc-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="fa5dc-119">Außerdem ist ein `ScriptManager`-Steuerelement erforderlich, um ASP.NET AJAX zu initialisieren:</span><span class="sxs-lookup"><span data-stu-id="fa5dc-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="fa5dc-120">Fügen Sie nun ein `RequiredFieldValidator` Steuerelement hinzu, mit dem überprüft wird, ob im Feld Text vorhanden ist, wenn das Formular übermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="fa5dc-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="fa5dc-121">Die `InitialValue`-Eigenschaft des Validierungs Steuer Elements muss auf denselben Wert festgelegt werden, der im `TextBoxWatermarkExtender`-Steuerelement verwendet wird: Wenn das Formular übermittelt wird, ist der Wert eines unverändert Textfelds der Wasserzeichen Wert.</span><span class="sxs-lookup"><span data-stu-id="fa5dc-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="fa5dc-122">Bei diesem Ansatz gibt es jedoch ein Problem: Wenn der Client JavaScript deaktiviert, wird das Textfeld nicht mit dem Wasserzeichen Text vorab ausgefüllt, daher löst das `RequiredFieldValidator` keine Fehlermeldung aus.</span><span class="sxs-lookup"><span data-stu-id="fa5dc-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="fa5dc-123">Aus diesem Grund ist ein zweites `RequiredFieldValidator`-Steuerelement erforderlich, das auf ein leeres Textfeld prüft (das `InitialValue`-Attribut wird weggelassen).</span><span class="sxs-lookup"><span data-stu-id="fa5dc-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="fa5dc-124">Da beide Validierungs Steuerelemente `Display`=`"Dynamic"`verwenden, kann der Endbenutzer nicht von der visuellen Darstellung unterscheiden, welche der beiden Validierungs Steuerelemente ausgelöst wurde. Stattdessen sieht es so aus, als ob nur einer davon vorhanden wäre.</span><span class="sxs-lookup"><span data-stu-id="fa5dc-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="fa5dc-125">Fügen Sie schließlich einigen serverseitigen Code hinzu, um den Text im Feld auszugeben, wenn kein Validierungs Steuerelement eine Fehlermeldung ausgegeben hat:</span><span class="sxs-lookup"><span data-stu-id="fa5dc-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="fa5dc-126">[![überprüft das Validierungs Steuerelement, dass kein Text im Feld vorhanden ist.](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fa5dc-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="fa5dc-127">Das Validierungs Steuerelement meldet, dass es keinen Text im Feld gibt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-textboxwatermark-with-validation-controls-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="fa5dc-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fa5dc-128">[Zurück](using-textboxwatermark-in-a-formview-cs.md)
> [Weiter](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="fa5dc-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>
