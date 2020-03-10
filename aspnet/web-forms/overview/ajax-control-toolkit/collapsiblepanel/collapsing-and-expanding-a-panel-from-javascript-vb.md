---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Reduzieren und Erweitern eines Panels von JavaScript (VB) | Microsoft-Dokumentation
author: wenz
description: Das Reduzier Bare Panel-Steuerelement im ASP.NET AJAX Control Toolkit erweitert ein Panel und bietet ihm die Möglichkeit, seinen Inhalt zu reduzieren und zu erweitern...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: aa9779c65fb587193dbabde55cc6900283ce239d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430593"
---
# <a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a>Reduzieren und Erweitern eines Bereichs über JavaScript (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)

> Das Reduzier Bare Panel-Steuerelement im ASP.NET AJAX Control Toolkit erweitert ein Panel und bietet ihm die Möglichkeit, seinen Inhalt zu reduzieren und erneut zu erweitern. Diese beiden Aktionen können auch über benutzerdefinierten JavaScript-Code ausgelöst werden.

## <a name="overview"></a>Übersicht

Das Reduzier Bare Panel-Steuerelement im ASP.NET AJAX Control Toolkit erweitert ein Panel und bietet ihm die Möglichkeit, seinen Inhalt zu reduzieren und erneut zu erweitern. Diese beiden Aktionen können auch über benutzerdefinierten JavaScript-Code ausgelöst werden.

## <a name="steps"></a>Schritte

Erstellen Sie zuerst eine neue ASP.NET-Seite, und fügen Sie die `ScriptManager` innerhalb des One `<form>`-Elements ein. Dadurch wird die ASP.NET AJAX-Bibliothek geladen, die für das Control Toolkit erforderlich ist:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

Erstellen Sie dann einen Bereich mit Text, sodass der Reduzierungs-/Erweiterungs Effekt angezeigt werden kann:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

Wie Sie sehen können, verweist der Bereich auf eine CSS-Klasse, die hier gezeigt wird (und definiert im Grunde eine Hintergrundfarbe und die Breite des Bereichs):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

Das `CollapsiblePanelExtender` Steuerelement benötigt das `TargetControlID`-Attribut, damit das Toolkit weiß, welcher Bereich auf Anforderung reduziert oder erweitert werden soll:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

Leider macht der Extender derzeit keine bestimmte API zum reduzieren oder Erweitern des Panels verfügbar, aber einige nicht dokumentierte Methoden werden nicht unterstützt. Fügen Sie zunächst der Seite drei HTML-Schaltflächen hinzu, die das Client seitige JavaScript auslöst, um den Inhalt des Bereichs zu reduzieren oder zu erweitern:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

Im Client seitigen JavaScript-Code (gestartet mit `<script type="text/javascript">`) muss die `$find()`-Methode für den Zugriff auf die `CollapsiblePanelExtender`verwendet werden. `$find("cpe")` gibt einen Verweis darauf zurück. Von dort aus lösen bestimmte Methoden die vorliegende Aufgabe aus.

Die Methode zum Öffnen (erweitern) des Panels wird `_doOpen()`; der folgende Code implementiert die `doOpen()`-Funktion, die aufgerufen wird, wenn auf die erste Schaltfläche geklickt wird:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

Um das Panel zu schließen oder zu reduzieren, muss die `_doClose()` Methode ausgeführt werden. Wenn der Benutzer auf die zweite Schaltfläche klickt, wird der folgende JavaScript-Code aufgerufen:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

Die dritte Schaltfläche schaltet den Status des Panels um: von reduziert zu erweitert und umgekehrt. Der `CollapsiblePanelExtender` macht die `toggle()`-Methode verfügbar, die genau diese Methode ausführt: kehrt den Zustand des Panels um. Es gibt jedoch auch einen anderen Ansatz (der intern von der `toggle()`-Methode verwendet wird): die `get_Collapsed()`-Methode der `CollapsiblePanelExtender()` gibt Aufschluss darüber, ob der Bereich reduziert wird. Abhängig vom Rückgabewert dieser Funktion wird der Bereich dann entweder erweitert (`_doOpen()` Methode) oder reduziert (`_doClose()`)-Methode:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]

[![die dritte Schaltfläche den Status des Panels ändert: von "reduziert" zu "Erweitert" und "zurück"](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)

Die dritte Schaltfläche ändert den Status des Panels: von "reduziert" zu "Erweitert" und "zurück" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Previous](collapsing-and-expanding-a-panel-from-javascript-cs.md)
