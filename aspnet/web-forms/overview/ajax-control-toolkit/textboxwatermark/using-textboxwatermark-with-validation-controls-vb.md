---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Verwenden von textboxwatermark mit Validierungs Steuerelementen (VB) | Microsoft-Dokumentation
author: wenz
description: Mit dem textboxwatermark-Steuerelement im AJAX Control Toolkit wird ein Textfeld erweitert, sodass ein Text im Feld angezeigt wird. Wenn ein Benutzer in das Feld klickt,
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 141cae26c9e52be510e2a5a8f816cbac977dcf3d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78508749"
---
# <a name="using-textboxwatermark-with-validation-controls-vb"></a>Verwenden von TextBoxWatermark mit Validierungssteuerelementen (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)

> Mit dem textboxwatermark-Steuerelement im AJAX Control Toolkit wird ein Textfeld erweitert, sodass ein Text im Feld angezeigt wird. Wenn ein Benutzer auf das Feld klickt, wird es geleert. Wenn der Benutzer das Kontrollkästchen verlässt, ohne Text einzugeben, wird der vorab gefüllte Text erneut angezeigt. Dies kann mit ASP.net-Validierungs Steuerelementen auf derselben Seite kollidieren, diese Probleme können jedoch möglicherweise behoben werden.

## <a name="overview"></a>Übersicht

Mit dem `TextBoxWatermark`-Steuerelement im AJAX Control Toolkit wird ein Textfeld erweitert, sodass ein Text im Feld angezeigt wird. Wenn ein Benutzer auf das Feld klickt, wird es geleert. Wenn der Benutzer das Kontrollkästchen verlässt, ohne Text einzugeben, wird der vorab gefüllte Text erneut angezeigt. Dies kann mit ASP.net-Validierungs Steuerelementen auf derselben Seite kollidieren, diese Probleme können jedoch möglicherweise behoben werden.

## <a name="steps"></a>Schritte

Die grundlegende Einrichtung des Beispiels sieht folgendermaßen aus: ein `TextBox` Steuerelement wird mithilfe eines `TextBoxWatermarkExtender` Steuer Elements als Wasserzeichen gekennzeichnet. Eine Schaltfläche löst ein Postback aus und wird später verwendet, um die Validierungs Steuerelemente auf der Seite zu lösen. Außerdem ist ein `ScriptManager`-Steuerelement erforderlich, um ASP.NET AJAX zu initialisieren:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

Fügen Sie nun ein `RequiredFieldValidator` Steuerelement hinzu, mit dem überprüft wird, ob im Feld Text vorhanden ist, wenn das Formular übermittelt wird. Die `InitialValue`-Eigenschaft des Validierungs Steuer Elements muss auf denselben Wert festgelegt werden, der im `TextBoxWatermarkExtender`-Steuerelement verwendet wird: Wenn das Formular übermittelt wird, ist der Wert eines unverändert Textfelds der Wasserzeichen Wert.

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

Bei diesem Ansatz gibt es jedoch ein Problem: Wenn der Client JavaScript deaktiviert, wird das Textfeld nicht mit dem Wasserzeichen Text vorab ausgefüllt, daher löst das `RequiredFieldValidator` keine Fehlermeldung aus. Aus diesem Grund ist ein zweites `RequiredFieldValidator`-Steuerelement erforderlich, das auf ein leeres Textfeld prüft (das `InitialValue`-Attribut wird weggelassen).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

Da beide Validierungs Steuerelemente `Display`=`"Dynamic"`verwenden, kann der Endbenutzer nicht von der visuellen Darstellung unterscheiden, welche der beiden Validierungs Steuerelemente ausgelöst wurde. Stattdessen sieht es so aus, als ob nur einer davon vorhanden wäre.

Fügen Sie schließlich einigen serverseitigen Code hinzu, um den Text im Feld auszugeben, wenn kein Validierungs Steuerelement eine Fehlermeldung ausgegeben hat:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]

[![überprüft das Validierungs Steuerelement, dass kein Text im Feld vorhanden ist.](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)

Das Validierungs Steuerelement meldet, dass es keinen Text im Feld gibt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-textboxwatermark-with-validation-controls-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Previous](using-textboxwatermark-in-a-formview-vb.md)
