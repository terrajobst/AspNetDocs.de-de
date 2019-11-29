---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Aufrufen eines odata-Dienstanbieter von einemC#.NET-Client () | Microsoft-Dokumentation
author: MikeWasson
description: In diesem Tutorial wird gezeigt, wie ein odata-Dienst C# von einer Client Anwendung aufgerufen wird. Im Tutorial verwendete Software Versionen Visual Studio 2013 (funktioniert mit Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 6a289fcb843634eeeefef1e0767e04e0be8b6973
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600383"
---
# <a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="99d16-104">Zugreifen auf einen OData-Dienst über einen .NET-Client (C#)</span><span class="sxs-lookup"><span data-stu-id="99d16-104">Calling an OData Service From a .NET Client (C#)</span></span>

<span data-ttu-id="99d16-105">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="99d16-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="99d16-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="99d16-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="99d16-107">In diesem Tutorial wird gezeigt, wie ein odata-Dienst C# von einer Client Anwendung aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="99d16-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="99d16-108">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="99d16-108">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="99d16-109">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (funktioniert mit Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="99d16-109">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="99d16-110">WCF Data Services-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="99d16-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="99d16-111">Web-API 2.</span><span class="sxs-lookup"><span data-stu-id="99d16-111">Web API 2.</span></span> <span data-ttu-id="99d16-112">(Der odata-Beispiel Dienst wird mit der Web-API 2 erstellt, aber die Client Anwendung ist nicht von der Web-API abhängig.)</span><span class="sxs-lookup"><span data-stu-id="99d16-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>

<span data-ttu-id="99d16-113">In diesem Tutorial werden die Schritte zum Erstellen einer Client Anwendung erläutert, die einen odata-Dienst aufruft.</span><span class="sxs-lookup"><span data-stu-id="99d16-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="99d16-114">Der odata-Dienst macht die folgenden Entitäten verfügbar:</span><span class="sxs-lookup"><span data-stu-id="99d16-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="99d16-115">In den folgenden Artikeln wird beschrieben, wie der odata-Dienst in der Web-API implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="99d16-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="99d16-116">(Sie müssen diese jedoch nicht lesen, um sich mit diesem Tutorial vertraut zu machen.)</span><span class="sxs-lookup"><span data-stu-id="99d16-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="99d16-117">Erstellen eines odata-Endpunkts in der Web-API 2</span><span class="sxs-lookup"><span data-stu-id="99d16-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="99d16-118">Odata-Entitäts Beziehungen in der Web-API 2</span><span class="sxs-lookup"><span data-stu-id="99d16-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="99d16-119">OData-Aktionen in der Web-API 2</span><span class="sxs-lookup"><span data-stu-id="99d16-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="99d16-120">Generieren des Dienst Proxys</span><span class="sxs-lookup"><span data-stu-id="99d16-120">Generate the Service Proxy</span></span>

<span data-ttu-id="99d16-121">Der erste Schritt besteht im Generieren eines Dienst Proxys.</span><span class="sxs-lookup"><span data-stu-id="99d16-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="99d16-122">Der Dienst Proxy ist eine .NET-Klasse, die Methoden für den Zugriff auf den odata-Dienst definiert.</span><span class="sxs-lookup"><span data-stu-id="99d16-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="99d16-123">Der Proxy übersetzt Methodenaufrufe in HTTP-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="99d16-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="99d16-124">Beginnen Sie, indem Sie das odata-Dienstprojekt in Visual Studio öffnen.</span><span class="sxs-lookup"><span data-stu-id="99d16-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="99d16-125">Drücken Sie STRG + F5, um den Dienst lokal in IIS Express auszuführen.</span><span class="sxs-lookup"><span data-stu-id="99d16-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="99d16-126">Notieren Sie sich die lokale Adresse, einschließlich der Portnummer, die Visual Studio zuweist.</span><span class="sxs-lookup"><span data-stu-id="99d16-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="99d16-127">Sie benötigen diese Adresse beim Erstellen des Proxys.</span><span class="sxs-lookup"><span data-stu-id="99d16-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="99d16-128">Öffnen Sie als nächstes eine andere Instanz von Visual Studio, und erstellen Sie ein Konsolen Anwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="99d16-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="99d16-129">Die Konsolenanwendung wird als odata-Client Anwendung verwendet.</span><span class="sxs-lookup"><span data-stu-id="99d16-129">The console application will be our OData client application.</span></span> <span data-ttu-id="99d16-130">(Sie können das Projekt auch der gleichen Projekt Mappe wie der Dienst hinzufügen.)</span><span class="sxs-lookup"><span data-stu-id="99d16-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="99d16-131">Die restlichen Schritte beziehen sich auf das Konsolen Projekt.</span><span class="sxs-lookup"><span data-stu-id="99d16-131">The remaining steps refer the console project.</span></span>

<span data-ttu-id="99d16-132">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf **Verweise** , und wählen Sie **Dienstverweis hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="99d16-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="99d16-133">Geben Sie im Dialogfeld **Dienstverweis hinzufügen** die Adresse des odata-Dienstanbieter ein:</span><span class="sxs-lookup"><span data-stu-id="99d16-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="99d16-134">Dabei ist *Port* die Portnummer.</span><span class="sxs-lookup"><span data-stu-id="99d16-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="99d16-135">Geben Sie als **Namespace**"productservice" ein.</span><span class="sxs-lookup"><span data-stu-id="99d16-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="99d16-136">Mit dieser Option wird der Namespace der Proxy Klasse definiert.</span><span class="sxs-lookup"><span data-stu-id="99d16-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="99d16-137">Klicken Sie auf **Go**.</span><span class="sxs-lookup"><span data-stu-id="99d16-137">Click **Go**.</span></span> <span data-ttu-id="99d16-138">Visual Studio liest das odata-Metadatendokument, um die Entitäten im Dienst zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="99d16-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="99d16-139">Klicken Sie auf **OK** , um die Proxy Klasse zu Ihrem Projekt hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="99d16-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="99d16-140">Erstellen einer Instanz der Dienst Proxy Klasse</span><span class="sxs-lookup"><span data-stu-id="99d16-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="99d16-141">Erstellen Sie in der `Main`-Methode wie folgt eine neue Instanz der Proxy Klasse:</span><span class="sxs-lookup"><span data-stu-id="99d16-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="99d16-142">Verwenden Sie erneut die tatsächliche Portnummer, in der der Dienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="99d16-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="99d16-143">Wenn Sie den Dienst bereitstellen, verwenden Sie den URI des Live Dienstanbieter.</span><span class="sxs-lookup"><span data-stu-id="99d16-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="99d16-144">Sie müssen den Proxy nicht aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="99d16-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="99d16-145">Der folgende Code fügt einen Ereignishandler hinzu, der die Anforderungs-URIs im Konsolenfenster ausgibt.</span><span class="sxs-lookup"><span data-stu-id="99d16-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="99d16-146">Dieser Schritt ist nicht erforderlich, aber es ist interessant, die URIs für jede Abfrage anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="99d16-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="99d16-147">Abfragen des Dienstanbieter</span><span class="sxs-lookup"><span data-stu-id="99d16-147">Query the Service</span></span>

<span data-ttu-id="99d16-148">Der folgende Code Ruft die Liste der Produkte aus dem odata-Dienst ab.</span><span class="sxs-lookup"><span data-stu-id="99d16-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="99d16-149">Beachten Sie, dass Sie keinen Code schreiben müssen, um die HTTP-Anforderung zu senden oder die Antwort zu analysieren.</span><span class="sxs-lookup"><span data-stu-id="99d16-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="99d16-150">Die Proxy Klasse führt dies automatisch aus, wenn Sie die `Container.Products` Auflistung in der **foreach** -Schleife auflisten.</span><span class="sxs-lookup"><span data-stu-id="99d16-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="99d16-151">Wenn Sie die Anwendung ausführen, sollte die Ausgabe wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="99d16-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="99d16-152">Um eine Entität anhand der ID zu erhalten, verwenden Sie eine `where`-Klausel.</span><span class="sxs-lookup"><span data-stu-id="99d16-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="99d16-153">Im restlichen Abschnitt dieses Themas zeige ich nicht die gesamte `Main` Funktion, sondern nur den Code, der zum Anrufen des Dienstanbieter benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="99d16-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="99d16-154">Abfrage Optionen anwenden</span><span class="sxs-lookup"><span data-stu-id="99d16-154">Apply Query Options</span></span>

<span data-ttu-id="99d16-155">Odata definiert [Abfrage Optionen](../supporting-odata-query-options.md) , die verwendet werden können, um Daten zu filtern, zu sortieren, abzufragen usw.</span><span class="sxs-lookup"><span data-stu-id="99d16-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="99d16-156">Im Dienst Proxy können Sie diese Optionen mithilfe verschiedener LINQ-Ausdrücke anwenden.</span><span class="sxs-lookup"><span data-stu-id="99d16-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="99d16-157">In diesem Abschnitt werden kurze Beispiele angezeigt.</span><span class="sxs-lookup"><span data-stu-id="99d16-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="99d16-158">Weitere Informationen finden Sie im Thema über [Legungen zu LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) auf MSDN.</span><span class="sxs-lookup"><span data-stu-id="99d16-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="99d16-159">Filtern ($Filter)</span><span class="sxs-lookup"><span data-stu-id="99d16-159">Filtering ($filter)</span></span>

<span data-ttu-id="99d16-160">Verwenden Sie zum Filtern eine `where`-Klausel.</span><span class="sxs-lookup"><span data-stu-id="99d16-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="99d16-161">Im folgenden Beispiel wird nach Produktkategorie gefiltert.</span><span class="sxs-lookup"><span data-stu-id="99d16-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="99d16-162">Dieser Code entspricht der folgenden odata-Abfrage.</span><span class="sxs-lookup"><span data-stu-id="99d16-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="99d16-163">Beachten Sie, dass der Proxy die `where`-Klausel in einen odata-`$filter` Ausdruck konvertiert.</span><span class="sxs-lookup"><span data-stu-id="99d16-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="99d16-164">Sortierung ($OrderBy)</span><span class="sxs-lookup"><span data-stu-id="99d16-164">Sorting ($orderby)</span></span>

<span data-ttu-id="99d16-165">Verwenden Sie zum Sortieren eine `orderby`-Klausel.</span><span class="sxs-lookup"><span data-stu-id="99d16-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="99d16-166">Das folgende Beispiel sortiert nach Preis, von der höchsten zum niedrigsten.</span><span class="sxs-lookup"><span data-stu-id="99d16-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="99d16-167">Im folgenden finden Sie die entsprechende odata-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="99d16-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="99d16-168">Client seitiges Paging ($Skip und $Top)</span><span class="sxs-lookup"><span data-stu-id="99d16-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="99d16-169">Bei großen Entitätenmengen kann es vorkommen, dass der Client die Anzahl der Ergebnisse einschränkt.</span><span class="sxs-lookup"><span data-stu-id="99d16-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="99d16-170">Ein Client kann z. b. 10 Einträge gleichzeitig anzeigen.</span><span class="sxs-lookup"><span data-stu-id="99d16-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="99d16-171">Dies wird als *Client seitiges Paging*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="99d16-171">This is called *client-side paging*.</span></span> <span data-ttu-id="99d16-172">(Es gibt auch [Serverseitiges Paging](../supporting-odata-query-options.md#server-paging), bei dem der Server die Anzahl der Ergebnisse einschränkt.) Verwenden Sie zum Ausführen von Client seitigem Paging die LINQ **Skip** -und **Take** -Methoden.</span><span class="sxs-lookup"><span data-stu-id="99d16-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="99d16-173">Im folgenden Beispiel werden die ersten 40-Ergebnisse ausgelassen und die nächsten 10.</span><span class="sxs-lookup"><span data-stu-id="99d16-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="99d16-174">Im folgenden finden Sie die entsprechende odata-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="99d16-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="99d16-175">Select ($Select) und erweitern ($Expand)</span><span class="sxs-lookup"><span data-stu-id="99d16-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="99d16-176">Verwenden Sie die `DataServiceQuery<t>.Expand`-Methode, um Verwandte Entitäten einzubeziehen.</span><span class="sxs-lookup"><span data-stu-id="99d16-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="99d16-177">Wenn Sie z. b. die `Supplier` für jede `Product`einschließen möchten:</span><span class="sxs-lookup"><span data-stu-id="99d16-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="99d16-178">Im folgenden finden Sie die entsprechende odata-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="99d16-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="99d16-179">Verwenden Sie die LINQ **Select** -Klausel, um die Form der Antwort zu ändern.</span><span class="sxs-lookup"><span data-stu-id="99d16-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="99d16-180">Im folgenden Beispiel wird nur der Name jedes Produkts ohne weitere Eigenschaften abgerufen.</span><span class="sxs-lookup"><span data-stu-id="99d16-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="99d16-181">Im folgenden finden Sie die entsprechende odata-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="99d16-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="99d16-182">Eine SELECT-Klausel kann Verwandte Entitäten einschließen.</span><span class="sxs-lookup"><span data-stu-id="99d16-182">A select clause can include related entities.</span></span> <span data-ttu-id="99d16-183">Wenn Sie in diesem Fall " **Expand**;" nicht aufzurufen. der Proxy enthält in diesem Fall automatisch die Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="99d16-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="99d16-184">Das folgende Beispiel ruft den Namen und den Lieferanten der einzelnen Produkte ab.</span><span class="sxs-lookup"><span data-stu-id="99d16-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="99d16-185">Im folgenden finden Sie die entsprechende odata-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="99d16-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="99d16-186">Beachten Sie, dass Sie die Option **$Expand** enthält.</span><span class="sxs-lookup"><span data-stu-id="99d16-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="99d16-187">Weitere Informationen zu $SELECT und $Expand finden Sie unter [Verwenden von $SELECT, $Expand und $Value in der Web-API 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="99d16-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="99d16-188">Neue Entität hinzufügen</span><span class="sxs-lookup"><span data-stu-id="99d16-188">Add a New Entity</span></span>

<span data-ttu-id="99d16-189">Um eine neue Entität zu einer Entitätenmenge hinzuzufügen, nennen Sie `AddToEntitySet`, wobei *EntitySet* der Name der Entitätenmenge ist.</span><span class="sxs-lookup"><span data-stu-id="99d16-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="99d16-190">`AddToProducts` fügt z. b. der `Products` Entitätenmenge eine neue `Product` hinzu.</span><span class="sxs-lookup"><span data-stu-id="99d16-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="99d16-191">Wenn Sie den Proxy generieren, erstellt WCF Data Services diese stark typisierten **AddTo** -Methoden automatisch.</span><span class="sxs-lookup"><span data-stu-id="99d16-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="99d16-192">Um einen Link zwischen zwei Entitäten hinzuzufügen, verwenden Sie die **AddLink** -Methode und die **SetLink** -Methode.</span><span class="sxs-lookup"><span data-stu-id="99d16-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="99d16-193">Der folgende Code fügt einen neuen Lieferanten und ein neues Produkt hinzu und erstellt dann Links zwischen Ihnen.</span><span class="sxs-lookup"><span data-stu-id="99d16-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="99d16-194">Verwenden Sie **AddLink** , wenn die Navigations Eigenschaft eine Auflistung ist.</span><span class="sxs-lookup"><span data-stu-id="99d16-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="99d16-195">In diesem Beispiel fügen wir ein Produkt zur `Products`-Sammlung auf dem Lieferanten hinzu.</span><span class="sxs-lookup"><span data-stu-id="99d16-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="99d16-196">Verwenden Sie **SetLink** , wenn die Navigations Eigenschaft eine einzelne Entität ist.</span><span class="sxs-lookup"><span data-stu-id="99d16-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="99d16-197">In diesem Beispiel legen wir die `Supplier`-Eigenschaft für das Produkt fest.</span><span class="sxs-lookup"><span data-stu-id="99d16-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="99d16-198">Aktualisieren/Patch</span><span class="sxs-lookup"><span data-stu-id="99d16-198">Update / Patch</span></span>

<span data-ttu-id="99d16-199">Um eine Entität zu aktualisieren, müssen Sie die **UpdateObject** -Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="99d16-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="99d16-200">Das Update wird ausgeführt, wenn Sie **SaveChanges**aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="99d16-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="99d16-201">WCF sendet standardmäßig eine HTTP-Merge-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="99d16-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="99d16-202">Die **patchonupdate** -Option weist WCF an, stattdessen einen HTTP-Patch zu senden.</span><span class="sxs-lookup"><span data-stu-id="99d16-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="99d16-203">Warum Patchen und Zusammenführen?</span><span class="sxs-lookup"><span data-stu-id="99d16-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="99d16-204">In der ursprünglichen HTTP 1,1-Spezifikation ([RCF 2616](http://tools.ietf.org/html/rfc2616)) wurde keine HTTP-Methode mit der Semantik "partielle Aktualisierung" definiert.</span><span class="sxs-lookup"><span data-stu-id="99d16-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="99d16-205">Zur Unterstützung von Teil Updates hat die odata-Spezifikation die Merge-Methode definiert.</span><span class="sxs-lookup"><span data-stu-id="99d16-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="99d16-206">In 2010 hat [RFC 5789](http://tools.ietf.org/html/rfc5789) die patchmethode für Teil Updates definiert.</span><span class="sxs-lookup"><span data-stu-id="99d16-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="99d16-207">Einen Teil des Verlaufs finden Sie in diesem [Blogbeitrag](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) im Blogbeitrag WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="99d16-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="99d16-208">Heute wird Patch für Merge bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="99d16-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="99d16-209">Der vom Web-API-Gerüst erstellte odata-Controller unterstützt beide Methoden.</span><span class="sxs-lookup"><span data-stu-id="99d16-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>

<span data-ttu-id="99d16-210">Wenn Sie die gesamte Entität (Put Semantik) ersetzen möchten, geben Sie die **replaceonupdate** -Option an.</span><span class="sxs-lookup"><span data-stu-id="99d16-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="99d16-211">Dies bewirkt, dass WCF eine HTTP PUT-Anforderung sendet.</span><span class="sxs-lookup"><span data-stu-id="99d16-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="99d16-212">Löschen einer Entität</span><span class="sxs-lookup"><span data-stu-id="99d16-212">Delete an Entity</span></span>

<span data-ttu-id="99d16-213">Zum Löschen einer Entität wird **DeleteObject**aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="99d16-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="99d16-214">Aufrufen einer odata-Aktion</span><span class="sxs-lookup"><span data-stu-id="99d16-214">Invoke an OData Action</span></span>

<span data-ttu-id="99d16-215">In odata können [Aktionen](odata-actions.md) serverseitiges Verhalten hinzugefügt werden, die nicht einfach als CRUD-Vorgänge für Entitäten definiert werden können.</span><span class="sxs-lookup"><span data-stu-id="99d16-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="99d16-216">Obwohl in den odata-Metadatendokumenten die Aktionen beschrieben werden, werden von der Proxy Klasse keine stark typisierten Methoden für Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="99d16-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="99d16-217">Sie können eine odata-Aktion weiterhin mithilfe der generischen **Execute** -Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="99d16-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="99d16-218">Sie müssen jedoch die Datentypen der Parameter und den Rückgabewert kennen.</span><span class="sxs-lookup"><span data-stu-id="99d16-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="99d16-219">Beispielsweise übernimmt die `RateProduct` Aktion einen Parameter mit dem Namen "Bewertung" vom Typ "`Int32`" und gibt eine `double`zurück.</span><span class="sxs-lookup"><span data-stu-id="99d16-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="99d16-220">Der folgende Code zeigt, wie diese Aktion aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="99d16-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="99d16-221">Weitere Informationen finden Sie unter[Aufrufen von Dienst Vorgängen und-Aktionen](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="99d16-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="99d16-222">Eine Möglichkeit besteht darin, die **Container** Klasse so zu erweitern, dass Sie eine stark typisierte Methode bereitstellt, die die Aktion aufruft:</span><span class="sxs-lookup"><span data-stu-id="99d16-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
