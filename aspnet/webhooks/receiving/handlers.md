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
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="2299f-103">ASP.net-webhooks-Handler</span><span class="sxs-lookup"><span data-stu-id="2299f-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="2299f-104">Nachdem webhooks-Anforderungen von einem webhook-Empfänger überprüft wurden, können Sie von Benutzercode verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="2299f-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="2299f-105">An dieser Stelle kommen *Handler* ins Spiel.</span><span class="sxs-lookup"><span data-stu-id="2299f-105">This is where *handlers* come in.</span></span> <span data-ttu-id="2299f-106">Handler werden von der [iwebhost](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) -Handlerschnittstelle abgeleitet, verwenden jedoch in der Regel die [Webhost-Handler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) -Klasse, anstatt direkt von der Schnittstelle abgeleitet zu werden.</span><span class="sxs-lookup"><span data-stu-id="2299f-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="2299f-107">Eine webhookanforderung kann von einem oder mehreren Handlern verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="2299f-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="2299f-108">Handler werden in der Reihenfolge aufgerufen, basierend auf ihrer jeweiligen *Order* -Eigenschaft, die von der niedrigsten zum höchsten ist, wobei Order eine einfache Ganzzahl ist (empfohlen von 1 bis 100):</span><span class="sxs-lookup"><span data-stu-id="2299f-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Eigenschaften Diagramm für die Reihenfolge der webhooks](_static/Handlers.png)

<span data-ttu-id="2299f-110">Ein Handler kann optional die *Response* -Eigenschaft für den Webhost-handlercontext festlegen, wodurch die Verarbeitung beendet wird und die Antwort als HTTP-Antwort an den webhook zurückgesendet wird.</span><span class="sxs-lookup"><span data-stu-id="2299f-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="2299f-111">Im obigen Fall wird Handler C nicht aufgerufen, da er eine höhere Reihenfolge als B aufweist und b die Antwort festlegt.</span><span class="sxs-lookup"><span data-stu-id="2299f-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="2299f-112">Das Festlegen der Antwort ist in der Regel nur für webhooks relevant, bei denen die Antwortinformationen an die ursprüngliche API zurückgeben kann.</span><span class="sxs-lookup"><span data-stu-id="2299f-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="2299f-113">Dies ist z. b. der Fall mit Slack-webhooks, bei denen die Antwort zurück an den Kanal gesendet wird, von dem aus der webhook stammt.</span><span class="sxs-lookup"><span data-stu-id="2299f-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="2299f-114">Handler können die Eigenschaft Receiver festlegen, wenn Sie nur webhooks von diesem bestimmten Empfänger empfangen möchten.</span><span class="sxs-lookup"><span data-stu-id="2299f-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="2299f-115">Wenn Sie den Empfänger nicht festlegen, werden Sie für alle aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="2299f-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="2299f-116">Eine andere häufige Verwendung einer Antwort besteht darin, dass eine Antwort mit dem Wert " *410* " verwendet wird, um anzugeben, dass der webhook nicht mehr aktiv ist und keine weiteren Anforderungen gesendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="2299f-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="2299f-117">Standardmäßig wird ein Handler von allen webhook-Empfängern aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="2299f-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="2299f-118">Wenn die Eigenschaft *Receiver* jedoch auf den Namen eines Handlers festgelegt ist, empfängt dieser Handler nur webhook-Anforderungen von diesem Empfänger.</span><span class="sxs-lookup"><span data-stu-id="2299f-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="2299f-119">Verarbeiten eines webhooks</span><span class="sxs-lookup"><span data-stu-id="2299f-119">Processing a WebHook</span></span>

<span data-ttu-id="2299f-120">Wenn ein Handler aufgerufen wird, ruft er einen [Webhost-handlercontext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) ab, der Informationen über die webhookanforderung enthält.</span><span class="sxs-lookup"><span data-stu-id="2299f-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="2299f-121">Die Daten, in der Regel der HTTP-Anforderungs Text, sind über die *Data* -Eigenschaft verfügbar.</span><span class="sxs-lookup"><span data-stu-id="2299f-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="2299f-122">Der Typ der Daten ist in der Regel JSON-oder HTML-Formulardaten, aber es ist möglich, bei Bedarf in einen spezifischeren Typ umzuwandeln.</span><span class="sxs-lookup"><span data-stu-id="2299f-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="2299f-123">Beispielsweise können die von ASP.net webhooks generierten benutzerdefinierten webhooks wie folgt in den Typ " [CustomNotification](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) " umgewandelt werden:</span><span class="sxs-lookup"><span data-stu-id="2299f-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

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

  ## <a name="queued-processing"></a><span data-ttu-id="2299f-124">Verarbeitung in der Warteschlange</span><span class="sxs-lookup"><span data-stu-id="2299f-124">Queued Processing</span></span>

<span data-ttu-id="2299f-125">Die meisten webhook-Absender senden einen webhook erneut, wenn eine Antwort nicht innerhalb weniger Sekunden generiert wird.</span><span class="sxs-lookup"><span data-stu-id="2299f-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="2299f-126">Dies bedeutet, dass der Handler die Verarbeitung innerhalb dieses Zeitrahmens beenden muss, damit er nicht erneut aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="2299f-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="2299f-127">Wenn die Verarbeitung länger dauert oder separat verarbeitet wird, kann der [webhook-queuehandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) verwendet werden, um die webhookanforderung an eine Warteschlange zu senden, z. b. [Azure Storage Warteschlange](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="2299f-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="2299f-128">Eine Gliederung einer [Webhost-queuehandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) -Implementierung finden Sie hier:</span><span class="sxs-lookup"><span data-stu-id="2299f-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

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
