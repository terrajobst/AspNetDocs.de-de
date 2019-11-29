---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Testen der Sicherheit eines Kennworts (VB) | Microsoft-Dokumentation
author: wenz
description: Kenn Wörter sind fast überall erforderlich, sodass verzögerte Benutzer tendenziell einfache Kenn Wörter auswählen, die leicht zu unterbrechen sind. Das passwordStrength-Steuerelement im ASP. N...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: b614e1788eeafc175dd792ec6d3e4619f9ea2b7a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606255"
---
# <a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="2b879-104">Testen der Sicherheit eines Kennworts (VB)</span><span class="sxs-lookup"><span data-stu-id="2b879-104">Testing the Strength of a Password (VB)</span></span>

<span data-ttu-id="2b879-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2b879-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2b879-106">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2b879-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="2b879-107">Kenn Wörter sind fast überall erforderlich, sodass verzögerte Benutzer tendenziell einfache Kenn Wörter auswählen, die leicht zu unterbrechen sind.</span><span class="sxs-lookup"><span data-stu-id="2b879-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="2b879-108">Das passwordStrength-Steuerelement im ASP.NET AJAX Control Toolkit kann prüfen, wie gut ein Kennwort ist.</span><span class="sxs-lookup"><span data-stu-id="2b879-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="overview"></a><span data-ttu-id="2b879-109">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="2b879-109">Overview</span></span>

<span data-ttu-id="2b879-110">Kenn Wörter sind fast überall erforderlich, sodass verzögerte Benutzer tendenziell einfache Kenn Wörter auswählen, die leicht zu unterbrechen sind.</span><span class="sxs-lookup"><span data-stu-id="2b879-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="2b879-111">Mit dem `PasswordStrength`-Steuerelement im ASP.NET AJAX Control Toolkit können Sie überprüfen, wie gut ein Kennwort ist.</span><span class="sxs-lookup"><span data-stu-id="2b879-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="2b879-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="2b879-112">Steps</span></span>

<span data-ttu-id="2b879-113">Das `PasswordStrength` Steuerelement erweitert ein Textfeld und überprüft, ob das Kennwort in diesem Feld ausreichend ist.</span><span class="sxs-lookup"><span data-stu-id="2b879-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="2b879-114">Es bietet eine Vielzahl von Optionen über Attribute. Dies sind nur einige der folgenden:</span><span class="sxs-lookup"><span data-stu-id="2b879-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="2b879-115">`MinimumNumericCharacters` Mindestanzahl numerischer Zeichen, die im Kennwort erforderlich sind</span><span class="sxs-lookup"><span data-stu-id="2b879-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="2b879-116">`MinimumSymbolCharacters` Mindestanzahl von Symbol Zeichen (keine Buchstaben und Ziffern), die im Kennwort erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="2b879-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="2b879-117">`PreferredPasswordLength` Mindestlänge des Kennworts</span><span class="sxs-lookup"><span data-stu-id="2b879-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="2b879-118">`RequiresUpperAndLowerCaseCharacters`, ob für das Kennwort sowohl groß-als auch Kleinbuchstaben verwendet werden müssen.</span><span class="sxs-lookup"><span data-stu-id="2b879-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="2b879-119">Der `StrengthIndicatorType` stellt die Informationen bereit, wie die Stärke des Kennworts, als Text (Value `"Text"`) oder als eine Art von Statusanzeige (Wert `"BarIndicator"`) angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="2b879-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="2b879-120">Im `DisplayPosition`-Attribut konfigurieren Sie, wo die Informationen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="2b879-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="2b879-121">Im folgenden finden Sie ein vollständiges Beispiel, einschließlich des ASP.net-`ScriptManager`-Steuer Elements, des `PasswordStrength` Steuer Elements und natürlich eines Textfelds, in dem der Benutzer ein Kennwort eingeben darf.</span><span class="sxs-lookup"><span data-stu-id="2b879-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="2b879-122">Zur Veranschaulichung ist das zweite Formularfeld ein reguläres Textfeld und kein Kenn Wortfeld, sodass Sie während der Entwicklung sehen können, was Sie eingeben.</span><span class="sxs-lookup"><span data-stu-id="2b879-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="2b879-123">Führen Sie die Seite aus, und geben Sie Folgendes ein: erst nachdem Sie Kleinbuchstaben, Großbuchstaben, Ziffern und Symbole eingegeben haben, gilt das Kennwort als nicht breakable.</span><span class="sxs-lookup"><span data-stu-id="2b879-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>

<span data-ttu-id="2b879-124">[![jetzt ist das Kennwort (Recht) gut.](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2b879-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="2b879-125">Das Kennwort ist nun (Recht) gut ([Klicken Sie, um das Bild in voller Größe anzuzeigen](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2b879-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2b879-126">Vorheriges</span><span class="sxs-lookup"><span data-stu-id="2b879-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
