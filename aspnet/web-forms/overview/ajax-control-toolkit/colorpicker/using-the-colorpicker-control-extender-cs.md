---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: Verwenden des ColorPicker-SteuerelementC#-Extenders () | Microsoft-Dokumentation
author: microsoft
description: ColorPicker ist ein ASP.NET AJAX-Extender, der die Client seitige Funktion zur Farbauswahl für die Benutzeroberfläche in einem Popup-Steuerelement bereitstellt. Sie kann an beliebige ASP.net angefügt werden...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: ac510ab353878038c1c7a103bfbf6d32fb1b2686
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497619"
---
# <a name="using-the-colorpicker-control-extender-c"></a>Verwenden des ColorPicker-SteuerelementC#-Extenders ()

von [Microsoft](https://github.com/microsoft)

> ColorPicker ist ein ASP.NET AJAX-Extender, der die Client seitige Funktion zur Farbauswahl für die Benutzeroberfläche in einem Popup-Steuerelement bereitstellt. Es kann an jedes beliebige ASP.NET-TextBox-Steuerelement angefügt werden. Ihm.

In diesem Tutorial wird erläutert, wie Sie das AJAX Control Toolkit ColorPicker-Steuerelement-Extender verwenden können. Der Steuerelement-Extender von ColorPicker zeigt ein Popup Dialogfeld an, in dem Sie eine Farbe auswählen können. ColorPicker ist nützlich, wenn Sie eine intuitive Benutzeroberfläche für einen Benutzer bereitstellen möchten, um eine Farbe auszuwählen.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Erweitern eines TextBox-Steuer Elements mit dem ColorPicker-Steuerelement-Extender

Stellen Sie sich beispielsweise vor, dass Sie eine Website erstellen möchten, die es Besuchern ermöglicht, angepasste Visitenkarten zu erstellen. Besucher können den Text für eine Visitenkarte eingeben und die Farbe auswählen. Die ASP.NET-Seite in der Liste 1 enthält zwei TextBox-Steuerelemente mit den Namen txtcardtext und txtcardcolor. Wenn Sie das Formular senden, werden die ausgewählten Werte angezeigt (siehe Abbildung 1).

[![einfache Form zum Erstellen einer Visitenkarte](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)

**Abbildung 01**: einfaches Formular zum Erstellen einer Visitenkarte ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-the-colorpicker-control-extender-cs/_static/image2.png))

**Codebeispiel 1: "kreatecard. aspx"**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

Das Formular in der Liste 1 funktioniert, bietet aber keine hervorragend benutzerfreundliche Benutzerfunktion. Der Benutzer muss eine Farbe in das Textfeld eingeben. Wenn der Benutzer eine spezielle Farbe wünscht, z. b. nur den rechten Schatten der Erbsen grün, dann muss der Benutzer den HTML-Farbcode ohne Hilfe ermitteln.

Mit dem ColorPicker-Steuerelement-Extender können Sie eine bessere Benutzer Leistung erzielen. ColorPicker zeigt ein Farb Dialogfeld an, wenn Sie den Fokus auf ein TextBox-Steuerelement verschieben (siehe Abbildung 2).

[![des ColorPicker-Steuerelement-Extenders](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)

**Abbildung 02**: der Steuerelement-Extender von ColorPicker ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-the-colorpicker-control-extender-cs/_static/image4.png))

Sie müssen zwei Schritte ausführen, um den ColorPicker-Steuerelement-Extender mit dem Formular in der Liste 1 zu verwenden:

1. Hinzufügen eines ScriptManager-Steuer Elements zur Seite
2. Hinzufügen des ColorPicker-Steuerelement-Extenders zur Seite

Bevor Sie ColorPicker verwenden können, müssen Sie der Seite einen ScriptManager hinzufügen. Ein guter Ort zum Hinzufügen von ScriptManager befindet sich direkt unterhalb des öffnenden serverseitigen &lt;Formulars&gt;-Tags. Sie können den ScriptManager auf die Seite aus der Toolbox ziehen (der ScriptManager befindet sich auf der Registerkarte AJAX-Erweiterungen). Alternativ können Sie das folgende Tag in die Quell Ansicht unterhalb des öffnenden serverseitigen Formular Tags eingeben:

&lt;ASP: ScriptManager ID = "ScriptManager1" runat = "Server"/&gt;

Die einfachste Möglichkeit zum Hinzufügen des ColorPicker-Steuerelement-Extenders zur Seite finden Sie in der Entwurfs Ansicht. Wenn Sie mit der Maus auf das Textfeld "txtcardcolor" zeigen, wird eine Option für intelligente Aufgaben angezeigt, mit der Sie einen Extender hinzufügen können (siehe Abbildung 3). Wenn Sie diese Option auswählen, wird der extenderassistent angezeigt (siehe Abbildung 4).

[![Hinzufügen eines Extenders](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)

**Abbildung 03**: Hinzufügen eines Extenders ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-the-colorpicker-control-extender-cs/_static/image6.png))

[![auswählen eines steuerungextender mit dem Extender-Assistenten](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)

**Abbildung 04**: Auswählen eines Steuerelement-Extenders mit dem Extender-Assistenten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-the-colorpicker-control-extender-cs/_static/image8.png))

Sie können den ColorPicker-Extender auswählen, um das Textfeld "txtcardcolor" mit dem ColorPicker-Extender zu erweitern. Klicken Sie auf OK, um das Dialogfeld zu schließen.

Nachdem Sie diese Änderungen vorgenommen haben, sieht die Quelle für die Seite wie in der Liste 2 aus.

Codebeispiel 2: "kreatecard. aspx" (mit colorpicker)

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

Beachten Sie, dass die Seite jetzt ein colorpickerextender-Steuerelement enthält, das direkt unterhalb des TextBox-Steuer Elements txtcardcolor angezeigt wird. Das colorpickerextender-Steuerelement erweitert das txtcardcolor-Steuerelement, sodass es ein Farbauswahl Dialogfeld anzeigt.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Verwenden einer Schaltfläche zum Starten des Farbauswahl Dialogfelds

Der ColorPicker-Extender unterstützt die folgenden Eigenschaften:

- PopupButtonID: die ID einer Schaltfläche auf der Seite, die bewirkt, dass das Dialogfeld für die Farbauswahl angezeigt wird.
- PopupPosition-die Position relativ zum Ziel Steuerelement des Farbauswahl Dialogfelds. Mögliche Werte sind absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right und Left (der Standardwert ist BottomLeft).
- Samplecontrolid: die ID eines Steuer Elements, das die ausgewählte Farbe anzeigt.
- SelectedColor: die anfängliche Farbe, die von ColorPicker ausgewählt wird.

Sie können diese Eigenschaften verwenden, um anzupassen, wie das Farbauswahl Dialogfeld angezeigt wird und wie die ausgewählte Farbe angezeigt wird. Die Seite in der Liste 3 zeigt, wie Sie mehrere dieser Eigenschaften verwenden können.

**Codebeispiel 3: "kreatecardbutton. aspx"**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

Die Seite in der Liste 3 enthält eine Schaltfläche zum Auswählen von Farben (siehe Abbildung 5). Wenn Sie auf diese Schaltfläche klicken, wird das Dialogfeld Farbauswahl oberhalb des Textfelds angezeigt. Wenn Sie eine Farbe aus dem Dialogfeld auswählen, wird die ausgewählte Farbe als Hintergrundfarbe des Steuer Elements lblsample Label angezeigt.

Die Eigenschaft "ColorPicker PopupButtonID" wird verwendet, um die Schaltfläche "Pick Color" dem ColorPicker-Extender zuzuordnen. Wenn Sie einen Wert für die PopupButtonID-Eigenschaft angeben, wird das Farbauswahl Dialogfeld nicht mehr angezeigt, wenn das Ziel Steuerelement den Fokus besitzt. Sie müssen auf die Schaltfläche klicken, um das Dialogfeld anzuzeigen.

Die samplecontrolid-Eigenschaft wird verwendet, um ein Steuerelement, das die ausgewählte Farbe anzeigt, dem ColorPicker zuzuordnen. ColorPicker ändert die Hintergrundfarbe dieses Steuer Elements in die aktuell ausgewählte Farbe.

[![Anzeigen des Farbauswahl Dialogfelds mit einer Schaltfläche](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)

**Abbildung 05**: Anzeigen des Farbauswahl Dialogfelds mit einer Schaltfläche ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-the-colorpicker-control-extender-cs/_static/image10.png))

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie Sie mit dem ColorPicker-Steuerelement-Extender ein Popup Farbauswahl-Dialogfeld anzeigen. Zunächst wurde untersucht, wie Sie das Dialogfeld anzeigen können, wenn der Fokus in ein TextBox-Steuerelement verschoben wird. Als nächstes haben Sie gelernt, wie Sie eine Schaltfläche erstellen, die beim Klicken auf die Schaltfläche das Farbauswahl Dialogfeld anzeigt.

> [!div class="step-by-step"]
> [Weiter](using-the-colorpicker-control-extender-vb.md)
