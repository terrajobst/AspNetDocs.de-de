---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Create Datenübertragung Objects (DTOs) | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: fc0463420207eba764014b8ec7123c5150e38247
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449025"
---
# <a name="create-data-transfer-objects-dtos"></a>Erstellen von Datentransferobjekten (DTOs)

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

Derzeit macht unsere Web-API die Daten Bank Entitäten für den Client verfügbar. Der Client empfängt Daten, die direkt den Datenbanktabellen zugeordnet sind. Das ist jedoch nicht immer eine gute Idee. Manchmal möchten Sie die Form der Daten ändern, die Sie an den Client senden. Auf diese Weise können Sie beispielsweise folgende Vorgänge durchführen:

- Entfernen Sie zirkuläre Verweise (siehe vorheriger Abschnitt).
- Blenden Sie bestimmte Eigenschaften aus, die Clients nicht anzeigen sollen.
- Lassen Sie einige Eigenschaften Weg, um die Nutzlastgröße zu verringern.
- Vereinfachen Sie Objekt Diagramme, die die verglichenen Objekte enthalten, damit Sie für Clients bequemer werden.
- Vermeiden Sie "overposting"-Sicherheitsrisiken. (Eine Erörterung der Übertragung finden Sie unter [Modell Validierung](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) .)
- Entkoppeln Sie die Dienst Ebene von der Datenbankebene.

Hierzu können Sie ein *Datenübertragungs Objekt (Data Transfer Object* , dto) definieren. Ein DTO ist ein Objekt, das definiert, wie die Daten über das Netzwerk gesendet werden. Sehen wir uns an, wie das mit der Buch-Entität funktioniert. Fügen Sie im Ordner Models zwei DTO-Klassen hinzu:

[!code-csharp[Main](part-5/samples/sample1.cs)]

Die `BookDetailDto`-Klasse enthält alle Eigenschaften aus dem Buch Modell, mit dem Unterschied, dass `AuthorName` eine Zeichenfolge ist, die den Namen des Autors enthält. Die `BookDto`-Klasse enthält eine Teilmenge der Eigenschaften von `BookDetailDto`.

Ersetzen Sie als nächstes die beiden Get-Methoden in der `BooksController`-Klasse mit den Versionen, die DTOs zurückgeben. Wir verwenden die LINQ **Select** -Anweisung, um von Book-Entitäten in DTOs zu konvertieren.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Hier ist das SQL, das von der neuen `GetBooks`-Methode generiert wird. Sie können sehen, dass EF die LINQ **Select** -Anweisung in eine SQL SELECT-Anweisung übersetzt.

[!code-sql[Main](part-5/samples/sample3.sql)]

Ändern Sie abschließend die `PostBook`-Methode, um einen DTO-Wert zurückzugeben.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> In diesem Tutorial werden Sie manuell in DTOs in den Code umwandelt. Eine andere Möglichkeit besteht darin, eine Bibliothek wie [AutoMapper](http://automapper.org/) zu verwenden, die die Konvertierung automatisch verarbeitet.
> 
> [!div class="step-by-step"]
> [Zurück](part-4.md)
> [Weiter](part-6.md)
