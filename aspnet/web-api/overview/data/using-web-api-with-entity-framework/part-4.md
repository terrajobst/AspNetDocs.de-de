---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Behandeln von Entitäts Beziehungen | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: be4948e5443a5eb4e1824c63dd0c445a7ee1928e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449121"
---
# <a name="handling-entity-relations"></a>Verarbeiten von Entitätsbeziehungen

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

In diesem Abschnitt werden einige Details erläutert, wie EF Verwandte Entitäten lädt und wie zirkuläre Navigations Eigenschaften in den Modellklassen behandelt werden. (Dieser Abschnitt enthält Hintergrundinformationen und ist nicht erforderlich, um das Tutorial abzuschließen. Wenn Sie möchten, fahren Sie mit [Teil 5 fort.](part-5.md))

## <a name="eager-loading-versus-lazy-loading"></a>Unverzügliches Laden und Lazy Load

Wenn EF mit einer relationalen Datenbank verwendet wird, ist es wichtig zu verstehen, wie EF verwandte Daten lädt.

Außerdem ist es hilfreich, die SQL-Abfragen anzuzeigen, die von EF generiert werden. Fügen Sie dem `BookServiceContext`-Konstruktor die folgende Codezeile hinzu, um die SQL-Ablauf Verfolgung zu verfolgen:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Wenn Sie eine GET-Anforderung an/API/Books senden, wird JSON wie folgt zurückgegeben:

[!code-console[Main](part-4/samples/sample2.cmd)]

Sie sehen, dass die Author-Eigenschaft NULL ist, auch wenn das Buch eine gültige AutorID enthält. Das liegt daran, dass EF die zugehörigen Autor Entitäten nicht lädt. Das Ablauf Verfolgungs Protokoll der SQL-Abfrage bestätigt Folgendes:

[!code-console[Main](part-4/samples/sample3.sql)]

Die SELECT-Anweisung nimmt aus der Bücher Tabelle und verweist nicht auf die Autoren Tabelle.

Im folgenden finden Sie die-Methode in der `BooksController`-Klasse, die die Liste der Bücher zurückgibt.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Sehen wir uns an, wie wir den Autor als Teil der JSON-Daten zurückgeben können. Es gibt drei Möglichkeiten, verwandte Daten in Entity Framework zu laden: Eager Loading, Lazy Loading und explizites laden. Es gibt Kompromisse bei den einzelnen Techniken, daher ist es wichtig zu verstehen, wie Sie funktionieren.

### <a name="eager-loading"></a>Eager Loading

Mit *Eager Loading*lädt EF Verwandte Entitäten als Teil der anfänglichen Datenbankabfrage. Verwenden Sie die " **System. Data. Entity. include** "-Erweiterungsmethode, um Eager Loading auszuführen.

[!code-csharp[Main](part-4/samples/sample5.cs)]

Dies weist EF an, die Autoren Daten in die Abfrage einzubeziehen. Wenn Sie diese Änderung vornehmen und die app ausführen, sehen die JSON-Daten nun wie folgt aus:

[!code-console[Main](part-4/samples/sample6.cmd)]

Das Ablauf Verfolgungs Protokoll zeigt, dass EF einen Join mit den Tabellen "Book" und "Author" ausgeführt hat.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Lazy Loading

Mit Lazy Loading lädt EF automatisch eine verknüpfte Entität, wenn die Navigations Eigenschaft für diese Entität dereferenziert wird. Um Lazy Loading zu aktivieren, stellen Sie die Navigations Eigenschaft als virtuell dar. Beispielsweise in der Book-Klasse:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Sehen Sie sich jetzt den folgenden Code an:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Wenn Lazy Loading aktiviert ist, wird der Zugriff auf die `Author`-Eigenschaft auf `books[0]` bewirkt, dass EF die Datenbank für den Autor abfragt.

Lazy Load erfordert mehrere Daten Bank Fahrten, da EF jedes Mal eine Abfrage sendet, wenn eine verknüpfte Entität abgerufen wird. Im Allgemeinen möchten Sie für Objekte, die Sie serialisieren, Lazy Loading deaktiviert werden. Das Serialisierungsprogramm muss alle Eigenschaften des Modells lesen, das das Laden der verknüpften Entitäten auslöst. Hier sind z. b. die SQL-Abfragen, bei denen EF die Liste der Bücher mit aktiviertem Lazy Loading serialisiert. Sie können sehen, dass EF drei separate Abfragen für die drei Autoren durchführt.

[!code-console[Main](part-4/samples/sample10.sql)]

Es gibt immer noch Zeiten, in denen Sie Lazy Loading verwenden möchten. Das unverzügliches Laden kann bewirken, dass EF einen sehr komplexen Join generiert. Möglicherweise benötigen Sie jedoch auch Verwandte Entitäten für eine kleine Teilmenge der Daten, und Lazy Loading wäre effizienter.

Eine Möglichkeit, um Serialisierungsprobleme zu vermeiden, ist das Serialisieren von DTOs (Data Transfer Objects) anstelle von Entitäts Objekten. Diesen Ansatz zeige ich später in diesem Artikel.

### <a name="explicit-loading"></a>Explizites Laden

Explizites Laden ähnelt Lazy Loading, mit dem Unterschied, dass Sie die zugehörigen Daten explizit im Code erhalten. Dies geschieht nicht automatisch, wenn Sie auf eine Navigations Eigenschaft zugreifen. Explizites Laden ermöglicht Ihnen mehr Kontrolle darüber, wann verwandte Daten geladen werden müssen, erfordert jedoch zusätzlichen Code. Weitere Informationen zum expliziten Laden finden Sie unter [Laden verwandter Entitäten](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Navigations Eigenschaften und Zirkel Verweise

Beim Definieren der Buch-und Autoren Modelle habe ich eine Navigations Eigenschaft für die `Book`-Klasse für die Book-Author-Beziehung definiert, aber ich habe keine Navigations Eigenschaft in der anderen Richtung definiert.

Was geschieht, wenn Sie der `Author` Klasse die entsprechende Navigations Eigenschaft hinzufügen?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Dies führt leider zu einem Problem, wenn Sie die Modelle serialisieren. Wenn Sie die zugehörigen Daten laden, wird ein Zirkel Objekt Diagramm erstellt.

![](part-4/_static/image1.png)

Wenn der JSON-oder XML-Formatierer versucht, das Diagramm zu serialisieren, wird eine Ausnahme ausgelöst. Die zwei Formatierer lösen verschiedene Ausnahme Meldungen aus. Im folgenden finden Sie ein Beispiel für den JSON-Formatierer:

[!code-console[Main](part-4/samples/sample12.cmd)]

Hier ist das XML-Formatierer:

[!code-xml[Main](part-4/samples/sample13.xml)]

Eine Lösung besteht darin, DTOs zu verwenden, die im nächsten Abschnitt beschrieben werden. Alternativ können Sie die JSON-und XML-Formatierer so konfigurieren, dass Sie Diagramm Zyklen verarbeiten. Weitere Informationen finden Sie unter [Handling Zirkel Objekt Verweise](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Für dieses Tutorial benötigen Sie die `Author.Book`-Navigations Eigenschaft nicht, sodass Sie sie weglassen können.

> [!div class="step-by-step"]
> [Zurück](part-3.md)
> [Weiter](part-5.md)
