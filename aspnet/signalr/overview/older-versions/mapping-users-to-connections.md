---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Zuordnung von signalr-Benutzern zu Verbindungen in signalr 1. x | Microsoft-Dokumentation
author: bradygaster
description: In diesem Thema wird gezeigt, wie Informationen zu Benutzern und deren Verbindungen beibehalten werden.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 9d948495e9b8821fcb465611b6926603c3756a19
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449997"
---
# <a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="639d4-103">Zuordnen von SignalR-Benutzern zu Verbindungen in SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="639d4-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>

<span data-ttu-id="639d4-104">von [Patrick Fletcher](https://github.com/pfletcher), [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="639d4-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="639d4-105">In diesem Thema wird gezeigt, wie Informationen zu Benutzern und deren Verbindungen beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="639d4-105">This topic shows how to retain information about users and their connections.</span></span>

## <a name="introduction"></a><span data-ttu-id="639d4-106">Einführung</span><span class="sxs-lookup"><span data-stu-id="639d4-106">Introduction</span></span>

<span data-ttu-id="639d4-107">Jeder Client, der eine Verbindung mit einem Hub herstellt, übergibt eine eindeutige Verbindungs-ID. Sie können diesen Wert in der `Context.ConnectionId`-Eigenschaft des Hub-Kontexts abrufen.</span><span class="sxs-lookup"><span data-stu-id="639d4-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="639d4-108">Wenn Ihre Anwendung einen Benutzer der Verbindungs-ID zuordnen und diese Zuordnung beibehalten muss, können Sie eine der folgenden Aktionen verwenden:</span><span class="sxs-lookup"><span data-stu-id="639d4-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="639d4-109">[In-Memory-Speicher](#inmemory), z. b. ein Wörterbuch</span><span class="sxs-lookup"><span data-stu-id="639d4-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="639d4-110">Signalr-Gruppe für jeden Benutzer</span><span class="sxs-lookup"><span data-stu-id="639d4-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="639d4-111">[Permanenter, externer Speicher](#database), z. b. eine Datenbanktabelle oder ein Azure-Tabellen Speicher</span><span class="sxs-lookup"><span data-stu-id="639d4-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="639d4-112">Jede dieser Implementierungen wird in diesem Thema gezeigt.</span><span class="sxs-lookup"><span data-stu-id="639d4-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="639d4-113">Verwenden Sie die Methoden `OnConnected`, `OnDisconnected`und `OnReconnected` der `Hub`-Klasse, um den Benutzer Verbindungsstatus zu verfolgen.</span><span class="sxs-lookup"><span data-stu-id="639d4-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="639d4-114">Der beste Ansatz für Ihre Anwendung hängt von folgenden Informationen ab:</span><span class="sxs-lookup"><span data-stu-id="639d4-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="639d4-115">Die Anzahl von Webservern, die Ihre Anwendung hosten.</span><span class="sxs-lookup"><span data-stu-id="639d4-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="639d4-116">Gibt an, ob Sie eine Liste der derzeit verbundenen Benutzer erhalten müssen.</span><span class="sxs-lookup"><span data-stu-id="639d4-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="639d4-117">Ob Sie Gruppen-und Benutzerinformationen beibehalten müssen, wenn die Anwendung oder der Server neu gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="639d4-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="639d4-118">Gibt an, ob die Latenz beim Aufrufen eines externen Servers ein Problem ist.</span><span class="sxs-lookup"><span data-stu-id="639d4-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="639d4-119">Die folgende Tabelle zeigt, welcher Ansatz für diese Überlegungen funktioniert.</span><span class="sxs-lookup"><span data-stu-id="639d4-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="639d4-120">Mehr als ein Server</span><span class="sxs-lookup"><span data-stu-id="639d4-120">More than one server</span></span> | <span data-ttu-id="639d4-121">Liste der derzeit verbundenen Benutzer erhalten</span><span class="sxs-lookup"><span data-stu-id="639d4-121">Get list of currently connected users</span></span> | <span data-ttu-id="639d4-122">Beibehalten von Informationen nach Neustarts</span><span class="sxs-lookup"><span data-stu-id="639d4-122">Persist information after restarts</span></span> | <span data-ttu-id="639d4-123">Optimale Leistung</span><span class="sxs-lookup"><span data-stu-id="639d4-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="639d4-124">Im Arbeitsspeicher</span><span class="sxs-lookup"><span data-stu-id="639d4-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="639d4-125">Einzelbenutzer Gruppen</span><span class="sxs-lookup"><span data-stu-id="639d4-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="639d4-126">Permanent, extern</span><span class="sxs-lookup"><span data-stu-id="639d4-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="639d4-127">In-Memory-Speicher</span><span class="sxs-lookup"><span data-stu-id="639d4-127">In-memory storage</span></span>

<span data-ttu-id="639d4-128">In den folgenden Beispielen wird veranschaulicht, wie die Verbindungs-und Benutzerinformationen in einem Wörterbuch beibehalten werden, das im Arbeitsspeicher gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="639d4-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="639d4-129">Das Wörterbuch verwendet einen `HashSet` zum Speichern der Verbindungs-ID. Zu jedem Zeitpunkt kann ein Benutzer über mehr als eine Verbindung mit der signalr-Anwendung verfügen.</span><span class="sxs-lookup"><span data-stu-id="639d4-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="639d4-130">Beispielsweise würde ein Benutzer, der über mehrere Geräte oder mehr als eine Browser Registerkarte verbunden ist, mehr als eine Verbindungs-ID aufweisen.</span><span class="sxs-lookup"><span data-stu-id="639d4-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="639d4-131">Wenn die Anwendung heruntergefahren wird, gehen alle Informationen verloren, aber Sie werden erneut aufgefüllt, wenn die Benutzer ihre Verbindungen wiederherstellen.</span><span class="sxs-lookup"><span data-stu-id="639d4-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="639d4-132">In-Memory-Speicher funktioniert nicht, wenn Ihre Umgebung mehr als einen Webserver umfasst, da jeder Server über eine separate Auflistung von Verbindungen verfügt.</span><span class="sxs-lookup"><span data-stu-id="639d4-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="639d4-133">Im ersten Beispiel wird eine Klasse gezeigt, die die Zuordnung von Benutzern zu Verbindungen verwaltet.</span><span class="sxs-lookup"><span data-stu-id="639d4-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="639d4-134">Der Schlüssel für das HashSet ist der Name des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="639d4-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="639d4-135">Im nächsten Beispiel wird gezeigt, wie die Verbindungs Mapping-Klasse von einem Hub verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="639d4-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="639d4-136">Die Instanz der-Klasse wird in einem Variablennamen `_connections`gespeichert.</span><span class="sxs-lookup"><span data-stu-id="639d4-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="639d4-137">Einzelbenutzer Gruppen</span><span class="sxs-lookup"><span data-stu-id="639d4-137">Single-user groups</span></span>

<span data-ttu-id="639d4-138">Sie können für jeden Benutzer eine Gruppe erstellen und dann eine Nachricht an diese Gruppe senden, wenn Sie nur diesen Benutzer erreichen möchten.</span><span class="sxs-lookup"><span data-stu-id="639d4-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="639d4-139">Der Name der einzelnen Gruppen ist der Name des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="639d4-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="639d4-140">Wenn ein Benutzer über mehrere Verbindungen verfügt, wird jede Verbindungs-ID der Gruppe des Benutzers hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="639d4-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="639d4-141">Sie sollten den Benutzer nicht manuell aus der Gruppe entfernen, wenn der Benutzer die Verbindung trennt.</span><span class="sxs-lookup"><span data-stu-id="639d4-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="639d4-142">Diese Aktion wird automatisch vom signalr-Framework ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="639d4-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="639d4-143">Im folgenden Beispiel wird gezeigt, wie Einzelbenutzer Gruppen implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="639d4-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="639d4-144">Permanenter, externer Speicher</span><span class="sxs-lookup"><span data-stu-id="639d4-144">Permanent, external storage</span></span>

<span data-ttu-id="639d4-145">In diesem Thema wird gezeigt, wie Sie eine Datenbank oder einen Azure-Tabellen Speicher zum Speichern von Verbindungsinformationen verwenden.</span><span class="sxs-lookup"><span data-stu-id="639d4-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="639d4-146">Diese Vorgehensweise funktioniert, wenn Sie über mehrere Webserver verfügen, da jeder Webserver mit demselben Datenrepository interagieren kann.</span><span class="sxs-lookup"><span data-stu-id="639d4-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="639d4-147">Wenn die Webserver nicht mehr funktionieren oder die Anwendung neu gestartet wird, wird die `OnDisconnected`-Methode nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="639d4-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="639d4-148">Daher ist es möglich, dass für Ihr Datenrepository Datensätze für Verbindungs-IDs vorhanden sind, die nicht mehr gültig sind.</span><span class="sxs-lookup"><span data-stu-id="639d4-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="639d4-149">Zum Bereinigen dieser verwaisten Datensätze möchten Sie möglicherweise alle Verbindungen ungültig machen, die außerhalb eines Zeitraums erstellt wurden, der für Ihre Anwendung relevant ist.</span><span class="sxs-lookup"><span data-stu-id="639d4-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="639d4-150">Die Beispiele in diesem Abschnitt enthalten einen Wert für die Nachverfolgung, wann die Verbindung erstellt wurde. Sie zeigen jedoch nicht, wie Sie alte Datensätze bereinigen, da dies möglicherweise als Hintergrundprozess gewünscht ist.</span><span class="sxs-lookup"><span data-stu-id="639d4-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="639d4-151">Datenbank</span><span class="sxs-lookup"><span data-stu-id="639d4-151">Database</span></span>

<span data-ttu-id="639d4-152">In den folgenden Beispielen wird gezeigt, wie die Verbindungs-und Benutzerinformationen in einer-Datenbank beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="639d4-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="639d4-153">Sie können beliebige Datenzugriffs Technologien verwenden; Das folgende Beispiel zeigt jedoch, wie Modelle mithilfe von Entity Framework definiert werden.</span><span class="sxs-lookup"><span data-stu-id="639d4-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="639d4-154">Diese Entitäts Modelle entsprechen Datenbanktabellen und-Feldern.</span><span class="sxs-lookup"><span data-stu-id="639d4-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="639d4-155">Abhängig von den Anforderungen Ihrer Anwendung kann die Datenstruktur erheblich variieren.</span><span class="sxs-lookup"><span data-stu-id="639d4-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="639d4-156">Im ersten Beispiel wird gezeigt, wie eine Benutzer Entität definiert wird, die vielen Verbindungs Entitäten zugeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="639d4-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="639d4-157">Anschließend können Sie über den Hub den Status jeder Verbindung mit dem unten gezeigten Code nachverfolgen.</span><span class="sxs-lookup"><span data-stu-id="639d4-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="639d4-158">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="639d4-158">Azure table storage</span></span>

<span data-ttu-id="639d4-159">Das folgende Beispiel für Azure Table Storage ähnelt dem Daten Bank Beispiel.</span><span class="sxs-lookup"><span data-stu-id="639d4-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="639d4-160">Sie enthält nicht alle Informationen, die Sie für den Einstieg in Azure Table Storage Service benötigen.</span><span class="sxs-lookup"><span data-stu-id="639d4-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="639d4-161">Weitere Informationen finden [Sie unter Verwenden des Tabellen Speichers mit .net](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="639d4-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="639d4-162">Das folgende Beispiel zeigt eine Tabellen Entität zum Speichern von Verbindungsinformationen.</span><span class="sxs-lookup"><span data-stu-id="639d4-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="639d4-163">Die Daten werden nach Benutzername partitioniert, und jede Entität wird durch die Verbindungs-ID identifiziert, sodass ein Benutzer jederzeit über mehrere Verbindungen verfügen kann.</span><span class="sxs-lookup"><span data-stu-id="639d4-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="639d4-164">Im Hub verfolgen Sie den Status der einzelnen Benutzer Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="639d4-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
