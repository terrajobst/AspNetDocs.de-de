---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Teil 3: Erstellen eines Administratorcontrollers | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 0fc533fb3673639769ecdfa8b3d02ff40133cb27
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2019
ms.locfileid: "58421622"
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="bc2d8-102">Teil 3: Erstellen eines Administratorcontrollers</span><span class="sxs-lookup"><span data-stu-id="bc2d8-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="bc2d8-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bc2d8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="bc2d8-104">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="bc2d8-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="bc2d8-105">Hinzufügen eines Administratorcontrollers</span><span class="sxs-lookup"><span data-stu-id="bc2d8-105">Add an Admin Controller</span></span>

<span data-ttu-id="bc2d8-106">In diesem Abschnitt fügen wir einen Web-API-Controller, der unterstützt CRUD (erstellen, lesen, aktualisieren und löschen) Vorgänge für Produkte.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="bc2d8-107">Der Controller wird Entity Framework verwenden, um mit der Datenbankebene kommunizieren zu können.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="bc2d8-108">Nur Administratoren werden dieser Controller verwenden können.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="bc2d8-109">Kunden werden die Produkte über einen anderen Controller zugreifen.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="bc2d8-110">Klicken Sie im Projektmappen-Explorer den Ordner "Controllers".</span><span class="sxs-lookup"><span data-stu-id="bc2d8-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="bc2d8-111">Wählen Sie **hinzufügen** und dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="bc2d8-112">In der **Controller hinzufügen** Dialogfeld benennen Sie den Controller `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="bc2d8-113">Klicken Sie unter **Vorlage**Option &quot;API-Controller mit Lese-/schreibaktionen, mithilfe von Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="bc2d8-114">Klicken Sie unter **Modellklasse**, wählen Sie "Product (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="bc2d8-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="bc2d8-115">Klicken Sie unter **Datenkontext**wählen "&lt;neuer Datenkontext&gt;".</span><span class="sxs-lookup"><span data-stu-id="bc2d8-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="bc2d8-116">Wenn die **Modellklasse** Dropdownliste zeigt keine Modellklassen und stellen Sie sicher, dass Sie das Projekt kompiliert.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="bc2d8-117">Entitätsframework verwendet Reflektion, daher muss die kompilierte Assembly festgelegt.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="bc2d8-118">Auswählen von "&lt;neuer Datenkontext&gt;" wird geöffnet, die **neuer Datenkontext** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="bc2d8-119">Benennen Sie den Datenkontext `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="bc2d8-120">Klicken Sie auf **OK** zum Verwerfen der **neuer Datenkontext** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="bc2d8-121">In der **Controller hinzufügen** Dialogfeld klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="bc2d8-122">Hier ist, was dem Projekt hinzugefügt haben:</span><span class="sxs-lookup"><span data-stu-id="bc2d8-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="bc2d8-123">Eine Klasse namens `OrdersContext` abgeleitet, die **"DbContext"**.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="bc2d8-124">Diese Klasse stellt die Verbindung zwischen der POCO-Modelle und die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="bc2d8-125">Eine Web-API-Controller mit dem Namen `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="bc2d8-126">Dieser Controller unterstützt CRUD-Vorgänge auf `Product` Instanzen.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="bc2d8-127">Er verwendet den `OrdersContext` Klasse für die Kommunikation mit Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="bc2d8-128">Eine neue Datenbank-Verbindungszeichenfolge in der Datei "Web.config".</span><span class="sxs-lookup"><span data-stu-id="bc2d8-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="bc2d8-129">Öffnen Sie die OrdersContext.cs-Datei.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="bc2d8-130">Beachten Sie, dass der Konstruktor den Namen der Verbindungszeichenfolge der Datenbank angibt.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="bc2d8-131">Dieser Name bezieht sich auf die Verbindungszeichenfolge, die Datei "Web.config" hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="bc2d8-132">Fügen Sie der Klasse `OrdersContext` die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="bc2d8-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="bc2d8-133">Ein **"DbSet"** stellt einen Satz von Entitäten, die abgefragt werden können.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="bc2d8-134">Hier ist die vollständige Auflistung für die `OrdersContext` Klasse:</span><span class="sxs-lookup"><span data-stu-id="bc2d8-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="bc2d8-135">Die `AdminController` -Klasse definiert fünf Methoden, die grundlegende CRUD-Funktionen zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="bc2d8-136">Jede Methode entspricht ein URI, der der Client aufrufen kann:</span><span class="sxs-lookup"><span data-stu-id="bc2d8-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="bc2d8-137">Controller-Methode</span><span class="sxs-lookup"><span data-stu-id="bc2d8-137">Controller Method</span></span> | <span data-ttu-id="bc2d8-138">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="bc2d8-138">Description</span></span> | <span data-ttu-id="bc2d8-139">URI</span><span class="sxs-lookup"><span data-stu-id="bc2d8-139">URI</span></span> | <span data-ttu-id="bc2d8-140">HTTP-Methode</span><span class="sxs-lookup"><span data-stu-id="bc2d8-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="bc2d8-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="bc2d8-141">GetProducts</span></span> | <span data-ttu-id="bc2d8-142">Ruft alle Produkte ab.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-142">Gets all products.</span></span> | <span data-ttu-id="bc2d8-143">API/Produkte</span><span class="sxs-lookup"><span data-stu-id="bc2d8-143">api/products</span></span> | <span data-ttu-id="bc2d8-144">GET</span><span class="sxs-lookup"><span data-stu-id="bc2d8-144">GET</span></span> |
| <span data-ttu-id="bc2d8-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="bc2d8-145">GetProduct</span></span> | <span data-ttu-id="bc2d8-146">Sucht nach einem Produkt nach ID auf.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-146">Finds a product by ID.</span></span> | <span data-ttu-id="bc2d8-147">API/Produkte/*Id*</span><span class="sxs-lookup"><span data-stu-id="bc2d8-147">api/products/*id*</span></span> | <span data-ttu-id="bc2d8-148">GET</span><span class="sxs-lookup"><span data-stu-id="bc2d8-148">GET</span></span> |
| <span data-ttu-id="bc2d8-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="bc2d8-149">PutProduct</span></span> | <span data-ttu-id="bc2d8-150">Ein Produkt wird aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-150">Updates a product.</span></span> | <span data-ttu-id="bc2d8-151">API/Produkte/*Id*</span><span class="sxs-lookup"><span data-stu-id="bc2d8-151">api/products/*id*</span></span> | <span data-ttu-id="bc2d8-152">PUT</span><span class="sxs-lookup"><span data-stu-id="bc2d8-152">PUT</span></span> |
| <span data-ttu-id="bc2d8-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="bc2d8-153">PostProduct</span></span> | <span data-ttu-id="bc2d8-154">Erstellt ein neues Produkt an.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-154">Creates a new product.</span></span> | <span data-ttu-id="bc2d8-155">API/Produkte</span><span class="sxs-lookup"><span data-stu-id="bc2d8-155">api/products</span></span> | <span data-ttu-id="bc2d8-156">POST</span><span class="sxs-lookup"><span data-stu-id="bc2d8-156">POST</span></span> |
| <span data-ttu-id="bc2d8-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="bc2d8-157">DeleteProduct</span></span> | <span data-ttu-id="bc2d8-158">Löscht ein Produkt.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-158">Deletes a product.</span></span> | <span data-ttu-id="bc2d8-159">API/Produkte/*Id*</span><span class="sxs-lookup"><span data-stu-id="bc2d8-159">api/products/*id*</span></span> | <span data-ttu-id="bc2d8-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="bc2d8-160">DELETE</span></span> |

<span data-ttu-id="bc2d8-161">Jede Methode ruft `OrdersContext` zum Abfragen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="bc2d8-162">Rufen Sie die Methoden, die die Auflistung aus (PUT, POST und DELETE) geändert werden `db.SaveChanges` , die Änderungen in der Datenbank beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="bc2d8-163">Controller pro HTTP-Anforderung erstellt und dann verworfen, daher ist es erforderlich, um Änderungen permanent zu speichern, bevor eine Methode einen Wert zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="bc2d8-164">Fügen Sie einen Datenbankinitialisierer hinzu.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-164">Add a Database Initializer</span></span>

<span data-ttu-id="bc2d8-165">Entitätsframework hat ein nützliches Feature, mit dem Sie das Auffüllen der Datenbank beim Start und die Datenbank automatisch neu, wenn die Modelle ändern.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="bc2d8-166">Dieses Feature ist während der Entwicklung nützlich, da Sie einige Testdaten immer haben, auch wenn Sie die Modelle ändern.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="bc2d8-167">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in den Ordner "Models", und erstellen Sie eine neue Klasse namens `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="bc2d8-168">Fügen Sie in der folgenden Implementierung:</span><span class="sxs-lookup"><span data-stu-id="bc2d8-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="bc2d8-169">Durch Erben von der **DropCreateDatabaseIfModelChanges** -Klasse, weisen wir Entity Framework für die Datenbank zu löschen, wenn wir die Modellklassen ändern.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="bc2d8-170">Wenn Entity Framework erstellt (oder neu erstellt) die Datenbank, ruft er die **Ausgangswert** Methode, um Tabellen aufzufüllen.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="bc2d8-171">Wir verwenden die **Ausgangswert** Methode, um einige Beispiel-Produkte sowie eine Beispiel-Reihenfolge hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="bc2d8-172">Diese Funktion ist gut für Tests geeignet, aber verwenden Sie nicht die **DropCreateDatabaseIfModelChanges** Klasse in der Produktion, da Sie die Daten verlieren können, wenn jemand eine Modellklasse ändert.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="bc2d8-173">Öffnen Sie "Global.asax", und fügen Sie den folgenden Code der **Anwendung\_starten** Methode:</span><span class="sxs-lookup"><span data-stu-id="bc2d8-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="bc2d8-174">Senden Sie eine Anforderung an den Controller</span><span class="sxs-lookup"><span data-stu-id="bc2d8-174">Send a Request to the Controller</span></span>

<span data-ttu-id="bc2d8-175">An diesem Punkt nicht getan haben wir keinen Clientcode geschrieben, aber Sie können die Web-API mit einem Webbrowser oder eine HTTP-debugging-wie Tool aufrufen [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="bc2d8-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="bc2d8-176">Drücken Sie in Visual Studio F5, um mit dem Debuggen beginnen.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="bc2d8-177">Ihr Webbrowser wird geöffnet, um `http://localhost:*portnum*/`, wobei *Portnum* ist eine Portnummer.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="bc2d8-178">Senden von HTTP-Anforderung an "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="bc2d8-179">Die erste Anforderung nur langsam abgeschlossen ist, möglicherweise benötigt Entity Framework zum Erstellen und das Seeding für die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="bc2d8-179">The first request may be slow to complete, because Entity Framework needs to create and seed the database.</span></span> <span data-ttu-id="bc2d8-180">Die Antwort sollte etwa wie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="bc2d8-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="bc2d8-181">[Zurück](using-web-api-with-entity-framework-part-2.md)
> [Weiter](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="bc2d8-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
