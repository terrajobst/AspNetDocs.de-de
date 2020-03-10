---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: Testen der Sicherheit eines Kennworts (C#) | Microsoft-Dokumentation
author: wenz
description: Kenn Wörter sind fast überall erforderlich, sodass verzögerte Benutzer tendenziell einfache Kenn Wörter auswählen, die leicht zu unterbrechen sind. Das passwordStrength-Steuerelement im ASP. N...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: e55eab9feebc18f39dd40c59cfb423208296b6c5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78508965"
---
# <a name="testing-the-strength-of-a-password-c"></a>Testen der Sicherheit eines Kennworts (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)

> Kenn Wörter sind fast überall erforderlich, sodass verzögerte Benutzer tendenziell einfache Kenn Wörter auswählen, die leicht zu unterbrechen sind. Das passwordStrength-Steuerelement im ASP.NET AJAX Control Toolkit kann prüfen, wie gut ein Kennwort ist.

## <a name="overview"></a>Übersicht

Kenn Wörter sind fast überall erforderlich, sodass verzögerte Benutzer tendenziell einfache Kenn Wörter auswählen, die leicht zu unterbrechen sind. Mit dem `PasswordStrength`-Steuerelement im ASP.NET AJAX Control Toolkit können Sie überprüfen, wie gut ein Kennwort ist.

## <a name="steps"></a>Schritte

Das `PasswordStrength` Steuerelement erweitert ein Textfeld und überprüft, ob das Kennwort in diesem Feld ausreichend ist. Es bietet eine Vielzahl von Optionen über Attribute. Dies sind nur einige der folgenden:

- `MinimumNumericCharacters` Mindestanzahl numerischer Zeichen, die im Kennwort erforderlich sind
- `MinimumSymbolCharacters` Mindestanzahl von Symbol Zeichen (keine Buchstaben und Ziffern), die im Kennwort erforderlich sind.
- `PreferredPasswordLength` Mindestlänge des Kennworts
- `RequiresUpperAndLowerCaseCharacters`, ob für das Kennwort sowohl groß-als auch Kleinbuchstaben verwendet werden müssen.

Der `StrengthIndicatorType` stellt die Informationen bereit, wie die Stärke des Kennworts, als Text (Value `"Text"`) oder als eine Art von Statusanzeige (Wert `"BarIndicator"`) angegeben wird. Im `DisplayPosition`-Attribut konfigurieren Sie, wo die Informationen angezeigt werden. Im folgenden finden Sie ein vollständiges Beispiel, einschließlich des ASP.net-`ScriptManager`-Steuer Elements, des `PasswordStrength` Steuer Elements und natürlich eines Textfelds, in dem der Benutzer ein Kennwort eingeben darf. Zur Veranschaulichung ist das zweite Formularfeld ein reguläres Textfeld und kein Kenn Wortfeld, sodass Sie während der Entwicklung sehen können, was Sie eingeben.

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

Führen Sie die Seite aus, und geben Sie Folgendes ein: erst nachdem Sie Kleinbuchstaben, Großbuchstaben, Ziffern und Symbole eingegeben haben, gilt das Kennwort als nicht breakable.

[![jetzt ist das Kennwort (Recht) gut.](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)

Das Kennwort ist nun (Recht) gut ([Klicken Sie, um das Bild in voller Größe anzuzeigen](testing-the-strength-of-a-password-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Weiter](testing-the-strength-of-a-password-vb.md)
