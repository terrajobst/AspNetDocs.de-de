---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Neues Element zur Datenbank hinzufügen | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 692269a2c11e529af78f24feca74bba704b5b54b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448971"
---
# <a name="add-a-new-item-to-the-database"></a>Hinzufügen eines neuen Elements zur Datenbank

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

In diesem Abschnitt fügen Sie Benutzern die Möglichkeit hinzu, ein neues Buch zu erstellen. Fügen Sie in "App. js" dem Ansichts Modell folgenden Code hinzu:

[!code-javascript[Main](part-9/samples/sample1.js)]

Ersetzen Sie in der Datei "index. cshtml" das folgende Markup:

[!code-html[Main](part-9/samples/sample2.html)]

Durch:

[!code-html[Main](part-9/samples/sample3.html)]

Dieses Markup erstellt ein Formular zum Übermitteln eines neuen Autors. Die Werte für die Dropdown Liste Autor sind an die `authors` Observable im Ansichts Modell gebunden. Für andere Formular Eingaben sind die Werte an die `newBook`-Eigenschaft des Ansichts Modells gebunden.

Der Sende Handler im Formular ist an die `addBook`-Funktion gebunden:

[!code-html[Main](part-9/samples/sample4.html)]

Die `addBook`-Funktion liest die aktuellen Werte der Daten gebundenen Formular Eingaben, um ein JSON-Objekt zu erstellen. Anschließend wird das JSON-Objekt an `/api/books`übermittelt.

> [!div class="step-by-step"]
> [Zurück](part-8.md)
> [Weiter](part-10.md)
