---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: Verwenden von AJAX Control Toolkit-Steuerelementen und Steuerelement-Extender (VB) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie Sie Ihren ASP.NET Seiten AJAX Control Toolkit-Steuerelemente und-Erweiterungen hinzufügen.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 90a6003ff50ba6e85196c25cf175e057810f0f84
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497049"
---
# <a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a>Verwenden von AJAX Control Toolkit-Steuerelementen und -Steuerelement-Extendern (VB)

von [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie Sie Ihren ASP.NET Seiten AJAX Control Toolkit-Steuerelemente und-Erweiterungen hinzufügen.

Das AJAX Control Toolkit enthält eine Reihe von Steuerelementen und Steuerelement Erweiterungen. In diesem kurzen Tutorial erfahren Sie, wie Sie einer ASP.NET Seite sowohl Steuerelemente als auch Steuerelement Erweiterungen hinzufügen.

> [!NOTE] 
> 
> Anweisungen zum Installieren des AJAX Control Toolkit und zum Hinzufügen des AJAX Control Toolkit zur Visual Studio-/Visual Web Developer-Toolbox finden Sie im Tutorial ersten [Einstieg in das AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).

## <a name="using-ajax-control-toolkit-controls"></a>Verwenden von AJAX Control Toolkit-Steuerelementen

Ein AJAX Control Toolkit-Steuerelement funktioniert genau wie ein normales ASP.NET-Steuerelement. Sie können das Steuerelement aus der Toolbox auf eine ASP.NET Seite ziehen. Sie können das Steuerelement der Seite entweder in Designansicht oder in der Quell Ansicht hinzufügen.

Es gibt eine besondere Anforderung, wenn die Steuerelemente aus dem AJAX Control Toolkit verwendet werden. Die Seite muss ein ScriptManager-Steuerelement enthalten. Das ScriptManager-Steuerelement ist dafür verantwortlich, das gesamte erforderliche JavaScript zu einschließen, das von den AJAX Control Toolkit-Steuerelementen benötigt wird.

Beispielsweise enthält die Registerkarte AJAX Control Toolkit ein Steuerelement mit dem Namen Editor-Steuerelement. Dieses Steuerelement zeigt einen Rich HTML-Editor an. Führen Sie die folgenden Schritte aus, um das Editor-Steuerelement einer Seite hinzuzufügen:

1. Erstellen Sie eine neue ASP.NET-Seite mit dem Namen Showeditor. aspx.
2. Wählen Sie in der Toolbox unter der Registerkarte AJAX-Erweiterungen das ScriptManager-Steuerelement aus, und ziehen Sie das Steuerelement auf die Seite.
3. Wählen Sie in der Toolbox unter der Registerkarte AJAX-Steuerelement-Toolkit das Steuerelement Editor aus, und ziehen Sie das Steuerelement auf die Seite (siehe Abbildung 1). Der Designer sollte wie in Abbildung 2 aussehen.
4. Führen Sie die Website aus, indem Sie die Menüoption **Debuggen, Debuggen starten** oder F5 drücken.
5. Die Seite in Abbildung 3 sollte angezeigt werden.

[![auswählen des HTML-Editor-Steuer Elements](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)

**Abbildung 01**: Auswählen des HTML-Editor-Steuer Elements ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))

[![von Visual Studio Designer mit ScriptManager und Bearbeitungs Steuerelement](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)

**Abbildung 02**: Visual Studio-Designer mit ScriptManager und Bearbeitungs Steuerelement ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))

[![der Seite "displayeditor. aspx"](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)

**Abbildung 03**: die Seite "displayeditor. aspx" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))

## <a name="using-ajax-control-toolkit-control-extenders"></a>Verwenden von AJAX Control Toolkit-Steuerelement Erweiterungen

