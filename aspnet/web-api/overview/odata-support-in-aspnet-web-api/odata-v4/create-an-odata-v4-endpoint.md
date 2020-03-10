---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Erstellen eines odata V4-Endpunkts mithilfe von ASP.net-Web-API 2,2 | Microsoft-Dokumentation
author: MikeWasson
description: Der Open Data Protocol (odata) ist ein Datenzugriffs Protokoll für das Web. Odata bietet eine einheitliche Möglichkeit zum Abfragen und Bearbeiten von Datasets mithilfe von CRUD-Vorgängen...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 81d134cbd3231b9a0d5537ccbd1bbfe6419254af
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484497"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a><span data-ttu-id="d6008-104">Erstellen eines odata V4-Endpunkts mithilfe von ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="d6008-104">Create an OData v4 Endpoint Using ASP.NET Web API</span></span> 

> <span data-ttu-id="d6008-105">Der Open Data Protocol (odata) ist ein Datenzugriffs Protokoll für das Web.</span><span class="sxs-lookup"><span data-stu-id="d6008-105">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="d6008-106">Odata bietet eine einheitliche Möglichkeit zum Abfragen und Bearbeiten von Datasets mithilfe von CRUD-Vorgängen (erstellen, lesen, aktualisieren und löschen).</span><span class="sxs-lookup"><span data-stu-id="d6008-106">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
>
> <span data-ttu-id="d6008-107">ASP.net-Web-API unterstützt sowohl V3 als auch V4 des Protokolls.</span><span class="sxs-lookup"><span data-stu-id="d6008-107">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="d6008-108">Sie können sogar einen V4-Endpunkt haben, der parallel mit einem v3-Endpunkt ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d6008-108">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
>
> <span data-ttu-id="d6008-109">In diesem Tutorial wird gezeigt, wie Sie einen odata V4-Endpunkt erstellen, der CRUD-Vorgänge unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d6008-109">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d6008-110">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="d6008-110">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="d6008-111">Web-API 5,2</span><span class="sxs-lookup"><span data-stu-id="d6008-111">Web API 5.2</span></span>
> - <span data-ttu-id="d6008-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="d6008-112">OData v4</span></span>
> - <span data-ttu-id="d6008-113">Visual Studio 2017 (Visual Studio 2017 [hier](https://visualstudio.microsoft.com/downloads/)herunterladen)</span><span class="sxs-lookup"><span data-stu-id="d6008-113">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/))</span></span>
> - <span data-ttu-id="d6008-114">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="d6008-114">Entity Framework 6</span></span>
> - <span data-ttu-id="d6008-115">.NET 4.7.2</span><span class="sxs-lookup"><span data-stu-id="d6008-115">.NET 4.7.2</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="d6008-116">Tutorialversionen</span><span class="sxs-lookup"><span data-stu-id="d6008-116">Tutorial versions</span></span>
>
> <span data-ttu-id="d6008-117">Informationen zu odata, Version 3, finden Sie unter [Erstellen eines odata-V3-Endpunkts](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="d6008-117">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="d6008-118">Erstellen des Visual Studio-Projekts</span><span class="sxs-lookup"><span data-stu-id="d6008-118">Create the Visual Studio Project</span></span>

<span data-ttu-id="d6008-119">Wählen Sie in Visual Studio im Menü **Datei** die Option **neu** &gt; **Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="d6008-119">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="d6008-120">Erweitern Sie **installierte** &gt;  **C# Visual** &gt; **Web**, und wählen Sie die Vorlage **ASP.net Web Application (.NET Framework)** aus.</span><span class="sxs-lookup"><span data-stu-id="d6008-120">Expand **Installed** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application (.NET Framework)** template.</span></span> <span data-ttu-id="d6008-121">Nennen Sie das Projekt &quot;productservice&quot;.</span><span class="sxs-lookup"><span data-stu-id="d6008-121">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

<span data-ttu-id="d6008-122">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6008-122">Select **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

<span data-ttu-id="d6008-123">Wählen Sie die Vorlage **Leer** aus.</span><span class="sxs-lookup"><span data-stu-id="d6008-123">Select the **Empty** template.</span></span> <span data-ttu-id="d6008-124">Wählen Sie unter **Ordner und Kern Verweise hinzufügen für:** die Option **Web-API**aus.</span><span class="sxs-lookup"><span data-stu-id="d6008-124">Under **Add folders and core references for:**, select **Web API**.</span></span> <span data-ttu-id="d6008-125">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6008-125">Select **OK**.</span></span>

## <a name="install-the-odata-packages"></a><span data-ttu-id="d6008-126">Installieren der odata-Pakete</span><span class="sxs-lookup"><span data-stu-id="d6008-126">Install the OData packages</span></span>

<span data-ttu-id="d6008-127">Wählen Sie im Menü **Tools** **NuGet-Paket-Manager** &gt; **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="d6008-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="d6008-128">Geben Sie im Fenster der Paket-Manager-Konsole Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="d6008-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="d6008-129">Mit diesem Befehl werden die neuesten odata-nuget-Pakete installiert.</span><span class="sxs-lookup"><span data-stu-id="d6008-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="d6008-130">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="d6008-130">Add a model class</span></span>

<span data-ttu-id="d6008-131">Ein *Modell* ist ein Objekt, das eine Daten Entität in der Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="d6008-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="d6008-132">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner „Modelle“.</span><span class="sxs-lookup"><span data-stu-id="d6008-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="d6008-133">Wählen Sie im Kontextmenü &gt; **Klasse** **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="d6008-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="d6008-134">Gemäß der Konvention werden Modellklassen in den Ordner "Models" eingefügt, aber Sie müssen diese Konvention nicht in ihren eigenen Projekten befolgen.</span><span class="sxs-lookup"><span data-stu-id="d6008-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>

<span data-ttu-id="d6008-135">Geben Sie der Klassen den Namen `Product`.</span><span class="sxs-lookup"><span data-stu-id="d6008-135">Name the class `Product`.</span></span> <span data-ttu-id="d6008-136">Ersetzen Sie den Code in der Datei Product.cs durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d6008-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="d6008-137">Die `Id`-Eigenschaft ist der Entitäts Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="d6008-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="d6008-138">Clients können Entitäten nach Schlüssel Abfragen.</span><span class="sxs-lookup"><span data-stu-id="d6008-138">Clients can query entities by key.</span></span> <span data-ttu-id="d6008-139">Um z. b. das Produkt mit der ID 5 zu erhalten, wird der URI `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="d6008-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="d6008-140">Die `Id`-Eigenschaft ist auch der Primärschlüssel in der Back-End-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d6008-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="d6008-141">Aktivieren von Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d6008-141">Enable Entity Framework</span></span>

<span data-ttu-id="d6008-142">In diesem Tutorial verwenden wir Entity Framework (EF) Code First, um die Back-End-Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d6008-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="d6008-143">Die Web-API-odata erfordert EF nicht.</span><span class="sxs-lookup"><span data-stu-id="d6008-143">Web API OData does not require EF.</span></span> <span data-ttu-id="d6008-144">Verwenden Sie eine beliebige Datenzugriffs Ebene, die Daten Bank Entitäten in Modelle übersetzen kann.</span><span class="sxs-lookup"><span data-stu-id="d6008-144">Use any data-access layer that can translate database entities into models.</span></span>

<span data-ttu-id="d6008-145">Installieren Sie zunächst das nuget-Paket für EF.</span><span class="sxs-lookup"><span data-stu-id="d6008-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="d6008-146">Wählen Sie im Menü **Tools** **NuGet-Paket-Manager** &gt; **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="d6008-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="d6008-147">Geben Sie im Fenster der Paket-Manager-Konsole Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="d6008-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="d6008-148">Öffnen Sie die Datei Web. config, und fügen Sie den folgenden Abschnitt innerhalb des **Konfigurations** Elements nach dem **configabschnitts** -Element hinzu.</span><span class="sxs-lookup"><span data-stu-id="d6008-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="d6008-149">Mit dieser Einstellung wird eine Verbindungs Zeichenfolge für eine localdb-Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d6008-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="d6008-150">Diese Datenbank wird verwendet, wenn Sie die APP lokal ausführen.</span><span class="sxs-lookup"><span data-stu-id="d6008-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="d6008-151">Fügen Sie als nächstes dem Ordner Models eine Klasse mit dem Namen `ProductsContext` hinzu:</span><span class="sxs-lookup"><span data-stu-id="d6008-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="d6008-152">Im Konstruktor gibt `"name=ProductsContext"` den Namen der Verbindungs Zeichenfolge an.</span><span class="sxs-lookup"><span data-stu-id="d6008-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="d6008-153">Konfigurieren des odata-Endpunkts</span><span class="sxs-lookup"><span data-stu-id="d6008-153">Configure the OData endpoint</span></span>

<span data-ttu-id="d6008-154">Öffnen Sie die Datei-App\_Start/webapiconfig. cs.</span><span class="sxs-lookup"><span data-stu-id="d6008-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="d6008-155">Fügen Sie die folgenden **using** -Anweisungen hinzu:</span><span class="sxs-lookup"><span data-stu-id="d6008-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="d6008-156">Fügen Sie dann der **Register** -Methode den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="d6008-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="d6008-157">Dieser Code führt zwei Aufgaben aus:</span><span class="sxs-lookup"><span data-stu-id="d6008-157">This code does two things:</span></span>

- <span data-ttu-id="d6008-158">Erstellt ein Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="d6008-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="d6008-159">Fügt eine Route hinzu.</span><span class="sxs-lookup"><span data-stu-id="d6008-159">Adds a route.</span></span>

<span data-ttu-id="d6008-160">Ein EDM ist ein abstraktes Modell der Daten.</span><span class="sxs-lookup"><span data-stu-id="d6008-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="d6008-161">Das EDM wird verwendet, um das dienstmetadatendokument zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d6008-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="d6008-162">Die **odataconaconmodelbuilder** -Klasse erstellt ein EDM mithilfe von Standard Benennungs Konventionen.</span><span class="sxs-lookup"><span data-stu-id="d6008-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="d6008-163">Diese Vorgehensweise erfordert den geringsten Code.</span><span class="sxs-lookup"><span data-stu-id="d6008-163">This approach requires the least code.</span></span> <span data-ttu-id="d6008-164">Wenn Sie mehr Kontrolle über das EDM haben möchten, können Sie das EDM mit der **odatamodelta Builder** -Klasse erstellen, indem Sie Eigenschaften, Schlüssel und Navigations Eigenschaften explizit hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d6008-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="d6008-165">Eine *Route* teilt der Web-API mit, wie HTTP-Anforderungen an den Endpunkt weitergeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="d6008-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="d6008-166">Um eine odata V4-Route zu erstellen, rufen Sie die **mapodataserviceroute** -Erweiterungsmethode auf.</span><span class="sxs-lookup"><span data-stu-id="d6008-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="d6008-167">Wenn Ihre Anwendung über mehrere odata-Endpunkte verfügt, erstellen Sie jeweils eine separate Route.</span><span class="sxs-lookup"><span data-stu-id="d6008-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="d6008-168">Übergeben Sie jeder Route einen eindeutigen Routennamen und ein Präfix.</span><span class="sxs-lookup"><span data-stu-id="d6008-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="d6008-169">Hinzufügen des odata-Controllers</span><span class="sxs-lookup"><span data-stu-id="d6008-169">Add the OData controller</span></span>

<span data-ttu-id="d6008-170">Ein *Controller* ist eine Klasse, die HTTP-Anforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="d6008-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="d6008-171">Sie erstellen einen separaten Controller für jede Entitätenmenge in Ihrem odata-Dienst.</span><span class="sxs-lookup"><span data-stu-id="d6008-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="d6008-172">In diesem Tutorial erstellen Sie einen Controller für die `Product`-Entität.</span><span class="sxs-lookup"><span data-stu-id="d6008-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="d6008-173">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controllers, und wählen Sie &gt; **Klasse** **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="d6008-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="d6008-174">Geben Sie der Klassen den Namen `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="d6008-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="d6008-175">In der Version dieses Tutorials für odata V3 wird das **Hinzufügen eines Controller** Gerüsts verwendet.</span><span class="sxs-lookup"><span data-stu-id="d6008-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="d6008-176">Derzeit gibt es kein Gerüst für odata v4.</span><span class="sxs-lookup"><span data-stu-id="d6008-176">Currently, there is no scaffolding for OData v4.</span></span>

<span data-ttu-id="d6008-177">Ersetzen Sie den Code in ProductsController.cs durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="d6008-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="d6008-178">Der Controller verwendet die `ProductsContext`-Klasse, um mithilfe von EF auf die Datenbank zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="d6008-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="d6008-179">Beachten Sie, **dass der Controller** die verwerfen-Methode überschreibt, um den **produczcontext**zu verwerfen.</span><span class="sxs-lookup"><span data-stu-id="d6008-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="d6008-180">Dies ist der Ausgangspunkt für den Controller.</span><span class="sxs-lookup"><span data-stu-id="d6008-180">This is the starting point for the controller.</span></span> <span data-ttu-id="d6008-181">Als Nächstes fügen wir Methoden für alle CRUD-Vorgänge hinzu.</span><span class="sxs-lookup"><span data-stu-id="d6008-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="query-the-entity-set"></a><span data-ttu-id="d6008-182">Abfragen der Entitätenmenge</span><span class="sxs-lookup"><span data-stu-id="d6008-182">Query the entity set</span></span>

<span data-ttu-id="d6008-183">Fügen Sie `ProductsController`die folgenden Methoden hinzu.</span><span class="sxs-lookup"><span data-stu-id="d6008-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="d6008-184">Die Parameter lose Version der `Get`-Methode gibt die gesamte Products-Auflistung zurück.</span><span class="sxs-lookup"><span data-stu-id="d6008-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="d6008-185">Die `Get`-Methode mit einem *Key* -Parameter sucht ein Produkt nach seinem Schlüssel (in diesem Fall die `Id`-Eigenschaft).</span><span class="sxs-lookup"><span data-stu-id="d6008-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="d6008-186">Das **[enablequery]** -Attribut ermöglicht es Clients, die Abfrage mithilfe von Abfrage Optionen wie $Filter, $Sort und $Page zu ändern.</span><span class="sxs-lookup"><span data-stu-id="d6008-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="d6008-187">Weitere Informationen finden Sie [unter unterstützen von odata-Abfrage Optionen](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="d6008-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="add-an-entity-to-the-entity-set"></a><span data-ttu-id="d6008-188">Hinzufügen einer Entität zur Entitätenmenge</span><span class="sxs-lookup"><span data-stu-id="d6008-188">Add an entity to the entity set</span></span>

<span data-ttu-id="d6008-189">Fügen Sie `ProductsController`die folgende Methode hinzu, damit Clients der Datenbank ein neues Produkt hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="d6008-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a><span data-ttu-id="d6008-190">Aktualisieren einer Entität</span><span class="sxs-lookup"><span data-stu-id="d6008-190">Update an entity</span></span>

<span data-ttu-id="d6008-191">Odata unterstützt zwei verschiedene Semantik zum Aktualisieren von Entitäten: Patch und Put.</span><span class="sxs-lookup"><span data-stu-id="d6008-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="d6008-192">Patch führt ein teilweises Update aus.</span><span class="sxs-lookup"><span data-stu-id="d6008-192">PATCH performs a partial update.</span></span> <span data-ttu-id="d6008-193">Der Client gibt nur die Eigenschaften an, die aktualisiert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="d6008-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="d6008-194">Put ersetzt die gesamte Entität.</span><span class="sxs-lookup"><span data-stu-id="d6008-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="d6008-195">Der Nachteil von Put besteht darin, dass der Client Werte für alle Eigenschaften in der Entität senden muss, einschließlich der Werte, die nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="d6008-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="d6008-196">Die [odata-Spezifikation](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) gibt an, dass Patch bevorzugt wird.</span><span class="sxs-lookup"><span data-stu-id="d6008-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="d6008-197">In jedem Fall ist dies der Code für die Patch-und Put-Methoden:</span><span class="sxs-lookup"><span data-stu-id="d6008-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="d6008-198">Im Fall von Patch verwendet der Controller den **Delta&lt;t&gt;** Typ, um die Änderungen zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="d6008-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="delete-an-entity"></a><span data-ttu-id="d6008-199">Löschen einer Entität</span><span class="sxs-lookup"><span data-stu-id="d6008-199">Delete an entity</span></span>

<span data-ttu-id="d6008-200">Um Clients das Löschen eines Produkts aus der Datenbank zu ermöglichen, fügen Sie `ProductsController`die folgende Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="d6008-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
