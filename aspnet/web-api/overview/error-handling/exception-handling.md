---
uid: web-api/overview/error-handling/exception-handling
title: Ausnahmebehandlung in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.custom: seoapril2019
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: dbdbab6aefec840e2fec9e9cd33f3d124093750e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504699"
---
# <a name="exception-handling-in-aspnet-web-api"></a>Ausnahmebehandlung in ASP.net-Web-API

von [Mike Wasson](https://github.com/MikeWasson)

Dieser Artikel beschreibt die Behandlung von Fehlern und Ausnahmen in ASP.net-Web-API.

- [Httpresponeinexception](#httpresponserexception)
- [Ausnahmefilter](#exception_filters)
- [Registrieren von Ausnahme Filtern](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Was geschieht, wenn ein Web-API-Controller eine nicht abgefangene Ausnahme auslöst? Standardmäßig werden die meisten Ausnahmen in eine HTTP-Antwort mit dem Statuscode 500, interner Server Fehler, übersetzt.

Der **httpresponserexception** -Typ ist ein Sonderfall. Diese Ausnahme gibt den HTTP-Statuscode zurück, den Sie im Ausnahmekonstruktor angeben. Beispielsweise gibt die folgende Methode 404 zurück, nicht gefunden, wenn der *ID* -Parameter nicht gültig ist.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Um die Antwort besser steuern zu können, können Sie auch die gesamte Antwortnachricht erstellen und mit **httpresponseexception einschließen:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Ausnahmefilter

Sie können anpassen, wie die Web-API Ausnahmen behandelt, indem Sie einen *Ausnahme Filter*schreiben. Ein Ausnahme Filter wird ausgeführt, wenn eine Controller Methode eine nicht behandelte Ausnahme auslöst, bei der es sich *nicht* um eine **httprespongexception** -Ausnahme handelt. Der **httpresponanexception** -Typ ist ein Sonderfall, da er speziell für die Rückgabe einer HTTP-Antwort entworfen wurde.

Ausnahme Filter implementieren die **System. Web. http. Filters. iexceptionfilter** -Schnittstelle. Die einfachste Möglichkeit, einen Ausnahme Filter zu schreiben, besteht darin, von der **System. Web. http. Filters. exceptionfilterattribute** -Klasse abzuleiten und die **OnException** -Methode zu überschreiben.

> [!NOTE]
> Ausnahme Filter in ASP.net-Web-API ähneln denen in ASP.NET MVC. Allerdings werden Sie in einem separaten Namespace und einer separaten Funktion deklariert. Insbesondere die in MVC verwendete Klasse " **typerrorattribute** " behandelt keine Ausnahmen, die von Web-API-Controllern ausgelöst werden.

Hier ist ein Filter, der **NotImplementedException** -Ausnahmen in den HTTP-Statuscode 501 konvertiert, nicht implementiert:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

Die **Response** -Eigenschaft des **httpactionexecutedcontext** -Objekts enthält die http-Antwortnachricht, die an den Client gesendet wird.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Registrieren von Ausnahme Filtern

Es gibt mehrere Möglichkeiten zum Registrieren eines Web-API-Ausnahmefilters:

- Nach Aktion
- Nach Controller
- Global

Um den Filter auf eine bestimmte Aktion anzuwenden, fügen Sie den Filter der Aktion als Attribut hinzu:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Fügen Sie der Controller Klasse den Filter als Attribut hinzu, um den Filter auf alle Aktionen auf einem Controller anzuwenden:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Wenn Sie den Filter auf alle Web-API-Controller Global anwenden möchten, fügen Sie eine Instanz des Filters zur **globalconfiguration. Configuration. Filters** -Auflistung hinzu. Ausnahmefilter in dieser Sammlung gelten für alle Web-API-Controlleraktionen.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Wenn Sie das Projekt mit der Projektvorlage "ASP.NET MVC 4-Webanwendung" erstellen, fügen Sie Ihren Web-API-Konfigurations Code in die `WebApiConfig`-Klasse ein, die sich im Ordner "App\_Start" befindet:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

Das **HttpError** -Objekt bietet eine konsistente Möglichkeit, Fehlerinformationen im Antworttext zurückzugeben. Im folgenden Beispiel wird gezeigt, wie der HTTP-Statuscode 404 (nicht gefunden) mit **HttpError** im Antworttext zurückgegeben wird.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

" **" Ist eine** Erweiterungsmethode, die in der Klasse " **System .net. http. httprequestmessageextensions** " definiert ist. Intern erstellt " **deateerrorresponse** " eine **HttpError** -Instanz und erstellt dann eine **HttpResponseMessage** , die **HttpError**enthält.

In diesem Beispiel wird das Produkt in der HTTP-Antwort zurückgegeben, wenn die Methode erfolgreich ist. Wenn das angeforderte Produkt jedoch nicht gefunden wird, enthält die HTTP-Antwort einen **HttpError** im Anforderungs Text. Die Antwort kann wie folgt aussehen:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Beachten Sie, dass **HttpError** in diesem Beispiel in JSON serialisiert wurde. Ein Vorteil der Verwendung von **HttpError** besteht darin, dass Sie den gleichen [inhaltsaushandlungs-](../formats-and-model-binding/content-negotiation.md) und Serialisierungsprozess wie jedes andere stark typisierte Modell durchläuft.

### <a name="httperror-and-model-validation"></a>HttpError und Modell Validierung

Bei der Modell Validierung können Sie den Modell Zustand an " **anateerrorresponse**" übergeben, um die Validierungs Fehler in die Antwort einzubeziehen:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

In diesem Beispiel wird möglicherweise die folgende Antwort zurückgegeben:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Weitere Informationen zur Modell Validierung finden Sie unter [Modell Validierung in ASP.net-Web-API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Verwenden von HttpError mit httpresponpexception

In den vorherigen Beispielen wird eine **httpresponsmessage** -Nachricht aus der Controller Aktion zurückgegeben, Sie können jedoch auch **httpresponsexception** verwenden, um einen **HttpError**-Wert zurückzugeben. Auf diese Weise können Sie ein stark typisiertes Modell im normalen Erfolgsfall zurückgeben, während bei einem Fehler weiterhin **HttpError** zurückgegeben wird:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
