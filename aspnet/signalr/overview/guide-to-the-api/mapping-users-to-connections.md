---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Zuordnung von signalr-Benutzern zu Verbindungen | Microsoft-Dokumentation
author: bradygaster
description: In diesem Thema wird gezeigt, wie Informationen zu Benutzern und deren Verbindungen beibehalten werden. Patrick Fletcher hat das Schreiben dieses Themas unterstützt. In diesem Thema verwendete Software Versionen...
ms.author: bradyg
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: d55d40848e1e9d40570850c3552b225235c5e814
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431391"
---
# <a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="d5be4-105">Zuordnen von SignalR-Benutzern zu Verbindungen</span><span class="sxs-lookup"><span data-stu-id="d5be4-105">Mapping SignalR Users to Connections</span></span>

<span data-ttu-id="d5be4-106">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d5be4-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="d5be4-107">In diesem Thema wird gezeigt, wie Informationen zu Benutzern und deren Verbindungen beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="d5be4-107">This topic shows how to retain information about users and their connections.</span></span>
>
> <span data-ttu-id="d5be4-108">Patrick Fletcher hat das Schreiben dieses Themas unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d5be4-108">Patrick Fletcher helped write this topic.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="d5be4-109">In diesem Thema verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="d5be4-109">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="d5be4-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d5be4-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="d5be4-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d5be4-111">.NET 4.5</span></span>
> - <span data-ttu-id="d5be4-112">Signalr Version 2</span><span class="sxs-lookup"><span data-stu-id="d5be4-112">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="d5be4-113">Vorherige Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="d5be4-113">Previous versions of this topic</span></span>
>
> <span data-ttu-id="d5be4-114">Informationen zu früheren Versionen von signalr finden Sie unter [signalr ältere Versionen](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="d5be4-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="d5be4-115">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="d5be4-115">Questions and comments</span></span>
>
> <span data-ttu-id="d5be4-116">Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten.</span><span class="sxs-lookup"><span data-stu-id="d5be4-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d5be4-117">Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="d5be4-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="introduction"></a><span data-ttu-id="d5be4-118">Einführung</span><span class="sxs-lookup"><span data-stu-id="d5be4-118">Introduction</span></span>

<span data-ttu-id="d5be4-119">Jeder Client, der eine Verbindung mit einem Hub herstellt, übergibt eine eindeutige Verbindungs-ID. Sie können diesen Wert in der `Context.ConnectionId`-Eigenschaft des Hub-Kontexts abrufen.</span><span class="sxs-lookup"><span data-stu-id="d5be4-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="d5be4-120">Wenn Ihre Anwendung einen Benutzer der Verbindungs-ID zuordnen und diese Zuordnung beibehalten muss, können Sie eine der folgenden Aktionen verwenden:</span><span class="sxs-lookup"><span data-stu-id="d5be4-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="d5be4-121">Der Benutzer-ID-Anbieter (signalr 2)</span><span class="sxs-lookup"><span data-stu-id="d5be4-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="d5be4-122">[In-Memory-Speicher](#inmemory), z. b. ein Wörterbuch</span><span class="sxs-lookup"><span data-stu-id="d5be4-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="d5be4-123">Signalr-Gruppe für jeden Benutzer</span><span class="sxs-lookup"><span data-stu-id="d5be4-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="d5be4-124">[Permanenter, externer Speicher](#database), z. b. eine Datenbanktabelle oder ein Azure-Tabellen Speicher</span><span class="sxs-lookup"><span data-stu-id="d5be4-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="d5be4-125">Jede dieser Implementierungen wird in diesem Thema gezeigt.</span><span class="sxs-lookup"><span data-stu-id="d5be4-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="d5be4-126">Verwenden Sie die Methoden `OnConnected`, `OnDisconnected`und `OnReconnected` der `Hub`-Klasse, um den Benutzer Verbindungsstatus zu verfolgen.</span><span class="sxs-lookup"><span data-stu-id="d5be4-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="d5be4-127">Der beste Ansatz für Ihre Anwendung hängt von folgenden Informationen ab:</span><span class="sxs-lookup"><span data-stu-id="d5be4-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="d5be4-128">Die Anzahl von Webservern, die Ihre Anwendung hosten.</span><span class="sxs-lookup"><span data-stu-id="d5be4-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="d5be4-129">Gibt an, ob Sie eine Liste der derzeit verbundenen Benutzer erhalten müssen.</span><span class="sxs-lookup"><span data-stu-id="d5be4-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="d5be4-130">Ob Sie Gruppen-und Benutzerinformationen beibehalten müssen, wenn die Anwendung oder der Server neu gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="d5be4-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="d5be4-131">Gibt an, ob die Latenz beim Aufrufen eines externen Servers ein Problem ist.</span><span class="sxs-lookup"><span data-stu-id="d5be4-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="d5be4-132">Die folgende Tabelle zeigt, welcher Ansatz für diese Überlegungen funktioniert.</span><span class="sxs-lookup"><span data-stu-id="d5be4-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="d5be4-133">Mehr als ein Server</span><span class="sxs-lookup"><span data-stu-id="d5be4-133">More than one server</span></span> | <span data-ttu-id="d5be4-134">Liste der derzeit verbundenen Benutzer erhalten</span><span class="sxs-lookup"><span data-stu-id="d5be4-134">Get list of currently connected users</span></span> | <span data-ttu-id="d5be4-135">Beibehalten von Informationen nach Neustarts</span><span class="sxs-lookup"><span data-stu-id="d5be4-135">Persist information after restarts</span></span> | <span data-ttu-id="d5be4-136">Optimale Leistung</span><span class="sxs-lookup"><span data-stu-id="d5be4-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="d5be4-137">UserID-Anbieter</span><span class="sxs-lookup"><span data-stu-id="d5be4-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="d5be4-138">Im Arbeitsspeicher</span><span class="sxs-lookup"><span data-stu-id="d5be4-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="d5be4-139">Einzelbenutzer Gruppen</span><span class="sxs-lookup"><span data-stu-id="d5be4-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="d5be4-140">Permanent, extern</span><span class="sxs-lookup"><span data-stu-id="d5be4-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="d5be4-141">Iuserid-Anbieter</span><span class="sxs-lookup"><span data-stu-id="d5be4-141">IUserID provider</span></span>

<span data-ttu-id="d5be4-142">Mit dieser Funktion können Benutzer angeben, welche Benutzer-ID auf einem irequest basiert, und zwar über eine neue Schnittstelle iuseridprovider.</span><span class="sxs-lookup"><span data-stu-id="d5be4-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="d5be4-143">**Der iuseridprovider**</span><span class="sxs-lookup"><span data-stu-id="d5be4-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="d5be4-144">Standardmäßig wird eine Implementierung verwendet, die die `IPrincipal.Identity.Name` des Benutzers als Benutzernamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="d5be4-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="d5be4-145">Um dies zu ändern, registrieren Sie Ihre Implementierung von `IUserIdProvider` beim globalen Host beim Starten der Anwendung:</span><span class="sxs-lookup"><span data-stu-id="d5be4-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="d5be4-146">Innerhalb eines Hubs können Sie Nachrichten über die folgende API an diese Benutzer senden:</span><span class="sxs-lookup"><span data-stu-id="d5be4-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="d5be4-147">**Senden einer Nachricht an einen bestimmten Benutzer**</span><span class="sxs-lookup"><span data-stu-id="d5be4-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="d5be4-148">In-Memory-Speicher</span><span class="sxs-lookup"><span data-stu-id="d5be4-148">In-memory storage</span></span>

<span data-ttu-id="d5be4-149">In den folgenden Beispielen wird veranschaulicht, wie die Verbindungs-und Benutzerinformationen in einem Wörterbuch beibehalten werden, das im Arbeitsspeicher gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="d5be4-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="d5be4-150">Das Wörterbuch verwendet einen `HashSet` zum Speichern der Verbindungs-ID. Zu jedem Zeitpunkt kann ein Benutzer über mehr als eine Verbindung mit der signalr-Anwendung verfügen.</span><span class="sxs-lookup"><span data-stu-id="d5be4-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="d5be4-151">Beispielsweise würde ein Benutzer, der über mehrere Geräte oder mehr als eine Browser Registerkarte verbunden ist, mehr als eine Verbindungs-ID aufweisen.</span><span class="sxs-lookup"><span data-stu-id="d5be4-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="d5be4-152">Wenn die Anwendung heruntergefahren wird, gehen alle Informationen verloren, aber Sie werden erneut aufgefüllt, wenn die Benutzer ihre Verbindungen wiederherstellen.</span><span class="sxs-lookup"><span data-stu-id="d5be4-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="d5be4-153">In-Memory-Speicher funktioniert nicht, wenn Ihre Umgebung mehr als einen Webserver umfasst, da jeder Server über eine separate Auflistung von Verbindungen verfügt.</span><span class="sxs-lookup"><span data-stu-id="d5be4-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="d5be4-154">Im ersten Beispiel wird eine Klasse gezeigt, die die Zuordnung von Benutzern zu Verbindungen verwaltet.</span><span class="sxs-lookup"><span data-stu-id="d5be4-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="d5be4-155">Der Schlüssel für das HashSet ist der Name des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="d5be4-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="d5be4-156">Im nächsten Beispiel wird gezeigt, wie die Verbindungs Mapping-Klasse von einem Hub verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="d5be4-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="d5be4-157">Die Instanz der-Klasse wird in einem Variablennamen `_connections`gespeichert.</span><span class="sxs-lookup"><span data-stu-id="d5be4-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="d5be4-158">Einzelbenutzer Gruppen</span><span class="sxs-lookup"><span data-stu-id="d5be4-158">Single-user groups</span></span>

<span data-ttu-id="d5be4-159">Sie können für jeden Benutzer eine Gruppe erstellen und dann eine Nachricht an diese Gruppe senden, wenn Sie nur diesen Benutzer erreichen möchten.</span><span class="sxs-lookup"><span data-stu-id="d5be4-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="d5be4-160">Der Name der einzelnen Gruppen ist der Name des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="d5be4-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="d5be4-161">Wenn ein Benutzer über mehrere Verbindungen verfügt, wird jede Verbindungs-ID der Gruppe des Benutzers hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d5be4-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="d5be4-162">Sie sollten den Benutzer nicht manuell aus der Gruppe entfernen, wenn der Benutzer die Verbindung trennt.</span><span class="sxs-lookup"><span data-stu-id="d5be4-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="d5be4-163">Diese Aktion wird automatisch vom signalr-Framework ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="d5be4-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="d5be4-164">Im folgenden Beispiel wird gezeigt, wie Einzelbenutzer Gruppen implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="d5be4-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="d5be4-165">Permanenter, externer Speicher</span><span class="sxs-lookup"><span data-stu-id="d5be4-165">Permanent, external storage</span></span>

<span data-ttu-id="d5be4-166">In diesem Thema wird gezeigt, wie Sie eine Datenbank oder einen Azure-Tabellen Speicher zum Speichern von Verbindungsinformationen verwenden.</span><span class="sxs-lookup"><span data-stu-id="d5be4-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="d5be4-167">Diese Vorgehensweise funktioniert, wenn Sie über mehrere Webserver verfügen, da jeder Webserver mit demselben Datenrepository interagieren kann.</span><span class="sxs-lookup"><span data-stu-id="d5be4-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="d5be4-168">Wenn die Webserver nicht mehr funktionieren oder die Anwendung neu gestartet wird, wird die `OnDisconnected`-Methode nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="d5be4-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="d5be4-169">Daher ist es möglich, dass für Ihr Datenrepository Datensätze für Verbindungs-IDs vorhanden sind, die nicht mehr gültig sind.</span><span class="sxs-lookup"><span data-stu-id="d5be4-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="d5be4-170">Zum Bereinigen dieser verwaisten Datensätze möchten Sie möglicherweise alle Verbindungen ungültig machen, die außerhalb eines Zeitraums erstellt wurden, der für Ihre Anwendung relevant ist.</span><span class="sxs-lookup"><span data-stu-id="d5be4-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="d5be4-171">Die Beispiele in diesem Abschnitt enthalten einen Wert für die Nachverfolgung, wann die Verbindung erstellt wurde. Sie zeigen jedoch nicht, wie Sie alte Datensätze bereinigen, da dies möglicherweise als Hintergrundprozess gewünscht ist.</span><span class="sxs-lookup"><span data-stu-id="d5be4-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="d5be4-172">Datenbank</span><span class="sxs-lookup"><span data-stu-id="d5be4-172">Database</span></span>

<span data-ttu-id="d5be4-173">In den folgenden Beispielen wird gezeigt, wie die Verbindungs-und Benutzerinformationen in einer-Datenbank beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="d5be4-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="d5be4-174">Sie können beliebige Datenzugriffs Technologien verwenden; Das folgende Beispiel zeigt jedoch, wie Modelle mithilfe von Entity Framework definiert werden.</span><span class="sxs-lookup"><span data-stu-id="d5be4-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="d5be4-175">Diese Entitäts Modelle entsprechen Datenbanktabellen und-Feldern.</span><span class="sxs-lookup"><span data-stu-id="d5be4-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="d5be4-176">Abhängig von den Anforderungen Ihrer Anwendung kann die Datenstruktur erheblich variieren.</span><span class="sxs-lookup"><span data-stu-id="d5be4-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="d5be4-177">Im ersten Beispiel wird gezeigt, wie eine Benutzer Entität definiert wird, die vielen Verbindungs Entitäten zugeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="d5be4-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="d5be4-178">Anschließend können Sie über den Hub den Status jeder Verbindung mit dem unten gezeigten Code nachverfolgen.</span><span class="sxs-lookup"><span data-stu-id="d5be4-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="d5be4-179">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="d5be4-179">Azure table storage</span></span>

<span data-ttu-id="d5be4-180">Das folgende Beispiel für Azure Table Storage ähnelt dem Daten Bank Beispiel.</span><span class="sxs-lookup"><span data-stu-id="d5be4-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="d5be4-181">Sie enthält nicht alle Informationen, die Sie für den Einstieg in Azure Table Storage Service benötigen.</span><span class="sxs-lookup"><span data-stu-id="d5be4-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="d5be4-182">Weitere Informationen finden [Sie unter Verwenden des Tabellen Speichers mit .net](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="d5be4-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="d5be4-183">Das folgende Beispiel zeigt eine Tabellen Entität zum Speichern von Verbindungsinformationen.</span><span class="sxs-lookup"><span data-stu-id="d5be4-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="d5be4-184">Die Daten werden nach Benutzername partitioniert, und jede Entität wird durch die Verbindungs-ID identifiziert, sodass ein Benutzer jederzeit über mehrere Verbindungen verfügen kann.</span><span class="sxs-lookup"><span data-stu-id="d5be4-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="d5be4-185">Im Hub verfolgen Sie den Status der einzelnen Benutzer Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="d5be4-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
