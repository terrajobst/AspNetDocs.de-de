---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Element Details anzeigen | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f48c5ad73ceb9a4873fbbb621b518553a498966
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449001"
---
# <a name="display-item-details"></a>Anzeigen von Elementdetails

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

In diesem Abschnitt fügen Sie die Möglichkeit zum Anzeigen von Details zu jedem Buch hinzu. Fügen Sie in "App. js" dem Ansichts Modell folgenden Code hinzu:

[!code-javascript[Main](part-8/samples/sample1.js)]

Fügen Sie in views/Home/Index. cshtml dem Link Details ein Daten Bindungs Element hinzu:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Dadurch wird der Click-Handler für die &lt;ein&gt;-Element an die `getBookDetail`-Funktion im Ansichts Modell gebunden.

Ersetzen Sie in der gleichen Datei die folgende Markierung:

[!code-html[Main](part-8/samples/sample3.html)]

durch diesen:

[!code-html[Main](part-8/samples/sample4.html)]

Dieses Markup erstellt eine Tabelle, die an die Eigenschaften der `detail` Observable im Ansichts Modell gebunden ist.

Mit der Syntax "&lt;!--ko-&gt;&quot; können Sie eine Knockout-Bindung außerhalb eines DOM-Elements einschließen. In diesem Fall bewirkt die `if` Bindung, dass dieser Abschnitt des Markups nur angezeigt wird, wenn `details` nicht NULL ist.

[!code-html[Main](part-8/samples/sample5.html)]

Wenn Sie nun die app ausführen und auf eine der &quot;Details&quot; Links klicken, werden die Buch Details in der App angezeigt.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Zurück](part-7.md)
> [Weiter](part-9.md)
