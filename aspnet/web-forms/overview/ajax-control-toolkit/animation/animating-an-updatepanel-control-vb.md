---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animieren eines Update Panel-Steuer Elements (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Für den Inhalt eines...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d66dda923940a328c0757049c9d8bfa3b2d2b9fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431013"
---
# <a name="animating-an-updatepanel-control-vb"></a>Animieren eines UpdatePanel-Steuerelements (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Für den Inhalt eines Update Panel ist ein spezieller Extender vorhanden, der sich stark auf das Animations Framework stützt: UpdatePanelAnimation. In diesem Tutorial wird gezeigt, wie eine solche Animation für ein Update Panel eingerichtet wird.

## <a name="overview"></a>Übersicht

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Für den Inhalt einer `UpdatePanel`ist ein spezieller Extender vorhanden, der sich stark auf das Animations Framework stützt: `UpdatePanelAnimation`. In diesem Tutorial wird gezeigt, wie Sie eine solche Animation für eine `UpdatePanel`einrichten.

## <a name="steps"></a>Schritte

Der erste Schritt besteht darin, die `ScriptManager` auf der Seite einzuschließen, sodass die ASP.NET AJAX-Bibliothek geladen und das steuerungstooltoolkit verwendet werden kann:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

Die Animation in diesem Szenario wird auf ein ASP.net-`Wizard`-websteuer Element angewendet, das sich in einem `UpdatePanel`befindet. Drei (beliebige) Schritte bieten genügend Optionen, um Postbacks auszublenden:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

Das für das `UpdatePanelAnimationExtender` Steuerelement erforderliche Markup ähnelt dem Markup, das für die `AnimationExtender`verwendet wird. Im `TargetControlID`-Attribut werden die `ID` des zu animierenden `UpdatePanel` bereitgestellt. im `UpdatePanelAnimationExtender`-Steuerelement enthält das `<Animations>`-Element XML-Markup für die Animation (en). Es gibt jedoch einen Unterschied: die Anzahl von Ereignissen und Ereignis Handlern ist im Vergleich zu `AnimationExtender`beschränkt. Bei `UpdatePanels`sind nur zwei davon vorhanden:

- `<OnUpdated>`, wenn Update Panel aktualisiert wurde.
- `<OnUpdating>`, wenn Update Panel mit der Aktualisierung beginnt

In diesem Szenario sollte der neue Inhalt des `UpdatePanel` (nach dem Postback) ausgeblendet werden. Dies ist das erforderliche Markup hierfür:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

Wenn nun ein Postback innerhalb von Update Panel auftritt, wird der neue Inhalt des Bereichs reibungslos ausgeblendet.

[![der nächste Schritt des Assistenten wird ausgeblendet.](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

Der nächste Schritt des Assistenten wird ausgeblendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](animating-an-updatepanel-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](changing-an-animation-using-client-side-code-vb.md)
> [Weiter](dynamically-controlling-updatepanel-animations-vb.md)
