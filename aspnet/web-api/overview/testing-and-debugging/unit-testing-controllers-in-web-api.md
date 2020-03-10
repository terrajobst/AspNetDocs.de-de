---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Komponenten Test Controller in ASP.net-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: In diesem Thema werden einige spezielle Techniken für Komponenten Test Controller in der Web-API 2 beschrieben. Bevor Sie dieses Thema lesen, sollten Sie die tutorialeinheit lesen...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: cdb1700537021e276669de1a9e0330a62659746c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447003"
---
# <a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Komponententests für Controller in ASP.NET-Web-API 2

von [Mike Wasson](https://github.com/MikeWasson)

> In diesem Thema werden einige spezielle Techniken für Komponenten Test Controller in der Web-API 2 beschrieben. Bevor Sie dieses Thema lesen, sollten Sie das Tutorial [Unit Testing ASP.net-Web-API 2](unit-testing-with-aspnet-web-api.md)lesen, das zeigt, wie Sie der Projekt Mappe ein Komponenten Testprojekt hinzufügen.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Web-API 2
> - [4.5.30](https://github.com/Moq)

> [!NOTE]
> Ich habe mich für die Verwendung von "muq", aber dieselbe Idee gilt für jedes andere Framework. "Muq 4.5.30 (und höher)" unterstützt Visual Studio 2017, Roslyn und .NET 4,5 und höhere Versionen.

Ein gängiges Muster in Komponententests ist &quot;"Arrange-Act-Assert"-&quot;:

- Anordnen: richten Sie alle Voraussetzungen ein, damit der Test ausgeführt werden muss.
- Act: führt den Test aus.
- Assert: Überprüfen Sie, ob der Test erfolgreich war.

Im Schritt "Anordnen" werden häufig Mock-oder Stub-Objekte verwendet. Dadurch wird die Anzahl der Abhängigkeiten minimiert, daher konzentriert sich der Test auf das Testen eines solchen.

Im folgenden finden Sie einige Punkte, die Sie in Ihren Web-API-Controllern testen sollten:

- Die Aktion gibt den korrekten Antworttyp zurück.
- Ungültige Parameter geben die korrekte Fehler Antwort zurück.
- Die Aktion ruft die richtige Methode für das Repository oder die Dienst Ebene auf.
- Wenn die Antwort ein Domänen Modell enthält, überprüfen Sie den Modelltyp.

Dies sind einige der allgemeinen Dinge, die getestet werden müssen, aber die Besonderheiten hängen von ihrer Controller Implementierung ab. Insbesondere ist ein großer Unterschied, ob die Controller Aktionen " **HttpResponseMessage** " oder " **ihttpactionresult**" zurückgeben. Weitere Informationen zu diesen Ergebnistypen finden Sie unter [Aktions Ergebnisse in der Web-API 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Testen von Aktionen, die httprespontmessage zurückgeben

Im folgenden finden Sie ein Beispiel für einen Controller, dessen Aktionen " **httpresponabmessage**" zurückgeben.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Beachten Sie, dass der Controller eine `IProductRepository`mit Abhängigkeitsinjektion Einschleusung Dadurch kann der Controller schneller getestet werden, da Sie ein Mock-Repository einfügen können. Der folgende Komponenten Test überprüft, ob die `Get`-Methode eine `Product` in den Antworttext schreibt. Angenommen, `repository` ist ein Mock `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Es ist wichtig, die **Anforderung** und **Konfiguration** auf dem Controller festzulegen. Andernfalls schlägt der Test mit einer **ArgumentNullException** oder **InvalidOperationException**fehl.

## <a name="testing-link-generation"></a>Test Link Generierung

Die `Post`-Methode ruft **Urlhelper. Link** auf, um Links in der Antwort zu erstellen. Dies erfordert ein wenig mehr Setup im Komponenten Test:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

Die **Urlhelper** -Klasse benötigt die Anforderungs-URL und die Routendaten, sodass der Test Werte für diese festlegen muss. Eine andere Möglichkeit ist Mock oder Stub **Urlhelper**. Bei diesem Ansatz ersetzen Sie den Standardwert von " [apicontroller. URL](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) " durch eine Mock-oder Stubversion, die einen festgelegten Wert zurückgibt.

Wir schreiben den Test mit dem " [muq](https://github.com/Moq) "-Framework neu. Installieren Sie das nuget-Paket `Moq` im Testprojekt.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

In dieser Version müssen Sie keine Routendaten einrichten, da der Mock- **Urlhelper** eine Konstante Zeichenfolge zurückgibt.

## <a name="testing-actions-that-return-ihttpactionresult"></a>Testen von Aktionen, die ihttpactionresult zurückgeben

In der Web-API 2 kann eine Controller Aktion **ihttpactionresult**zurückgeben, was analog zu **Action result** in ASP.NET MVC ist. Die **ihttpactionresult** -Schnittstelle definiert ein Befehls Muster zum Erstellen von HTTP-Antworten. Anstatt die Antwort direkt zu erstellen, gibt der Controller ein **ihttpactionresult**zurück. Später Ruft die Pipeline das **ihttpactionresult** auf, um die Antwort zu erstellen. Diese Vorgehensweise erleichtert das Schreiben von Komponententests, da Sie eine große Menge an Setup überspringen können, die für **HttpResponseMessage**benötigt wird.

Hier ist ein Beispiel Controller, dessen Aktionen **ihttpactionresult**zurückgeben.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

In diesem Beispiel werden einige gängige Muster mithilfe von **ihttpactionresult**veranschaulicht. Sehen wir uns an, wie Sie Komponententests ausführen.

### <a name="action-returns-200-ok-with-a-response-body"></a>Action gibt 200 (OK) mit einem Antworttext zurück.

Die `Get`-Methode ruft `Ok(product)` auf, wenn das Produkt gefunden wurde. Stellen Sie im Komponenten Test sicher, dass der Rückgabetyp **okaushandatedcontentresult** und das zurückgegebene Produkt die Rechte-ID aufweist.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Beachten Sie, dass der Komponenten Test das Aktions Ergebnis nicht ausführt. Sie können davon ausgehen, dass das Aktions Ergebnis die HTTP-Antwort ordnungsgemäß erstellt. (Aus diesem Grund verfügt das Web-API-Framework über eigene Komponententests!)

### <a name="action-returns-404-not-found"></a>Aktion gibt 404 zurück (nicht gefunden)

Die `Get`-Methode ruft `NotFound()` auf, wenn das Produkt nicht gefunden wurde. In diesem Fall prüft der Komponenten Test nur, ob der Rückgabetyp **notfoundresult**ist.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Action gibt 200 (OK) ohne Antworttext zurück.

Die `Delete`-Methode ruft `Ok()` auf, um eine leere HTTP 200-Antwort zurückzugeben. Wie im vorherigen Beispiel überprüft der UnitTest den Rückgabetyp, in diesem Fall **okresult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Aktion gibt 201 (erstellt) mit einem Location-Header zurück.

Die `Post`-Methode ruft `CreatedAtRoute` auf, um eine HTTP 201-Antwort mit einem URI im Location-Header zurückzugeben. Überprüfen Sie im Komponenten Test, ob die richtigen Routing Werte von der Aktion festgelegt werden.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Action gibt einen weiteren 2xx mit einem Antworttext zurück.

Mit der `Put`-Methode wird `Content` aufgerufen, um eine HTTP 202-Antwort (akzeptiert) mit einem Antworttext zurückzugeben. Dieser Fall ähnelt der Rückgabe von 200 (OK), aber der Komponenten Test sollte auch den Statuscode überprüfen.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Entity Framework, wenn Komponententests ASP.net-Web-API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Schreiben von Tests für einen ASP.net-Web-API-Dienst](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (Blogbeitrag von Youssef Moussaoui).
- [Debuggen von ASP.net-Web-API mit dem Routen Debugger](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
