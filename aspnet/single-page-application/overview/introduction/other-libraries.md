---
uid: single-page-application/overview/introduction/other-libraries
title: Andere Bibliotheken als Knockout | Microsoft-Dokumentation
author: madskristensen
description: Andere Bibliotheken als Knockout
ms.author: riande
ms.date: 02/05/2013
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 64a4ad1fb411f7291a5cba634afdf4d2fdb16d55
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467193"
---
# <a name="know-a-library-other-than-knockout"></a>Andere Bibliotheken als Knockout

von [Mads Kristensen](https://github.com/madskristensen)

Die [Vorlage für die Einzelseiten Anwendung (Single Page Application, Spa)](knockoutjs-template.md) ist eine gute Möglichkeit, um mit dem Schreiben von einseitigen Anwendungen zu beginnen. Die Vorlage verwendet " [knockoutjs](http://knockoutjs.com/) ", um Anwendungsdaten an DOM-Elemente zu binden.

Knockout ist jedoch nicht die einzige JavaScript-Bibliothek zum Erstellen von Rich-Client-Anwendungen. Andere Bibliotheken lösen ähnliche Herausforderungen auf verschiedene Weise aus. Möglicherweise bevorzugen Sie eine einzige Bibliothek, daher haben wir mehrere von der Community erstellte Vorlagen zum Download zur Verfügung gestellt. Jede dieser Vorlagen verwendet eine andere Mischung aus JavaScript-Client Bibliotheken.

Um eine von der Community erstellte Vorlage zu installieren, besuchen Sie eine der unten aufgeführten Vorlagen Seiten, und klicken Sie auf die Schaltfläche herunterladen. Die Vorlagen werden als VSIX-Dateien bereitgestellt.

## <a name="backbonejs"></a>Backbonejs

[Backbone. js-Spa-Vorlage](../templates/backbonejs-template.md). Diese Vorlage stellt ein erstes Gerüst für die Entwicklung einer [Backbone. js](http://backbonejs.org/) -Anwendung in ASP.NET MVC dar. Standardmäßig bietet Sie grundlegende Benutzer Anmelde Funktionen, einschließlich Benutzerregistrierung, Anmeldung, Kenn Wort Zurücksetzung und Benutzer Bestätigung mit grundlegenden e-Mail-Vorlagen.

## <a name="breezejs"></a>Breezejs

" [Breezejs](http://www.breezejs.com/?utm_source=ms-spa) " ist eine Open-Source-Bibliothek zum Verwalten von Rich-Data in einem JavaScript-Client. Breeze übernimmt Abfragen, Caching, Änderungs Nachverfolgung, Validierung und vieles mehr. Zwei Vorlagen Feature Breeze:

- Die Vorlage [Breeze/Knockout](../templates/breezeknockout-template.md) erweitert die Vorlage Knockout Spa und zeigt, wie einfach eine einseitige Anwendung mit Breeze für die Datenverwaltung und knockoutjs für die Datenbindung erstellt werden kann.
- Die Vorlage [Breeze/Angular](../templates/breezeangular-template.md) erweitert auch die Knockout-Spa-Vorlage mit Breeze, aber die [angularjs](http://angularjs.org) -Bibliothek wird für die Datenbindung, die Abhängigkeitsinjektion und die Bildschirm Verwaltung verwendet.

Außerdem wird in der [Spa-Vorlage Hot Handtuch](../templates/hottowel-template.md) die Datei "breezejs" verwendet.

## <a name="emberjs"></a>Emberjs

[Emberjs-Spa-Vorlage](../templates/emberjs-template.md). Diese Vorlage verwendet [Ember](http://emberjs.com/), eine leistungsstarke MVC-JavaScript-Bibliothek, die eine Vielzahl von Herausforderungen beim Erstellen von Rich Client-Anwendungen löst.

Die Ember Spa-Vorlage ist eine Neuimplementierung der Knockout-Spa-Vorlage, die die Verwendung von emberjs und der über Lagerungs leisten Vorlage verwendet.

## <a name="hot-towel"></a>Heißes Handtuch

[Hot-Handtuch-Spa-Vorlage](../templates/hottowel-template.md). Diese Vorlage enthält mehrere JavaScript-Bibliotheken, einschließlich Breeze, Knockout, Requirements js und Twitter Bootstrap.

Im Vergleich zu den anderen hier aufgeführten Vorlagen bietet die Vorlage "Hot-Handtuch" eine umfassendere Anwendung, aus der Sie Ihre eigenen erstellen können. Es gibt weitere Konzepte, die Sie kennen sollten, aber wenn Sie Sie verstehen, ist diese Vorlage möglicherweise nur das, was Sie suchen. Wenn Sie eine Spa erstellen, aber nicht entscheiden können, wo Sie beginnen sollen, verwenden Sie das heiße Handtuch, und in Sekunden haben Sie eine Spa und alle Tools, die Sie benötigen, um darauf aufzubauen.

## <a name="feature-table"></a>Funktions Tabelle

Die folgenden Funktionen werden von den einzelnen Spa-Vorlagen bereitgestellt:

|                        | ASP.NET SPA | Liebene | Breeze/Angular | Breeze/Ko |  FON   | Heißes Handtuch |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      TODO-Beispiel       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Bare Vorlage      |             | &#10003; |                |           |          | &#10003;  |
| Navigation und Verlauf |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Bibliotheken       |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Liebene     |             | &#10003; |                |           |          |           |
|         Versorgt         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         FON          |             |          |                |           | &#10003; |           |
|        K.o.        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |
