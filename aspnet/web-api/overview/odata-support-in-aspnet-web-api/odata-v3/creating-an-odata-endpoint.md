---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Erstellen eines odata-V3-Endpunkts mit der Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Der Open Data Protocol (odata) ist ein Datenzugriffs Protokoll für das Web. Odata bietet eine einheitliche Möglichkeit zum Strukturieren von Daten, Abfragen der Daten und Bearbeiten der Daten...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: e68a454398f109dfd089be9c9a44d3fe662acc2f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448221"
---
# <a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="8289b-104">Erstellen eines odata-V3-Endpunkts mit der Web-API 2</span><span class="sxs-lookup"><span data-stu-id="8289b-104">Creating an OData v3 Endpoint with Web API 2</span></span>

<span data-ttu-id="8289b-105">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8289b-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8289b-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="8289b-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="8289b-107">Der [Open Data Protocol](http://www.odata.org/) (odata) ist ein Datenzugriffs Protokoll für das Web.</span><span class="sxs-lookup"><span data-stu-id="8289b-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="8289b-108">Odata bietet eine einheitliche Möglichkeit zum Strukturieren von Daten, Abfragen der Daten und Bearbeiten des Datasets mithilfe von CRUD-Vorgängen (erstellen, lesen, aktualisieren und löschen).</span><span class="sxs-lookup"><span data-stu-id="8289b-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="8289b-109">Odata unterstützt die Formate AtomPub (XML) und JSON.</span><span class="sxs-lookup"><span data-stu-id="8289b-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="8289b-110">Odata definiert auch eine Möglichkeit, Metadaten zu den Daten verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="8289b-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="8289b-111">Clients können die Metadaten zum Ermitteln der Typinformationen und Beziehungen für das DataSet verwenden.</span><span class="sxs-lookup"><span data-stu-id="8289b-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
>
> <span data-ttu-id="8289b-112">ASP.net-Web-API erleichtert das Erstellen eines odata-Endpunkts für ein DataSet.</span><span class="sxs-lookup"><span data-stu-id="8289b-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="8289b-113">Sie können genau steuern, welche odata-Vorgänge der Endpunkt unterstützt.</span><span class="sxs-lookup"><span data-stu-id="8289b-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="8289b-114">Sie können mehrere odata-Endpunkte neben nicht-odata-Endpunkten hosten.</span><span class="sxs-lookup"><span data-stu-id="8289b-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="8289b-115">Sie haben vollständige Kontrolle über das Datenmodell, die Back-End-Geschäftslogik und die Datenschicht.</span><span class="sxs-lookup"><span data-stu-id="8289b-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8289b-116">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="8289b-116">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="8289b-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8289b-117">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="8289b-118">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="8289b-118">Web API 2</span></span>
> - <span data-ttu-id="8289b-119">Odata, Version 3</span><span class="sxs-lookup"><span data-stu-id="8289b-119">OData Version 3</span></span>
> - <span data-ttu-id="8289b-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8289b-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="8289b-121">Webdebugproxy für das webdebugging (optional)</span><span class="sxs-lookup"><span data-stu-id="8289b-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
>
> <span data-ttu-id="8289b-122">Die Web-API-odata-Unterstützung wurde in [ASP.net and Web Tools 2012,2-Update](https://go.microsoft.com/fwlink/?LinkId=282650)hinzugefügt</span><span class="sxs-lookup"><span data-stu-id="8289b-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="8289b-123">In diesem Tutorial wird jedoch Gerüstbau verwendet, der in Visual Studio 2013 hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="8289b-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>

<span data-ttu-id="8289b-124">In diesem Tutorial erstellen Sie einen einfachen odata-Endpunkt, der von Clients abgefragt werden kann.</span><span class="sxs-lookup"><span data-stu-id="8289b-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="8289b-125">Außerdem erstellen Sie einen C# Client für den Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="8289b-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="8289b-126">Nachdem Sie dieses Lernprogramm durchgeführt haben, zeigen Sie im nächsten Satz von Tutorials, wie Sie weitere Funktionen hinzufügen, einschließlich Entitäts Beziehungen, Aktionen und $Expand/$SELECT.</span><span class="sxs-lookup"><span data-stu-id="8289b-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="8289b-127">Erstellen des Visual Studio-Projekts</span><span class="sxs-lookup"><span data-stu-id="8289b-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="8289b-128">Hinzufügen eines Entitäts Modells</span><span class="sxs-lookup"><span data-stu-id="8289b-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="8289b-129">Hinzufügen eines odata-Controllers</span><span class="sxs-lookup"><span data-stu-id="8289b-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="8289b-130">Hinzufügen des EDM und der Route</span><span class="sxs-lookup"><span data-stu-id="8289b-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="8289b-131">Ausgangsdaten der Datenbank (optional)</span><span class="sxs-lookup"><span data-stu-id="8289b-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="8289b-132">Untersuchen des odata-Endpunkts</span><span class="sxs-lookup"><span data-stu-id="8289b-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="8289b-133">Odata-Serialisierungsformate</span><span class="sxs-lookup"><span data-stu-id="8289b-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="8289b-134">Erstellen des Visual Studio-Projekts</span><span class="sxs-lookup"><span data-stu-id="8289b-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="8289b-135">In diesem Tutorial erstellen Sie einen odata-Endpunkt, der grundlegende CRUD-Vorgänge unterstützt.</span><span class="sxs-lookup"><span data-stu-id="8289b-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="8289b-136">Der Endpunkt macht eine einzelne Ressource, eine Liste mit Produkten, verfügbar.</span><span class="sxs-lookup"><span data-stu-id="8289b-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="8289b-137">In späteren Tutorials werden weitere Funktionen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="8289b-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="8289b-138">Starten Sie Visual Studio, und wählen Sie auf der Start Seite die Option **Neues Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="8289b-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="8289b-139">Oder wählen Sie im Menü **Datei** die Option **neu** und dann **Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="8289b-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="8289b-140">Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und C# erweitern Sie den Knoten visuelle Knoten.</span><span class="sxs-lookup"><span data-stu-id="8289b-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="8289b-141">Wählen Sie unter **Visualisierung C#** die Option **Web**aus.</span><span class="sxs-lookup"><span data-stu-id="8289b-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="8289b-142">Wählen Sie **die Vorlage ASP.NET Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="8289b-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="8289b-143">Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die Vorlage **leer** aus.</span><span class="sxs-lookup"><span data-stu-id="8289b-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="8289b-144">Überprüfen Sie unter &quot;Ordner und Kern Verweise hinzufügen für...&quot;die **Web-API**.</span><span class="sxs-lookup"><span data-stu-id="8289b-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="8289b-145">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="8289b-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="8289b-146">Hinzufügen eines Entitäts Modells</span><span class="sxs-lookup"><span data-stu-id="8289b-146">Add an Entity Model</span></span>

<span data-ttu-id="8289b-147">Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="8289b-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="8289b-148">Für dieses Tutorial benötigen wir ein Modell, das ein Produkt darstellt.</span><span class="sxs-lookup"><span data-stu-id="8289b-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="8289b-149">Das Modell entspricht dem odata-Entitätstyp.</span><span class="sxs-lookup"><span data-stu-id="8289b-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="8289b-150">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner „Modelle“.</span><span class="sxs-lookup"><span data-stu-id="8289b-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="8289b-151">Wählen Sie im Kontextmenü die Option **Hinzufügen** und dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="8289b-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="8289b-152">Benennen Sie die Klasse im Dialogfeld **Neues Element hinzufügen** &quot;Product&quot;.</span><span class="sxs-lookup"><span data-stu-id="8289b-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="8289b-153">Gemäß der Konvention werden Modellklassen in den Ordner "Models" eingefügt.</span><span class="sxs-lookup"><span data-stu-id="8289b-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="8289b-154">Sie müssen diese Konvention nicht in ihren eigenen Projekten befolgen, aber wir verwenden Sie für dieses Tutorial.</span><span class="sxs-lookup"><span data-stu-id="8289b-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>

<span data-ttu-id="8289b-155">Fügen Sie in der Datei Product.cs die folgende Klassendefinition hinzu:</span><span class="sxs-lookup"><span data-stu-id="8289b-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="8289b-156">Die ID-Eigenschaft wird als Entitäts Schlüssel verwendet.</span><span class="sxs-lookup"><span data-stu-id="8289b-156">The ID property will be the entity key.</span></span> <span data-ttu-id="8289b-157">Clients können Produkte nach ID Abfragen.</span><span class="sxs-lookup"><span data-stu-id="8289b-157">Clients can query products by ID.</span></span> <span data-ttu-id="8289b-158">Dieses Feld wäre auch der Primärschlüssel in der Back-End-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="8289b-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="8289b-159">Erstellen Sie jetzt das Projekt.</span><span class="sxs-lookup"><span data-stu-id="8289b-159">Build the project now.</span></span> <span data-ttu-id="8289b-160">Im nächsten Schritt verwenden wir ein Visual Studio-Gerüst, das Reflektion verwendet, um den Produkttyp zu suchen.</span><span class="sxs-lookup"><span data-stu-id="8289b-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="8289b-161">Hinzufügen eines odata-Controllers</span><span class="sxs-lookup"><span data-stu-id="8289b-161">Add an OData Controller</span></span>

<span data-ttu-id="8289b-162">Ein *Controller* ist eine Klasse, die HTTP-Anforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="8289b-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="8289b-163">Sie definieren einen separaten Controller für jede Entitätenmenge in Ihrem odata-Dienst.</span><span class="sxs-lookup"><span data-stu-id="8289b-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="8289b-164">In diesem Tutorial erstellen wir einen einzelnen Controller.</span><span class="sxs-lookup"><span data-stu-id="8289b-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="8289b-165">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controller.</span><span class="sxs-lookup"><span data-stu-id="8289b-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="8289b-166">Wählen Sie **Hinzufügen** und dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="8289b-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="8289b-167">Wählen Sie im Dialogfeld **Gerüst hinzufügen** &quot;Web-API 2-odata-Controller mit Aktionen aus, indem Sie Entity Framework&quot;verwenden.</span><span class="sxs-lookup"><span data-stu-id="8289b-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="8289b-168">Benennen Sie den Controller im Dialogfeld **Controller hinzufügen** mit "ProductController".</span><span class="sxs-lookup"><span data-stu-id="8289b-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="8289b-169">Aktivieren Sie das Kontrollkästchen &quot;Aktionen für Async-Controller verwenden&quot;.</span><span class="sxs-lookup"><span data-stu-id="8289b-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="8289b-170">Wählen Sie in der Dropdown Liste **Modell** die Product-Klasse aus.</span><span class="sxs-lookup"><span data-stu-id="8289b-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="8289b-171">Klicken Sie auf die Schaltfläche **neuer Datenkontext...** .</span><span class="sxs-lookup"><span data-stu-id="8289b-171">Click the **New data context...** button.</span></span> <span data-ttu-id="8289b-172">Belassen Sie den Standardnamen für den Daten Kontexttyp, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="8289b-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="8289b-173">Klicken Sie im Dialogfeld Controller hinzufügen auf Hinzufügen, um den Controller hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="8289b-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="8289b-174">Hinweis: Wenn Sie eine Fehlermeldung erhalten, die &quot;besagt, dass beim Eingeben des&quot;Typs ein Fehler aufgetreten ist, stellen Sie sicher, dass Sie das Visual Studio-Projekt erstellt haben, nachdem Sie die Product-Klasse hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="8289b-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="8289b-175">Das Gerüst verwendet Reflektion, um die Klasse zu finden.</span><span class="sxs-lookup"><span data-stu-id="8289b-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="8289b-176">Das Gerüst fügt dem Projekt zwei Code Dateien hinzu:</span><span class="sxs-lookup"><span data-stu-id="8289b-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="8289b-177">Products.cs definiert den Web-API-Controller, der den odata-Endpunkt implementiert.</span><span class="sxs-lookup"><span data-stu-id="8289b-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="8289b-178">ProductServiceContext.cs stellt Methoden zum Abfragen der zugrunde liegenden Datenbank mithilfe Entity Framework bereit.</span><span class="sxs-lookup"><span data-stu-id="8289b-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="8289b-179">Hinzufügen des EDM und der Route</span><span class="sxs-lookup"><span data-stu-id="8289b-179">Add the EDM and Route</span></span>

<span data-ttu-id="8289b-180">Erweitern Sie in Projektmappen-Explorer den Ordner App\_Start, und öffnen Sie die Datei mit dem Namen WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="8289b-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="8289b-181">Diese Klasse enthält Konfigurations Code für die Web-API.</span><span class="sxs-lookup"><span data-stu-id="8289b-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="8289b-182">Ersetzen Sie diesen Code durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="8289b-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="8289b-183">Dieser Code führt zwei Aufgaben aus:</span><span class="sxs-lookup"><span data-stu-id="8289b-183">This code does two things:</span></span>

- <span data-ttu-id="8289b-184">Erstellt ein Entity Data Model (EDM) für den odata-Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="8289b-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="8289b-185">Fügt eine Route für den Endpunkt hinzu.</span><span class="sxs-lookup"><span data-stu-id="8289b-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="8289b-186">Ein EDM ist ein abstraktes Modell der Daten.</span><span class="sxs-lookup"><span data-stu-id="8289b-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="8289b-187">Das EDM wird verwendet, um das Metadatendokument zu erstellen und die URIs für den Dienst zu definieren.</span><span class="sxs-lookup"><span data-stu-id="8289b-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="8289b-188">Der **odataconaconmodelbuilder** erstellt mithilfe eines Satzes von Standard Benennungs Konventionen EDM ein EDM.</span><span class="sxs-lookup"><span data-stu-id="8289b-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="8289b-189">Diese Vorgehensweise erfordert den geringsten Code.</span><span class="sxs-lookup"><span data-stu-id="8289b-189">This approach requires the least code.</span></span> <span data-ttu-id="8289b-190">Wenn Sie mehr Kontrolle über das EDM haben möchten, können Sie das EDM mit der **odatamodelta Builder** -Klasse erstellen, indem Sie Eigenschaften, Schlüssel und Navigations Eigenschaften explizit hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="8289b-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="8289b-191">Die **EntitySet** -Methode fügt dem EDM eine Entitätenmenge hinzu:</span><span class="sxs-lookup"><span data-stu-id="8289b-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="8289b-192">Die Zeichenfolge "Products" definiert den Namen der Entitätenmenge.</span><span class="sxs-lookup"><span data-stu-id="8289b-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="8289b-193">Der Name des Controllers muss mit dem Namen der Entitätenmenge identisch sein.</span><span class="sxs-lookup"><span data-stu-id="8289b-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="8289b-194">In diesem Tutorial heißt die Entitätenmenge "Products", und der Controller hat den Namen "`ProductsController`".</span><span class="sxs-lookup"><span data-stu-id="8289b-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="8289b-195">Wenn Sie die Entitätenmenge "productset" genannt haben, benennen Sie den Controller `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="8289b-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="8289b-196">Beachten Sie, dass ein Endpunkt über mehrere Entitätenmengen verfügen kann.</span><span class="sxs-lookup"><span data-stu-id="8289b-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="8289b-197">Nennen Sie **EntitySet&lt;t&gt;** für jede Entitätenmenge, und definieren Sie dann einen entsprechenden Controller.</span><span class="sxs-lookup"><span data-stu-id="8289b-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="8289b-198">Die **mapodataroute** -Methode fügt eine Route für den odata-Endpunkt hinzu.</span><span class="sxs-lookup"><span data-stu-id="8289b-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="8289b-199">Der erste Parameter ist ein Anzeige Name für die Route.</span><span class="sxs-lookup"><span data-stu-id="8289b-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="8289b-200">Clients Ihres Dienstanbieter sehen diesen Namen nicht.</span><span class="sxs-lookup"><span data-stu-id="8289b-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="8289b-201">Der zweite Parameter ist das URI-Präfix für den Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="8289b-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="8289b-202">Bei diesem Code ist der URI für die Products-Entitätenmenge http://<em>Hostname</em>/odata/Products.</span><span class="sxs-lookup"><span data-stu-id="8289b-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="8289b-203">Die Anwendung kann mehr als einen odata-Endpunkt aufweisen.</span><span class="sxs-lookup"><span data-stu-id="8289b-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="8289b-204">Nennen Sie für jeden Endpunkt <strong>mapodataroute</strong> , und geben Sie einen eindeutigen Routennamen und ein eindeutiges URI-Präfix an.</span><span class="sxs-lookup"><span data-stu-id="8289b-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="8289b-205">Ausgangsdaten der Datenbank (optional)</span><span class="sxs-lookup"><span data-stu-id="8289b-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="8289b-206">In diesem Schritt verwenden Sie Entity Framework, um für die Datenbank einen Ausgangswert für einige Testdaten zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="8289b-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="8289b-207">Dieser Schritt ist optional, aber Sie können den odata-Endpunkt sofort testen.</span><span class="sxs-lookup"><span data-stu-id="8289b-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="8289b-208">Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="8289b-208">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="8289b-209">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="8289b-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="8289b-210">Dadurch wird ein Ordner namens Migrationen und eine Codedatei mit dem Namen Configuration.cs hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="8289b-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="8289b-211">Öffnen Sie diese Datei, und fügen Sie der `Configuration.Seed`-Methode den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="8289b-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="8289b-212">Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="8289b-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="8289b-213">Diese Befehle generieren Code, der die Datenbank erstellt, und führt dann den Code aus.</span><span class="sxs-lookup"><span data-stu-id="8289b-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="8289b-214">Untersuchen des odata-Endpunkts</span><span class="sxs-lookup"><span data-stu-id="8289b-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="8289b-215">In diesem Abschnitt verwenden wir den [webdebugproxy von "fddler](http://www.fiddler2.com) ", um Anforderungen an den Endpunkt zu senden und die Antwort Nachrichten zu untersuchen.</span><span class="sxs-lookup"><span data-stu-id="8289b-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="8289b-216">Auf diese Weise können Sie die Funktionen eines odata-Endpunkts verstehen.</span><span class="sxs-lookup"><span data-stu-id="8289b-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="8289b-217">Drücken Sie in Visual Studio F5, um das Debuggen zu starten.</span><span class="sxs-lookup"><span data-stu-id="8289b-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="8289b-218">Standardmäßig öffnet Visual Studio Ihren Browser, um zu `http://localhost:*port*`, wobei *Port* die in den Projekteinstellungen konfigurierte Portnummer ist.</span><span class="sxs-lookup"><span data-stu-id="8289b-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="8289b-219">Sie können die Portnummer in den Projekteinstellungen ändern.</span><span class="sxs-lookup"><span data-stu-id="8289b-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="8289b-220">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften**aus.</span><span class="sxs-lookup"><span data-stu-id="8289b-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="8289b-221">Wählen Sie im Fenster Eigenschaften die Option **Web**aus.</span><span class="sxs-lookup"><span data-stu-id="8289b-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="8289b-222">Geben Sie Unterprojekt- **URL**die Portnummer ein.</span><span class="sxs-lookup"><span data-stu-id="8289b-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="8289b-223">Dienst Dokument</span><span class="sxs-lookup"><span data-stu-id="8289b-223">Service Document</span></span>

<span data-ttu-id="8289b-224">Das *Dienst Dokument* enthält eine Liste der Entitätenmengen für den odata-Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="8289b-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="8289b-225">Um das Dienst Dokument zu erhalten, senden Sie eine GET-Anforderung an den Stamm-URI des Dienstanbieter.</span><span class="sxs-lookup"><span data-stu-id="8289b-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="8289b-226">Geben Sie auf der Registerkarte **Composer** den folgenden URI ein: `http://localhost:port/odata/`, wobei *Port* die Portnummer ist.</span><span class="sxs-lookup"><span data-stu-id="8289b-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="8289b-227">Klicken Sie auf die Schaltfläche **Ausführen** .</span><span class="sxs-lookup"><span data-stu-id="8289b-227">Click the **Execute** button.</span></span> <span data-ttu-id="8289b-228">Der "fddler" sendet eine HTTP GET-Anforderung an Ihre Anwendung.</span><span class="sxs-lookup"><span data-stu-id="8289b-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="8289b-229">Die Antwort sollte in der Liste Websitzungen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="8289b-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="8289b-230">Wenn alles funktioniert, lautet der Statuscode 200.</span><span class="sxs-lookup"><span data-stu-id="8289b-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="8289b-231">Doppelklicken Sie auf die Antwort in der Liste Websitzungen, um die Details der Antwortnachricht auf der Registerkarte Inspektoren anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="8289b-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="8289b-232">Die unformatierte http-Antwortnachricht sollte in etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="8289b-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="8289b-233">Standardmäßig gibt die Web-API das Dienst Dokument im AtomPub-Format zurück.</span><span class="sxs-lookup"><span data-stu-id="8289b-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="8289b-234">Fügen Sie der HTTP-Anforderung den folgenden Header hinzu, um JSON anzufordern:</span><span class="sxs-lookup"><span data-stu-id="8289b-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="8289b-235">Nun enthält die HTTP-Antwort eine JSON-Nutzlast:</span><span class="sxs-lookup"><span data-stu-id="8289b-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="8289b-236">Dienstmetadatendokument</span><span class="sxs-lookup"><span data-stu-id="8289b-236">Service Metadata Document</span></span>

<span data-ttu-id="8289b-237">Das *dienstmetadatendokument* beschreibt das Datenmodell des Dienstanbieter mithilfe einer XML-Sprache, die als konzeptionelle Schema Definitions Sprache (CSDL) bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="8289b-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="8289b-238">Das Metadatendokument zeigt die Struktur der Daten im Dienst und kann zum Generieren von Client Code verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8289b-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="8289b-239">Um das Metadatendokument zu erhalten, senden Sie eine GET-Anforderung an `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="8289b-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="8289b-240">Hier finden Sie die Metadaten für den in diesem Tutorial gezeigten Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="8289b-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="8289b-241">Entitätenmenge</span><span class="sxs-lookup"><span data-stu-id="8289b-241">Entity Set</span></span>

<span data-ttu-id="8289b-242">Um die Products-Entitätenmenge zu erhalten, senden Sie eine GET-Anforderung an `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="8289b-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="8289b-243">Entität</span><span class="sxs-lookup"><span data-stu-id="8289b-243">Entity</span></span>

<span data-ttu-id="8289b-244">Um ein einzelnes Produkt zu erhalten, senden Sie eine GET-Anforderung an `http://localhost:port/odata/Products(1)`, wobei "1" die Produkt-ID ist.</span><span class="sxs-lookup"><span data-stu-id="8289b-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="8289b-245">Odata-Serialisierungsformate</span><span class="sxs-lookup"><span data-stu-id="8289b-245">OData Serialization Formats</span></span>

<span data-ttu-id="8289b-246">Odata unterstützt mehrere Serialisierungsformate:</span><span class="sxs-lookup"><span data-stu-id="8289b-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="8289b-247">Atom-Pub (XML)</span><span class="sxs-lookup"><span data-stu-id="8289b-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="8289b-248">JSON "Light" (eingeführt in odata v3)</span><span class="sxs-lookup"><span data-stu-id="8289b-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="8289b-249">JSON "Verbose" (odata v2)</span><span class="sxs-lookup"><span data-stu-id="8289b-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="8289b-250">Standardmäßig verwendet die Web-API atompubjson "Light"-Format.</span><span class="sxs-lookup"><span data-stu-id="8289b-250">By default, Web API uses AtomPubJSON "light" format.</span></span>

<span data-ttu-id="8289b-251">Um das AtomPub-Format zu erhalten, legen Sie den Accept-Header auf "Application/Atom + XML" fest.</span><span class="sxs-lookup"><span data-stu-id="8289b-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="8289b-252">Hier ist ein Beispiel-Antworttext:</span><span class="sxs-lookup"><span data-stu-id="8289b-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="8289b-253">Sie können einen offensichtlichen Nachteil des Atom-Formats sehen: Es ist viel ausführlicher als das JSON-Licht.</span><span class="sxs-lookup"><span data-stu-id="8289b-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="8289b-254">Wenn Sie jedoch über einen Client verfügen, der AtomPub versteht, könnte der Client dieses Format über JSON bevorzugen.</span><span class="sxs-lookup"><span data-stu-id="8289b-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="8289b-255">Hier ist die JSON Light-Version der gleichen Entität:</span><span class="sxs-lookup"><span data-stu-id="8289b-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="8289b-256">Das JSON Light-Format wurde in Version 3 des odata-Protokolls eingeführt.</span><span class="sxs-lookup"><span data-stu-id="8289b-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="8289b-257">Aus Gründen der Abwärtskompatibilität kann ein Client das ältere JSON-Format "Verbose" anfordern.</span><span class="sxs-lookup"><span data-stu-id="8289b-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="8289b-258">Um ausführliche JSON anzufordern, legen Sie den Accept-Header auf `application/json;odata=verbose`fest.</span><span class="sxs-lookup"><span data-stu-id="8289b-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="8289b-259">Hier ist die ausführliche Version:</span><span class="sxs-lookup"><span data-stu-id="8289b-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="8289b-260">Dieses Format vermittelt mehr Metadaten im Antworttext, was einen beträchtlichen Aufwand für eine gesamte Sitzung verursachen kann.</span><span class="sxs-lookup"><span data-stu-id="8289b-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="8289b-261">Außerdem wird eine Dereferenzierungsebene hinzugefügt, indem das Objekt in eine Eigenschaft mit dem Namen "d" umwickelt wird.</span><span class="sxs-lookup"><span data-stu-id="8289b-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="8289b-262">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="8289b-262">Next Steps</span></span>

- [<span data-ttu-id="8289b-263">Entitäts Beziehungen hinzufügen</span><span class="sxs-lookup"><span data-stu-id="8289b-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="8289b-264">Odata-Aktionen hinzufügen</span><span class="sxs-lookup"><span data-stu-id="8289b-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="8289b-265">Odata-Dienst über einen .NET-Client aufzurufen</span><span class="sxs-lookup"><span data-stu-id="8289b-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
