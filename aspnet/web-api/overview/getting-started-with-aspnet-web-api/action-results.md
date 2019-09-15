---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Aktions Ergebnisse in der Web-API 2-ASP.NET 4. x
author: MikeWasson
description: Beschreibt, wie ASP.net-Web-API den Rückgabewert einer Controller Aktion in eine HTTP-Antwortnachricht in ASP.NET 4. x konvertiert.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: f00ac0db453053e53d6d6942dd1557b409f4167b
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985839"
---
# <a name="action-results-in-web-api-2"></a>Aktionsergebnisse in Web-API 2

[!INCLUDE[](~/includes/coreWebAPI.md)]

In diesem Thema wird beschrieben, wie ASP.net-Web-API den Rückgabewert einer Controller Aktion in eine HTTP-Antwortnachricht konvertiert.

Eine Web-API-Controller Aktion kann Folgendes zurückgeben:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Ein anderer Typ

Abhängig davon, welche dieser zurückgegeben wird, verwendet die Web-API einen anderen Mechanismus zum Erstellen der HTTP-Antwort.

| Rückgabetyp | Wie die Web-API die Antwort erstellt |
| --- | --- |
| void | Rückgabe von leeren 204 (kein Inhalt) |
| **HttpResponseMessage** | Direkt in eine HTTP-Antwortnachricht konvertieren. |
| **IHttpActionResult** | Rufen Sie **ExecuteAsync** auf, um ein **httpresponanmessage**-Element zu erstellen und dann in eine HTTP-Antwortnachricht zu konvertieren. |
| Anderer Typ | Schreiben Sie den serialisierten Rückgabewert in den Antworttext. gibt 200 zurück (OK). |

Im weiteren Verlauf dieses Themas werden die einzelnen Optionen ausführlicher beschrieben.

## <a name="void"></a>void

Wenn der Rückgabetyp ist `void`, gibt die Web-API einfach eine leere HTTP-Antwort mit dem Statuscode 204 (kein Inhalt) zurück.

Beispiel Controller:

[!code-csharp[Main](action-results/samples/sample1.cs)]

HTTP-Antwort:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

Wenn die Aktion ein [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx)-Objekt zurückgibt, konvertiert die Web-API den Rückgabewert direkt in eine HTTP-Antwortnachricht, wobei die Eigenschaften des **HttpResponseMessage** -Objekts verwendet werden, um die Antwort aufzufüllen.

Diese Option bietet eine große Kontrolle über die Antwortnachricht. Beispielsweise wird mit der folgenden Controller Aktion der Cache-Control-Header festgelegt.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Antwort:

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Wenn Sie ein Domänen Modell an die Methode " **samateresponse** " übergeben, verwendet die Web-API einen [medienformatierer](../formats-and-model-binding/media-formatters.md) , um das serialisierte Modell in den Antworttext zu schreiben.

[!code-csharp[Main](action-results/samples/sample5.cs)]

Die Web-API verwendet den Accept-Header in der Anforderung, um das Formatierer auszuwählen. Weitere Informationen finden Sie unter [inhaltsaus](../formats-and-model-binding/content-negotiation.md)Handlung.

## <a name="ihttpactionresult"></a>IHttpActionResult

Die **ihttpactionresult** -Schnittstelle wurde in der Web-API 2 eingeführt. Im Wesentlichen wird eine **httpresponsmessage** -Factory definiert. Dies sind einige Vorteile der Verwendung der **ihttpactionresult** -Schnittstelle:

- Vereinfacht das Komponenten [Testen](../testing-and-debugging/unit-testing-controllers-in-web-api.md) ihrer Controller.
- Verschiebt gängige Logik zum Erstellen von HTTP-Antworten in separate Klassen.
- Macht die Absicht der Controller Aktion klarer, indem die Details auf niedriger Ebene zum Erstellen der Antwort ausgeblendet werden.

**Ihttpactionresult** enthält eine einzige Methode, **ExecuteAsync**, die asynchron eine **HttpResponseMessage** -Instanz erstellt.

[!code-csharp[Main](action-results/samples/sample6.cs)]

Wenn eine Controller Aktion ein **ihttpactionresult**zurückgibt, ruft die Web-API die **ExecuteAsync** -Methode auf, um eine **HttpResponseMessage**-Methode zu erstellen. Anschließend wird **httpresponanmessage** in eine HTTP-Antwortnachricht konvertiert.

Im folgenden finden Sie eine einfache Implementierung von **ihttpactionresult** , die eine nur-Text-Antwort erstellt:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Beispiel für eine Controller Aktion:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Antwort:

[!code-console[Main](action-results/samples/sample9.cmd)]

In den meisten Fällen verwenden Sie die im **[System. Web. http. results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** -Namespace definierten **ihttpactionresult** -Implementierungen. Die **apicontroller** -Klasse definiert Hilfsmethoden, die diese integrierten Aktions Ergebnisse zurückgeben.

Wenn die Anforderung im folgenden Beispiel nicht mit einer vorhandenen Produkt-ID identisch ist, ruft der Controller [apicontroller. NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) auf, um eine 404-Antwort (nicht gefunden) zu erstellen. Andernfalls ruft der Controller [apicontroller. OK](https://msdn.microsoft.com/library/dn314591.aspx)auf, wodurch eine 200-Antwort (OK) erstellt wird, die das Produkt enthält.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Andere Rückgabe Typen

Für alle anderen Rückgabe Typen verwendet die Web-API einen [medienformatierer](../formats-and-model-binding/media-formatters.md) , um den Rückgabewert zu serialisieren. Die Web-API schreibt den serialisierten Wert in den Antworttext. Der Antwortstatus Code ist 200 (OK).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Ein Nachteil dieses Ansatzes besteht darin, dass Sie keinen Fehlercode direkt zurückgeben können, z. b. 404. Sie können jedoch eine **httpresponmenexception** für Fehlercodes auslösen. Weitere Informationen finden Sie unter [Ausnahmebehandlung in ASP.net-Web-API](../error-handling/exception-handling.md).

Die Web-API verwendet den Accept-Header in der Anforderung, um das Formatierer auszuwählen. Weitere Informationen finden Sie unter [inhaltsaus](../formats-and-model-binding/content-negotiation.md)Handlung.

Beispiel Anforderung

[!code-console[Main](action-results/samples/sample12.cmd)]

Beispiel Antwort

[!code-console[Main](action-results/samples/sample13.cmd)]
