---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: Dynamisches Steuern von Update Panel-Animationen (C#) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Für den Inhalt eines...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 183974564764aab9c0d8a4e577995f3c444bf2d3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606788"
---
# <a name="dynamically-controlling-updatepanel-animations-c"></a>Dynamisches Steuern von UpdatePanel-Animationen (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Für den Inhalt eines Update Panel ist ein spezieller Extender vorhanden, der sich stark auf das Animations Framework stützt: UpdatePanelAnimation. Es kann auch zusammen mit Update Panel-Triggern verwendet werden.

## <a name="overview"></a>Übersicht über

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Für den Inhalt einer `UpdatePanel`ist ein spezieller Extender vorhanden, der sich stark auf das Animations Framework stützt: `UpdatePanelAnimation`. Sie kann auch mit `UpdatePanel` Triggern zusammenarbeiten.

## <a name="steps"></a>Schritte

Der erste Schritt besteht darin, die `ScriptManager` auf der Seite einzuschließen, sodass die ASP.NET AJAX-Bibliothek geladen und das steuerungstooltoolkit verwendet werden kann:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

Die Animation in diesem Szenario wird auf eine Anzeige der aktuellen Zeit angewendet. Diese Informationen können mit der `Page_Load()`-Methode in eine Bezeichnung geschrieben werden, oder (aus Gründen der Einfachheit) der folgende Inline Code wird verwendet:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

Außerdem wird eine Schaltfläche zum Aktualisieren der Zeit erstellt:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

Dieser Code wird dann in den `<ContentTemplate>` Abschnitt eines `UpdatePanel` Elements eingefügt. Das `UpdateMode`-Attribut des Bereichs muss auf `"Conditional"`festgelegt werden, da nur Trigger den Inhalt des Panels aktualisieren können. Im Abschnitt `<Triggers>` der `UpdatePanel`wird ein asynchroner Postback-ausgelöst, der mit dem `Click`-Ereignis der Schaltfläche verknüpft ist. Wenn der Benutzer auf die Schaltfläche klickt, wird der `UpdatePanel` aktualisiert. Hier sehen Sie das Markup für das `UpdatePanel`-Steuerelement:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

Zum Schluss muss der `UpdatePanelAnimationExtender` konfiguriert werden: Legen Sie das `TargetControlID`-Attribut auf die ID des Panels fest, und definieren Sie eine Animation innerhalb des Extenders. Das Ausblenden in ist sinnvoll, wodurch eine schöne visuelle Betonung der aktualisierten Zeit entsteht. Das Extendermarkup kann dann wie folgt aussehen:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

Führen Sie die Datei im Browser aus. Wenn Sie auf die Schaltfläche klicken, wird die aktuelle Uhrzeit im Panel angezeigt, während die Dauer von einer Sekunde immer ausgeblendet wird.

[![die aktuelle Zeit in](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)

Die aktuelle Zeit wird ausgeblendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](animating-an-updatepanel-control-cs.md)
> [Weiter](adding-animation-to-a-control-vb.md)
