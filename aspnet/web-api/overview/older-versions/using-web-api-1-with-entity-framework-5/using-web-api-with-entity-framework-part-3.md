---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Teil 3: Erstellen eines Administrator Controllers | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: f39be7a84e85db93487d246e9f8cb59c401fe5ce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447909"
---
# <a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="a4e0d-102">Teil 3: Erstellen eines Administrator Controllers</span><span class="sxs-lookup"><span data-stu-id="a4e0d-102">Part 3: Creating an Admin Controller</span></span>

<span data-ttu-id="a4e0d-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a4e0d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a4e0d-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="a4e0d-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="a4e0d-105">Hinzufügen eines Administrator Controllers</span><span class="sxs-lookup"><span data-stu-id="a4e0d-105">Add an Admin Controller</span></span>

<span data-ttu-id="a4e0d-106">In diesem Abschnitt wird ein Web-API-Controller hinzugefügt, der CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen) für Produkte unterstützt.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="a4e0d-107">Der Controller verwendet Entity Framework, um mit der Datenbankebene zu kommunizieren.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="a4e0d-108">Nur Administratoren können diesen Controller verwenden.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="a4e0d-109">Kunden greifen über einen anderen Controller auf die Produkte zu.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="a4e0d-110">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controller.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="a4e0d-111">Wählen Sie **Hinzufügen** und dann **Controller**aus.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="a4e0d-112">Benennen Sie den Controller im Dialogfeld **Controller hinzufügen** `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="a4e0d-113">Wählen Sie unter **Vorlage**&quot;API-Controller mit Lese-/Schreibaktionen aus, indem Sie Entity Framework&quot;verwenden.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="a4e0d-114">Wählen Sie unter **Modell Klasse**die Option "Product (productstore. Models)" aus.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="a4e0d-115">Wählen Sie unter **Datenkontext**die Option "&lt;neuer Datenkontext&gt;" aus.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="a4e0d-116">Wenn in der Dropdown-Dropdown-Dropdown- **Klasse** keine Modellklassen angezeigt werden, stellen Sie sicher, dass das Projekt kompiliert wurde.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="a4e0d-117">Entity Framework die Reflektion verwendet, muss die kompilierte Assembly verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>

<span data-ttu-id="a4e0d-118">Wenn Sie "&lt;neuen Datenkontext&gt;" auswählen, wird das Dialogfeld " **Neues Datenkontext** " geöffnet.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="a4e0d-119">Benennen Sie den Datenkontext `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="a4e0d-120">Klicken Sie auf **OK** , um das Dialogfeld **neuer Datenkontext** zu schließen.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="a4e0d-121">Klicken Sie im Dialogfeld **Controller hinzufügen** auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="a4e0d-122">Das Projekt wurde dem Projekt hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="a4e0d-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="a4e0d-123">Eine Klasse mit dem Namen `OrdersContext`, die von **dbcontext**abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="a4e0d-124">Diese Klasse stellt den Klebstoff zwischen den poco-Modellen und der-Datenbank bereit.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="a4e0d-125">Ein Web-API-Controller mit dem Namen `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="a4e0d-126">Dieser Controller unterstützt CRUD-Vorgänge auf `Product` Instanzen.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="a4e0d-127">Die `OrdersContext`-Klasse wird für die Kommunikation mit Entity Framework verwendet.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="a4e0d-128">Eine neue Daten bankverbindungs Zeichenfolge in der Datei "Web. config".</span><span class="sxs-lookup"><span data-stu-id="a4e0d-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="a4e0d-129">Öffnen Sie die Datei OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="a4e0d-130">Beachten Sie, dass der-Konstruktor den Namen der Daten bankverbindungs Zeichenfolge angibt.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="a4e0d-131">Dieser Name verweist auf die Verbindungs Zeichenfolge, die der Datei "Web. config" hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="a4e0d-132">Fügen Sie der Klasse `OrdersContext` die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="a4e0d-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="a4e0d-133">Ein **dbset** stellt einen Satz von Entitäten dar, die abgefragt werden können.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="a4e0d-134">Im folgenden finden Sie die komplette Auflistung für die `OrdersContext`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="a4e0d-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="a4e0d-135">Die `AdminController`-Klasse definiert fünf Methoden, die grundlegende CRUD-Funktionen implementieren.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="a4e0d-136">Jede Methode entspricht einem URI, den der Client aufrufen kann:</span><span class="sxs-lookup"><span data-stu-id="a4e0d-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="a4e0d-137">Controller-Methode</span><span class="sxs-lookup"><span data-stu-id="a4e0d-137">Controller Method</span></span> | <span data-ttu-id="a4e0d-138">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="a4e0d-138">Description</span></span> | <span data-ttu-id="a4e0d-139">URI</span><span class="sxs-lookup"><span data-stu-id="a4e0d-139">URI</span></span> | <span data-ttu-id="a4e0d-140">HTTP-Methode</span><span class="sxs-lookup"><span data-stu-id="a4e0d-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a4e0d-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="a4e0d-141">GetProducts</span></span> | <span data-ttu-id="a4e0d-142">Ruft alle Produkte ab.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-142">Gets all products.</span></span> | <span data-ttu-id="a4e0d-143">API/Produkte</span><span class="sxs-lookup"><span data-stu-id="a4e0d-143">api/products</span></span> | <span data-ttu-id="a4e0d-144">GET</span><span class="sxs-lookup"><span data-stu-id="a4e0d-144">GET</span></span> |
| <span data-ttu-id="a4e0d-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="a4e0d-145">GetProduct</span></span> | <span data-ttu-id="a4e0d-146">Sucht nach einem Produkt anhand der ID.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-146">Finds a product by ID.</span></span> | <span data-ttu-id="a4e0d-147">API/Produkte/*ID*</span><span class="sxs-lookup"><span data-stu-id="a4e0d-147">api/products/*id*</span></span> | <span data-ttu-id="a4e0d-148">GET</span><span class="sxs-lookup"><span data-stu-id="a4e0d-148">GET</span></span> |
| <span data-ttu-id="a4e0d-149">Putproduct</span><span class="sxs-lookup"><span data-stu-id="a4e0d-149">PutProduct</span></span> | <span data-ttu-id="a4e0d-150">Aktualisiert ein Produkt.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-150">Updates a product.</span></span> | <span data-ttu-id="a4e0d-151">API/Produkte/*ID*</span><span class="sxs-lookup"><span data-stu-id="a4e0d-151">api/products/*id*</span></span> | <span data-ttu-id="a4e0d-152">PUT</span><span class="sxs-lookup"><span data-stu-id="a4e0d-152">PUT</span></span> |
| <span data-ttu-id="a4e0d-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="a4e0d-153">PostProduct</span></span> | <span data-ttu-id="a4e0d-154">Erstellt ein neues Produkt.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-154">Creates a new product.</span></span> | <span data-ttu-id="a4e0d-155">API/Produkte</span><span class="sxs-lookup"><span data-stu-id="a4e0d-155">api/products</span></span> | <span data-ttu-id="a4e0d-156">POST</span><span class="sxs-lookup"><span data-stu-id="a4e0d-156">POST</span></span> |
| <span data-ttu-id="a4e0d-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="a4e0d-157">DeleteProduct</span></span> | <span data-ttu-id="a4e0d-158">Löscht ein Produkt.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-158">Deletes a product.</span></span> | <span data-ttu-id="a4e0d-159">API/Produkte/*ID*</span><span class="sxs-lookup"><span data-stu-id="a4e0d-159">api/products/*id*</span></span> | <span data-ttu-id="a4e0d-160">Delete</span><span class="sxs-lookup"><span data-stu-id="a4e0d-160">DELETE</span></span> |

<span data-ttu-id="a4e0d-161">Jede Methode ruft `OrdersContext` auf, um die Datenbank abzufragen.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="a4e0d-162">Die Methoden, die den Auflistungs-(Put-, Post-und DELETE-) Befehl ändern, `db.SaveChanges`, um die Änderungen in der Datenbank beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="a4e0d-163">Controller werden pro HTTP-Anforderung erstellt und dann verworfen, sodass Änderungen persistent gespeichert werden müssen, bevor eine Methode zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="a4e0d-164">Datenbankinitialisierer hinzufügen</span><span class="sxs-lookup"><span data-stu-id="a4e0d-164">Add a Database Initializer</span></span>

<span data-ttu-id="a4e0d-165">Entity Framework verfügt über ein nettes Feature, mit dem Sie die Datenbank beim Start auffüllen und die Datenbank automatisch neu erstellen können, wenn sich die Modelle ändern.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="a4e0d-166">Diese Funktion ist während der Entwicklung nützlich, da Sie immer einige Testdaten haben, auch wenn Sie die Modelle ändern.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="a4e0d-167">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Modelle, und erstellen Sie eine neue Klasse mit dem Namen `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="a4e0d-168">Verwenden Sie die folgenden Implementierung:</span><span class="sxs-lookup"><span data-stu-id="a4e0d-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="a4e0d-169">Durch die Vererbung von der **dropkreatedatabaseifmodelchanges** -Klasse wird Entity Framework, die Datenbank zu löschen, wenn die Modellklassen geändert werden.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="a4e0d-170">Wenn Entity Framework die Datenbank erstellt (oder neu erstellt), ruft Sie die **Seed** -Methode auf, um die Tabellen aufzufüllen.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="a4e0d-171">Wir verwenden die **Seed** -Methode, um einige Beispiel Produkte sowie eine Beispiel Reihenfolge hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="a4e0d-172">Diese Funktion eignet sich hervorragend für Tests, aber verwenden Sie nicht die **dropkreatedatabaseifmodelchanges** -Klasse in der Produktion,, weil Sie die Daten verlieren könnten, wenn jemand eine Modell Klasse ändert.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="a4e0d-173">Öffnen Sie als nächstes Global. asax, und fügen Sie den folgenden Code in die **Anwendungs\_Start** -Methode ein:</span><span class="sxs-lookup"><span data-stu-id="a4e0d-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="a4e0d-174">Senden einer Anforderung an den Controller</span><span class="sxs-lookup"><span data-stu-id="a4e0d-174">Send a Request to the Controller</span></span>

<span data-ttu-id="a4e0d-175">An diesem Punkt haben wir keinen Client Code geschrieben, aber Sie können die Web-API mithilfe eines Webbrowsers oder eines HTTP-Debugtools wie z. b. " [fddler](http://www.fiddler2.com/fiddler2/)" aufrufen.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="a4e0d-176">Drücken Sie in Visual Studio F5, um das Debuggen zu starten.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="a4e0d-177">Der Webbrowser wird geöffnet, um `http://localhost:*portnum*/`, wobei *portnum* eine Portnummer ist.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="a4e0d-178">Senden Sie eine HTTP-Anforderung an "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="a4e0d-179">Die erste Anforderung kann langsam sein, da Entity Framework die Datenbank erstellen und darauf basieren muss.</span><span class="sxs-lookup"><span data-stu-id="a4e0d-179">The first request may be slow to complete, because Entity Framework needs to create and seed the database.</span></span> <span data-ttu-id="a4e0d-180">Die Antwort sollte etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="a4e0d-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="a4e0d-181">[Zurück](using-web-api-with-entity-framework-part-2.md)
> [Weiter](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="a4e0d-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
