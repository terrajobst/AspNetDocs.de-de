---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Medien Formatierer in ASP.net-Web-API 2-ASP.NET 4. x
author: MikeWasson
description: Zeigt, wie zusätzliche Medienformate in ASP.net-Web-API für ASP.NET 4. x unterstützt werden.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: da0c566dad302054d7d0a6435e4c6df178c64772
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448941"
---
# <a name="media-formatters-in-aspnet-web-api-2"></a>Medien Formatierer in ASP.net-Web-API 2

von [Mike Wasson](https://github.com/MikeWasson)

In diesem Tutorial wird gezeigt, wie zusätzliche Medienformate in ASP.net-Web-API unterstützt werden.

## <a name="internet-media-types"></a>Internet Medientypen

Ein Medientyp, auch als MIME-Typ bezeichnet, identifiziert das Format der Daten. In http beschreiben Medientypen das Format des Nachrichten Texts. Ein Medientyp besteht aus zwei Zeichen folgen, einem Typ und einem Untertyp. Beispiel:

- text/html
- image/png
- Anwendung/json

Wenn eine HTTP-Nachricht einen Entitäts Text enthält, gibt der Content-Type-Header das Format des Nachrichten Texts an. Dadurch wird dem Empfänger mitgeteilt, wie der Inhalt des Nachrichten Texts analysiert werden soll.

Wenn eine HTTP-Antwort beispielsweise ein PNG-Bild enthält, weist die Antwort möglicherweise die folgenden Header auf.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Wenn der Client eine Anforderungs Nachricht sendet, kann er einen Accept-Header enthalten. Der Accept-Header teilt dem Server mit, welche Medientypen der Client vom Server anfordert. Beispiel:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Dieser Header weist den Server an, dass der Client HTML, XHTML oder XML wünscht.

Der Medientyp bestimmt, wie die Web-API den HTTP-Nachrichtentext serialisiert und deserialisiert. Die Web-API verfügt über integrierte Unterstützung für XML-, JSON-, bson-und form-urlencoded-Daten, und Sie können zusätzliche Medientypen unterstützen, indem Sie einen *medienformatierer*schreiben.

Um einen medienformatierer zu erstellen, leiten Sie von einer der folgenden Klassen ab:

- [Mediatypeer Formatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Diese Klasse verwendet asynchrone Lese-und Schreib Methoden.
- [Bufferedmediatypeer Formatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Diese Klasse wird von **mediatypformatter** abgeleitet, verwendet jedoch synchrone Lese-/Schreibmethoden.

Die Ableitung von **bufferedmediatypformatter** ist einfacher, da es keinen asynchronen Code gibt, aber auch bedeutet, dass der aufrufende Thread während der e/a-Vorgänge blockiert werden kann.

## <a name="example-creating-a-csv-media-formatter"></a>Beispiel: Erstellen eines CSV-Medien Formatierers

Das folgende Beispiel zeigt einen Medientyp Formatierer, mit dem ein Product-Objekt in ein CSV-Format (Comma-Separated Values, Komma getrennte Werte) serialisiert werden kann. In diesem Beispiel wird der Produkttyp verwendet, der im Tutorial [Erstellen einer Web-API definiert ist, die CRUD-Vorgänge unterstützt](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Hier ist die Definition des Product-Objekts:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Um einen CSV-Formatierer zu implementieren, definieren Sie eine Klasse, die von **bufferedmediatypformatter**abgeleitet wird:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

Fügen Sie im Konstruktor die vom Formatierer unterstützten Medientypen hinzu. In diesem Beispiel unterstützt der Formatierer einen einzelnen Medientyp, &quot;Text/CSV&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Überschreiben Sie die Methode **canschreitetype** , um anzugeben, welche Typen der Formatierer serialisieren kann:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

In diesem Beispiel kann der Formatierer einzelne `Product` Objekte und Auflistungen von `Product` Objekten serialisieren.

Überschreiben Sie auf ähnliche Weise die **CanRead Type** -Methode, um anzugeben, welche Typen der Formatierer deserialisieren kann. In diesem Beispiel unterstützt der Formatierer die Deserialisierung nicht, sodass die Methode einfach **false**zurückgibt.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Überschreiben Sie schließlich die Methode " **Write-to-Stream** ". Diese Methode serialisiert einen Typ durch Schreiben in einen Stream. Wenn Ihr Formatierer die Deserialisierung unterstützt, überschreiben Sie auch die Read **FromStream** -Methode.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Hinzufügen eines medienformatierers zur Web-API-Pipeline

Um der Web-API-Pipeline einen Medientyp-Formatierer hinzuzufügen, verwenden Sie die **Formatters** -Eigenschaft für das **httpconfiguration** -Objekt.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Zeichen Codierungen

Optional kann ein medienformatierer mehrere Zeichen Codierungen unterstützen, z. b. UTF-8 oder ISO 8859-1.

Fügen Sie im Konstruktor einen oder mehrere [System. Text. Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) -Typen zur **supportedencodings** -Auflistung hinzu. Legen Sie zuerst die Standard Codierung.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

Rufen Sie in den Methoden " **Write to Stream** " und "read **FromStream** " [mediatypformatter. selectcharakteriencoding](https://msdn.microsoft.com/library/hh969054.aspx) auf, um die bevorzugte Zeichencodierung auszuwählen. Diese Methode entspricht den Anforderungs Headern mit der Liste der unterstützten Codierungen. Verwenden Sie die zurückgegebene **Codierung** , wenn Sie aus dem Stream lesen oder schreiben:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
