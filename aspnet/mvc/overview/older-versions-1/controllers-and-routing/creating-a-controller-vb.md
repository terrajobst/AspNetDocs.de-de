---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Erstellen eines Controllers (VB) | Microsoft-Dokumentation
author: StephenWalther
description: In diesem Tutorial demonstriert Stephen Walther, wie Sie einen Controller zu einer ASP.NET MVC-Anwendung hinzufügen können.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: 60636b79ab5fc06ca904dee90ce74f256e046d12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486831"
---
# <a name="creating-a-controller-vb"></a>Erstellen eines Controllers (VB)

von [Stephen Walther](https://github.com/StephenWalther)

> In diesem Tutorial demonstriert Stephen Walther, wie Sie einen Controller zu einer ASP.NET MVC-Anwendung hinzufügen können.

In diesem Tutorial wird erläutert, wie Sie neue ASP.NET MVC-Controller erstellen. Sie erfahren, wie Sie mithilfe der Menüoption Controller Hinzufügen von Visual Studio Controller und durch manuelles Erstellen einer Klassendatei Controller erstellen.

### <a name="using-the-add-controller-menu-option"></a>Verwenden der Menü Option "Controller hinzufügen"

Am einfachsten können Sie einen neuen Controller erstellen, indem Sie im Fenster Visual Studio Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controllers klicken und die Menüoption **hinzufügen, Controller** auswählen (siehe Abbildung 1). Wenn Sie diese Menüoption auswählen, wird das Dialogfeld **Controller hinzufügen** geöffnet (siehe Abbildung 2).

[![des Dialog Felds "Neues Projekt"](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Abbildung 01**: Hinzufügen eines neuen Controllers ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-controller-vb/_static/image2.png))

[![des Dialog Felds "Neues Projekt"](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Abbildung 02**: Dialogfeld "Controller hinzufügen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-controller-vb/_static/image4.png))

Beachten Sie, dass der erste Teil des Controller namens im Dialogfeld " **Controller hinzufügen** " hervorgehoben ist. Jeder Controller Name muss mit dem Suffix *Controller*enden. Beispielsweise können Sie einen Controller namens *ProductController* erstellen, aber keinen Controller mit dem Namen *Product*.

Wenn Sie einen Controller erstellen, dem das *Controller* Suffix fehlt, können Sie den Controller nicht aufrufen. Dies ist nicht der Fall: Ich habe nach diesem Fehler unzählige Stunden meines Lebenszyklus verschwendet.

**Codebeispiel 1-controllers\productcontroller.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

Sie sollten immer Controller im Ordner Controller erstellen. Andernfalls verletzen Sie die Konventionen von ASP.NET MVC, und andere Entwickler haben eine schwierigere Zeit, Ihre Anwendung zu verstehen.

### <a name="scaffolding-action-methods"></a>Gerüst Aktionsmethoden

Wenn Sie einen Controller erstellen, haben Sie die Möglichkeit, automatisch Create-, Update-und Details-Aktionsmethoden zu generieren (siehe Abbildung 3). Wenn Sie diese Option auswählen, wird die Controller Klasse in der Liste 2 generiert.

[Automatisches Erstellen von Aktionsmethoden ![](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Abbildung 03**: Automatisches Erstellen von Aktionsmethoden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-controller-vb/_static/image6.png))

**Codebeispiel 2: controllers\customercontroller.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Diese generierten Methoden sind Stub-Methoden. Sie müssen die eigentliche Logik zum Erstellen, aktualisieren und Anzeigen von Details für einen Kunden hinzufügen. Die Stub-Methoden bieten Ihnen aber einen guten Ausgangspunkt.

### <a name="creating-a-controller-class"></a>Erstellen einer Controller Klasse

Der ASP.NET MVC-Controller ist nur eine Klasse. Wenn Sie möchten, können Sie das bequeme Visual Studio-Controller Gerüst ignorieren und eine Controller Klasse per Hand erstellen. Folgen Sie diesen Schritten:

1. Klicken Sie mit der rechten Maustaste auf den Ordner Controllers, und wählen Sie die Menüoption **hinzufügen, neues Element** und dann die **Klassen** Vorlage aus (siehe Abbildung 4).
2. Nennen Sie die neue Klasse personcontroller. vb, und klicken Sie auf die Schaltfläche **Hinzufügen** .
3. Ändern Sie die resultierende Klassendatei so, dass die Klasse von der System. Web. MVC. Controller-Basisklasse erbt (siehe Codebeispiel 3).

[![Erstellen einer neuen Klasse](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Abbildung 04**: Erstellen einer neuen Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-controller-vb/_static/image8.png))

**Codebeispiel 3-controllers\personcontroller.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

Der Controller in der Liste 3 macht eine Aktion mit dem Namen Index () verfügbar, die die Zeichenfolge "Hallo Welt!" zurückgibt. Sie können diese Controller Aktion aufrufen, indem Sie die Anwendung ausführen und eine URL wie die folgende anfordern:

`http://localhost:40071/Person`

> [!NOTE]
> 
> Der ASP.NET Development Server verwendet eine zufällige Portnummer (z. b. 40071). Wenn Sie eine URL eingeben, um einen Controller aufzurufen, müssen Sie die richtige Portnummer angeben. Sie können die Portnummer ermitteln, indem Sie mit der Maus auf das Symbol für das ASP.NET Development Server im Infobereich von Windows (unten rechts auf dem Bildschirm) zeigen.
> 
> [!div class="step-by-step"]
> [Zurück](adding-dynamic-content-to-a-cached-page-vb.md)
> [Weiter](creating-an-action-vb.md)
