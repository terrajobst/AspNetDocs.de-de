---
uid: web-api/overview/advanced/httpclient-message-handlers
title: HttpClient-Meldungs Handler in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: Erstellen von benutzerdefinierten Meldungs Handlern für ASP.net-Web-API in ASP.NET 4. x
ms.author: riande
ms.date: 10/01/2012
ms.custom: seoapril2019
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 265bd9b2f48ed7d1e955f3c4947d10fd589b3e17
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449277"
---
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="916d5-103">HttpClient-Meldungs Handler in ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="916d5-103">HttpClient Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="916d5-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="916d5-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="916d5-105">Ein *Nachrichten Handler* ist eine Klasse, die eine HTTP-Anforderung empfängt und eine HTTP-Antwort zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="916d5-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="916d5-106">In der Regel werden eine Reihe von Meldungs Handlern verkettet.</span><span class="sxs-lookup"><span data-stu-id="916d5-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="916d5-107">Der erste Handler empfängt eine HTTP-Anforderung, verarbeitet einige Verarbeitungsschritte und übergibt die Anforderung an den nächsten Handler.</span><span class="sxs-lookup"><span data-stu-id="916d5-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="916d5-108">An einem bestimmten Punkt wird die Antwort erstellt, und die Kette wird wieder hergestellt.</span><span class="sxs-lookup"><span data-stu-id="916d5-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="916d5-109">Dieses Muster wird als *delegier ender* Handler bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="916d5-109">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="916d5-110">Auf der Clientseite verwendet die **HttpClient** -Klasse einen Meldungs Handler zum Verarbeiten von Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="916d5-110">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="916d5-111">Der Standard Handler ist **httpclienthandler**, der die Anforderung über das Netzwerk sendet und die Antwort vom Server erhält.</span><span class="sxs-lookup"><span data-stu-id="916d5-111">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="916d5-112">Sie können benutzerdefinierte Meldungs Handler in die Client Pipeline einfügen:</span><span class="sxs-lookup"><span data-stu-id="916d5-112">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="916d5-113">ASP.net-Web-API verwendet auch Nachrichten Handler auf der Serverseite.</span><span class="sxs-lookup"><span data-stu-id="916d5-113">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="916d5-114">Weitere Informationen finden Sie unter [http-Nachrichten Handler](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="916d5-114">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="916d5-115">Benutzerdefinierte Meldungs Handler</span><span class="sxs-lookup"><span data-stu-id="916d5-115">Custom Message Handlers</span></span>

<span data-ttu-id="916d5-116">Zum Schreiben eines benutzerdefinierten Nachrichten Handlers leiten Sie von **System .net. http. delegatinghandler** ab und überschreiben die **SendAsync** -Methode.</span><span class="sxs-lookup"><span data-stu-id="916d5-116">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="916d5-117">Hier die Signatur der Methode:</span><span class="sxs-lookup"><span data-stu-id="916d5-117">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="916d5-118">Die Methode nimmt eine **httprequestmessage** als Eingabe an und gibt asynchron eine **httpresponsmessage**zurück.</span><span class="sxs-lookup"><span data-stu-id="916d5-118">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="916d5-119">Eine typische-Implementierung führt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="916d5-119">A typical implementation does the following:</span></span>

1. <span data-ttu-id="916d5-120">Verarbeiten Sie die Anforderungs Nachricht.</span><span class="sxs-lookup"><span data-stu-id="916d5-120">Process the request message.</span></span>
2. <span data-ttu-id="916d5-121">Ruft `base.SendAsync` auf, um die Anforderung an den inneren Handler zu senden.</span><span class="sxs-lookup"><span data-stu-id="916d5-121">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="916d5-122">Der innere Handler gibt eine Antwortnachricht zurück.</span><span class="sxs-lookup"><span data-stu-id="916d5-122">The inner handler returns a response message.</span></span> <span data-ttu-id="916d5-123">(Dieser Schritt ist asynchron.)</span><span class="sxs-lookup"><span data-stu-id="916d5-123">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="916d5-124">Verarbeiten Sie die Antwort und geben Sie Sie an den Aufrufer zurück.</span><span class="sxs-lookup"><span data-stu-id="916d5-124">Process the response and return it to the caller.</span></span>

<span data-ttu-id="916d5-125">Das folgende Beispiel zeigt einen Nachrichten Handler, der der ausgehenden Anforderung einen benutzerdefinierten Header hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="916d5-125">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="916d5-126">Der Aufruf von `base.SendAsync` ist asynchron.</span><span class="sxs-lookup"><span data-stu-id="916d5-126">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="916d5-127">Wenn der Handler nach diesem-Befehl eine beliebige Aufgabe ausführt, verwenden Sie das **Erwartungs Wort, um die Ausführung** nach Abschluss der-Methode fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="916d5-127">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="916d5-128">Das folgende Beispiel zeigt einen Handler, der Fehlercodes protokolliert.</span><span class="sxs-lookup"><span data-stu-id="916d5-128">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="916d5-129">Die Protokollierung selbst ist nicht sehr interessant, aber das Beispiel zeigt, wie Sie die Antwort innerhalb des Handlers erhalten.</span><span class="sxs-lookup"><span data-stu-id="916d5-129">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="916d5-130">Hinzufügen von Meldungs Handlern zur Client Pipeline</span><span class="sxs-lookup"><span data-stu-id="916d5-130">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="916d5-131">Verwenden Sie die **httpclientfactory. Create** -Methode, um **HttpClient**benutzerdefinierte Handler hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="916d5-131">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="916d5-132">Meldungs Handler werden in der Reihenfolge aufgerufen, in der Sie Sie an die **Create** -Methode übergeben.</span><span class="sxs-lookup"><span data-stu-id="916d5-132">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="916d5-133">Da Handler Schaltflächen sind, wird die Antwortnachricht in der anderen Richtung bewegt.</span><span class="sxs-lookup"><span data-stu-id="916d5-133">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="916d5-134">Das heißt, der letzte Handler ist der erste, der die Antwortnachricht erhält.</span><span class="sxs-lookup"><span data-stu-id="916d5-134">That is, the last handler is the first to get the response message.</span></span>
