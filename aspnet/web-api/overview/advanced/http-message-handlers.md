---
uid: web-api/overview/advanced/http-message-handlers
title: HTTP-Nachrichten Handler in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: Übersicht über HTTP-Nachrichten Handler in ASP.net-Web-API für ASP.NET 4. x
ms.author: riande
ms.date: 02/13/2012
ms.custom: seoapril2019
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: a8e6f1da8df4802e1acf7779a2fc75bfe8ab876f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504927"
---
# <a name="http-message-handlers-in-aspnet-web-api"></a>HTTP-Nachrichten Handler in ASP.net-Web-API

von [Mike Wasson](https://github.com/MikeWasson)

Ein *Nachrichten Handler* ist eine Klasse, die eine HTTP-Anforderung empfängt und eine HTTP-Antwort zurückgibt. Meldungs Handler werden von der abstrakten **httpmessagehandler** -Klasse abgeleitet.

In der Regel werden eine Reihe von Meldungs Handlern verkettet. Der erste Handler empfängt eine HTTP-Anforderung, verarbeitet einige Verarbeitungsschritte und übergibt die Anforderung an den nächsten Handler. An einem bestimmten Punkt wird die Antwort erstellt, und die Kette wird wieder hergestellt. Dieses Muster wird als *delegier ender* Handler bezeichnet.

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Server seitige Meldungs Handler

Auf der Serverseite verwendet die Web-API-Pipeline einige integrierte Nachrichten Handler:

- **HTTPServer** Ruft die Anforderung vom Host ab.
- **Httproutingdispatcher** sendet die Anforderung basierend auf der Route.
- **Httpcontrollerdispatcher** sendet die Anforderung an einen Web-API-Controller.

Sie können der Pipeline benutzerdefinierte Handler hinzufügen. Meldungs Handler sind für übergreifende Aspekte geeignet, die auf der Ebene von HTTP-Nachrichten (anstelle von Controller Aktionen) funktionieren. Ein Meldungs Handler könnte beispielsweise Folgendes:

- Lesen oder Ändern von Anforderungs Headern.
- Fügen Sie Antworten einen Antwortheader hinzu.
- Überprüfen Sie die Anforderungen, bevor Sie den Controller erreichen.

Dieses Diagramm zeigt zwei benutzerdefinierte Handler, die in die Pipeline eingefügt werden:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> Auf der Clientseite verwendet httpclient auch Nachrichten Handler. Weitere Informationen finden Sie unter [HttpClient](httpclient-message-handlers.md)-Meldungs Handler.

## <a name="custom-message-handlers"></a>Benutzerdefinierte Meldungs Handler

Zum Schreiben eines benutzerdefinierten Nachrichten Handlers leiten Sie von **System .net. http. delegatinghandler** ab und überschreiben die **SendAsync** -Methode. Diese Methode hat die folgende Signatur:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

Die Methode nimmt eine **httprequestmessage** als Eingabe an und gibt asynchron eine **httpresponsmessage**zurück. Eine typische-Implementierung führt Folgendes aus:

1. Verarbeiten Sie die Anforderungs Nachricht.
2. Ruft `base.SendAsync` auf, um die Anforderung an den inneren Handler zu senden.
3. Der innere Handler gibt eine Antwortnachricht zurück. (Dieser Schritt ist asynchron.)
4. Verarbeiten Sie die Antwort und geben Sie Sie an den Aufrufer zurück.

Im folgenden finden Sie ein einfaches Beispiel:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> Der Aufruf von `base.SendAsync` ist asynchron. Wenn der Handler nach diesem-Befehl eine beliebige Aufgabe durchführt, verwenden Sie das **Erwartungs Wort,** wie gezeigt.

Ein delegier ender Handler kann auch den inneren Handler überspringen und die Antwort direkt erstellen:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Wenn ein delegier Ende Handler die Antwort erstellt, ohne `base.SendAsync`zu aufrufen, überspringt die Anforderung den Rest der Pipeline. Dies kann für einen Handler nützlich sein, der die Anforderung überprüft (Erstellen einer Fehler Antwort).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Hinzufügen eines Handlers zur Pipeline

Zum Hinzufügen eines Nachrichten Handlers auf der Serverseite fügen Sie den Handler der **httpconfiguration. messagehandlers** -Auflistung hinzu. Wenn Sie die Vorlage "ASP.NET MVC 4-Webanwendung" verwendet haben, um das Projekt zu erstellen, können Sie dies innerhalb der **webapiconfig** -Klasse tun:

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Meldungs Handler werden in derselben Reihenfolge aufgerufen, in der Sie in der **messagehandlers** -Auflistung angezeigt werden. Da Sie eingebettet sind, wird die Antwortnachricht in der anderen Richtung bewegt. Das heißt, der letzte Handler ist der erste, der die Antwortnachricht erhält.

Beachten Sie, dass Sie die inneren Handler nicht festlegen müssen. das Web-API-Framework verbindet automatisch die Nachrichten Handler.

Wenn Sie [selbst Hosting](../older-versions/self-host-a-web-api.md)haben, erstellen Sie eine Instanz der **httpselfhostconfiguration** -Klasse, und fügen Sie die Handler der **messagehandlers** -Auflistung hinzu.

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Sehen wir uns nun einige Beispiele für benutzerdefinierte Meldungs Handler an.

## <a name="example-x-http-method-override"></a>Beispiel: X-http-Method-override

X-http-Method-override ist ein nicht-Standard-HTTP-Header. Es ist für Clients konzipiert, die bestimmte HTTP-Anforderungs Typen, z. b. Put oder DELETE, nicht senden können. Stattdessen sendet der Client eine Post-Anforderung und legt den Header "X-http-Method-override" auf die gewünschte Methode fest. Beispiel:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Hier ist ein Meldungs Handler, der die Unterstützung für X-http-Method-override hinzufügt:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

In der **SendAsync** -Methode überprüft der Handler, ob die Anforderungs Nachricht eine Post-Anforderung ist und ob Sie den Header "X-http-Method-override" enthält. Wenn dies der Fall ist, wird der Header Wert überprüft und dann die Anforderungs Methode geändert. Schließlich ruft der Handler `base.SendAsync` auf, um die Nachricht an den nächsten Handler zu übergeben.

Wenn die Anforderung die **httpcontrollerdispatcher** -Klasse erreicht, leitet **httpcontrollerdispatcher** die Anforderung basierend auf der aktualisierten Anforderungs Methode weiter.

## <a name="example-adding-a-custom-response-header"></a>Beispiel: Hinzufügen eines benutzerdefinierten Antwort Headers

Hier ist ein Meldungs Handler, der jeder Antwortnachricht einen benutzerdefinierten Header hinzufügt:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

Zuerst ruft der Handler `base.SendAsync` auf, um die Anforderung an den inneren Nachrichten Handler zu übergeben. Der innere Handler gibt eine Antwortnachricht zurück, aber dies erfolgt asynchron mit einem **Task&lt;t&gt;** -Objekts. Die Antwortnachricht ist erst verfügbar, wenn `base.SendAsync` asynchron abgeschlossen ist.

In diesem Beispiel wird das **-Schlüsselwort** mit dem Erwartungs Wort zum asynchronen Ausführen von arbeiten nach Abschluss `SendAsync` verwendet Wenn Sie .NET Framework 4,0 verwenden, verwenden Sie den **Task**&lt;t&gt; **. ContinueWith** -Methode:

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Beispiel: Überprüfen auf einen API-Schlüssel

Einige Webdienste erfordern, dass Clients einen API-Schlüssel in Ihre Anforderung einschließen. Das folgende Beispiel zeigt, wie ein Nachrichten Handler Anforderungen auf einen gültigen API-Schlüssel überprüfen kann:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Dieser Handler sucht in der URI-Abfrage Zeichenfolge nach dem API-Schlüssel. (In diesem Beispiel wird davon ausgegangen, dass es sich bei dem Schlüssel um eine statische Zeichenfolge handelt. Eine wirkliche Implementierung würde wahrscheinlich eine komplexere Validierung verwenden.) Wenn die Abfrage Zeichenfolge den Schlüssel enthält, übergibt der Handler die Anforderung an den inneren Handler.

Wenn die Anforderung keinen gültigen Schlüssel hat, erstellt der Handler eine Antwortnachricht mit dem Status 403, verboten. In diesem Fall ruft der Handler `base.SendAsync`nicht auf, sodass der innere Handler die Anforderung nie empfängt, und auch nicht den Controller. Daher kann der Controller davon ausgehen, dass alle eingehenden Anforderungen über einen gültigen API-Schlüssel verfügen.

> [!NOTE]
> Wenn der API-Schlüssel nur für bestimmte Controller Aktionen gilt, empfiehlt es sich, anstelle eines Nachrichten Handlers einen Aktionsfilter zu verwenden. Aktionsfilter werden nach dem URI-Routing ausgeführt.

## <a name="per-route-message-handlers"></a>Nachrichten Handler pro Route

Handler in der **httpconfiguration. messagehandlers** -Auflistung gelten global.

Alternativ können Sie einen Nachrichten Handler zu einer bestimmten Route hinzufügen, wenn Sie die Route definieren:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

In diesem Beispiel wird die Anforderung an `MessageHandler2`gesendet, wenn der Anforderungs-URI "Route2" entspricht. Das folgende Diagramm zeigt die Pipeline für diese beiden Routen:

![](http-message-handlers/_static/image4.png)

Beachten Sie, dass `MessageHandler2` den Standardwert **httpcontrollerdispatcher**ersetzt. In diesem Beispiel erstellt `MessageHandler2` die Antwort, und Anforderungen, die "Route2" entsprechen, werden nie an einen Controller gesendet. Auf diese Weise können Sie den gesamten Web-API-Controller Mechanismus durch ihren eigenen benutzerdefinierten Endpunkt ersetzen.

Alternativ kann ein Nachrichten Handler pro Route an **httpcontrollerdispatcher**delegieren, der dann an einen Controller weitergeleitet wird.

![](http-message-handlers/_static/image5.png)

Der folgende Code zeigt, wie Sie diese Route konfigurieren:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