Das AJAX Control Toolkit enthält auch Steuerelement Erweiterungen. Wie der Name bereits vermuten lässt, erweitert ein Steuerelement-Extender die Funktionalität eines vorhandenen Steuer Elements. Der Steuerelement-Extender von ConfirmButton erweitert z. b. das Standard-ASP.NET Button-Steuerelement. Der Extender ändert das Verhalten des Schaltflächen-Steuer Elements, sodass die Schaltfläche ein Bestätigungs Dialogfeld anzeigt, wenn Sie darauf klicken.

Ein Steuerelement-Extender, wie ein AJAX Control Toolkit-Steuerelement, erfordert ein ScriptManager-Steuerelement. Sie müssen ein ScriptManager-Steuerelement zu einer Seite hinzufügen, bevor Sie mit der Verwendung von Steuerelement Erweiterungen auf der Seite beginnen.

Führen Sie diese Schritte aus, um den ConfirmButton-Steuerelement-Extender zu verwenden

1. Erstellen einer neuen ASP.NET-Seite mit dem Namen "showconfirmbutton. aspx"
2. Fügen Sie der Seite ein ScriptManager-Steuerelement hinzu, indem Sie das Steuerelement von unterhalb der Registerkarte AJAX-Erweiterungen auf die Seite ziehen.
3. Fügen Sie der Seite ein Standard-Schaltflächen-Steuerelement hinzu, indem Sie die Schaltfläche von unterhalb der Registerkarte Standard in der Toolbox auf die Designer Oberfläche ziehen.
4. Klicken Sie auf die Option **extenderaufgabe hinzufügen** (siehe Abbildung 4).
5. Wählen Sie im Dialogfeld Extender auswählen die Option ConfirmButtonExtender (siehe Abbildung 5) aus, und klicken Sie auf die Schaltfläche OK.
6. Wählen Sie im Designer das Schaltflächen-Steuerelement aus, und erweitern Sie den Knoten Extenders, Button1\_ConfirmButtonExtender in der Eigenschaftenfenster (siehe Abbildung 6). Weisen Sie den Wert *wirklich* der ConfirmText-Eigenschaft zu.
7. Führen Sie die Seite aus, indem Sie die Menüoption **Debuggen, Debuggen starten** oder die Taste F5 drücken.

[![der Option "extenderaufgabe hinzufügen"](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)

**Abbildung 04**: die Option "extenderaufgabe hinzufügen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))

[![auswählen des ConfirmButton-Steuerelement-Extenders](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)

**Abbildung 05**: Auswählen des ConfirmButton-Steuerelement-Extenders ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))

[![Festlegen einer ConfirmButton-Eigenschaft](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)

**Abbildung 06**: Festlegen einer ConfirmButton-Eigenschaft ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))

Wenn die Seite geöffnet wird, sollte eine Schaltfläche angezeigt werden. Wenn Sie auf die Schaltfläche klicken, wird das Bestätigungs Dialogfeld in Abbildung 7 angezeigt.

[![Anzeigen des Bestätigungs Dialogfelds](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)

**Abbildung 07**: Anzeigen des Bestätigungs Dialogfelds ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))

Beachten Sie, dass Sie einen Steuerelement-Extender normalerweise nicht auf eine Seite ziehen. Stattdessen verwenden Sie die Option **Extender-Aufgabe hinzufügen** , um einem Steuerelement, das Sie bereits einer Seite hinzugefügt haben, einen Extender hinzuzufügen. Beachten Sie, dass Sie die Steuerelement-Extendereigenschaften festlegen, indem Sie das Eigenschaften Blatt für das erweiterte Steuerelement öffnen.

Ein einzelnes ASP.NET-Steuerelement kann von mehreren Steuerelement Erweiterungen erweitert werden. Auf dem Eigenschaften Blatt für das Steuerelement, das erweitert wird, werden alle Steuerelement Erweiterungen aufgelistet, die dem Steuerelement zugeordnet sind.

> [!div class="step-by-step"]
> [Zurück](get-started-with-the-ajax-control-toolkit-vb.md)
> [Weiter](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)
