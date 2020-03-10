---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing in ASP.net-Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449247"
---
# <a name="routing-in-aspnet-web-api"></a>Routing in ASP.net-Web-API

von [Mike Wasson](https://github.com/MikeWasson)

In diesem Artikel wird beschrieben, wie ASP.net-Web-API HTTP-Anforderungen an Controller weiterleitet.

> [!NOTE]
> Wenn Sie mit ASP.NET MVC vertraut sind, ähnelt das Web-API-Routing sehr dem MVC-Routing. Der Hauptunterschied besteht darin, dass die Web-API das HTTP-Verb und nicht den URI-Pfad verwendet, um die Aktion auszuwählen. Sie können auch das MVC-Routing in der Web-API verwenden. In diesem Artikel wird davon ausgegangen, dass keine Kenntnisse über ASP.NET MVC vorhanden sind.

## <a name="routing-tables"></a>Routing Tabellen

In ASP.net-Web-API ist ein *Controller* eine Klasse, die HTTP-Anforderungen verarbeitet. Die öffentlichen Methoden des Controllers werden als *Aktionsmethoden* oder einfach als *Aktionen*bezeichnet. Wenn das Web-API-Framework eine Anforderung empfängt, leitet es die Anforderung an eine Aktion weiter.

Um zu ermitteln, welche Aktion aufgerufen werden soll, verwendet das Framework eine *Routing Tabelle*. Die Visual Studio-Projektvorlage für die Web-API erstellt eine Standardroute:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Diese Route wird in der Datei *WebApiConfig.cs* definiert, die in der *App\_Start* -Verzeichnisses abgelegt wird:

![](routing-in-aspnet-web-api/_static/image1.png)

Weitere Informationen zum `WebApiConfig`-Klasse finden Sie unter [Konfigurieren von ASP.net-Web-API](../advanced/configuring-aspnet-web-api.md).

Wenn Sie die Web-API selbst hosten, müssen Sie die Routing Tabelle direkt auf das `HttpSelfHostConfiguration`-Objekt festlegen. Weitere Informationen finden Sie unter [Self-Hosting einer Web-API](../older-versions/self-host-a-web-api.md).

Jeder Eintrag in der Routing Tabelle enthält eine *Routen Vorlage*. Die Standardrouten Vorlage für die Web-API ist &quot;API/{Controller}/{ID}&quot;. In dieser Vorlage ist &quot;API-&quot; ein literales Pfad Segment, und {Controller} und {ID} sind Platzhalter Variablen.

Wenn das Web-API-Framework eine HTTP-Anforderung empfängt, versucht es, den URI mit einer der Routen Vorlagen in der Routing Tabelle abzugleichen. Wenn keine Route übereinstimmt, empfängt der Client einen 404-Fehler. Die folgenden URIs entsprechen z. b. der Standardroute:

- /api/contacts
- /api/contacts/1
- /api/products/gizmo1

Der folgende URI stimmt jedoch nicht, da der &quot;-API-&quot; Segment fehlt:

- /contacts/1

> [!NOTE]
> Der Grund für die Verwendung von "API" in der Route besteht darin, Konflikte mit ASP.NET MVC-Routing zu vermeiden. Auf diese Weise können Sie &quot;/Contacts-&quot; zu einem MVC-Controller wechseln und &quot;/API/Contacts-&quot; zu einem Web-API-Controller wechseln. Wenn Ihnen diese Konvention nicht gefällt, können Sie natürlich auch die Standardrouten Tabelle ändern.

Wenn eine übereinstimmende Route gefunden wird, wählt die Web-API den Controller und die Aktion aus:

- Um den Controller zu finden, fügt die Web-API &quot;Controller&quot; dem Wert der Variablen *{Controller}* hinzu.
- Um die Aktion zu finden, untersucht die Web-API das HTTP-Verb und sucht dann nach einer Aktion, deren Name mit diesem http-Verb Namen beginnt. Beispielsweise sucht die Web-API mit einer GET-Anforderung nach einer Aktion, der &quot;Get&quot;vorangestellt ist, z. b. &quot;GetContact&quot; oder &quot;getallcontacts&quot;. Diese Konvention gilt nur für Get-, Post-, Put-, DELETE-, Head-, Options-und Patch-Verben. Sie können andere HTTP-Verben mithilfe von Attributen auf dem Controller aktivieren. Ein Beispiel hierfür wird später angezeigt.
- Andere Platzhalter Variablen in der Routen Vorlage (z. b. *{ID}* ) werden Aktions Parametern zugeordnet.

Schauen wir uns ein Beispiel an. Angenommen, Sie definieren den folgenden Controller:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Im folgenden finden Sie einige mögliche HTTP-Anforderungen zusammen mit der Aktion, die für jede aufgerufen wird:

| HTTP-Verb | URI-Pfad | Aktion | Parameter |
| --- | --- | --- | --- |
| GET | API/Produkte | Getallproducts | *gar* |
| GET | API/Produkte/4 | GetProductById | 4 |
| Delete | API/Produkte/4 | DeleteProduct | 4 |
| POST | API/Produkte | *(keine Entsprechung)* |  |

Beachten Sie, dass das *{ID}* -Segment des URIs, sofern vorhanden, dem *ID* -Parameter der Aktion zugeordnet ist. In diesem Beispiel definiert der Controller zwei Get-Methoden, eine mit einem *ID* -Parameter und eine ohne Parameter.

Beachten Sie außerdem, dass die Post-Anforderung fehlschlägt, da der Controller keine &quot;Post...&quot;-Methode definiert.

## <a name="routing-variations"></a>Routing Variationen

Im vorherigen Abschnitt wurde der grundlegende Routing Mechanismus für ASP.net-Web-API beschrieben. In diesem Abschnitt werden einige Variationen beschrieben.

### <a name="http-verbs"></a>HTTP-Verben

Anstatt die Benennungs Konvention für HTTP-Verben zu verwenden, können Sie das HTTP-Verb für eine Aktion explizit angeben, indem Sie die Aktionsmethode mit einem der folgenden Attribute versehen:

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

Im folgenden Beispiel wird die `FindProduct`-Methode GET-Anforderungen zugeordnet:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Verwenden Sie das `[AcceptVerbs]`-Attribut, das eine Liste von HTTP-Verben annimmt, um mehrere HTTP-Verben für eine Aktion zuzulassen oder um andere HTTP-Verben als Get, Put, Post, DELETE, Head, Options und Patch zuzulassen.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Routing nach Aktions Name

Mit der Standard Routing Vorlage verwendet die Web-API das HTTP-Verb, um die Aktion auszuwählen. Sie können jedoch auch eine Route erstellen, bei der der Aktionsname im URI enthalten ist:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

In dieser Routen Vorlage benennt der Parameter " *{Action}* " die Aktionsmethode auf dem Controller. Verwenden Sie bei diesem Routing Stil Attribute, um die zulässigen HTTP-Verben anzugeben. Nehmen Sie beispielsweise an, dass Ihr Controller die folgende Methode hat:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

In diesem Fall würde eine GET-Anforderung für "API/Products/Details/1" der `Details`-Methode zugeordnet werden. Diese Art des Routings ähnelt ASP.NET MVC und eignet sich möglicherweise für eine API im RPC-Stil.

Sie können den Aktions Namen überschreiben, indem Sie das `[ActionName]`-Attribut verwenden. Im folgenden Beispiel werden zwei Aktionen &quot;API/Products/Miniaturansicht/*ID*zugeordnet. Eine unterstützt Get und die andere unterstützt Post:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Nicht-Aktionen

Verwenden Sie das `[NonAction]`-Attribut, um zu verhindern, dass eine Methode als Aktion aufgerufen wird. Dies signalisiert dem Framework, dass es sich bei der Methode nicht um eine Aktion handelt, auch wenn Sie andernfalls den Routing Regeln entspricht.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Weitere nützliche Informationen

Dieses Thema bietet eine allgemeine Übersicht über das Routing. Weitere Informationen finden Sie unter [Routing und Aktions Auswahl](routing-and-action-selection.md), die genau beschreiben, wie das Framework mit einem URI einer Route übereinstimmt, einen Controller auswählt und dann die aufzurufende Aktion auswählt.
