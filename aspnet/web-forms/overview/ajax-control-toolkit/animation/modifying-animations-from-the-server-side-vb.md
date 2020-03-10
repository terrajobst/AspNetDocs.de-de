---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Ändern von Animationen von der Server Seite (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animationen können auch...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: ebc311d1a931ad611d9556799c94440d41a9cf49
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78483867"
---
# <a name="modifying-animations-from-the-server-side-vb"></a>Ändern von Animationen von der Server Seite (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animationen können auch auf Serverseite geändert werden.

## <a name="overview"></a>Übersicht

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animationen können auch auf Serverseite geändert werden.

## <a name="steps"></a>Schritte

Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

Der Rest des Codes wird auf Serverseite ausgeführt und verwendet kein Markup. Stattdessen wird Code verwendet, um das `AnimationExtender` Steuerelement zu erstellen:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Das steuerungstooltoolkit bietet derzeit jedoch keinen API-Zugriff zum Erstellen der einzelnen Animationen. Es ist jedoch möglich, die Animation-Eigenschaft des `AnimationExtender`auf eine Zeichenfolge festzulegen, die das XML-Markup enthält, das beim deklarativen Zuweisen der Animationen verwendet wird. Um den XML-Code zu erstellen, der das `<Animations>`-Element nicht enthalten darf, können Sie die XML-Unterstützung des .NET Framework verwenden oder, wie im folgenden Code, einfach die Zeichenfolge angeben:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Fügen Sie abschließend der aktuellen Seite im `<form runat="server">`-Element das `AnimationExtender`-Steuerelement hinzu, und stellen Sie sicher, dass die Animation eingeschlossen ist und ausgeführt wird:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]

[![die Animation mit Server seitigem C#/VB-Code erstellt wird](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

Die Animation wird mithilfe serverseitiger C#/VB-Code erstellt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](modifying-animations-from-the-server-side-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](triggering-an-animation-in-another-control-vb.md)
> [Weiter](executing-animations-using-client-side-code-vb.md)
