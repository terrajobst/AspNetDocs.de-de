---
uid: signalr/overview/older-versions/signalr-performance
title: Signalr-Leistung (signalr 1. x) | Microsoft-Dokumentation
author: bradygaster
description: SignalR-Leistung
ms.author: bradyg
ms.date: 07/03/2013
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 915fd822caae9bbcb0a688c6dd7a5b2bda12c9df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468105"
---
# <a name="signalr-performance-signalr-1x"></a><span data-ttu-id="46c0a-103">SignalR-Leistung (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="46c0a-103">SignalR Performance (SignalR 1.x)</span></span>

<span data-ttu-id="46c0a-104">von [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="46c0a-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="46c0a-105">In diesem Thema wird beschrieben, wie Sie die Leistung in einer signalr-Anwendung entwerfen, Messen und verbessern.</span><span class="sxs-lookup"><span data-stu-id="46c0a-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>

<span data-ttu-id="46c0a-106">Eine aktuelle Präsentation der signalr-Leistung und-Skalierung finden Sie unter [Skalieren des Echt Zeit Web mit ASP.net signalr](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="46c0a-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="46c0a-107">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="46c0a-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="46c0a-108">Überlegungen zum Entwurf</span><span class="sxs-lookup"><span data-stu-id="46c0a-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="46c0a-109">Optimieren des signalr-Servers auf die Leistung</span><span class="sxs-lookup"><span data-stu-id="46c0a-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="46c0a-110">Problembehandlung bei Leistungsproblemen</span><span class="sxs-lookup"><span data-stu-id="46c0a-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="46c0a-111">Verwenden von signalr-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="46c0a-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="46c0a-112">Verwenden anderer Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="46c0a-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="46c0a-113">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="46c0a-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="46c0a-114">Überlegungen zum Entwurf</span><span class="sxs-lookup"><span data-stu-id="46c0a-114">Design considerations</span></span>

<span data-ttu-id="46c0a-115">In diesem Abschnitt werden Muster beschrieben, die während des Entwurfs einer signalr-Anwendung implementiert werden können, um sicherzustellen, dass die Leistung nicht beeinträchtigt wird, indem unnötiger Netzwerkverkehr erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="46c0a-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="46c0a-116">Häufigkeit der Drosselung von Nachrichten</span><span class="sxs-lookup"><span data-stu-id="46c0a-116">Throttling message frequency</span></span>

<span data-ttu-id="46c0a-117">Selbst in einer Anwendung, die Nachrichten mit hoher Häufigkeit sendet (z. b. eine Echtzeit-Gaming-Anwendung), müssen die meisten Anwendungen nicht mehr als ein paar Nachrichten pro Sekunde senden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="46c0a-118">Um die Menge des von den einzelnen Clients generierten Datenverkehrs zu reduzieren, kann eine Nachrichten Schleife implementiert werden, die Nachrichten nicht häufiger als eine festgelegte Rate, sondern in eine Warteschlange eingereiht und sendet (d. h. bis zu eine bestimmte Anzahl von Nachrichten pro Sekunde gesendet werden, wenn Nachrichten in dieser Zeit vorhanden sind. das zu sendende euerstellungsintervall).</span><span class="sxs-lookup"><span data-stu-id="46c0a-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="46c0a-119">Eine Beispielanwendung, die Nachrichten auf eine bestimmte Rate (sowohl vom Client als auch vom Server) drosselt, finden Sie unter [Hochfrequenz in Echtzeit mit signalr](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="46c0a-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="46c0a-120">Verringern der Nachrichtengröße</span><span class="sxs-lookup"><span data-stu-id="46c0a-120">Reducing message size</span></span>

<span data-ttu-id="46c0a-121">Sie können die Größe einer signalr-Nachricht verringern, indem Sie die Größe der serialisierten Objekte verringern.</span><span class="sxs-lookup"><span data-stu-id="46c0a-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="46c0a-122">Wenn Sie in Servercode ein Objekt senden, das Eigenschaften enthält, die nicht übertragen werden müssen, verhindern Sie, dass diese Eigenschaften mithilfe des `JsonIgnore`-Attributs serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="46c0a-123">Die Namen von Eigenschaften werden ebenfalls in der Nachricht gespeichert. die Namen von Eigenschaften können mit dem `JsonProperty`-Attribut verkürzt werden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="46c0a-124">Im folgenden Codebeispiel wird veranschaulicht, wie eine Eigenschaft vom senden an den Client ausgeschlossen wird und wie Eigenschaftsnamen verkürzt werden:</span><span class="sxs-lookup"><span data-stu-id="46c0a-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="46c0a-125">**.Net-Servercode, der das jsonignore-Attribut veranschaulicht, um zu verhindern, dass Daten an den Client gesendet werden, und das jsonproperty-Attribut, um die Nachrichtengröße zu verringern**</span><span class="sxs-lookup"><span data-stu-id="46c0a-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="46c0a-126">Um die Lesbarkeit/Wartbarkeit im Client Code beizubehalten, können die abgekürzten Eigenschaftsnamen nach dem Empfang der Nachricht benutzerfreundlichen Namen zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="46c0a-127">Im folgenden Codebeispiel wird eine Möglichkeit veranschaulicht, wie Sie gekürzte Namen in längere Weise neu zuordnen können, indem Sie einen Nachrichten Vertrag (Mapping) definieren und die `reMap`-Funktion verwenden, um den Vertrag auf die optimierte Nachrichten Klasse anzuwenden:</span><span class="sxs-lookup"><span data-stu-id="46c0a-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="46c0a-128">**Client seitiger JavaScript-Code, mit dem gekürzte Eigenschaftsnamen zu lesbaren Namen neu zugeordnet werden**</span><span class="sxs-lookup"><span data-stu-id="46c0a-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="46c0a-129">Namen können auch in Nachrichten vom Client an den Server mit derselben Methode verkürzt werden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="46c0a-130">Wenn Sie den Speicherbedarf (d. h. die Menge an Arbeitsspeicher, der für die Nachricht verwendet wird) des Nachrichten Objekts verringern, kann auch die Leistung verbessert werden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="46c0a-131">Wenn z. b. der vollständige Bereich eines `int` nicht benötigt wird, kann stattdessen ein `short` oder `byte` verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="46c0a-132">Da Nachrichten im Nachrichtenbus im Server Speicher gespeichert werden, kann das Verringern der Größe der Nachrichten auch Probleme mit dem Server Arbeitsspeicher beheben.</span><span class="sxs-lookup"><span data-stu-id="46c0a-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="46c0a-133">Optimieren des signalr-Servers auf die Leistung</span><span class="sxs-lookup"><span data-stu-id="46c0a-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="46c0a-134">Die folgenden Konfigurationseinstellungen können verwendet werden, um Ihren Server für eine bessere Leistung in einer signalr-Anwendung zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="46c0a-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="46c0a-135">Allgemeine Informationen zum Verbessern der Leistung in einer ASP.NET-Anwendung finden Sie unter Verbessern der [Leistung von ASP.net](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="46c0a-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="46c0a-136">**Signalr-Konfigurationseinstellungen**</span><span class="sxs-lookup"><span data-stu-id="46c0a-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="46c0a-137">**Defaultmessagebuffersize**: Standardmäßig speichert signalr 1000 Nachrichten pro Hub und Verbindung im Arbeitsspeicher.</span><span class="sxs-lookup"><span data-stu-id="46c0a-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="46c0a-138">Wenn große Nachrichten verwendet werden, kann dies zu Arbeitsspeicher Problemen führen, die durch verringern dieses Werts verringert werden können.</span><span class="sxs-lookup"><span data-stu-id="46c0a-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="46c0a-139">Diese Einstellung kann im `Application_Start` Ereignishandler in einer ASP.NET-Anwendung oder in der `Configuration`-Methode einer owin-Startklasse in einer selbst gehosteten Anwendung festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="46c0a-140">Im folgenden Beispiel wird veranschaulicht, wie dieser Wert verringert wird, um den Speicherbedarf Ihrer Anwendung zu verringern, um die Menge des verwendeten Server Arbeitsspeichers zu reduzieren:</span><span class="sxs-lookup"><span data-stu-id="46c0a-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="46c0a-141">**.Net-Servercode in "Global. asax" zum Verringern der Standard Nachrichten Puffergröße**</span><span class="sxs-lookup"><span data-stu-id="46c0a-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="46c0a-142">**IIS-Konfigurationseinstellungen**</span><span class="sxs-lookup"><span data-stu-id="46c0a-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="46c0a-143">**Maximale Anzahl gleichzeitiger Anforderungen pro Anwendung**: das Erhöhen der Anzahl gleichzeitiger IIS-Anforderungen erhöht die verfügbaren Server Ressourcen für die Bereitstellung von Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="46c0a-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="46c0a-144">Der Standardwert ist 5000; um diese Einstellung zu erhöhen, führen Sie die folgenden Befehle an einer Eingabeaufforderung mit erhöhten Rechten aus:</span><span class="sxs-lookup"><span data-stu-id="46c0a-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="46c0a-145">**ASP.NET-Konfigurationseinstellungen**</span><span class="sxs-lookup"><span data-stu-id="46c0a-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="46c0a-146">Dieser Abschnitt enthält Konfigurationseinstellungen, die in der `aspnet.config`-Datei festgelegt werden können.</span><span class="sxs-lookup"><span data-stu-id="46c0a-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="46c0a-147">Diese Datei befindet sich an einem von zwei Speicherorten, abhängig von der Plattform:</span><span class="sxs-lookup"><span data-stu-id="46c0a-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="46c0a-148">ASP.NET Einstellungen, die möglicherweise die signalr-Leistung verbessern, umfassen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="46c0a-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="46c0a-149">**Maximale Anzahl gleichzeitiger Anforderungen pro CPU**: durch das erhöhen dieser Einstellung können Leistungsengpässe verringert werden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="46c0a-150">Fügen Sie die folgende Konfigurationseinstellung in die `aspnet.config` Datei ein, um diese Einstellung zu erhöhen:</span><span class="sxs-lookup"><span data-stu-id="46c0a-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="46c0a-151">**Limit für Anforderungs Warteschlange**: Wenn die Gesamtzahl der Verbindungen die `maxConcurrentRequestsPerCPU` Einstellung überschreitet, beginnt ASP.net mit der Drosselung von Anforderungen mithilfe einer Warteschlange.</span><span class="sxs-lookup"><span data-stu-id="46c0a-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="46c0a-152">Um die Größe der Warteschlange zu vergrößern, können Sie die `requestQueueLimit` Einstellung erhöhen.</span><span class="sxs-lookup"><span data-stu-id="46c0a-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="46c0a-153">Fügen Sie zu diesem Zweck dem Knoten `processModel` in `config/machine.config` (anstatt `aspnet.config`) die folgende Konfigurationseinstellung hinzu:</span><span class="sxs-lookup"><span data-stu-id="46c0a-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="46c0a-154">Problembehandlung bei Leistungsproblemen</span><span class="sxs-lookup"><span data-stu-id="46c0a-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="46c0a-155">In diesem Abschnitt werden Möglichkeiten zum Auffinden von Leistungs Engpässen in Ihrer Anwendung beschrieben.</span><span class="sxs-lookup"><span data-stu-id="46c0a-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="46c0a-156">Es wird überprüft, ob WebSocket verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="46c0a-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="46c0a-157">Obwohl signalr eine Vielzahl von Transporten für die Kommunikation zwischen Client und Server verwenden kann, bietet WebSocket einen erheblichen Leistungsvorteil und sollte verwendet werden, wenn der Client und der Server dies unterstützen.</span><span class="sxs-lookup"><span data-stu-id="46c0a-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="46c0a-158">Informationen zum ermitteln, ob der Client und der Server die Anforderungen für WebSocket erfüllen, finden Sie unter [Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="46c0a-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="46c0a-159">Um zu ermitteln, welcher Transport in der Anwendung verwendet wird, können Sie die Browser Entwicklertools verwenden und die Protokolle überprüfen, um festzustellen, welcher Transport für die Verbindung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="46c0a-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="46c0a-160">Informationen zur Verwendung der Browser-Entwicklungs Tools in Internet Explorer und Chrome finden Sie unter [Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="46c0a-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="46c0a-161">Verwenden von signalr-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="46c0a-161">Using SignalR performance counters</span></span>

<span data-ttu-id="46c0a-162">In diesem Abschnitt wird beschrieben, wie Sie signalr-Leistungsindikatoren aktivieren und verwenden, die im `Microsoft.AspNet.SignalR.Utils` Paket enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="46c0a-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="46c0a-163">Installieren von signalr. exe</span><span class="sxs-lookup"><span data-stu-id="46c0a-163">Installing signalr.exe</span></span>

<span data-ttu-id="46c0a-164">Leistungsindikatoren können dem Server mithilfe eines Hilfsprogramms namens signalr. exe hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-164">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="46c0a-165">Um dieses Hilfsprogramm zu installieren, führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="46c0a-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="46c0a-166">**Wählen Sie** in Visual Studio Extras > **nuget-Paket-Manager** > **nuget-Pakete für Projekt Mappe verwalten aus** .</span><span class="sxs-lookup"><span data-stu-id="46c0a-166">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="46c0a-167">Suchen Sie nach **signalr. utils**, und wählen Sie installieren aus.</span><span class="sxs-lookup"><span data-stu-id="46c0a-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="46c0a-168">Akzeptieren Sie den Lizenzvertrag, um das Paket zu installieren.</span><span class="sxs-lookup"><span data-stu-id="46c0a-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="46c0a-169">"Signalr. exe" wird auf `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`installiert.</span><span class="sxs-lookup"><span data-stu-id="46c0a-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="46c0a-170">Installieren von Leistungsindikatoren mit "signalr. exe"</span><span class="sxs-lookup"><span data-stu-id="46c0a-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="46c0a-171">Um signalr-Leistungsindikatoren zu installieren, führen Sie signalr. exe an einer Eingabeaufforderung mit erhöhten Rechten mit dem folgenden Parameter aus:</span><span class="sxs-lookup"><span data-stu-id="46c0a-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="46c0a-172">Um signalr-Leistungsindikatoren zu entfernen, führen Sie signalr. exe an einer Eingabeaufforderung mit erhöhten Rechten mit dem folgenden Parameter aus:</span><span class="sxs-lookup"><span data-stu-id="46c0a-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="46c0a-173">Signalr-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="46c0a-173">SignalR Performance counters</span></span>

<span data-ttu-id="46c0a-174">Das Hilfsprogrammpaket installiert die folgenden Leistungsindikatoren.</span><span class="sxs-lookup"><span data-stu-id="46c0a-174">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="46c0a-175">Die "Gesamt"-Leistungsindikatoren messen die Anzahl der Ereignisse seit dem letzten Anwendungs Pool oder Server Neustart.</span><span class="sxs-lookup"><span data-stu-id="46c0a-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="46c0a-176">**Verbindungsmetriken**</span><span class="sxs-lookup"><span data-stu-id="46c0a-176">**Connection metrics**</span></span>

<span data-ttu-id="46c0a-177">Die folgenden Metriken messen die auftretenden Ereignisse der Verbindungs Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="46c0a-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="46c0a-178">Weitere Informationen finden Sie unter [verstehen und behandeln von Verbindungs Lebensdauer-Ereignissen](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="46c0a-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="46c0a-179">**Verbundene Verbindungen**</span><span class="sxs-lookup"><span data-stu-id="46c0a-179">**Connections Connected**</span></span>
- <span data-ttu-id="46c0a-180">**Verbindung wieder hergestellt**</span><span class="sxs-lookup"><span data-stu-id="46c0a-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="46c0a-181">**Verbindungen getrennt**</span><span class="sxs-lookup"><span data-stu-id="46c0a-181">**Connections Disconnected**</span></span>
- <span data-ttu-id="46c0a-182">**Verbindungen aktuell**</span><span class="sxs-lookup"><span data-stu-id="46c0a-182">**Connections Current**</span></span>

<span data-ttu-id="46c0a-183">**Nachrichtenmetriken**</span><span class="sxs-lookup"><span data-stu-id="46c0a-183">**Message metrics**</span></span>

<span data-ttu-id="46c0a-184">Die folgenden Metriken messen den Nachrichtenverkehr, der von signalr generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="46c0a-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="46c0a-185">**Empfangene Verbindungs Nachrichten gesamt**</span><span class="sxs-lookup"><span data-stu-id="46c0a-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="46c0a-186">**Gesendete Verbindungs Nachrichten gesamt**</span><span class="sxs-lookup"><span data-stu-id="46c0a-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="46c0a-187">**Empfangene Verbindungs Nachrichten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="46c0a-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="46c0a-188">**Gesendete Verbindungs Nachrichten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="46c0a-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="46c0a-189">**Nachrichtenbus Metriken**</span><span class="sxs-lookup"><span data-stu-id="46c0a-189">**Message bus metrics**</span></span>

<span data-ttu-id="46c0a-190">Die folgenden Metriken messen den Datenverkehr über den internen signalr-Nachrichtenbus, die Warteschlange, in der alle eingehenden und ausgehenden signalr-Nachrichten abgelegt werden</span><span class="sxs-lookup"><span data-stu-id="46c0a-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="46c0a-191">Eine Meldung wird **veröffentlicht** , wenn Sie gesendet oder übertragen wird.</span><span class="sxs-lookup"><span data-stu-id="46c0a-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="46c0a-192">Ein **Abonnent** in diesem Kontext ist ein Abonnement für den Nachrichtenbus. Dies sollte gleich der Anzahl von Clients und dem Server selbst sein.</span><span class="sxs-lookup"><span data-stu-id="46c0a-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="46c0a-193">Ein **zugewiesener Worker** ist eine Komponente, die Daten an aktive Verbindungen sendet. ein **ausgelasteter Worker** sendet eine Nachricht aktiv.</span><span class="sxs-lookup"><span data-stu-id="46c0a-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="46c0a-194">**Empfangene Nachrichtenbus-Nachrichten gesamt**</span><span class="sxs-lookup"><span data-stu-id="46c0a-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="46c0a-195">**Empfangene Message Bus-Nachrichten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="46c0a-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="46c0a-196">**Veröffentlichte Message Bus-Nachrichten gesamt**</span><span class="sxs-lookup"><span data-stu-id="46c0a-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="46c0a-197">**Veröffentlichte Message Bus-Nachrichten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="46c0a-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="46c0a-198">**Nachrichtenbus Abonnenten aktuell**</span><span class="sxs-lookup"><span data-stu-id="46c0a-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="46c0a-199">**Nachrichten busabonnenten Gesamt**</span><span class="sxs-lookup"><span data-stu-id="46c0a-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="46c0a-200">**Nachrichtenbus Abonnenten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="46c0a-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="46c0a-201">**Von Message Bus zugewiesene Worker**</span><span class="sxs-lookup"><span data-stu-id="46c0a-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="46c0a-202">**Ausgelastete Worker für Nachrichtenbus**</span><span class="sxs-lookup"><span data-stu-id="46c0a-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="46c0a-203">**Message Bus-Themen aktuell**</span><span class="sxs-lookup"><span data-stu-id="46c0a-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="46c0a-204">**Fehlermetriken**</span><span class="sxs-lookup"><span data-stu-id="46c0a-204">**Error metrics**</span></span>

<span data-ttu-id="46c0a-205">Die folgenden Metriken messen die vom signalr-Nachrichten Datenverkehr generierten Fehler.</span><span class="sxs-lookup"><span data-stu-id="46c0a-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="46c0a-206">**Hub-Auflösungs** Fehler treten auf, wenn eine Hub-oder Hub-Methode nicht aufgelöst werden kann.</span><span class="sxs-lookup"><span data-stu-id="46c0a-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="46c0a-207">**Hubaufruf** Fehler sind Ausnahmen, die beim Aufrufen einer hubmethode ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="46c0a-208">**Transport** Fehler sind Verbindungsfehler, die während einer HTTP-Anforderung oder-Antwort ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="46c0a-209">**Fehler: alle Gesamt**</span><span class="sxs-lookup"><span data-stu-id="46c0a-209">**Errors: All Total**</span></span>
- <span data-ttu-id="46c0a-210">**Fehler: alle/Sek.**</span><span class="sxs-lookup"><span data-stu-id="46c0a-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="46c0a-211">**Fehler: Hub-Auflösung Gesamt**</span><span class="sxs-lookup"><span data-stu-id="46c0a-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="46c0a-212">**Fehler: Hub-Auflösung/Sek.**</span><span class="sxs-lookup"><span data-stu-id="46c0a-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="46c0a-213">**Fehler: hubaufruf Gesamt**</span><span class="sxs-lookup"><span data-stu-id="46c0a-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="46c0a-214">**Fehler: hubaufrufe/Sek.**</span><span class="sxs-lookup"><span data-stu-id="46c0a-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="46c0a-215">**Fehler: Gesamt Transport**</span><span class="sxs-lookup"><span data-stu-id="46c0a-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="46c0a-216">**Fehler: Transport/Sek.**</span><span class="sxs-lookup"><span data-stu-id="46c0a-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="46c0a-217">**Skalierbare Metriken**</span><span class="sxs-lookup"><span data-stu-id="46c0a-217">**Scaleout metrics**</span></span>

<span data-ttu-id="46c0a-218">Die folgenden Metriken Messen Datenverkehr und Fehler, die vom Anbieter für horizontales Skalieren generiert werden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="46c0a-219">Ein Daten **Strom** in diesem Kontext ist eine Skalierungs Einheit, die vom Anbieter für horizontales Skalieren verwendet wird. Dies ist eine Tabelle, wenn SQL Server verwendet wird, ein Thema, wenn Service Bus verwendet wird, und ein Abonnement, wenn redis verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="46c0a-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="46c0a-220">Standardmäßig wird nur ein Stream verwendet. Dies kann jedoch durch Konfiguration auf SQL Server und Service Bus gesteigert werden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="46c0a-221">Ein **Puffer** Datenstrom ist ein Puffer, der in einen Fehlerzustand versetzt wurde. Wenn sich der Stream im Faulted-Zustand befindet, schlagen alle an die Rückwand gesendeten Nachrichten sofort fehl, bis der Datenstrom nicht mehr fehlerhaft ist.</span><span class="sxs-lookup"><span data-stu-id="46c0a-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="46c0a-222">Die **Länge der Sende Warteschlange** entspricht der Anzahl der Nachrichten, die gesendet, aber noch nicht gesendet wurden.</span><span class="sxs-lookup"><span data-stu-id="46c0a-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="46c0a-223">**Horizontal hochskalierte Nachrichtenbus Nachrichten/Sek.**</span><span class="sxs-lookup"><span data-stu-id="46c0a-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="46c0a-224">**Gesamtanzahl von horizontal skalierbaren Streams**</span><span class="sxs-lookup"><span data-stu-id="46c0a-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="46c0a-225">**Hochskalierte Streams geöffnet**</span><span class="sxs-lookup"><span data-stu-id="46c0a-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="46c0a-226">**Skalieren von streamingstreams**</span><span class="sxs-lookup"><span data-stu-id="46c0a-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="46c0a-227">**ScaleOut-Fehler Gesamt**</span><span class="sxs-lookup"><span data-stu-id="46c0a-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="46c0a-228">**Skalierungs Fehler/Sek.**</span><span class="sxs-lookup"><span data-stu-id="46c0a-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="46c0a-229">**Länge der Sende Warteschlange für horizontale Skalierung**</span><span class="sxs-lookup"><span data-stu-id="46c0a-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="46c0a-230">Weitere Informationen zu den Leistungsindikatoren, die von diesen Indikatoren gemessen werden, finden Sie im Thema zum horizontalen hoch [Skalieren mit Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="46c0a-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="46c0a-231">Verwenden anderer Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="46c0a-231">Using other performance counters</span></span>

<span data-ttu-id="46c0a-232">Die folgenden Leistungsindikatoren können auch beim Überwachen der Leistung Ihrer Anwendung nützlich sein.</span><span class="sxs-lookup"><span data-stu-id="46c0a-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="46c0a-233">**Arbeitsspeicher**</span><span class="sxs-lookup"><span data-stu-id="46c0a-233">**Memory**</span></span>

- <span data-ttu-id="46c0a-234">.NET CLR-Speicher Anzahl von Bytes in allen Heaps (für w3wp)</span><span class="sxs-lookup"><span data-stu-id="46c0a-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="46c0a-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="46c0a-235">**ASP.NET**</span></span>

- <span data-ttu-id="46c0a-236">ASP. NET\Aktuelle Anforderungen</span><span class="sxs-lookup"><span data-stu-id="46c0a-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="46c0a-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="46c0a-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="46c0a-238">ASP. net\abgelehnte</span><span class="sxs-lookup"><span data-stu-id="46c0a-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="46c0a-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="46c0a-239">**CPU**</span></span>

- <span data-ttu-id="46c0a-240">Prozessorinformationen\prozessorzeit</span><span class="sxs-lookup"><span data-stu-id="46c0a-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="46c0a-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="46c0a-241">**TCP/IP**</span></span>

- <span data-ttu-id="46c0a-242">TCPv6/Verbindungen hergestellt</span><span class="sxs-lookup"><span data-stu-id="46c0a-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="46c0a-243">TCPv4/Verbindungen hergestellt</span><span class="sxs-lookup"><span data-stu-id="46c0a-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="46c0a-244">**Webdienst**</span><span class="sxs-lookup"><span data-stu-id="46c0a-244">**Web Service**</span></span>

- <span data-ttu-id="46c0a-245">Webdienst\aktuelle Verbindungen</span><span class="sxs-lookup"><span data-stu-id="46c0a-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="46c0a-246">Webdienst\maximale Verbindungen</span><span class="sxs-lookup"><span data-stu-id="46c0a-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="46c0a-247">**Threading**</span><span class="sxs-lookup"><span data-stu-id="46c0a-247">**Threading**</span></span>

- <span data-ttu-id="46c0a-248">.NET CLR locksandthreads\# aktueller logischer Threads</span><span class="sxs-lookup"><span data-stu-id="46c0a-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="46c0a-249">.NET CLR locksandthreads\# aktueller physischer Threads</span><span class="sxs-lookup"><span data-stu-id="46c0a-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="46c0a-250">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="46c0a-250">Other Resources</span></span>

<span data-ttu-id="46c0a-251">Weitere Informationen zur Leistungsüberwachung und-Optimierung in ASP.net finden Sie in den folgenden Themen:</span><span class="sxs-lookup"><span data-stu-id="46c0a-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="46c0a-252">[ASP.NET Performance Overview (Die Leistung von ASP.NET im Überblick)](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="46c0a-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="46c0a-253">ASP.net Thread Verwendung auf IIS 7,5, IIS 7,0 und IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="46c0a-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="46c0a-254">&lt;ApplicationPool&gt; Element (Webeinstellungen)</span><span class="sxs-lookup"><span data-stu-id="46c0a-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
