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
# <a name="allowing-only-certain-characters-in-a-text-box-c"></a>Zulassen von nur bestimmten Zeichen in einem Textfeld (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)

> ASP.net-Validierungs Steuerelemente können sicherstellen, dass nur bestimmte Zeichen in Benutzereingaben zulässig sind. Dadurch wird jedoch immer noch nicht verhindert, dass Benutzer ungültige Zeichen eingeben und das Formular übermitteln.

## <a name="overview"></a>Übersicht

ASP.net-Validierungs Steuerelemente können sicherstellen, dass nur bestimmte Zeichen in Benutzereingaben zulässig sind. Dadurch wird jedoch immer noch nicht verhindert, dass Benutzer ungültige Zeichen eingeben und das Formular übermitteln.

## <a name="steps"></a>Schritte

Das ASP.NET AJAX Control Toolkit enthält das `FilteredTextBox` Steuerelement, das ein Textfeld erweitert. Nach der Aktivierung kann nur ein bestimmter Satz von Zeichen in das Feld eingegeben werden.

Damit dies funktioniert, benötigen wir zuerst das ASP.NET AJAX-`ScriptManager`, das die JavaScript-Bibliotheken lädt, die auch vom ASP.NET AJAX Control Toolkit verwendet werden:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

Anschließend benötigen wir ein Textfeld:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

Zum Schluss übernimmt das `FilteredTextBoxExtender`-Steuerelement die Einschränkung der Zeichen, die der Benutzer eingeben darf. Legen Sie zunächst das `TargetControlID`-Attribut auf die `ID` des `TextBox`-Steuer Elements fest. Wählen Sie dann einen der verfügbaren `FilterType` Werte aus:

- Standard `Custom`; Sie müssen eine Liste gültiger Zeichen angeben.
- nur `LowercaseLetters` Kleinbuchstaben
- nur `Numbers` Ziffern
- nur `UppercaseLetters` Großbuchstaben

Wenn die `Custom FilterType` verwendet wird, muss die `ValidChars`-Eigenschaft festgelegt werden, und es muss eine Liste von Zeichen bereitgestellt werden, die eingegeben werden können. Übrigens: Wenn Sie versuchen, Text in das Textfeld einzufügen, werden alle ungültigen Zeichen entfernt.

Hier sehen Sie das Markup für das `FilteredTextBoxExtender` Steuerelement, das nur Ziffern zulässt (etwas, das auch mit `FilterType="Numbers"`möglich wäre):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

Führen Sie die Seite aus, und versuchen Sie, einen Buchstaben einzugeben, wenn JavaScript aktiviert ist. Ziffern werden jedoch auf der Seite angezeigt. Beachten Sie jedoch, dass der von `FilteredTextBox` bereitgestellte Schutz nicht Aufzählungs sicher ist: Wenn JavaScript aktiviert ist, können alle Daten in das Textfeld eingegeben werden, sodass Sie zusätzliche Validierungs Mittel, d. h. ASP, verwenden müssen. Die Validierungs Steuerelemente des Netzes.

[Es können ![nur Ziffern eingegeben werden.](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)

Es können nur Ziffern eingegeben werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Weiter](allowing-only-certain-characters-in-a-text-box-vb.md)
