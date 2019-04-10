---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: Erste Schritte mit dem AJAX Control Toolkit (VB) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, Sie müssen wissen, erste Schritte mit dem AJAX Control Toolkit.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 0b00fd5dc12c21183ef61d7ebb23211a1aa4719e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59418962"
---
# <a name="get-started-with-the-ajax-control-toolkit-vb"></a>Erste Schritte mit dem AJAX Control Toolkit (VB)

by [Microsoft](https://github.com/microsoft)

> Erfahren Sie, Sie müssen wissen, erste Schritte mit dem AJAX Control Toolkit.


Das AJAX Control Toolkit enthält mehr als 30 kostenlosen Steuerelemente, die Sie in Ihren ASP.NET-Anwendungen verwenden können. In diesem Tutorial erfahren Sie, wie das AJAX Control Toolkit herunterladen und Ihre Toolbox Visual Studio/Visual Web Developer Express Toolkit-Steuerelementen hinzufügen.

## <a name="downloading-the-ajax-control-toolkit"></a>Das AJAX Control Toolkit herunterladen

Die [AJAX Control Toolkit](http://devexpress.com/act) ist ein open-Source-Projekt entwickelt wurde, durch die Mitglieder der ASP.NET-Community und das ASP.NET-Team.


[![DOwnloading AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)

**Abbildung 01**: Das AJAX Control Toolkit herunterladen ([klicken Sie, um das Bild in voller Größe anzeigen](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))


Nachdem Sie die Datei heruntergeladen haben, müssen Sie die Datei zu entsperren. Mit der rechten Maustaste in der das, wählen Sie Eigenschaften und klicken Sie auf die **Unblock** Schaltfläche (siehe Abbildung 2).


[![UNblocking das AJAX Control Toolkit-ZIP-Datei](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)

**Abbildung 02**: Aufhebung der Blockierung der ZIP-AJAX Control Toolkit-Datei ([klicken Sie, um das Bild in voller Größe anzeigen](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))


Nachdem Sie die Blockierung der Datei aufzuheben, können Sie die Datei zu entpacken: Mit der rechten Maustaste in der das, und wählen Sie die **alle extrahieren** Option des Menüs. Jetzt können wir das Toolkit zur Visual Studio/Visual Web Developer Toolbox hinzufügen.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Hinzufügen von AJAX Control Toolkit zur Toolbox

Die einfachste Möglichkeit zum Verwenden von AJAX Control Toolkit wird der Toolbox von Visual Studio/Visual Web Developer das Toolkit hinzugefügt (siehe Abbildung 3). Auf diese Weise können Sie einfach ziehen ein Toolkit-Steuerelement auf eine Seite, wenn Sie sie verwenden möchten.


[![AJAX-Steuerelement-Toolkit wird in Toolbox angezeigt werden](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)

**Abbildung 03**: AJAX Control Toolkit wird in Toolbox angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))


Zunächst müssen Sie eine AJAX Control Toolkit-Registerkarte der Toolbox hinzu. Gehen Sie wie folgt vor.

1. Erstellen Sie eine neue ASP.NET-Website durch Auswählen der Menüoption Datei neue Website. Doppelklicken Sie auf der "default.aspx" im Projektmappen-Explorer-Fenster, um die Datei im Editor zu öffnen.
2. Mit der rechten Maustaste in der Toolbox unter der Registerkarte "Allgemein", und wählen Sie die Menüoption **Registerkarte hinzufügen** (siehe Abbildung 4).
3. Geben Sie eine neue Registerkarte mit dem Namen AJAX Control Toolkit.


[![AHinzufügen einer neuen Registerkarte](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)

**Abbildung 04**: Hinzufügen einer neuen Registerkarte ([klicken Sie, um das Bild in voller Größe anzeigen](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))


Als Nächstes müssen Sie die neue Registerkarte dem AJAX Control Toolkit-Steuerelemente hinzufügen. Führen Sie folgende Schritte aus:

- Mit der rechten Maustaste unterhalb der Registerkarte "AJAX Control Toolkit", und wählen Sie die Menüoption **"Elemente auswählen" (siehe Abbildung 5)**.
- Navigieren Sie zum Speicherort, in dem Sie das AJAX Control Toolkit entpackt und wählen Sie die Assembly AjaxControlToolkit.dll.


[![CWählen Sie aus den Elementen zur Toolbox hinzufügen](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)

**Abbildung 05**: Wählen Sie Elemente aus der Toolbox hinzuzufügende ([klicken Sie, um das Bild in voller Größe anzeigen](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))


Nachdem Sie diese Schritte abgeschlossen haben, werden sämtliche Toolkit-Steuerelementen in der Toolbox angezeigt.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Ein Upgrade auf eine neue Version des Toolkits

Wenn Sie eine ältere Version des Toolkits verwenden würde, und nun verschieben, müssen hier eine höhere Version sind die empfohlenen Schritte:

- Binärdateien - löschen die alte Version der Assembly AjaxControlToolkit.dll aus Ihrem Ordner "Website" Bin ".
- Toolboxelemente - löschen die Registerkarte "AJAX Control Toolkit" und führen Sie die Schritte aus, um die Registerkarte mit der neuen Version der Assembly AjaxControlToolkit.dll neu zu erstellen.

> [!div class="step-by-step"]
> [Zurück](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [Weiter](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
