---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Erstellen einer Routen Einschränkung (C#) | Microsoft-Dokumentation
author: StephenWalther
description: In diesem Tutorial demonstriert Stephen Walther, wie Sie steuern können, wie Browser Anforderungen Routen entsprechen, indem Sie Routen Einschränkungen mit regulären Ausdrücken erstellen.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 51ef859287b3424faf85f4a3606a220ab48a9466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78470163"
---
# <a name="creating-a-route-constraint-c"></a>Erstellen einer Routeneinschränkung (C#)

von [Stephen Walther](https://github.com/StephenWalther)

> In diesem Tutorial demonstriert Stephen Walther, wie Sie steuern können, wie Browser Anforderungen Routen entsprechen, indem Sie Routen Einschränkungen mit regulären Ausdrücken erstellen.

Sie verwenden Routen Einschränkungen, um die Browser Anforderungen zu beschränken, die einer bestimmten Route entsprechen. Sie können einen regulären Ausdruck verwenden, um eine Routen Einschränkung anzugeben.

Angenommen, Sie haben die Route in der Datei "Global. asax" in der Datei "Global. asax" definiert.

**Codebeispiel 1-Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

Codebeispiel 1 enthält eine Route mit dem Namen Product. Sie können die Produkt Route verwenden, um dem ProductController in der Liste 2 Browser Anforderungen zuzuordnen.

**Codebeispiel 2: controllers\productcontroller.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

Beachten Sie, dass die vom Produkt Controller verfügbar gemachte Aktion "Details ()" einen einzelnen Parameter namens "ProductID" annimmt. Dieser Parameter ist ein ganzzahliger Parameter.

Die in der Liste 1 definierte Route entspricht einer der folgenden URLs:

- /Product/23
- /Product/7

Leider entspricht die Route auch den folgenden URLs:

- /Product/blah
- /Product/apple

Da die Details ()-Aktion einen Integer-Parameter erwartet, wird eine Anforderung, die einen anderen Wert als einen ganzzahligen Wert enthält, zu einem Fehler führen. Wenn Sie z. b. die URL/Product/Apple in Ihren Browser eingeben, erhalten Sie die Fehlerseite in Abbildung 1.

[![des Dialog Felds "Neues Projekt"](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)

**Abbildung 01**: Anzeigen einer Seiten Explosion ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-route-constraint-cs/_static/image2.png))

Sie möchten nur URLs zuordnen, die eine richtige ganzzahlige ProductID enthalten. Sie können eine Einschränkung verwenden, wenn Sie eine Route definieren, um die URLs einzuschränken, die der Route entsprechen. Die geänderte Produkt Route in der Liste 3 enthält eine Einschränkung für reguläre Ausdrücke, die nur mit ganzen Zahlen übereinstimmt.

**Codebeispiel 3-Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

Der reguläre Ausdruck \d + stimmt mit mindestens einem integerausdruck überein. Diese Einschränkung bewirkt, dass die Produkt Route den folgenden URLs entspricht:

- /Product/3
- /Product/8999

Aber nicht die folgenden URLs:

- /Product/apple
- /Product

- Diese Browser Anforderungen werden von einer anderen Route verarbeitet, oder, wenn keine übereinstimmenden Routen vorhanden sind, wird die Fehlermeldung " *die Ressource wurde nicht gefunden* " zurückgegeben.

> [!div class="step-by-step"]
> [Zurück](creating-custom-routes-cs.md)
> [Weiter](creating-a-custom-route-constraint-cs.md)
