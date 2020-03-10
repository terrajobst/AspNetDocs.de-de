---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Beginnen Sie mit ASP.net-Web-API 2 (C#)-ASP.NET 4. x
author: MikeWasson
description: Tutorial mit Code. Verwenden Sie ASP.net-Web-API, um eine Web-API zu erstellen, die eine Liste von Produkten zurückgibt.
ms.author: riande
ms.date: 11/28/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 3e35c2bc0e46dfdb4544b772775eddd533f27be3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448551"
---
# <a name="get-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="f61d4-104">Beginnen Sie mit ASP.net-Web-API 2 (C#)</span><span class="sxs-lookup"><span data-stu-id="f61d4-104">Get Started with ASP.NET Web API 2 (C#)</span></span>

<span data-ttu-id="f61d4-105">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f61d4-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f61d4-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="f61d4-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="f61d4-107">In diesem Tutorial verwenden Sie ASP.net-Web-API, um eine Web-API zu erstellen, die eine Liste der Produkte zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="f61d4-107">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

<span data-ttu-id="f61d4-108">HTTP dient nicht nur zum Reservieren von Webseiten.</span><span class="sxs-lookup"><span data-stu-id="f61d4-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="f61d4-109">HTTP ist auch eine leistungsstarke Plattform zum Entwickeln von APIs, die Dienste und Daten verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="f61d4-110">HTTP ist einfach, flexibel und allgegenwärtig.</span><span class="sxs-lookup"><span data-stu-id="f61d4-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="f61d4-111">Fast jede Plattform, die Sie sich vorstellen können, verfügt über eine HTTP-Bibliothek, sodass HTTP-Dienste eine breite Palette von Clients erreichen können, einschließlich Browsern, mobiler Geräte und herkömmlicher Desktop Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>

<span data-ttu-id="f61d4-112">ASP.net-Web-API ist ein Framework zum Entwickeln von Web-APIs auf der .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f61d4-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> 

## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f61d4-113">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="f61d4-113">Software versions used in the tutorial</span></span>

- [<span data-ttu-id="f61d4-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="f61d4-114">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- <span data-ttu-id="f61d4-115">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="f61d4-115">Web API 2</span></span>

<span data-ttu-id="f61d4-116">Eine neuere Version dieses Tutorials finden Sie [unter Erstellen einer Web-API mit ASP.net Core und Visual Studio für Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) .</span><span class="sxs-lookup"><span data-stu-id="f61d4-116">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="f61d4-117">Erstellen eines Web-API-Projekts</span><span class="sxs-lookup"><span data-stu-id="f61d4-117">Create a Web API Project</span></span>

<span data-ttu-id="f61d4-118">In diesem Tutorial verwenden Sie ASP.net-Web-API, um eine Web-API zu erstellen, die eine Liste der Produkte zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="f61d4-118">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="f61d4-119">Die Front-End-Webseite verwendet jQuery, um die Ergebnisse anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-119">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="f61d4-120">Starten Sie Visual Studio, und wählen Sie auf der **Start** Seite die Option **Neues Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="f61d4-120">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="f61d4-121">Oder wählen Sie im Menü **Datei** die Option **neu** und dann **Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="f61d4-121">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="f61d4-122">Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten.</span><span class="sxs-lookup"><span data-stu-id="f61d4-122">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="f61d4-123">Wählen Sie unter **Visualisierung C#** die Option **Web**aus.</span><span class="sxs-lookup"><span data-stu-id="f61d4-123">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="f61d4-124">Wählen Sie in der Liste der Projektvorlagen die Option **ASP.NET Webanwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="f61d4-124">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="f61d4-125">Nennen Sie das Projekt "productapp", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f61d4-125">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="f61d4-126">Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die Vorlage **leer** aus.</span><span class="sxs-lookup"><span data-stu-id="f61d4-126">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="f61d4-127">Überprüfen Sie unter &quot;Ordner und Kern Verweise hinzufügen für&quot;die **Web-API**.</span><span class="sxs-lookup"><span data-stu-id="f61d4-127">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="f61d4-128">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f61d4-128">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="f61d4-129">Sie können auch ein Web-API-Projekt mithilfe der &quot;Web-API&quot; Vorlage erstellen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-129">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="f61d4-130">Die Web-API-Vorlage verwendet ASP.NET MVC, um API-Hilfeseiten bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-130">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="f61d4-131">Ich verwende die leere Vorlage für dieses Tutorial, weil ich die Web-API ohne MVC anzeigen möchte.</span><span class="sxs-lookup"><span data-stu-id="f61d4-131">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="f61d4-132">Im Allgemeinen müssen Sie ASP.NET MVC nicht kennen, um die Web-API zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="f61d4-132">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="f61d4-133">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="f61d4-133">Adding a Model</span></span>

<span data-ttu-id="f61d4-134">Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="f61d4-134">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="f61d4-135">ASP.net-Web-API können das Modell automatisch in JSON, XML oder ein anderes Format serialisieren und dann die serialisierten Daten in den Text der http-Antwortnachricht schreiben.</span><span class="sxs-lookup"><span data-stu-id="f61d4-135">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="f61d4-136">Solange ein Client das Serialisierungsformat lesen kann, kann das Objekt deserialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="f61d4-136">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="f61d4-137">Die meisten Clients können entweder XML oder JSON analysieren.</span><span class="sxs-lookup"><span data-stu-id="f61d4-137">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="f61d4-138">Darüber hinaus kann der Client das gewünschte Format angeben, indem der Accept-Header in der HTTP-Anforderungs Nachricht festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="f61d4-138">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="f61d4-139">Erstellen Sie zunächst ein einfaches Modell, das ein Produkt darstellt.</span><span class="sxs-lookup"><span data-stu-id="f61d4-139">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="f61d4-140">Wenn der Projektmappen-Explorer nicht bereits sichtbar ist, können Sie auf das Menü **Ansicht** klicken und **Projektmappen-Explorer** wählen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-140">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="f61d4-141">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner „Modelle“.</span><span class="sxs-lookup"><span data-stu-id="f61d4-141">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="f61d4-142">Wählen Sie im Kontextmenü die Option **Hinzufügen** und dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="f61d4-142">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="f61d4-143">Benennen Sie die Klasse &quot;Product&quot;.</span><span class="sxs-lookup"><span data-stu-id="f61d4-143">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="f61d4-144">Fügen Sie der `Product`-Klasse die folgenden Eigenschaften hinzu.</span><span class="sxs-lookup"><span data-stu-id="f61d4-144">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="f61d4-145">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="f61d4-145">Adding a Controller</span></span>

<span data-ttu-id="f61d4-146">In der Web-API ist ein *Controller* ein Objekt zum Verarbeiten von HTTP-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-146">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="f61d4-147">Wir fügen einen Controller hinzu, der entweder eine Liste mit Produkten oder ein einzelnes von der ID angegebenes Produkt zurückgeben kann.</span><span class="sxs-lookup"><span data-stu-id="f61d4-147">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="f61d4-148">Wenn Sie ASP.NET MVC verwendet haben, sind Sie bereits mit Controllern vertraut.</span><span class="sxs-lookup"><span data-stu-id="f61d4-148">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="f61d4-149">Web-API-Controller ähneln MVC-Controllern, erben aber die **apicontroller** -Klasse anstelle der **Controller** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="f61d4-149">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="f61d4-150">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner „Controller“.</span><span class="sxs-lookup"><span data-stu-id="f61d4-150">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="f61d4-151">Wählen Sie **Hinzufügen** und dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="f61d4-151">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="f61d4-152">Wählen Sie im Dialogfeld **Gerüst hinzufügen** die Option **Web API Controller - Empty** (Web-API-Controller – Leer).</span><span class="sxs-lookup"><span data-stu-id="f61d4-152">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="f61d4-153">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="f61d4-153">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="f61d4-154">Benennen Sie im Dialogfeld " **Controller hinzufügen** " den Controller &quot;ProductController-&quot;.</span><span class="sxs-lookup"><span data-stu-id="f61d4-154">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="f61d4-155">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="f61d4-155">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="f61d4-156">Das Gerüst erstellt im Ordner "Controllers" eine Datei mit dem Namen ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="f61d4-156">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="f61d4-157">Sie müssen die Controller nicht in einem Ordner mit dem Namen "Controllers" platzieren.</span><span class="sxs-lookup"><span data-stu-id="f61d4-157">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="f61d4-158">Der Ordnername ist nur eine bequeme Möglichkeit zum Organisieren der Quelldateien.</span><span class="sxs-lookup"><span data-stu-id="f61d4-158">The folder name is just a convenient way to organize your source files.</span></span>

<span data-ttu-id="f61d4-159">Doppelklicken Sie auf die Datei, um sie zu öffnen, falls sie noch nicht geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="f61d4-159">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="f61d4-160">Ersetzen Sie den Code in dieser Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="f61d4-160">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="f61d4-161">Um das Beispiel einfach zu halten, werden Produkte in einem Fixed-Array in der Controller Klasse gespeichert.</span><span class="sxs-lookup"><span data-stu-id="f61d4-161">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="f61d4-162">Natürlich würden Sie in einer echten Anwendung eine Datenbank Abfragen oder eine andere externe Datenquelle verwenden.</span><span class="sxs-lookup"><span data-stu-id="f61d4-162">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="f61d4-163">Der Controller definiert zwei Methoden, die Produkte zurückgeben:</span><span class="sxs-lookup"><span data-stu-id="f61d4-163">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="f61d4-164">Die `GetAllProducts`-Methode gibt die gesamte Liste der Produkte als **IEnumerable-&lt;Product&gt;** Type zurück.</span><span class="sxs-lookup"><span data-stu-id="f61d4-164">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="f61d4-165">Die `GetProduct`-Methode sucht nach ihrer ID nach einem einzelnen Produkt.</span><span class="sxs-lookup"><span data-stu-id="f61d4-165">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="f61d4-166">Das ist alles!</span><span class="sxs-lookup"><span data-stu-id="f61d4-166">That's it!</span></span> <span data-ttu-id="f61d4-167">Sie verfügen über eine funktionierende Web-API.</span><span class="sxs-lookup"><span data-stu-id="f61d4-167">You have a working web API.</span></span> <span data-ttu-id="f61d4-168">Jede Methode auf dem Controller entspricht einem oder mehreren URIs:</span><span class="sxs-lookup"><span data-stu-id="f61d4-168">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="f61d4-169">Controller-Methode</span><span class="sxs-lookup"><span data-stu-id="f61d4-169">Controller Method</span></span> | <span data-ttu-id="f61d4-170">URI</span><span class="sxs-lookup"><span data-stu-id="f61d4-170">URI</span></span> |
| --- | --- |
| <span data-ttu-id="f61d4-171">Getallproducts</span><span class="sxs-lookup"><span data-stu-id="f61d4-171">GetAllProducts</span></span> | <span data-ttu-id="f61d4-172">/api/products</span><span class="sxs-lookup"><span data-stu-id="f61d4-172">/api/products</span></span> |
| <span data-ttu-id="f61d4-173">GetProduct</span><span class="sxs-lookup"><span data-stu-id="f61d4-173">GetProduct</span></span> | <span data-ttu-id="f61d4-174">/API/Products/-*ID*</span><span class="sxs-lookup"><span data-stu-id="f61d4-174">/api/products/*id*</span></span> |

<span data-ttu-id="f61d4-175">Bei der `GetProduct`-Methode ist die *ID* im URI ein Platzhalter.</span><span class="sxs-lookup"><span data-stu-id="f61d4-175">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="f61d4-176">Um z. b. das Produkt mit der ID 5 zu erhalten, wird der URI `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="f61d4-176">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="f61d4-177">Weitere Informationen zur Weiterleitung von HTTP-Anforderungen an Controller Methoden durch die Web-API finden Sie unter [Routing in ASP.net-Web-API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f61d4-177">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="f61d4-178">Aufrufen der Web-API mit JavaScript und jQuery</span><span class="sxs-lookup"><span data-stu-id="f61d4-178">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="f61d4-179">In diesem Abschnitt fügen wir eine HTML-Seite hinzu, die AJAX verwendet, um die Web-API aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-179">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="f61d4-180">Wir verwenden jQuery zum Erstellen der AJAX-Aufrufe und zum Aktualisieren der Seite mit den Ergebnissen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-180">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="f61d4-181">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen**und dann **Neues Element**aus.</span><span class="sxs-lookup"><span data-stu-id="f61d4-181">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="f61d4-182">Wählen Sie im Dialogfeld **Neues Element hinzufügen** unter **Visual C#** den **webknoten** aus, und wählen Sie dann das Element **HTML-Seite** aus.</span><span class="sxs-lookup"><span data-stu-id="f61d4-182">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="f61d4-183">Benennen Sie die Seite &quot;Index. html&quot;.</span><span class="sxs-lookup"><span data-stu-id="f61d4-183">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="f61d4-184">Ersetzen Sie alles in dieser Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="f61d4-184">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="f61d4-185">Es gibt mehrere Möglichkeiten, um jQuery herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-185">There are several ways to get jQuery.</span></span> <span data-ttu-id="f61d4-186">In diesem Beispiel habe ich das [Microsoft AJAX CDN](../../../ajax/cdn/overview.md)verwendet.</span><span class="sxs-lookup"><span data-stu-id="f61d4-186">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="f61d4-187">Sie können Sie auch aus [http://jquery.com/](http://jquery.com/)herunterladen, und die Projektvorlage "Web-API" ASP.NET enthält auch jQuery.</span><span class="sxs-lookup"><span data-stu-id="f61d4-187">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="f61d4-188">Eine Liste der Produkte wird erhalten.</span><span class="sxs-lookup"><span data-stu-id="f61d4-188">Getting a List of Products</span></span>

<span data-ttu-id="f61d4-189">Senden Sie eine HTTP GET-Anforderung an &quot;/API/Products-&quot;, um eine Liste der Produkte zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="f61d4-189">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="f61d4-190">Die jQuery-Funktion [getjson](http://api.jquery.com/jQuery.getJSON/) sendet eine AJAX-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="f61d4-190">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="f61d4-191">Für Response enthält ein Array von JSON-Objekten.</span><span class="sxs-lookup"><span data-stu-id="f61d4-191">For response contains array of JSON objects.</span></span> <span data-ttu-id="f61d4-192">Die `done`-Funktion gibt einen Rückruf an, der aufgerufen wird, wenn die Anforderung erfolgreich ist.</span><span class="sxs-lookup"><span data-stu-id="f61d4-192">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="f61d4-193">Im Rückruf aktualisieren wir das DOM mit den Produktinformationen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-193">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="f61d4-194">Erhalten eines Produkts nach ID</span><span class="sxs-lookup"><span data-stu-id="f61d4-194">Getting a Product By ID</span></span>

<span data-ttu-id="f61d4-195">Wenn Sie ein Produkt anhand der ID erhalten möchten, senden Sie eine HTTP GET-Anforderung an &quot;/API/Products/*ID*&quot;, wobei *ID* die Produkt-ID ist.</span><span class="sxs-lookup"><span data-stu-id="f61d4-195">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="f61d4-196">Es wird weiterhin `getJSON` aufgerufen, um die AJAX-Anforderung zu senden, aber dieses Mal wird die ID in den Anforderungs-URI eingefügt.</span><span class="sxs-lookup"><span data-stu-id="f61d4-196">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="f61d4-197">Die Antwort dieser Anforderung ist eine JSON-Darstellung eines einzelnen Produkts.</span><span class="sxs-lookup"><span data-stu-id="f61d4-197">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="f61d4-198">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="f61d4-198">Running the Application</span></span>

<span data-ttu-id="f61d4-199">Drücken Sie die F5-TASTE, um das Debuggen der Anwendung zu starten.</span><span class="sxs-lookup"><span data-stu-id="f61d4-199">Press F5 to start debugging the application.</span></span> <span data-ttu-id="f61d4-200">Die Webseite sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="f61d4-200">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="f61d4-201">Geben Sie die ID ein, und klicken Sie auf suchen, um ein Produkt nach ID zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="f61d4-201">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="f61d4-202">Wenn Sie eine ungültige ID eingeben, gibt der Server einen HTTP-Fehler zurück:</span><span class="sxs-lookup"><span data-stu-id="f61d4-202">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="f61d4-203">Verwenden von F12 zum Anzeigen der HTTP-Anforderung und-Antwort</span><span class="sxs-lookup"><span data-stu-id="f61d4-203">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="f61d4-204">Wenn Sie mit einem HTTP-Dienst arbeiten, kann es sehr nützlich sein, die HTTP-Anforderung anzuzeigen und Nachrichten anzufordern.</span><span class="sxs-lookup"><span data-stu-id="f61d4-204">When you are working with an HTTP service, it can be very useful to see the HTTP request and request messages.</span></span> <span data-ttu-id="f61d4-205">Hierfür können Sie die F12-Entwicklertools in Internet Explorer 9 verwenden.</span><span class="sxs-lookup"><span data-stu-id="f61d4-205">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="f61d4-206">Drücken Sie in Internet Explorer 9 die Taste **F12** , um die Tools zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-206">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="f61d4-207">Klicken Sie auf die Registerkarte **Netzwerk** , und drücken Sie **Aufzeichnung starten**.</span><span class="sxs-lookup"><span data-stu-id="f61d4-207">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="f61d4-208">Kehren Sie nun zurück zur Webseite, und drücken Sie **F5** , um die Webseite neu zu laden.</span><span class="sxs-lookup"><span data-stu-id="f61d4-208">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="f61d4-209">Internet Explorer erfasst den HTTP-Datenverkehr zwischen dem Browser und dem Webserver.</span><span class="sxs-lookup"><span data-stu-id="f61d4-209">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="f61d4-210">In der Zusammenfassungs Ansicht wird der gesamte Netzwerk Datenverkehr für eine Seite angezeigt:</span><span class="sxs-lookup"><span data-stu-id="f61d4-210">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="f61d4-211">Suchen Sie den Eintrag für den relativen URI "API/Products/".</span><span class="sxs-lookup"><span data-stu-id="f61d4-211">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="f61d4-212">Wählen Sie diesen Eintrag aus, und klicken Sie **auf zur detaillierten Ansicht**wechseln.</span><span class="sxs-lookup"><span data-stu-id="f61d4-212">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="f61d4-213">In der Detailansicht gibt es Registerkarten, um die Anforderungs-und Antwortheader und Textkörper anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-213">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="f61d4-214">Wenn Sie z. b. auf die Registerkarte **Anforderungs Header** klicken, sehen Sie, dass der Client &quot;Anwendung/JSON-&quot; im Accept-Header angefordert hat.</span><span class="sxs-lookup"><span data-stu-id="f61d4-214">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="f61d4-215">Wenn Sie auf die Registerkarte "Antworttext" klicken, können Sie sehen, wie die Produktliste in JSON serialisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="f61d4-215">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="f61d4-216">Andere Browser verfügen über eine ähnliche Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="f61d4-216">Other browsers have similar functionality.</span></span> <span data-ttu-id="f61d4-217">Ein weiteres nützliches Tool ist " [fddler](http://www.fiddler2.com/fiddler2/)", ein webdebugproxy.</span><span class="sxs-lookup"><span data-stu-id="f61d4-217">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="f61d4-218">Sie können mit "fddler" den HTTP-Datenverkehr anzeigen und auch HTTP-Anforderungen verfassen, um Ihnen die vollständige Kontrolle über die HTTP-Header in der Anforderung zu bieten.</span><span class="sxs-lookup"><span data-stu-id="f61d4-218">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="f61d4-219">Diese APP wird in Azure ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="f61d4-219">See this App Running on Azure</span></span>

<span data-ttu-id="f61d4-220">Möchten Sie sehen, dass die fertige Site als Live-Web-App ausgeführt wird?</span><span class="sxs-lookup"><span data-stu-id="f61d4-220">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="f61d4-221">Sie können eine vollständige Version der APP für Ihr Azure-Konto bereitstellen, indem Sie einfach auf die folgende Schaltfläche klicken.</span><span class="sxs-lookup"><span data-stu-id="f61d4-221">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="f61d4-222">Sie benötigen ein Azure-Konto, um diese Lösung in Azure bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-222">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="f61d4-223">Wenn Sie noch nicht über ein Konto verfügen, haben Sie die folgenden Optionen:</span><span class="sxs-lookup"><span data-stu-id="f61d4-223">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="f61d4-224">[Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) . Sie erhalten Gutschriften, die Sie verwenden können, um kostenpflichtige Azure-Dienste zu testen. selbst nach ihrer Nutzung können Sie das Konto behalten und kostenlose Azure-Dienste nutzen.</span><span class="sxs-lookup"><span data-stu-id="f61d4-224">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="f61d4-225">[Aktivieren von MSDN-Abonnenten Vorteilen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : Ihr MSDN-Abonnement bietet Ihnen jeden Monat ein Guthaben, das Sie für kostenpflichtige Azure-Dienste nutzen können.</span><span class="sxs-lookup"><span data-stu-id="f61d4-225">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f61d4-226">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="f61d4-226">Next Steps</span></span>

- <span data-ttu-id="f61d4-227">Ein ausführlichere Beispiel für einen HTTP-Dienst, der Post-, Put-und DELETE-Aktionen und Schreibvorgänge in eine Datenbank unterstützt, finden [Sie unter Verwenden der Web-API 2 mit Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="f61d4-227">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="f61d4-228">Weitere Informationen zum Erstellen von flüssigen und reaktionsfähigen Webanwendungen zusätzlich zu einem HTTP-Dienst finden Sie unter [ASP.net Single Page Application](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="f61d4-228">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="f61d4-229">Weitere Informationen zum Bereitstellen eines Visual Studio-Webprojekts für Azure App Service finden Sie unter [Erstellen einer ASP.net-Web-App in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="f61d4-229">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
