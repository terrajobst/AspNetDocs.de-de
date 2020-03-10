---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutorial: Server Übertragung mit signalr 2 | Microsoft-Dokumentation'
author: tdykstra
description: In diesem Tutorial wird gezeigt, wie Sie eine Webanwendung erstellen, die ASP.net signalr 2 verwendet, um Server Broadcast Funktionen bereitzustellen.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14924109fff8db3e537e6bc08b6dc868792ee660
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431241"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="2843d-103">Tutorial: Server Übertragung mit signalr 2</span><span class="sxs-lookup"><span data-stu-id="2843d-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="2843d-104">In diesem Tutorial wird gezeigt, wie Sie eine Webanwendung erstellen, die ASP.net signalr 2 verwendet, um Server Broadcast Funktionen bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="2843d-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="2843d-105">Server Broadcast bedeutet, dass der Server die an Clients gesendeten Mitteilungen startet.</span><span class="sxs-lookup"><span data-stu-id="2843d-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="2843d-106">Die Anwendung, die Sie in diesem Tutorial erstellen, simuliert einen Börsen Ticker, ein typisches Szenario für die Server Broadcast-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="2843d-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="2843d-107">Der Server aktualisiert die Aktienpreise in regelmäßigen Abständen nach dem Zufallsprinzip und überträgt die Updates an alle verbundenen Clients.</span><span class="sxs-lookup"><span data-stu-id="2843d-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="2843d-108">Im Browser ändern sich die Zahlen und Symbole in den **Änderungs** -und **%** Spalten dynamisch in Reaktion auf Benachrichtigungen vom Server.</span><span class="sxs-lookup"><span data-stu-id="2843d-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="2843d-109">Wenn Sie zusätzliche Browser in derselben URL öffnen, zeigen Sie alle dieselben Daten und die gleichen Änderungen an den Daten gleichzeitig an.</span><span class="sxs-lookup"><span data-stu-id="2843d-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Web erstellen](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="2843d-111">In diesem Tutorial führen Sie Folgendes durch:</span><span class="sxs-lookup"><span data-stu-id="2843d-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2843d-112">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="2843d-112">Create the project</span></span>
> * <span data-ttu-id="2843d-113">Einrichten des Servercodes</span><span class="sxs-lookup"><span data-stu-id="2843d-113">Set up the server code</span></span>
> * <span data-ttu-id="2843d-114">Überprüfen des Server Codes</span><span class="sxs-lookup"><span data-stu-id="2843d-114">Examine the server code</span></span>
> * <span data-ttu-id="2843d-115">Einrichten des Client Codes</span><span class="sxs-lookup"><span data-stu-id="2843d-115">Set up the client code</span></span>
> * <span data-ttu-id="2843d-116">Überprüfen des Client Codes</span><span class="sxs-lookup"><span data-stu-id="2843d-116">Examine the client code</span></span>
> * <span data-ttu-id="2843d-117">Testen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="2843d-117">Test the application</span></span>
> * <span data-ttu-id="2843d-118">Aktivieren der Protokollierung</span><span class="sxs-lookup"><span data-stu-id="2843d-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2843d-119">Wenn Sie die Schritte zum Entwickeln der Anwendung nicht durcharbeiten möchten, können Sie das signalr. Sample-Paket in einem neuen, leeren ASP.NET-Webanwendungs Projekt installieren.</span><span class="sxs-lookup"><span data-stu-id="2843d-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="2843d-120">Wenn Sie das nuget-Paket installieren, ohne die Schritte in diesem Tutorial auszuführen, müssen Sie die Anweisungen in der Datei "Infodatei *. txt* " befolgen.</span><span class="sxs-lookup"><span data-stu-id="2843d-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="2843d-121">Zum Ausführen des Pakets müssen Sie eine owin-Startklasse hinzufügen, die die `ConfigureSignalR`-Methode im installierten Paket aufruft.</span><span class="sxs-lookup"><span data-stu-id="2843d-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="2843d-122">Wenn Sie die owin-Startklasse nicht hinzufügen, erhalten Sie eine Fehlermeldung.</span><span class="sxs-lookup"><span data-stu-id="2843d-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="2843d-123">Weitere Informationen finden Sie im Abschnitt [Installieren des stocktickbeispiels](#install-the-stockticker-sample) in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="2843d-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2843d-124">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="2843d-124">Prerequisites</span></span>

* <span data-ttu-id="2843d-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="2843d-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="2843d-126">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="2843d-126">Create the project</span></span>

<span data-ttu-id="2843d-127">In diesem Abschnitt wird gezeigt, wie Sie mit Visual Studio 2017 eine leere ASP.NET-Webanwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="2843d-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="2843d-128">Erstellen Sie in Visual Studio eine ASP.NET-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="2843d-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Web erstellen](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="2843d-130">Lassen Sie im Fenster **neue ASP.NET Webanwendung-signalr. StockTicker** den Eintrag **leer** , und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="2843d-131">Einrichten des Servercodes</span><span class="sxs-lookup"><span data-stu-id="2843d-131">Set up the server code</span></span>

<span data-ttu-id="2843d-132">In diesem Abschnitt richten Sie den Code ein, der auf dem Server ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2843d-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="2843d-133">Erstellen der Aktienklasse</span><span class="sxs-lookup"><span data-stu-id="2843d-133">Create the Stock class</span></span>

<span data-ttu-id="2843d-134">Beginnen Sie mit dem Erstellen *der Kurs* Modell Klasse, die Sie zum Speichern und übertragen von Informationen zu einem Kurs verwenden.</span><span class="sxs-lookup"><span data-stu-id="2843d-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="2843d-135">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Klasse** **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="2843d-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="2843d-136">Benennen Sie die Klasse *Stock* , und fügen Sie Sie dem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="2843d-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="2843d-137">Ersetzen Sie den Code in der Datei *Stock.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2843d-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="2843d-138">Die zwei Eigenschaften, die Sie beim Erstellen von Beständen festlegen, werden `Symbol` (z. b. msft für Microsoft) und `Price`.</span><span class="sxs-lookup"><span data-stu-id="2843d-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="2843d-139">Die anderen Eigenschaften sind davon abhängig, wie und wann Sie `Price`festlegen.</span><span class="sxs-lookup"><span data-stu-id="2843d-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="2843d-140">Wenn Sie `Price`zum ersten Mal festlegen, wird der Wert an `DayOpen`weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="2843d-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="2843d-141">Nachdem Sie `Price`festgelegt haben, berechnet die APP die `Change`-und `PercentChange` Eigenschaftswerte basierend auf dem Unterschied zwischen `Price` und `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="2843d-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="2843d-142">Erstellen der Stock tickerhub-und StockTicker-Klassen</span><span class="sxs-lookup"><span data-stu-id="2843d-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="2843d-143">Sie verwenden die signalr Hub-API, um die Server-zu-Client-Interaktion zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="2843d-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="2843d-144">Eine `StockTickerHub` Klasse, die von der signalr-`Hub` Klasse abgeleitet wird, verarbeitet den Empfang von Verbindungen und Methoden Aufrufen von Clients.</span><span class="sxs-lookup"><span data-stu-id="2843d-144">A `StockTickerHub` class that derives from the SignalR `Hub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="2843d-145">Außerdem müssen Sie Aktiendaten aufbewahren und ein `Timer` Objekt ausführen.</span><span class="sxs-lookup"><span data-stu-id="2843d-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="2843d-146">Das `Timer`-Objekt löst regelmäßig Preis Aktualisierungen unabhängig von Clientverbindungen aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="2843d-147">Diese Funktionen können nicht in einer `Hub` Klasse abgelegt werden, da Hubs flüchtig sind.</span><span class="sxs-lookup"><span data-stu-id="2843d-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="2843d-148">Die App erstellt eine Instanz einer `Hub`-Klasse für jede Aufgabe auf dem Hub, wie z. b. Verbindungen und Aufrufe vom Client zum Server.</span><span class="sxs-lookup"><span data-stu-id="2843d-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="2843d-149">Der Mechanismus, der Aktiendaten beibehält, Preise aktualisiert und übermittelt, muss in einer separaten Klasse ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="2843d-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="2843d-150">Benennen Sie die Klasse `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="2843d-150">You'll name the class `StockTicker`.</span></span>

![Senden aus StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="2843d-152">Sie möchten nur, dass eine Instanz der `StockTicker`-Klasse auf dem Server ausgeführt werden kann. Daher müssen Sie einen Verweis von jeder `StockTickerHub`-Instanz auf die Singleton `StockTicker`-Instanz einrichten.</span><span class="sxs-lookup"><span data-stu-id="2843d-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="2843d-153">Die `StockTicker` Klasse muss an Clients übertragen werden, da Sie über die Aktiendaten und die triggerupdates verfügt, `StockTicker` aber keine `Hub` Klasse ist.</span><span class="sxs-lookup"><span data-stu-id="2843d-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="2843d-154">Die `StockTicker` Klasse muss einen Verweis auf das signalr-Hub-Verbindungs Kontext Objekt erhalten.</span><span class="sxs-lookup"><span data-stu-id="2843d-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="2843d-155">Anschließend kann das signalr-Verbindungs Kontext Objekt für die Übertragung an Clients verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2843d-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="2843d-156">StockTickerHub.cs erstellen</span><span class="sxs-lookup"><span data-stu-id="2843d-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="2843d-157">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Neues Element** **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="2843d-158">Wählen Sie unter **Neues Element hinzufügen-signalr. StockTicker**die Option **installiert** > **Visual C#**  > **Web** > **signalr** aus, und wählen Sie dann **signalr Hub-Klasse (v2)** aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="2843d-159">Benennen Sie die Klasse *stocktickerhub* , und fügen Sie Sie dem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="2843d-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="2843d-160">In diesem Schritt wird die *StockTickerHub.cs* -Klassendatei erstellt.</span><span class="sxs-lookup"><span data-stu-id="2843d-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="2843d-161">Gleichzeitig werden dem Projekt ein Satz von Skriptdateien und Assemblyverweisen hinzugefügt, die signalr unterstützen.</span><span class="sxs-lookup"><span data-stu-id="2843d-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="2843d-162">Ersetzen Sie den Code in der Datei *StockTickerHub.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2843d-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="2843d-163">Speichern Sie die Datei .</span><span class="sxs-lookup"><span data-stu-id="2843d-163">Save the file.</span></span>

<span data-ttu-id="2843d-164">Die APP verwendet die [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) -Klasse, um Methoden zu definieren, die von Clients auf dem Server aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="2843d-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="2843d-165">Sie definieren eine Methode: `GetAllStocks()`.</span><span class="sxs-lookup"><span data-stu-id="2843d-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="2843d-166">Wenn ein Client zum ersten Mal eine Verbindung mit dem Server herstellt, wird diese Methode aufgerufen, um eine Liste aller Aktien mit ihren aktuellen Preisen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="2843d-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="2843d-167">Die-Methode kann synchron ausgeführt werden und `IEnumerable<Stock>` zurückgeben, da Sie Daten aus dem Arbeitsspeicher zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="2843d-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="2843d-168">Wenn die Methode die Daten abrufen musste, indem Sie eine bestimmte Aktion durchführen, wie z. b. eine Datenbanksuche oder einen Webdienst-Befehl, geben Sie `Task<IEnumerable<Stock>>` als Rückgabewert an, um die asynchrone Verarbeitung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="2843d-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="2843d-169">Weitere Informationen finden Sie unter [ASP.net signalr Hubs API Guide-Server-when for asynchron Execute](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span><span class="sxs-lookup"><span data-stu-id="2843d-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="2843d-170">Das `HubName`-Attribut gibt an, wie die APP auf den Hub im JavaScript-Code auf dem Client verweist.</span><span class="sxs-lookup"><span data-stu-id="2843d-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="2843d-171">Wenn Sie dieses Attribut nicht verwenden, ist der Standardname auf dem Client eine "CamelCase"-Version des Klassen namens, die in diesem Fall `stockTickerHub`ist.</span><span class="sxs-lookup"><span data-stu-id="2843d-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="2843d-172">Wie Sie später sehen werden, wenn Sie die `StockTicker`-Klasse erstellen, erstellt die APP eine Singleton Instanz dieser Klasse in der statischen `Instance`-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="2843d-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="2843d-173">Diese Singleton Instanz `StockTicker` befindet sich im Arbeitsspeicher, unabhängig davon, wie viele Clients eine Verbindung herstellen oder trennen.</span><span class="sxs-lookup"><span data-stu-id="2843d-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="2843d-174">Diese Instanz wird von der `GetAllStocks()`-Methode verwendet, um aktuelle Aktieninformationen zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="2843d-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="2843d-175">StockTicker.cs erstellen</span><span class="sxs-lookup"><span data-stu-id="2843d-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="2843d-176">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Klasse** **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="2843d-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="2843d-177">Benennen Sie die Klasse *StockTicker* , und fügen Sie Sie dem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="2843d-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="2843d-178">Ersetzen Sie den Code in der Datei *StockTicker.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2843d-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="2843d-179">Da alle Threads dieselbe Instanz von stocktickcode ausführen, muss die StockTicker-Klasse Thread sicher sein.</span><span class="sxs-lookup"><span data-stu-id="2843d-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="2843d-180">Überprüfen des Server Codes</span><span class="sxs-lookup"><span data-stu-id="2843d-180">Examine the server code</span></span>

<span data-ttu-id="2843d-181">Wenn Sie den Servercode untersuchen, hilft er Ihnen, die Funktionsweise der APP zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="2843d-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="2843d-182">Speichern der Singleton-Instanz in einem statischen Feld</span><span class="sxs-lookup"><span data-stu-id="2843d-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="2843d-183">Der Code initialisiert das statische `_instance` Feld, das die `Instance`-Eigenschaft mit einer Instanz der-Klasse unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2843d-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="2843d-184">Da der Konstruktor privat ist, ist dies die einzige Instanz der-Klasse, die von der App erstellt werden kann.</span><span class="sxs-lookup"><span data-stu-id="2843d-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="2843d-185">Die APP verwendet die verzögerte [Initialisierung](/dotnet/framework/performance/lazy-initialization) für das `_instance` Feld.</span><span class="sxs-lookup"><span data-stu-id="2843d-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="2843d-186">Dies ist aus Leistungsgründen nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="2843d-186">It's not for performance reasons.</span></span> <span data-ttu-id="2843d-187">Es muss sichergestellt werden, dass die Instanzerstellung Thread sicher ist.</span><span class="sxs-lookup"><span data-stu-id="2843d-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="2843d-188">Jedes Mal, wenn ein Client eine Verbindung mit dem Server herstellt, ruft eine neue Instanz der stocktickerhub-Klasse, die in einem separaten Thread ausgeführt wird, die StockTicker Singleton-Instanz aus der `StockTicker.Instance` static-Eigenschaft ab, wie Sie zuvor in der `StockTickerHub`-Klasse gesehen haben.</span><span class="sxs-lookup"><span data-stu-id="2843d-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="2843d-189">Speichern von Aktiendaten in einem ConcurrentDictionary</span><span class="sxs-lookup"><span data-stu-id="2843d-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="2843d-190">Der Konstruktor initialisiert die `_stocks` Auflistung mit einigen Beispiel Daten für die Aktien, und `GetAllStocks` gibt die Bestände zurück.</span><span class="sxs-lookup"><span data-stu-id="2843d-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="2843d-191">Wie Sie bereits gesehen haben, wird diese Auflistung von Beständen von `StockTickerHub.GetAllStocks`zurückgegeben. dabei handelt es sich um eine Server Methode in der `Hub` Klasse, die von Clients aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="2843d-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="2843d-192">Die Bestands Sammlung ist als [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) -Typ für Thread Sicherheit definiert.</span><span class="sxs-lookup"><span data-stu-id="2843d-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="2843d-193">Als Alternative können Sie ein [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) -Objekt verwenden und das Wörterbuch explizit sperren, wenn Sie Änderungen daran vornehmen.</span><span class="sxs-lookup"><span data-stu-id="2843d-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="2843d-194">Bei dieser Beispielanwendung ist es in Ordnung, Anwendungsdaten im Arbeitsspeicher zu speichern und die Daten zu verlieren, wenn die APP die `StockTicker` Instanz freigibt.</span><span class="sxs-lookup"><span data-stu-id="2843d-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="2843d-195">In einer echten Anwendung würden Sie mit einem Back-End-Datenspeicher wie einer Datenbank arbeiten.</span><span class="sxs-lookup"><span data-stu-id="2843d-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="2843d-196">Regelmäßiges Aktualisieren von Aktienkursen</span><span class="sxs-lookup"><span data-stu-id="2843d-196">Periodically updating stock prices</span></span>

<span data-ttu-id="2843d-197">Der-Konstruktor startet ein `Timer` Objekt, das regelmäßig Methoden aufruft, die Aktienkurse nach dem Zufallsprinzip aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2843d-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="2843d-198">`Timer` ruft `UpdateStockPrices`auf, der im State-Parameter NULL übergibt.</span><span class="sxs-lookup"><span data-stu-id="2843d-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="2843d-199">Vor dem Aktualisieren der Preise nimmt die APP eine Sperre für das `_updateStockPricesLock` Objekt an.</span><span class="sxs-lookup"><span data-stu-id="2843d-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="2843d-200">Der Code prüft, ob die Preise bereits von einem anderen Thread aktualisiert werden, und ruft dann `TryUpdateStockPrice` für jeden bestand in der Liste auf.</span><span class="sxs-lookup"><span data-stu-id="2843d-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="2843d-201">Die `TryUpdateStockPrice`-Methode entscheidet, ob der Aktienkurs geändert und wie viel geändert werden muss.</span><span class="sxs-lookup"><span data-stu-id="2843d-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="2843d-202">Wenn sich der Aktienkurs ändert, ruft die APP `BroadcastStockPrice` auf, um die Aktienkurs Änderung an alle verbundenen Clients zu senden.</span><span class="sxs-lookup"><span data-stu-id="2843d-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="2843d-203">Das `_updatingStockPrices` Flag, das als [flüchtig](https://msdn.microsoft.com/library/x13ttww7.aspx) gekennzeichnet ist, um sicherzustellen, dass es Thread sicher ist</span><span class="sxs-lookup"><span data-stu-id="2843d-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="2843d-204">In einer echten Anwendung ruft die `TryUpdateStockPrice`-Methode einen Webdienst auf, um den Preis zu suchen.</span><span class="sxs-lookup"><span data-stu-id="2843d-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="2843d-205">In diesem Code verwendet die APP einen Zufallszahlengenerator, um Änderungen nach dem Zufallsprinzip vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="2843d-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="2843d-206">So erhalten Sie den signalr-Kontext, sodass die StockTicker-Klasse an Clients gesendet werden kann</span><span class="sxs-lookup"><span data-stu-id="2843d-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="2843d-207">Da die Preisänderungen hier in das `StockTicker` Objekt kommen, ist das Objekt, das eine `updateStockPrice` Methode auf allen verbundenen Clients abrufen muss.</span><span class="sxs-lookup"><span data-stu-id="2843d-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="2843d-208">In einer `Hub`-Klasse verfügen Sie über eine API zum Aufrufen von Client Methoden, `StockTicker` jedoch nicht von der `Hub`-Klasse abgeleitet ist und keinen Verweis auf ein `Hub` Objekt hat.</span><span class="sxs-lookup"><span data-stu-id="2843d-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="2843d-209">Zum Übertragen an verbundene Clients muss die `StockTicker` Klasse die signalr-Kontext Instanz für die `StockTickerHub`-Klasse abrufen und diese zum Abrufen von Methoden auf Clients verwenden.</span><span class="sxs-lookup"><span data-stu-id="2843d-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="2843d-210">Der Code Ruft einen Verweis auf den signalr-Kontext ab, wenn er die Singleton-Klasseninstanz erstellt, übergibt diesen Verweis an den Konstruktor, und der Konstruktor legt ihn in der `Clients`-Eigenschaft ab.</span><span class="sxs-lookup"><span data-stu-id="2843d-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="2843d-211">Es gibt zwei Gründe dafür, warum Sie den Kontext nur einmal erhalten möchten: das erhalten des Kontexts ist eine aufwändige Aufgabe und der einmalige Abschluss stellt sicher, dass die APP die beabsichtigte Reihenfolge der an die Clients gesendeten Nachrichten beibehält.</span><span class="sxs-lookup"><span data-stu-id="2843d-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="2843d-212">Wenn Sie die `Clients`-Eigenschaft des Kontexts abrufen und in der `StockTickerClient`-Eigenschaft platzieren, können Sie Code schreiben, um Client Methoden aufzurufen, die wie in einer `Hub`-Klasse aussehen.</span><span class="sxs-lookup"><span data-stu-id="2843d-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="2843d-213">Zum Senden von Daten an alle Clients können Sie beispielsweise `Clients.All.updateStockPrice(stock)`schreiben.</span><span class="sxs-lookup"><span data-stu-id="2843d-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="2843d-214">Die `updateStockPrice` Methode, die Sie in `BroadcastStockPrice` aufrufen, ist noch nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="2843d-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="2843d-215">Sie fügen Sie später hinzu, wenn Sie Code schreiben, der auf dem Client ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2843d-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="2843d-216">Sie können hier auf `updateStockPrice` verweisen, da `Clients.All` dynamisch ist, was bedeutet, dass die APP den Ausdruck zur Laufzeit evaluiert.</span><span class="sxs-lookup"><span data-stu-id="2843d-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="2843d-217">Wenn dieser Methodenaufrufe ausgeführt wird, sendet signalr den Methodennamen und den Parameterwert an den Client. wenn der Client über eine Methode mit dem Namen `updateStockPrice`verfügt, ruft die APP diese Methode auf und übergibt den Parameterwert an den Client.</span><span class="sxs-lookup"><span data-stu-id="2843d-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="2843d-218">`Clients.All` bedeutet, dass an alle Clients gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="2843d-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="2843d-219">Signalr bietet Ihnen andere Optionen, um anzugeben, an welche Clients oder Client Gruppen die Clients gesendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="2843d-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="2843d-220">Weitere Informationen finden Sie unter [hubconnectioncontext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="2843d-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="2843d-221">Registrieren der signalr-Route</span><span class="sxs-lookup"><span data-stu-id="2843d-221">Register the SignalR route</span></span>

<span data-ttu-id="2843d-222">Der Server muss wissen, welche URL abgefangen und an signalr weitergeleitet werden soll.</span><span class="sxs-lookup"><span data-stu-id="2843d-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="2843d-223">Fügen Sie hierzu eine owin-Startklasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="2843d-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="2843d-224">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Neues Element** **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="2843d-225">Wählen Sie unter **Neues Element hinzufügen-signalr. StockTicker** die Option **installiert** > **Visual C#**  > **Web** aus, und wählen Sie dann **owin-Startklasse**aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="2843d-226">Benennen Sie die Klasse *Startup* , und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="2843d-227">Ersetzen Sie den Standard Code in der Datei *Startup.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2843d-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="2843d-228">Die Einrichtung des Server Codes ist nun abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="2843d-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="2843d-229">Im nächsten Abschnitt richten Sie den-Client ein.</span><span class="sxs-lookup"><span data-stu-id="2843d-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="2843d-230">Einrichten des Client Codes</span><span class="sxs-lookup"><span data-stu-id="2843d-230">Set up the client code</span></span>

<span data-ttu-id="2843d-231">In diesem Abschnitt richten Sie den Code ein, der auf dem Client ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2843d-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="2843d-232">Erstellen der HTML-Seite und der JavaScript-Datei</span><span class="sxs-lookup"><span data-stu-id="2843d-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="2843d-233">Auf der HTML-Seite werden die Daten angezeigt, und die JavaScript-Datei organisiert die Daten.</span><span class="sxs-lookup"><span data-stu-id="2843d-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="2843d-234">Erstellen von StockTicker. html</span><span class="sxs-lookup"><span data-stu-id="2843d-234">Create StockTicker.html</span></span>

<span data-ttu-id="2843d-235">Zuerst fügen Sie den HTML-Client hinzu.</span><span class="sxs-lookup"><span data-stu-id="2843d-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="2843d-236">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **HTML-Seite** **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="2843d-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="2843d-237">Nennen Sie die Datei *StockTicker* , und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="2843d-238">Ersetzen Sie den Standard Code in der Datei " *StockTicker. html* " durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2843d-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="2843d-239">Der HTML-Code erstellt eine Tabelle mit fünf Spalten, einer Kopfzeile und einer Daten Zeile mit einer einzelnen Zelle, die alle fünf Spalten umfasst.</span><span class="sxs-lookup"><span data-stu-id="2843d-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="2843d-240">In der Daten Zeile wird "laden..." angezeigt. vorübergehend, wenn die APP gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="2843d-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="2843d-241">Der JavaScript-Code entfernt diese Zeile und fügt Ihre Platz Zeilen mit den vom Server abgerufenen Aktiendaten hinzu.</span><span class="sxs-lookup"><span data-stu-id="2843d-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="2843d-242">Die Skript Tags geben Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="2843d-242">The script tags specify:</span></span>

    * <span data-ttu-id="2843d-243">Die jQuery-Skriptdatei.</span><span class="sxs-lookup"><span data-stu-id="2843d-243">The jQuery script file.</span></span>

    * <span data-ttu-id="2843d-244">Die signalr Core-Skriptdatei.</span><span class="sxs-lookup"><span data-stu-id="2843d-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="2843d-245">Die Skriptdatei für signalr-Proxys.</span><span class="sxs-lookup"><span data-stu-id="2843d-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="2843d-246">Eine StockTicker-Skriptdatei, die Sie später erstellen.</span><span class="sxs-lookup"><span data-stu-id="2843d-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="2843d-247">Die APP generiert dynamisch die Skriptdatei für signalr-Proxys.</span><span class="sxs-lookup"><span data-stu-id="2843d-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="2843d-248">Sie gibt die URL "/signalr/Hubs" an und definiert Proxy Methoden für die Methoden in der hubklasse, in diesem Fall für die `StockTickerHub.GetAllStocks`.</span><span class="sxs-lookup"><span data-stu-id="2843d-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="2843d-249">Wenn Sie möchten, können Sie diese JavaScript-Datei mithilfe von [signalr-Hilfsprogramme](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)manuell generieren.</span><span class="sxs-lookup"><span data-stu-id="2843d-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="2843d-250">Vergessen Sie nicht, die Erstellung dynamischer Dateien im `MapHubs` Methoden aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="2843d-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="2843d-251">Erweitern Sie in **Projektmappen-Explorer**den Eintrag **Skripts**.</span><span class="sxs-lookup"><span data-stu-id="2843d-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="2843d-252">Skript Bibliotheken für jQuery und signalr sind im Projekt sichtbar.</span><span class="sxs-lookup"><span data-stu-id="2843d-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="2843d-253">Der Paket-Manager installiert eine spätere Version der signalr-Skripts.</span><span class="sxs-lookup"><span data-stu-id="2843d-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="2843d-254">Aktualisieren Sie die Skript Verweise im Codeblock, damit Sie den Versionen der Skriptdateien im Projekt entsprechen.</span><span class="sxs-lookup"><span data-stu-id="2843d-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="2843d-255">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf *StockTicker. html*, und wählen Sie dann **als Start Seite festlegen**aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="2843d-256">Erstellen von "StockTicker. js"</span><span class="sxs-lookup"><span data-stu-id="2843d-256">Create StockTicker.js</span></span>

<span data-ttu-id="2843d-257">Erstellen Sie jetzt die JavaScript-Datei.</span><span class="sxs-lookup"><span data-stu-id="2843d-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="2843d-258">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **JavaScript-Datei** **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="2843d-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="2843d-259">Nennen Sie die Datei *StockTicker* , und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="2843d-260">Fügen Sie den folgenden Code in die Datei " *StockTicker. js* " ein:</span><span class="sxs-lookup"><span data-stu-id="2843d-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="2843d-261">Überprüfen des Client Codes</span><span class="sxs-lookup"><span data-stu-id="2843d-261">Examine the client code</span></span>

<span data-ttu-id="2843d-262">Wenn Sie den Client Code überprüfen, erfahren Sie, wie der Client Code mit dem Servercode interagiert, damit die APP funktioniert.</span><span class="sxs-lookup"><span data-stu-id="2843d-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="2843d-263">Verbindung wird gestartet</span><span class="sxs-lookup"><span data-stu-id="2843d-263">Starting the connection</span></span>

<span data-ttu-id="2843d-264">`$.connection` verweist auf die signalr-Proxys.</span><span class="sxs-lookup"><span data-stu-id="2843d-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="2843d-265">Der Code Ruft einen Verweis auf den Proxy für die `StockTickerHub`-Klasse ab und legt ihn in der `ticker`-Variablen ab.</span><span class="sxs-lookup"><span data-stu-id="2843d-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="2843d-266">Der Proxy Name ist der Name, der vom `HubName`-Attribut festgelegt wurde:</span><span class="sxs-lookup"><span data-stu-id="2843d-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="2843d-267">Nachdem Sie alle Variablen und Funktionen definiert haben, initialisiert die letzte Codezeile in der Datei die signalr-Verbindung durch Aufrufen der signalr-`start` Funktion.</span><span class="sxs-lookup"><span data-stu-id="2843d-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="2843d-268">Die `start`-Funktion wird asynchron ausgeführt und gibt ein [Verzögertes jQuery-Objekt](http://api.jquery.com/category/deferred-object/)zurück.</span><span class="sxs-lookup"><span data-stu-id="2843d-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="2843d-269">Sie können die done-Funktion aufzurufen, um die Funktion anzugeben, die aufgerufen werden soll, wenn die APP die asynchrone Aktion beendet.</span><span class="sxs-lookup"><span data-stu-id="2843d-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="2843d-270">Alle Bestände erhalten</span><span class="sxs-lookup"><span data-stu-id="2843d-270">Getting all the stocks</span></span>

<span data-ttu-id="2843d-271">Die `init`-Funktion Ruft die `getAllStocks`-Funktion auf dem Server auf und verwendet die vom Server zurückgegebenen Informationen, um die Aktien Tabelle zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2843d-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="2843d-272">Beachten Sie, dass Sie auf dem Client standardmäßig die "camelschreibweise" verwenden müssen, obwohl der Methodenname auf dem Server Pascal-Schreibweise ist.</span><span class="sxs-lookup"><span data-stu-id="2843d-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="2843d-273">Die Regel für die Groß-/Kleinschreibung gilt nur für Methoden, nicht für Objekte.</span><span class="sxs-lookup"><span data-stu-id="2843d-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="2843d-274">Beispielsweise verweisen Sie auf `stock.Symbol` und `stock.Price`, nicht auf `stock.symbol` oder `stock.price`.</span><span class="sxs-lookup"><span data-stu-id="2843d-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="2843d-275">In der `init`-Methode erstellt die APP HTML für eine Tabellenzeile für jedes vom Server empfangene Lager Objekt, indem `formatStock` zum Formatieren von Eigenschaften des `stock` Objekts aufgerufen wird. Anschließend wird `supplant` aufgerufen, um die Platzhalter in der `rowTemplate` Variablen durch die Eigenschaftswerte `stock` Objekts zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="2843d-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="2843d-276">Der resultierende HTML-Code wird dann an die Aktien Tabelle angehängt.</span><span class="sxs-lookup"><span data-stu-id="2843d-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="2843d-277">Sie werden `init` aufgerufen, indem Sie Sie als `callback` Funktion übergeben, die ausgeführt wird, nachdem die asynchrone `start` Funktion abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="2843d-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="2843d-278">Wenn Sie nach dem Aufrufen von `start``init` als separate JavaScript-Anweisung aufgerufen haben, schlägt die Funktion fehl, da Sie sofort ausgeführt werden würde, ohne zu warten, bis die Start Funktion die Verbindung hergestellt hat.</span><span class="sxs-lookup"><span data-stu-id="2843d-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="2843d-279">In diesem Fall versucht die `init` Funktion, die `getAllStocks`-Funktion aufzurufen, bevor die APP eine Server Verbindung herstellt.</span><span class="sxs-lookup"><span data-stu-id="2843d-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="2843d-280">Erhalten von aktualisierten Aktienkursen</span><span class="sxs-lookup"><span data-stu-id="2843d-280">Getting updated stock prices</span></span>

<span data-ttu-id="2843d-281">Wenn der Server den Kurs eines Aktien ändert, wird der `updateStockPrice` auf verbundenen Clients aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="2843d-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="2843d-282">Die APP fügt die Funktion der Client-Eigenschaft des `stockTicker` Proxys hinzu, um Sie für Aufrufe vom Server verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="2843d-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="2843d-283">Die `updateStockPrice`-Funktion formatiert ein vom Server empfangenes Aktien Objekt auf die gleiche Weise wie in der `init`-Funktion in eine Tabellenzeile.</span><span class="sxs-lookup"><span data-stu-id="2843d-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="2843d-284">Anstatt die Zeile an die Tabelle zu anhängen, wird die aktuelle Zeile der Aktien Tabelle in der Tabelle gefunden und diese Zeile durch die neue Zeile ersetzt.</span><span class="sxs-lookup"><span data-stu-id="2843d-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="2843d-285">Testen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="2843d-285">Test the application</span></span>

<span data-ttu-id="2843d-286">Sie können die APP testen, um sicherzustellen, dass Sie funktioniert.</span><span class="sxs-lookup"><span data-stu-id="2843d-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="2843d-287">Sie sehen, dass in allen Browserfenstern die Live Aktien Tabelle angezeigt wird, bei der Aktienkurse schwanken.</span><span class="sxs-lookup"><span data-stu-id="2843d-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="2843d-288">Aktivieren Sie auf der Symbolleiste das **Skript Debugging** , und wählen Sie dann die Schaltfläche wiedergeben aus, um die APP im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="2843d-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![Screenshot des Benutzers zum Aktivieren des Debugmodus und zum Auswählen von Play.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="2843d-290">Ein Browserfenster wird geöffnet, in dem die **Live Aktien Tabelle**angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="2843d-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="2843d-291">Die Aktien Tabelle zeigt anfänglich das "laden..." Line, dann zeigt die APP nach kurzer Zeit die anfänglichen Aktiendaten an, und die Aktienpreise werden geändert.</span><span class="sxs-lookup"><span data-stu-id="2843d-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="2843d-292">Kopieren Sie die URL aus dem Browser, öffnen Sie zwei andere Browser, und fügen Sie die URLs in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="2843d-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="2843d-293">Die anfängliche Aktien Anzeige ist identisch mit dem ersten Browser, und die Änderungen werden gleichzeitig durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="2843d-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="2843d-294">Schließen Sie alle Browser, öffnen Sie einen neuen Browser, und wechseln Sie zur gleichen URL.</span><span class="sxs-lookup"><span data-stu-id="2843d-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="2843d-295">Das StockTicker-Singleton-Objekt wird weiterhin auf dem Server ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2843d-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="2843d-296">Die **Live Aktien Tabelle** zeigt an, dass sich die Bestände weiterhin geändert haben.</span><span class="sxs-lookup"><span data-stu-id="2843d-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="2843d-297">Die Anfangs Tabelle mit 0 (null) Änderungs Zahlen wird nicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2843d-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="2843d-298">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="2843d-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="2843d-299">Aktivieren der Protokollierung</span><span class="sxs-lookup"><span data-stu-id="2843d-299">Enable logging</span></span>

<span data-ttu-id="2843d-300">Signalr verfügt über eine integrierte Protokollierungsfunktion, die Sie auf dem Client aktivieren können, um die Problembehandlung zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="2843d-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="2843d-301">In diesem Abschnitt aktivieren Sie die Protokollierung und sehen anhand von Beispielen, wie Protokolle anzeigen, welche der folgenden Transportmethoden von signalr verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="2843d-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="2843d-302">[Websockets](http://en.wikipedia.org/wiki/WebSocket), die von IIS 8 und aktuellen Browsern unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="2843d-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="2843d-303">Vom [Server gesendete Ereignisse](http://en.wikipedia.org/wiki/Server-sent_events), die von anderen Browsern als Internet Explorer unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="2843d-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="2843d-304">" [Forever Frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)", unterstützt von Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="2843d-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="2843d-305">[AJAX Long](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)-Abruf, von allen Browsern unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2843d-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="2843d-306">Für eine beliebige Verbindung wählt signalr die beste Transportmethode aus, die sowohl vom Server als auch vom Client unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="2843d-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="2843d-307">Öffnen Sie *StockTicker. js*.</span><span class="sxs-lookup"><span data-stu-id="2843d-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="2843d-308">Fügen Sie diese hervorgehobene Codezeile hinzu, um die Protokollierung direkt vor dem Code zu aktivieren, der die Verbindung am Ende der Datei initialisiert:</span><span class="sxs-lookup"><span data-stu-id="2843d-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="2843d-309">Drücken Sie **F5**, um das Projekt auszuführen.</span><span class="sxs-lookup"><span data-stu-id="2843d-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="2843d-310">Öffnen Sie das Fenster "Entwicklertools" des Browsers, und wählen Sie die Konsole aus, um die Protokolle anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="2843d-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="2843d-311">Möglicherweise müssen Sie die Seite aktualisieren, um die Protokolle von signalr zum Aushandeln der Transportmethode für eine neue Verbindung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="2843d-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="2843d-312">Wenn Sie Internet Explorer 10 unter Windows 8 (IIS 8) ausführen, ist die Transportmethode **websockets**.</span><span class="sxs-lookup"><span data-stu-id="2843d-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="2843d-313">Wenn Sie Internet Explorer 10 unter Windows 7 (IIS 7,5) ausführen, ist die Transportmethode **iframe**.</span><span class="sxs-lookup"><span data-stu-id="2843d-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="2843d-314">Wenn Sie Firefox 19 auf Windows 8 (IIS 8) ausführen, ist die Transportmethode **websockets**.</span><span class="sxs-lookup"><span data-stu-id="2843d-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="2843d-315">Installieren Sie in Firefox das Firebug-Add-in, um ein Konsolenfenster zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="2843d-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="2843d-316">Wenn Sie Firefox 19 unter Windows 7 (IIS 7,5) ausführen, handelt es sich bei der Transportmethode um vom **Server gesendete** Ereignisse.</span><span class="sxs-lookup"><span data-stu-id="2843d-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="2843d-317">Installieren des StockTicker-Beispiels</span><span class="sxs-lookup"><span data-stu-id="2843d-317">Install the StockTicker sample</span></span>

<span data-ttu-id="2843d-318">[Microsoft. Aspnet. signalr. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installiert die StockTicker-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2843d-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="2843d-319">Das nuget-Paket enthält mehr Features als die vereinfachte Version, die Sie von Grund auf neu erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="2843d-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="2843d-320">In diesem Abschnitt des Tutorials installieren Sie das nuget-Paket und überprüfen die neuen Features und den Code, der diese implementiert.</span><span class="sxs-lookup"><span data-stu-id="2843d-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2843d-321">Wenn Sie das Paket installieren, ohne die vorherigen Schritte dieses Tutorials auszuführen, müssen Sie dem Projekt eine owin-Startklasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="2843d-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="2843d-322">Dieser Schritt wird in dieser Datei "Infodatei. txt" für das nuget-Paket erläutert.</span><span class="sxs-lookup"><span data-stu-id="2843d-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="2843d-323">Installieren des "signalr. Sample"-nuget-Pakets</span><span class="sxs-lookup"><span data-stu-id="2843d-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="2843d-324">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="2843d-325">Wählen Sie im **nuget-Paket-Manager: signalr. StockTicker**die Option **Durchsuchen**aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="2843d-326">Wählen Sie aus **Paketquelle**die Option **nuget.org**aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="2843d-327">Geben Sie *signalr. Sample* in das Suchfeld ein, und wählen Sie " **Microsoft. Aspnet. signalr. Sample** > **install**" aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="2843d-328">Erweitern Sie in **Projektmappen-Explorer**den Ordner *signalr. Sample* .</span><span class="sxs-lookup"><span data-stu-id="2843d-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="2843d-329">Bei der Installation des Pakets "signalr. Sample" wurden der Ordner und dessen Inhalt erstellt.</span><span class="sxs-lookup"><span data-stu-id="2843d-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="2843d-330">Klicken Sie im Ordner *signalr. Sample* mit der rechten Maustaste auf *StockTicker. html*, und wählen Sie dann **als Start Seite festlegen**aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2843d-331">Durch die Installation des nuget-Pakets "signalr. Sample" kann die Version von jQuery geändert werden, die Sie im Ordner " *Scripts* " haben.</span><span class="sxs-lookup"><span data-stu-id="2843d-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="2843d-332">Die neue Datei " *StockTicker. html* ", die das Paket im Ordner " *signalr. Sample* " installiert, ist synchron mit der jQuery-Version, die vom Paket installiert wird. Wenn Sie jedoch die ursprüngliche Datei " *StockTicker. html* " erneut ausführen möchten, müssen Sie möglicherweise zuerst den jQuery-Verweis im Skript-Tag aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2843d-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="2843d-333">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="2843d-333">Run the application</span></span>

 <span data-ttu-id="2843d-334">Die Tabelle, die Sie in der ersten APP gesehen haben, hatte nützliche Features.</span><span class="sxs-lookup"><span data-stu-id="2843d-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="2843d-335">Die vollständige Börsen Ticker-Anwendung zeigt neue Features an: ein horizontales Bild Lauf Fenster, in dem die Aktiendaten und-Bestände angezeigt werden, die die Farbe bei steigender und steigender Farbe ändern</span><span class="sxs-lookup"><span data-stu-id="2843d-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="2843d-336">Drücken Sie **F5**, um die App auszuführen.</span><span class="sxs-lookup"><span data-stu-id="2843d-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="2843d-337">Wenn Sie die APP zum ersten Mal ausführen, wird der "Markt" als "geschlossen" angezeigt, und Sie sehen eine statische Tabelle und ein Tickerfenster, das keinen Bildlauf durchführt.</span><span class="sxs-lookup"><span data-stu-id="2843d-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="2843d-338">Wählen Sie **Open Market**aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-338">Select **Open Market**.</span></span>

    ![Screenshot des Live-Tickers.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="2843d-340">Im Feld " **Live Stock Ticker** " wird ein horizontaler Bildlauf durchlaufen, und der Server beginnt, in regelmäßigen Abständen Aktienkurs Änderungen zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="2843d-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="2843d-341">Jedes Mal, wenn sich ein Aktienkurs ändert, aktualisiert die APP sowohl die **Live Aktien Tabelle** als auch den **Live Stock-Ticker**.</span><span class="sxs-lookup"><span data-stu-id="2843d-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="2843d-342">Wenn die Preisänderung eines Bestands positiv ist, zeigt die APP die Aktien mit einem grünen Hintergrund an.</span><span class="sxs-lookup"><span data-stu-id="2843d-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="2843d-343">Wenn die Änderung negativ ist, wird die Aktie in der APP mit einem roten Hintergrund angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2843d-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="2843d-344">Wählen Sie **Markt beenden**aus.</span><span class="sxs-lookup"><span data-stu-id="2843d-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="2843d-345">Die Tabellen Updates werden beendet.</span><span class="sxs-lookup"><span data-stu-id="2843d-345">The table updates stop.</span></span>

    * <span data-ttu-id="2843d-346">Der Ticker beendet den Bildlauf.</span><span class="sxs-lookup"><span data-stu-id="2843d-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="2843d-347">Klicken Sie auf **Zurücksetzen**.</span><span class="sxs-lookup"><span data-stu-id="2843d-347">Select **Reset**.</span></span>

    * <span data-ttu-id="2843d-348">Alle Aktiendaten werden zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="2843d-348">All stock data is reset.</span></span>

    * <span data-ttu-id="2843d-349">Die APP stellt den Anfangszustand vor dem Start der Preisänderungen wieder her.</span><span class="sxs-lookup"><span data-stu-id="2843d-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="2843d-350">Kopieren Sie die URL aus dem Browser, öffnen Sie zwei andere Browser, und fügen Sie die URLs in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="2843d-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="2843d-351">Sie sehen, dass die gleichen Daten in jedem Browser dynamisch gleichzeitig aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="2843d-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="2843d-352">Wenn Sie eines der Steuerelemente auswählen, reagieren alle Browser gleichzeitig auf dieselbe Weise.</span><span class="sxs-lookup"><span data-stu-id="2843d-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="2843d-353">Live Stock Ticker-Anzeige</span><span class="sxs-lookup"><span data-stu-id="2843d-353">Live Stock Ticker display</span></span>

<span data-ttu-id="2843d-354">Die **Live Stock Ticker** -Anzeige ist eine ungeordnete Liste in einem `<div>`-Element, das in einer einzelnen Zeile von CSS-Stilen formatiert ist.</span><span class="sxs-lookup"><span data-stu-id="2843d-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="2843d-355">Die APP initialisiert und aktualisiert den Ticker auf dieselbe Weise wie die Tabelle: durch Ersetzen von Platzhaltern in einer `<li>` Vorlagen Zeichenfolge und dynamisches Hinzufügen der `<li>` Elemente zum `<ul>` Element.</span><span class="sxs-lookup"><span data-stu-id="2843d-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="2843d-356">Die APP umfasst den Bildlauf mithilfe der jQuery-`animate` Funktion, um den Rand Links von der ungeordneten Liste innerhalb der `<div>`zu variieren.</span><span class="sxs-lookup"><span data-stu-id="2843d-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="2843d-357">Signalr. Sample StockTicker. html</span><span class="sxs-lookup"><span data-stu-id="2843d-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="2843d-358">Der HTML-Code des Aktien Tickers:</span><span class="sxs-lookup"><span data-stu-id="2843d-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="2843d-359">Signalr. Sample StockTicker. CSS</span><span class="sxs-lookup"><span data-stu-id="2843d-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="2843d-360">Der Aktien-Ticker-CSS-Code:</span><span class="sxs-lookup"><span data-stu-id="2843d-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="2843d-361">Signalr. Sample signalr. StockTicker. js</span><span class="sxs-lookup"><span data-stu-id="2843d-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="2843d-362">Der jQuery-Code, der den Bildlauf durchführt:</span><span class="sxs-lookup"><span data-stu-id="2843d-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="2843d-363">Zusätzliche Methoden auf dem Server, die vom Client aufgerufen werden können</span><span class="sxs-lookup"><span data-stu-id="2843d-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="2843d-364">Um der APP Flexibilität zu verleihen, gibt es zusätzliche Methoden, die die App abrufen kann.</span><span class="sxs-lookup"><span data-stu-id="2843d-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="2843d-365">Signalr. Sample StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="2843d-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="2843d-366">Die `StockTickerHub`-Klasse definiert vier zusätzliche Methoden, die vom Client aufgerufen werden können:</span><span class="sxs-lookup"><span data-stu-id="2843d-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="2843d-367">Die App Ruft `OpenMarket`, `CloseMarket`und `Reset` als Antwort auf die Schaltflächen oben auf der Seite auf.</span><span class="sxs-lookup"><span data-stu-id="2843d-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="2843d-368">Sie veranschaulichen das Muster eines Clients, der eine Änderung des Zustands auslöst, der sofort an alle Clients weitergegeben wird.</span><span class="sxs-lookup"><span data-stu-id="2843d-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="2843d-369">Jede dieser Methoden Ruft eine Methode in der `StockTicker` Klasse auf, die die Markt Zustandsänderung verursacht und dann den neuen Status überträgt.</span><span class="sxs-lookup"><span data-stu-id="2843d-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="2843d-370">Signalr. Sample StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="2843d-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="2843d-371">In der `StockTicker`-Klasse behält die APP den Marktstatus mit einer `MarketState`-Eigenschaft bei, die einen `MarketState` Enumerationswert zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="2843d-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="2843d-372">Jede Methode, die den Marktstatus ändert, wird in einem Sperrblock angezeigt, da die `StockTicker` Klasse Thread sicher sein muss:</span><span class="sxs-lookup"><span data-stu-id="2843d-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="2843d-373">Um sicherzustellen, dass dieser Code Thread sicher ist, wird das `_marketState` Feld, das die `MarketState` Eigenschaft unterstützt, die `volatile`festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="2843d-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="2843d-374">Die Methoden `BroadcastMarketStateChange` und `BroadcastMarketReset` ähneln der von Ihnen bereits erkannten broadcaststockprice-Methode, mit der Ausnahme, dass Sie verschiedene Methoden aufruft, die auf dem Client definiert sind:</span><span class="sxs-lookup"><span data-stu-id="2843d-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="2843d-375">Zusätzliche Funktionen auf dem Client, die vom Server aufgerufen werden können</span><span class="sxs-lookup"><span data-stu-id="2843d-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="2843d-376">Die `updateStockPrice`-Funktion verarbeitet nun sowohl die Tabelle als auch die tickeranzeige und verwendet `jQuery.Color`, um rote und grüne Farben zu blinken.</span><span class="sxs-lookup"><span data-stu-id="2843d-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="2843d-377">Neue Funktionen in *signalr. StockTicker. js* aktivieren und deaktivieren die Schaltflächen auf der Grundlage des Markt Zustands.</span><span class="sxs-lookup"><span data-stu-id="2843d-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="2843d-378">Außerdem wird der horizontale **Bildlauf** für den Live Kurs-Ticker beendet oder gestartet.</span><span class="sxs-lookup"><span data-stu-id="2843d-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="2843d-379">Da viele Funktionen `ticker.client`hinzugefügt werden, verwendet die APP die [Funktion "jQuery Extend](http://api.jquery.com/jQuery.extend/) ", um Sie hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="2843d-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="2843d-380">Zusätzliche Client Einrichtung nach dem Herstellen der Verbindung</span><span class="sxs-lookup"><span data-stu-id="2843d-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="2843d-381">Nachdem der Client die Verbindung hergestellt hat, müssen zusätzliche Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="2843d-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="2843d-382">Stellen Sie fest, ob der Markt geöffnet oder geschlossen ist, um die entsprechende `marketOpened` oder `marketClosed` Funktion aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="2843d-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="2843d-383">Fügen Sie die Server Methodenaufrufe an die Schaltflächen an.</span><span class="sxs-lookup"><span data-stu-id="2843d-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="2843d-384">Die Server Methoden sind erst dann mit den Schaltflächen verknüpft, wenn die APP die Verbindung herstellt.</span><span class="sxs-lookup"><span data-stu-id="2843d-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="2843d-385">Dies ist der Fall, wenn der Code die Server Methoden nicht abrufen kann, bevor diese verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="2843d-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2843d-386">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="2843d-386">Additional resources</span></span>

<span data-ttu-id="2843d-387">In diesem Tutorial haben Sie gelernt, wie Sie eine signalr-Anwendung programmieren, die Nachrichten vom Server an alle verbundenen Clients überträgt.</span><span class="sxs-lookup"><span data-stu-id="2843d-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="2843d-388">Nun können Sie Nachrichten in regelmäßigen Abständen und als Antwort auf Benachrichtigungen von einem beliebigen Client übertragen.</span><span class="sxs-lookup"><span data-stu-id="2843d-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="2843d-389">Sie können das Konzept der Multithread-Singleton-Instanz verwenden, um den Serverstatus in Online-Spielszenarien mit mehreren spielen beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="2843d-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="2843d-390">Ein Beispiel finden Sie [auf der Grundlage von signalr im shootr-Spiel](https://github.com/NTaylorMullen/ShootR).</span><span class="sxs-lookup"><span data-stu-id="2843d-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="2843d-391">Lernprogramme, in denen Peer-zu-Peer-Kommunikations Szenarios angezeigt werden, finden Sie unter [Getting Started with signalr](introduction-to-signalr.md) und [Echt Zeit Aktualisierung mit signalr](tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="2843d-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="2843d-392">Weitere Informationen zu signalr finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="2843d-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="2843d-393">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="2843d-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="2843d-394">Signalr-Projekt</span><span class="sxs-lookup"><span data-stu-id="2843d-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="2843d-395">Signalr GitHub und Beispiele</span><span class="sxs-lookup"><span data-stu-id="2843d-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="2843d-396">Signalr-wiki</span><span class="sxs-lookup"><span data-stu-id="2843d-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="2843d-397">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="2843d-397">Next steps</span></span>

<span data-ttu-id="2843d-398">In diesem Tutorial führen Sie Folgendes durch:</span><span class="sxs-lookup"><span data-stu-id="2843d-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2843d-399">Das Projekt wurde erstellt.</span><span class="sxs-lookup"><span data-stu-id="2843d-399">Created the project</span></span>
> * <span data-ttu-id="2843d-400">Einrichten des Servercodes</span><span class="sxs-lookup"><span data-stu-id="2843d-400">Set up the server code</span></span>
> * <span data-ttu-id="2843d-401">Untersuchen des Server Codes</span><span class="sxs-lookup"><span data-stu-id="2843d-401">Examined the server code</span></span>
> * <span data-ttu-id="2843d-402">Einrichten des Client Codes</span><span class="sxs-lookup"><span data-stu-id="2843d-402">Set up the client code</span></span>
> * <span data-ttu-id="2843d-403">Der Client Code wurde untersucht.</span><span class="sxs-lookup"><span data-stu-id="2843d-403">Examined the client code</span></span>
> * <span data-ttu-id="2843d-404">Testen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="2843d-404">Tested the application</span></span>
> * <span data-ttu-id="2843d-405">Aktivierte Protokollierung</span><span class="sxs-lookup"><span data-stu-id="2843d-405">Enabled logging</span></span>

<span data-ttu-id="2843d-406">Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie eine echt Zeit Webanwendung erstellen, die ASP.net signalr 2 verwendet.</span><span class="sxs-lookup"><span data-stu-id="2843d-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="2843d-407">Erstellen einer echt Zeit-Web-App mit signalr</span><span class="sxs-lookup"><span data-stu-id="2843d-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)
