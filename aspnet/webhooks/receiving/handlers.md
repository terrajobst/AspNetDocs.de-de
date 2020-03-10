---
uid: webhooks/receiving/handlers
title: ASP.net-webhooks-Handler | Microsoft-Dokumentation
author: rick-anderson
description: 'Gewusst wie: Verarbeiten von Anforderungen in ASP.net-webhooks'
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518031"
---
# <a name="aspnet-webhooks-handlers"></a>ASP.net-webhooks-Handler

Nachdem webhooks-Anforderungen von einem webhook-Empfänger überprüft wurden, können Sie von Benutzercode verarbeitet werden. An dieser Stelle kommen *Handler* ins Spiel. Handler werden von der [iwebhost](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) -Handlerschnittstelle abgeleitet, verwenden jedoch in der Regel die [Webhost-Handler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) -Klasse, anstatt direkt von der Schnittstelle abgeleitet zu werden.

Eine webhookanforderung kann von einem oder mehreren Handlern verarbeitet werden. Handler werden in der Reihenfolge aufgerufen, basierend auf ihrer jeweiligen *Order* -Eigenschaft, die von der niedrigsten zum höchsten ist, wobei Order eine einfache Ganzzahl ist (empfohlen von 1 bis 100):

![Eigenschaften Diagramm für die Reihenfolge der webhooks](_static/Handlers.png)

Ein Handler kann optional die *Response* -Eigenschaft für den Webhost-handlercontext festlegen, wodurch die Verarbeitung beendet wird und die Antwort als HTTP-Antwort an den webhook zurückgesendet wird. Im obigen Fall wird Handler C nicht aufgerufen, da er eine höhere Reihenfolge als B aufweist und b die Antwort festlegt.

Das Festlegen der Antwort ist in der Regel nur für webhooks relevant, bei denen die Antwortinformationen an die ursprüngliche API zurückgeben kann. Dies ist z. b. der Fall mit Slack-webhooks, bei denen die Antwort zurück an den Kanal gesendet wird, von dem aus der webhook stammt. Handler können die Eigenschaft Receiver festlegen, wenn Sie nur webhooks von diesem bestimmten Empfänger empfangen möchten. Wenn Sie den Empfänger nicht festlegen, werden Sie für alle aufgerufen.

Eine andere häufige Verwendung einer Antwort besteht darin, dass eine Antwort mit dem Wert " *410* " verwendet wird, um anzugeben, dass der webhook nicht mehr aktiv ist und keine weiteren Anforderungen gesendet werden sollen.

Standardmäßig wird ein Handler von allen webhook-Empfängern aufgerufen. Wenn die Eigenschaft *Receiver* jedoch auf den Namen eines Handlers festgelegt ist, empfängt dieser Handler nur webhook-Anforderungen von diesem Empfänger.

## <a name="processing-a-webhook"></a>Verarbeiten eines webhooks

Wenn ein Handler aufgerufen wird, ruft er einen [Webhost-handlercontext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) ab, der Informationen über die webhookanforderung enthält. Die Daten, in der Regel der HTTP-Anforderungs Text, sind über die *Data* -Eigenschaft verfügbar.

Der Typ der Daten ist in der Regel JSON-oder HTML-Formulardaten, aber es ist möglich, bei Bedarf in einen spezifischeren Typ umzuwandeln. Beispielsweise können die von ASP.net webhooks generierten benutzerdefinierten webhooks wie folgt in den Typ " [CustomNotification](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) " umgewandelt werden:

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a>Verarbeitung in der Warteschlange

Die meisten webhook-Absender senden einen webhook erneut, wenn eine Antwort nicht innerhalb weniger Sekunden generiert wird. Dies bedeutet, dass der Handler die Verarbeitung innerhalb dieses Zeitrahmens beenden muss, damit er nicht erneut aufgerufen werden kann.

Wenn die Verarbeitung länger dauert oder separat verarbeitet wird, kann der [webhook-queuehandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) verwendet werden, um die webhookanforderung an eine Warteschlange zu senden, z. b. [Azure Storage Warteschlange](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Eine Gliederung einer [Webhost-queuehandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) -Implementierung finden Sie hier:

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
