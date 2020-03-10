---
uid: signalr/overview/performance/signalr-performance
title: Signalr-Leistung | Microsoft-Dokumentation
author: bradygaster
description: SignalR-Leistung
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: b8a44f4c924c94cdfff1ce7630539b45fe269bbf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449859"
---
# <a name="signalr-performance"></a><span data-ttu-id="728e1-103">SignalR-Leistung</span><span class="sxs-lookup"><span data-stu-id="728e1-103">SignalR Performance</span></span>

<span data-ttu-id="728e1-104">von [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="728e1-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="728e1-105">In diesem Thema wird beschrieben, wie Sie die Leistung in einer signalr-Anwendung entwerfen, Messen und verbessern.</span><span class="sxs-lookup"><span data-stu-id="728e1-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="728e1-106">In diesem Thema verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="728e1-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="728e1-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="728e1-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="728e1-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="728e1-108">.NET 4.5</span></span>
> - <span data-ttu-id="728e1-109">Signalr Version 2</span><span class="sxs-lookup"><span data-stu-id="728e1-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="728e1-110">Vorherige Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="728e1-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="728e1-111">Informationen zu früheren Versionen von signalr finden Sie unter [signalr ältere Versionen](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="728e1-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="728e1-112">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="728e1-112">Questions and comments</span></span>
>
> <span data-ttu-id="728e1-113">Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten.</span><span class="sxs-lookup"><span data-stu-id="728e1-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="728e1-114">Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="728e1-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="728e1-115">Eine aktuelle Präsentation der signalr-Leistung und-Skalierung finden Sie unter [Skalieren des Echt Zeit Web mit ASP.net signalr](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="728e1-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="728e1-116">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="728e1-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="728e1-117">Überlegungen zum Entwurf</span><span class="sxs-lookup"><span data-stu-id="728e1-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="728e1-118">Optimieren des signalr-Servers auf die Leistung</span><span class="sxs-lookup"><span data-stu-id="728e1-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="728e1-119">Problembehandlung bei Leistungsproblemen</span><span class="sxs-lookup"><span data-stu-id="728e1-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="728e1-120">Verwenden von signalr-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="728e1-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="728e1-121">Verwenden anderer Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="728e1-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="728e1-122">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="728e1-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="728e1-123">Überlegungen zum Entwurf</span><span class="sxs-lookup"><span data-stu-id="728e1-123">Design considerations</span></span>

<span data-ttu-id="728e1-124">In diesem Abschnitt werden Muster beschrieben, die während des Entwurfs einer signalr-Anwendung implementiert werden können, um sicherzustellen, dass die Leistung nicht beeinträchtigt wird, indem unnötiger Netzwerkverkehr erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="728e1-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="728e1-125">Häufigkeit der Drosselung von Nachrichten</span><span class="sxs-lookup"><span data-stu-id="728e1-125">Throttling message frequency</span></span>

<span data-ttu-id="728e1-126">Selbst in einer Anwendung, die Nachrichten mit hoher Häufigkeit sendet (z. b. eine Echtzeit-Gaming-Anwendung), müssen die meisten Anwendungen nicht mehr als ein paar Nachrichten pro Sekunde senden.</span><span class="sxs-lookup"><span data-stu-id="728e1-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="728e1-127">Um die Menge des von den einzelnen Clients generierten Datenverkehrs zu reduzieren, kann eine Nachrichten Schleife implementiert werden, die Nachrichten nicht häufiger als eine festgelegte Rate, sondern in eine Warteschlange eingereiht und sendet (d. h. bis zu eine bestimmte Anzahl von Nachrichten pro Sekunde gesendet werden, wenn Nachrichten in dieser Zeit vorhanden sind. das zu sendende euerstellungsintervall).</span><span class="sxs-lookup"><span data-stu-id="728e1-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="728e1-128">Eine Beispielanwendung, die Nachrichten auf eine bestimmte Rate (sowohl vom Client als auch vom Server) drosselt, finden Sie unter [Hochfrequenz in Echtzeit mit signalr](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="728e1-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="728e1-129">Verringern der Nachrichtengröße</span><span class="sxs-lookup"><span data-stu-id="728e1-129">Reducing message size</span></span>

<span data-ttu-id="728e1-130">Sie können die Größe einer signalr-Nachricht verringern, indem Sie die Größe der serialisierten Objekte verringern.</span><span class="sxs-lookup"><span data-stu-id="728e1-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="728e1-131">Wenn Sie in Servercode ein Objekt senden, das Eigenschaften enthält, die nicht übertragen werden müssen, verhindern Sie, dass diese Eigenschaften mithilfe des `JsonIgnore`-Attributs serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="728e1-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="728e1-132">Die Namen von Eigenschaften werden ebenfalls in der Nachricht gespeichert. die Namen von Eigenschaften können mit dem `JsonProperty`-Attribut verkürzt werden.</span><span class="sxs-lookup"><span data-stu-id="728e1-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="728e1-133">Im folgenden Codebeispiel wird veranschaulicht, wie eine Eigenschaft vom senden an den Client ausgeschlossen wird und wie Eigenschaftsnamen verkürzt werden:</span><span class="sxs-lookup"><span data-stu-id="728e1-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="728e1-134">**.Net-Servercode, der das jsonignore-Attribut veranschaulicht, um zu verhindern, dass Daten an den Client gesendet werden, und das jsonproperty-Attribut, um die Nachrichtengröße zu verringern**</span><span class="sxs-lookup"><span data-stu-id="728e1-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="728e1-135">Um die Lesbarkeit/Wartbarkeit im Client Code beizubehalten, können die abgekürzten Eigenschaftsnamen nach dem Empfang der Nachricht benutzerfreundlichen Namen zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="728e1-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="728e1-136">Im folgenden Codebeispiel wird eine Möglichkeit veranschaulicht, wie Sie gekürzte Namen in längere Weise neu zuordnen können, indem Sie einen Nachrichten Vertrag (Mapping) definieren und die `reMap`-Funktion verwenden, um den Vertrag auf die optimierte Nachrichten Klasse anzuwenden:</span><span class="sxs-lookup"><span data-stu-id="728e1-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="728e1-137">**Client seitiger JavaScript-Code, mit dem gekürzte Eigenschaftsnamen zu lesbaren Namen neu zugeordnet werden**</span><span class="sxs-lookup"><span data-stu-id="728e1-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="728e1-138">Namen können auch in Nachrichten vom Client an den Server mit derselben Methode verkürzt werden.</span><span class="sxs-lookup"><span data-stu-id="728e1-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="728e1-139">Wenn Sie den Speicherbedarf (d. h. die Menge an Arbeitsspeicher, der für die Nachricht verwendet wird) des Nachrichten Objekts verringern, kann auch die Leistung verbessert werden.</span><span class="sxs-lookup"><span data-stu-id="728e1-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="728e1-140">Wenn z. b. der vollständige Bereich eines `int` nicht benötigt wird, kann stattdessen ein `short` oder `byte` verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="728e1-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="728e1-141">Da Nachrichten im Nachrichtenbus im Server Speicher gespeichert werden, kann das Verringern der Größe der Nachrichten auch Probleme mit dem Server Arbeitsspeicher beheben.</span><span class="sxs-lookup"><span data-stu-id="728e1-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="728e1-142">Optimieren des signalr-Servers auf die Leistung</span><span class="sxs-lookup"><span data-stu-id="728e1-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="728e1-143">Die folgenden Konfigurationseinstellungen können verwendet werden, um Ihren Server für eine bessere Leistung in einer signalr-Anwendung zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="728e1-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="728e1-144">Allgemeine Informationen zum Verbessern der Leistung in einer ASP.NET-Anwendung finden Sie unter Verbessern der [Leistung von ASP.net](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="728e1-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="728e1-145">**Signalr-Konfigurationseinstellungen**</span><span class="sxs-lookup"><span data-stu-id="728e1-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="728e1-146">**Defaultmessagebuffersize**: Standardmäßig speichert signalr 1000 Nachrichten pro Hub und Verbindung im Arbeitsspeicher.</span><span class="sxs-lookup"><span data-stu-id="728e1-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="728e1-147">Wenn große Nachrichten verwendet werden, kann dies zu Arbeitsspeicher Problemen führen, die durch verringern dieses Werts verringert werden können.</span><span class="sxs-lookup"><span data-stu-id="728e1-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="728e1-148">Diese Einstellung kann im `Application_Start` Ereignishandler in einer ASP.NET-Anwendung oder in der `Configuration`-Methode einer owin-Startklasse in einer selbst gehosteten Anwendung festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="728e1-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="728e1-149">Im folgenden Beispiel wird veranschaulicht, wie dieser Wert verringert wird, um den Speicherbedarf Ihrer Anwendung zu verringern, um die Menge des verwendeten Server Arbeitsspeichers zu reduzieren:</span><span class="sxs-lookup"><span data-stu-id="728e1-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="728e1-150">**.Net-Servercode in Startup.cs zum Verringern der standardmäßigen Nachrichten Puffergröße**</span><span class="sxs-lookup"><span data-stu-id="728e1-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="728e1-151">**IIS-Konfigurationseinstellungen**</span><span class="sxs-lookup"><span data-stu-id="728e1-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="728e1-152">**Maximale Anzahl gleichzeitiger Anforderungen pro Anwendung**: das Erhöhen der Anzahl gleichzeitiger IIS-Anforderungen erhöht die verfügbaren Server Ressourcen für die Bereitstellung von Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="728e1-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="728e1-153">Der Standardwert ist 5000; um diese Einstellung zu erhöhen, führen Sie die folgenden Befehle an einer Eingabeaufforderung mit erhöhten Rechten aus:</span><span class="sxs-lookup"><span data-stu-id="728e1-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="728e1-154">**ApplicationPool QueueLength**: Dies ist die maximale Anzahl von Anforderungen, die http. sys für den Anwendungs Pool in die Warteschlange stellt.</span><span class="sxs-lookup"><span data-stu-id="728e1-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="728e1-155">Wenn die Warteschlange voll ist, erhalten neue Anforderungen die Antwort 503 "Dienst nicht verfügbar".</span><span class="sxs-lookup"><span data-stu-id="728e1-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="728e1-156">Der Standardwert lautet „1000“.</span><span class="sxs-lookup"><span data-stu-id="728e1-156">The default value is 1000.</span></span>

    <span data-ttu-id="728e1-157">Durch Verkürzen der Warteschlangen Länge für den Workerprozess in dem Anwendungs Pool, der Ihre Anwendung gehostet, werden Speicherressourcen gespart.</span><span class="sxs-lookup"><span data-stu-id="728e1-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="728e1-158">Weitere Informationen finden Sie unter [verwalten, optimieren und Konfigurieren von Anwendungs Pools](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="728e1-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="728e1-159">**ASP.NET-Konfigurationseinstellungen**</span><span class="sxs-lookup"><span data-stu-id="728e1-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="728e1-160">Dieser Abschnitt enthält Konfigurationseinstellungen, die in der `aspnet.config`-Datei festgelegt werden können.</span><span class="sxs-lookup"><span data-stu-id="728e1-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="728e1-161">Diese Datei befindet sich an einem von zwei Speicherorten, abhängig von der Plattform:</span><span class="sxs-lookup"><span data-stu-id="728e1-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="728e1-162">ASP.NET Einstellungen, die möglicherweise die signalr-Leistung verbessern, umfassen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="728e1-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="728e1-163">**Maximale Anzahl gleichzeitiger Anforderungen pro CPU**: durch das erhöhen dieser Einstellung können Leistungsengpässe verringert werden.</span><span class="sxs-lookup"><span data-stu-id="728e1-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="728e1-164">Fügen Sie die folgende Konfigurationseinstellung in die `aspnet.config` Datei ein, um diese Einstellung zu erhöhen:</span><span class="sxs-lookup"><span data-stu-id="728e1-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="728e1-165">**Limit für Anforderungs Warteschlange**: Wenn die Gesamtzahl der Verbindungen die `maxConcurrentRequestsPerCPU` Einstellung überschreitet, beginnt ASP.net mit der Drosselung von Anforderungen mithilfe einer Warteschlange.</span><span class="sxs-lookup"><span data-stu-id="728e1-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="728e1-166">Um die Größe der Warteschlange zu vergrößern, können Sie die `requestQueueLimit` Einstellung erhöhen.</span><span class="sxs-lookup"><span data-stu-id="728e1-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="728e1-167">Fügen Sie zu diesem Zweck dem Knoten `processModel` in `config/machine.config` (anstatt `aspnet.config`) die folgende Konfigurationseinstellung hinzu:</span><span class="sxs-lookup"><span data-stu-id="728e1-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="728e1-168">Problembehandlung bei Leistungsproblemen</span><span class="sxs-lookup"><span data-stu-id="728e1-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="728e1-169">In diesem Abschnitt werden Möglichkeiten zum Auffinden von Leistungs Engpässen in Ihrer Anwendung beschrieben.</span><span class="sxs-lookup"><span data-stu-id="728e1-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="728e1-170">Es wird überprüft, ob WebSocket verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="728e1-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="728e1-171">Obwohl signalr eine Vielzahl von Transporten für die Kommunikation zwischen Client und Server verwenden kann, bietet WebSocket einen erheblichen Leistungsvorteil und sollte verwendet werden, wenn der Client und der Server dies unterstützen.</span><span class="sxs-lookup"><span data-stu-id="728e1-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="728e1-172">Informationen zum ermitteln, ob der Client und der Server die Anforderungen für WebSocket erfüllen, finden Sie unter [Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="728e1-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="728e1-173">Um zu ermitteln, welcher Transport in der Anwendung verwendet wird, können Sie die Browser Entwicklertools verwenden und die Protokolle überprüfen, um festzustellen, welcher Transport für die Verbindung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="728e1-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="728e1-174">Informationen zur Verwendung der Browser-Entwicklungs Tools in Internet Explorer und Chrome finden Sie unter [Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="728e1-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="728e1-175">Verwenden von signalr-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="728e1-175">Using SignalR performance counters</span></span>

<span data-ttu-id="728e1-176">In diesem Abschnitt wird beschrieben, wie Sie signalr-Leistungsindikatoren aktivieren und verwenden, die im `Microsoft.AspNet.SignalR.Utils` Paket enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="728e1-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="728e1-177">Installieren von signalr. exe</span><span class="sxs-lookup"><span data-stu-id="728e1-177">Installing signalr.exe</span></span>

<span data-ttu-id="728e1-178">Leistungsindikatoren können dem Server mithilfe eines Hilfsprogramms namens signalr. exe hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="728e1-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="728e1-179">Um dieses Hilfsprogramm zu installieren, führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="728e1-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="728e1-180">**Wählen Sie** in Visual Studio Extras > **nuget-Paket-Manager** > **nuget-Pakete für Projekt Mappe verwalten aus** .</span><span class="sxs-lookup"><span data-stu-id="728e1-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="728e1-181">Suchen Sie nach **signalr. utils**, und wählen Sie installieren aus.</span><span class="sxs-lookup"><span data-stu-id="728e1-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="728e1-182">Akzeptieren Sie den Lizenzvertrag, um das Paket zu installieren.</span><span class="sxs-lookup"><span data-stu-id="728e1-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="728e1-183">"Signalr. exe" wird auf `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`installiert.</span><span class="sxs-lookup"><span data-stu-id="728e1-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="728e1-184">Installieren von Leistungsindikatoren mit "signalr. exe"</span><span class="sxs-lookup"><span data-stu-id="728e1-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="728e1-185">Um signalr-Leistungsindikatoren zu installieren, führen Sie signalr. exe an einer Eingabeaufforderung mit erhöhten Rechten mit dem folgenden Parameter aus:</span><span class="sxs-lookup"><span data-stu-id="728e1-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="728e1-186">Um signalr-Leistungsindikatoren zu entfernen, führen Sie signalr. exe an einer Eingabeaufforderung mit erhöhten Rechten mit dem folgenden Parameter aus:</span><span class="sxs-lookup"><span data-stu-id="728e1-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="728e1-187">Signalr-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="728e1-187">SignalR Performance counters</span></span>

<span data-ttu-id="728e1-188">Das Hilfsprogrammpaket installiert die folgenden Leistungsindikatoren.</span><span class="sxs-lookup"><span data-stu-id="728e1-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="728e1-189">Die "Gesamt"-Leistungsindikatoren messen die Anzahl der Ereignisse seit dem letzten Anwendungs Pool oder Server Neustart.</span><span class="sxs-lookup"><span data-stu-id="728e1-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="728e1-190">**Verbindungsmetriken**</span><span class="sxs-lookup"><span data-stu-id="728e1-190">**Connection metrics**</span></span>

<span data-ttu-id="728e1-191">Die folgenden Metriken messen die auftretenden Ereignisse der Verbindungs Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="728e1-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="728e1-192">Weitere Informationen finden Sie unter [verstehen und behandeln von Verbindungs Lebensdauer-Ereignissen](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="728e1-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="728e1-193">**Verbundene Verbindungen**</span><span class="sxs-lookup"><span data-stu-id="728e1-193">**Connections Connected**</span></span>
- <span data-ttu-id="728e1-194">**Verbindung wieder hergestellt**</span><span class="sxs-lookup"><span data-stu-id="728e1-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="728e1-195">**Verbindungen getrennt**</span><span class="sxs-lookup"><span data-stu-id="728e1-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="728e1-196">**Verbindungen aktuell**</span><span class="sxs-lookup"><span data-stu-id="728e1-196">**Connections Current**</span></span>

<span data-ttu-id="728e1-197">**Nachrichtenmetriken**</span><span class="sxs-lookup"><span data-stu-id="728e1-197">**Message metrics**</span></span>

<span data-ttu-id="728e1-198">Die folgenden Metriken messen den Nachrichtenverkehr, der von signalr generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="728e1-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="728e1-199">**Empfangene Verbindungs Nachrichten gesamt**</span><span class="sxs-lookup"><span data-stu-id="728e1-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="728e1-200">**Gesendete Verbindungs Nachrichten gesamt**</span><span class="sxs-lookup"><span data-stu-id="728e1-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="728e1-201">**Empfangene Verbindungs Nachrichten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="728e1-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="728e1-202">**Gesendete Verbindungs Nachrichten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="728e1-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="728e1-203">**Nachrichtenbus Metriken**</span><span class="sxs-lookup"><span data-stu-id="728e1-203">**Message bus metrics**</span></span>

<span data-ttu-id="728e1-204">Die folgenden Metriken messen den Datenverkehr über den internen signalr-Nachrichtenbus, die Warteschlange, in der alle eingehenden und ausgehenden signalr-Nachrichten abgelegt werden</span><span class="sxs-lookup"><span data-stu-id="728e1-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="728e1-205">Eine Meldung wird **veröffentlicht** , wenn Sie gesendet oder übertragen wird.</span><span class="sxs-lookup"><span data-stu-id="728e1-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="728e1-206">Ein **Abonnent** in diesem Kontext ist ein Abonnement für den Nachrichtenbus. Dies sollte gleich der Anzahl von Clients und dem Server selbst sein.</span><span class="sxs-lookup"><span data-stu-id="728e1-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="728e1-207">Ein **zugewiesener Worker** ist eine Komponente, die Daten an aktive Verbindungen sendet. ein **ausgelasteter Worker** sendet eine Nachricht aktiv.</span><span class="sxs-lookup"><span data-stu-id="728e1-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="728e1-208">**Empfangene Nachrichtenbus-Nachrichten gesamt**</span><span class="sxs-lookup"><span data-stu-id="728e1-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="728e1-209">**Empfangene Message Bus-Nachrichten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="728e1-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="728e1-210">**Veröffentlichte Message Bus-Nachrichten gesamt**</span><span class="sxs-lookup"><span data-stu-id="728e1-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="728e1-211">**Veröffentlichte Message Bus-Nachrichten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="728e1-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="728e1-212">**Nachrichtenbus Abonnenten aktuell**</span><span class="sxs-lookup"><span data-stu-id="728e1-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="728e1-213">**Nachrichten busabonnenten Gesamt**</span><span class="sxs-lookup"><span data-stu-id="728e1-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="728e1-214">**Nachrichtenbus Abonnenten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="728e1-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="728e1-215">**Von Message Bus zugewiesene Worker**</span><span class="sxs-lookup"><span data-stu-id="728e1-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="728e1-216">**Ausgelastete Worker für Nachrichtenbus**</span><span class="sxs-lookup"><span data-stu-id="728e1-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="728e1-217">**Message Bus-Themen aktuell**</span><span class="sxs-lookup"><span data-stu-id="728e1-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="728e1-218">**Fehlermetriken**</span><span class="sxs-lookup"><span data-stu-id="728e1-218">**Error metrics**</span></span>

<span data-ttu-id="728e1-219">Die folgenden Metriken messen die vom signalr-Nachrichten Datenverkehr generierten Fehler.</span><span class="sxs-lookup"><span data-stu-id="728e1-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="728e1-220">**Hub-Auflösungs** Fehler treten auf, wenn eine Hub-oder Hub-Methode nicht aufgelöst werden kann.</span><span class="sxs-lookup"><span data-stu-id="728e1-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="728e1-221">**Hubaufruf** Fehler sind Ausnahmen, die beim Aufrufen einer hubmethode ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="728e1-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="728e1-222">**Transport** Fehler sind Verbindungsfehler, die während einer HTTP-Anforderung oder-Antwort ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="728e1-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="728e1-223">**Fehler: alle Gesamt**</span><span class="sxs-lookup"><span data-stu-id="728e1-223">**Errors: All Total**</span></span>
- <span data-ttu-id="728e1-224">**Fehler: alle/Sek.**</span><span class="sxs-lookup"><span data-stu-id="728e1-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="728e1-225">**Fehler: Hub-Auflösung Gesamt**</span><span class="sxs-lookup"><span data-stu-id="728e1-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="728e1-226">**Fehler: Hub-Auflösung/Sek.**</span><span class="sxs-lookup"><span data-stu-id="728e1-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="728e1-227">**Fehler: hubaufruf Gesamt**</span><span class="sxs-lookup"><span data-stu-id="728e1-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="728e1-228">**Fehler: hubaufrufe/Sek.**</span><span class="sxs-lookup"><span data-stu-id="728e1-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="728e1-229">**Fehler: Gesamt Transport**</span><span class="sxs-lookup"><span data-stu-id="728e1-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="728e1-230">**Fehler: Transport/Sek.**</span><span class="sxs-lookup"><span data-stu-id="728e1-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="728e1-231">**Skalierbare Metriken**</span><span class="sxs-lookup"><span data-stu-id="728e1-231">**Scaleout metrics**</span></span>

<span data-ttu-id="728e1-232">Die folgenden Metriken Messen Datenverkehr und Fehler, die vom Anbieter für horizontales Skalieren generiert werden.</span><span class="sxs-lookup"><span data-stu-id="728e1-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="728e1-233">Ein Daten **Strom** in diesem Kontext ist eine Skalierungs Einheit, die vom Anbieter für horizontales Skalieren verwendet wird. Dies ist eine Tabelle, wenn SQL Server verwendet wird, ein Thema, wenn Service Bus verwendet wird, und ein Abonnement, wenn redis verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="728e1-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="728e1-234">Jeder Stream stellt geordnete Lese-und Schreibvorgänge sicher. ein einzelner Stream ist ein potenzieller Skalierungs Engpass. Daher kann die Anzahl der Streams angehoben werden, um den Engpass zu verringern.</span><span class="sxs-lookup"><span data-stu-id="728e1-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="728e1-235">Wenn mehrere Streams verwendet werden, verteilt signalr (Shard) Nachrichten automatisch auf eine Weise, die sicherstellt, dass die von einer beliebigen Verbindung gesendeten Nachrichten in der richtigen Reihenfolge vorliegen.</span><span class="sxs-lookup"><span data-stu-id="728e1-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="728e1-236">Mit der Einstellung [maxqueuelength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) wird die Länge der von signalr verwalteten Sende Warteschlange für horizontales Skalieren gesteuert.</span><span class="sxs-lookup"><span data-stu-id="728e1-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="728e1-237">Wenn ein Wert größer als 0 festgelegt wird, werden alle Nachrichten in einer Sende Warteschlange nacheinander an die konfigurierte Messaging-Backplane gesendet.</span><span class="sxs-lookup"><span data-stu-id="728e1-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="728e1-238">Wenn die Größe der Warteschlange die konfigurierte Länge überschreitet, tritt bei nachfolgenden Sende aufrufen sofort ein Fehler mit einer [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) auf, bis die Anzahl der Nachrichten in der Warteschlange kleiner ist als die Einstellung.</span><span class="sxs-lookup"><span data-stu-id="728e1-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="728e1-239">Die Warteschlange ist standardmäßig deaktiviert, da die implementierten Backplane in der Regel über eine eigene Warteschlange oder Fluss Steuerung verfügen.</span><span class="sxs-lookup"><span data-stu-id="728e1-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="728e1-240">Im Fall von SQL Server schränkt das Verbindungspooling die Anzahl von gesendeten Sendungen zu einem beliebigen Zeitpunkt ein.</span><span class="sxs-lookup"><span data-stu-id="728e1-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="728e1-241">Standardmäßig wird nur ein Stream für SQL Server und redis verwendet, fünf Streams werden für Service Bus verwendet, und die Warteschlange ist deaktiviert. diese Einstellungen können jedoch mithilfe der Konfiguration auf SQL Server und Service Bus geändert werden:</span><span class="sxs-lookup"><span data-stu-id="728e1-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="728e1-242">**.NET-Server Code zum Konfigurieren der Tabellen Anzahl und der Warteschlangen Länge für SQL Server Rückwand**</span><span class="sxs-lookup"><span data-stu-id="728e1-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="728e1-243">**.NET-Server Code zum Konfigurieren der Themen Anzahl und der Warteschlangen Länge für Service Bus Rückwand**</span><span class="sxs-lookup"><span data-stu-id="728e1-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="728e1-244">Ein **Puffer** Datenstrom ist ein Puffer, der in einen Fehlerzustand versetzt wurde. Wenn sich der Stream im Faulted-Zustand befindet, schlagen alle an die Rückwand gesendeten Nachrichten sofort fehl, bis der Datenstrom nicht mehr fehlerhaft ist.</span><span class="sxs-lookup"><span data-stu-id="728e1-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="728e1-245">Die **Länge der Sende Warteschlange** entspricht der Anzahl der Nachrichten, die gesendet, aber noch nicht gesendet wurden.</span><span class="sxs-lookup"><span data-stu-id="728e1-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="728e1-246">**Horizontal hochskalierte Nachrichtenbus Nachrichten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="728e1-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="728e1-247">**Gesamtanzahl von horizontal skalierbaren Streams**</span><span class="sxs-lookup"><span data-stu-id="728e1-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="728e1-248">**Hochskalierte Streams geöffnet**</span><span class="sxs-lookup"><span data-stu-id="728e1-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="728e1-249">**Skalieren von streamingstreams**</span><span class="sxs-lookup"><span data-stu-id="728e1-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="728e1-250">**ScaleOut-Fehler Gesamt**</span><span class="sxs-lookup"><span data-stu-id="728e1-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="728e1-251">**Skalierungs Fehler/Sek.**</span><span class="sxs-lookup"><span data-stu-id="728e1-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="728e1-252">**Länge der Sende Warteschlange für horizontale Skalierung**</span><span class="sxs-lookup"><span data-stu-id="728e1-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="728e1-253">Weitere Informationen zu den Leistungsindikatoren, die von diesen Indikatoren gemessen werden, finden Sie im Thema zum horizontalen hoch [Skalieren mit Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="728e1-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="728e1-254">Verwenden anderer Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="728e1-254">Using other performance counters</span></span>

<span data-ttu-id="728e1-255">Die folgenden Leistungsindikatoren können auch beim Überwachen der Leistung Ihrer Anwendung nützlich sein.</span><span class="sxs-lookup"><span data-stu-id="728e1-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="728e1-256">**Arbeitsspeicher**</span><span class="sxs-lookup"><span data-stu-id="728e1-256">**Memory**</span></span>

- <span data-ttu-id="728e1-257">.NET CLR-Speicher\\# Bytes in allen Heaps (für w3wp)</span><span class="sxs-lookup"><span data-stu-id="728e1-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="728e1-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="728e1-258">**ASP.NET**</span></span>

- <span data-ttu-id="728e1-259">ASP. NET\Aktuelle Anforderungen</span><span class="sxs-lookup"><span data-stu-id="728e1-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="728e1-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="728e1-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="728e1-261">ASP. net\abgelehnte</span><span class="sxs-lookup"><span data-stu-id="728e1-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="728e1-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="728e1-262">**CPU**</span></span>

- <span data-ttu-id="728e1-263">Prozessorinformationen\prozessorzeit</span><span class="sxs-lookup"><span data-stu-id="728e1-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="728e1-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="728e1-264">**TCP/IP**</span></span>

- <span data-ttu-id="728e1-265">TCPv6/Verbindungen hergestellt</span><span class="sxs-lookup"><span data-stu-id="728e1-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="728e1-266">TCPv4/Verbindungen hergestellt</span><span class="sxs-lookup"><span data-stu-id="728e1-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="728e1-267">**Webdienst**</span><span class="sxs-lookup"><span data-stu-id="728e1-267">**Web Service**</span></span>

- <span data-ttu-id="728e1-268">Webdienst\aktuelle Verbindungen</span><span class="sxs-lookup"><span data-stu-id="728e1-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="728e1-269">Webdienst\maximale Verbindungen</span><span class="sxs-lookup"><span data-stu-id="728e1-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="728e1-270">**Threading**</span><span class="sxs-lookup"><span data-stu-id="728e1-270">**Threading**</span></span>

- <span data-ttu-id="728e1-271">.NET CLR-Sperren und-Threads\\Anzahl der aktuellen logischen Threads</span><span class="sxs-lookup"><span data-stu-id="728e1-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="728e1-272">.NET CLR-Sperren und-Threads\\Anzahl der aktuellen physischen Threads</span><span class="sxs-lookup"><span data-stu-id="728e1-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="728e1-273">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="728e1-273">Other Resources</span></span>

<span data-ttu-id="728e1-274">Weitere Informationen zur Leistungsüberwachung und-Optimierung in ASP.net finden Sie in den folgenden Themen:</span><span class="sxs-lookup"><span data-stu-id="728e1-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="728e1-275">[ASP.NET Performance Overview (Die Leistung von ASP.NET im Überblick)](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="728e1-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="728e1-276">ASP.net Thread Verwendung auf IIS 7,5, IIS 7,0 und IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="728e1-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="728e1-277">&lt;ApplicationPool&gt; Element (Webeinstellungen)</span><span class="sxs-lookup"><span data-stu-id="728e1-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
