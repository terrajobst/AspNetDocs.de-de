---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: Beginnen Sie mit dem AJAX Control Toolkit (VB) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie alles, was Sie wissen müssen, um mit dem AJAX Control Toolkit zu beginnen.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: ff614308555b31710b11f408e12e9a6fadbf98d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497139"
---
# <a name="get-started-with-the-ajax-control-toolkit-vb"></a>Erste Schritte mit dem AJAX Control Toolkit (VB)

von [Microsoft](https://github.com/microsoft)

> Erfahren Sie alles, was Sie wissen müssen, um mit dem AJAX Control Toolkit zu beginnen.

Das AJAX Control Toolkit enthält mehr als 30 kostenlose Steuerelemente, die Sie in Ihren ASP.NET-Anwendungen verwenden können. In diesem Tutorial erfahren Sie, wie Sie das AJAX Control Toolkit herunterladen und die Toolkit-Steuerelemente zu Ihrer Visual Studio-/Visual Web Developer Express-Toolbox hinzufügen.

## <a name="downloading-the-ajax-control-toolkit"></a>Herunterladen des AJAX Control Toolkit

Das [AJAX Control Toolkit](http://devexpress.com/act) ist ein Open Source-Projekt, das von den Mitgliedern der ASP.net-Community und des ASP.NET-Teams entwickelt wurde.

[![das AJAX Control Toolkit herunterladen](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)

**Abbildung 01**: Herunterladen des AJAX Control Toolkit ([Klicken Sie, um das Bild in voller Größe anzuzeigen](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))

Nachdem Sie die Datei heruntergeladen haben, müssen Sie die Blockierung der Datei entsperren. Klicken Sie mit der rechten Maustaste auf die Datei, wählen Sie Eigenschaften aus, und klicken Sie **auf die Schaltfläche entsperren (** siehe Abbildung 2).

[Aufheben der Blockierung der ZIP-Datei für das AJAX Control Toolkit ![](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)

**Abbildung 02**: Aufheben der Blockierung der AJAX Control Toolkit-ZIP-Datei ([Klicken Sie, um das Bild in voller Größe anzuzeigen](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))

Nach dem Entsperren der Datei können Sie die Datei entpacken: Klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie die Menüoption **Alle extrahieren** aus. Nun können wir das Toolkit der Visual Studio-/Visual Web Developer-Toolbox hinzufügen.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Hinzufügen des AJAX Control Toolkit zur Toolbox

Die einfachste Möglichkeit, das AJAX Control Toolkit zu verwenden, besteht darin, das Toolkit Ihrer Visual Studio-/Visual Web Developer-Toolbox hinzuzufügen (siehe Abbildung 3). Auf diese Weise können Sie einfach ein Toolkit-Steuerelement auf eine Seite ziehen, wenn Sie es verwenden möchten.

[![AJAX Control Toolkit wird in Toolbox angezeigt.](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)

**Abbildung 03**: AJAX Control Toolkit wird in der Toolbox angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))

Zunächst müssen Sie der Toolbox eine Registerkarte des AJAX-Steuerelement-Toolkits hinzufügen. Führen Sie folgende Schritte durch:

1. Erstellen Sie eine neue ASP.NET-Website, indem Sie die Menü Optionsdatei, neue Website auswählen. Doppelklicken Sie im Fenster Projektmappen-Explorer auf die Datei default. aspx, um die Datei im Editor zu öffnen.
2. Klicken Sie mit der rechten Maustaste unterhalb der Registerkarte Allgemein auf die Toolbox, und wählen Sie die Menüoption **Registerkarte hinzufügen** (siehe Abbildung 4)
3. Geben Sie eine neue Registerkarte mit dem Namen AJAX Control Toolkit ein.

[![Hinzufügen einer neuen Registerkarte](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)

**Abbildung 04**: Hinzufügen einer neuen Registerkarte ([Klicken Sie, um das Bild in voller Größe anzuzeigen](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))

Als nächstes müssen Sie die AJAX Control Toolkit-Steuerelemente zur neuen Registerkarte hinzufügen. führen Sie die folgenden Schritte aus:

- Klicken Sie mit der rechten Maustaste unterhalb der Registerkarte AJAX Control Toolkit, und wählen Sie die Menüoption **Elemente auswählen (siehe Abbildung 5)** .
- Navigieren Sie zu dem Speicherort, an dem Sie das AJAX Control Toolkit entzippt haben, und wählen Sie die Assembly "AjaxControlToolkit. dll"

[![Elemente auswählen, die der Toolbox hinzugefügt werden sollen.](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)

**Abbildung 05**: Auswählen der Elemente, die der Toolbox hinzugefügt werden sollen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))

Nachdem Sie diese Schritte ausgeführt haben, werden alle Toolkit-Steuerelemente in der Toolbox angezeigt.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Aktualisieren auf eine neue Version des Toolkits

Wenn Sie eine ältere Version des Toolkits verwendet haben und jetzt zu einer späteren Version wechseln müssen, werden die empfohlenen Schritte ausgeführt:

- Binärdateien: Löschen Sie die alte Version der Datei "AjaxControlToolkit. dll" aus dem Ordner "bin" der Website.
- Toolbox Items-löschen Sie die Registerkarte "AJAX Control Toolkit", und befolgen Sie die obigen Schritte, um die Registerkarte mit der neuen Version der "AjaxControlToolkit. dll"-Assembly neu zu erstellen.

> [!div class="step-by-step"]
> [Zurück](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [Weiter](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
