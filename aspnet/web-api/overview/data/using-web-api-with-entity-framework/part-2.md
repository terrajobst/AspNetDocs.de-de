---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Hinzufügen von Modellen und Controllern | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 57dacda421968f341284d89c9a3ad80040c16e25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449187"
---
# <a name="add-models-and-controllers"></a><span data-ttu-id="c31a9-102">Hinzufügen von Modellen und Controllern</span><span class="sxs-lookup"><span data-stu-id="c31a9-102">Add Models and Controllers</span></span>

<span data-ttu-id="c31a9-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c31a9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c31a9-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="c31a9-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="c31a9-105">In diesem Abschnitt fügen Sie Modellklassen hinzu, die die Daten Bank Entitäten definieren.</span><span class="sxs-lookup"><span data-stu-id="c31a9-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="c31a9-106">Anschließend werden Sie Web-API-Controller hinzufügen, die CRUD-Vorgänge für diese Entitäten ausführen.</span><span class="sxs-lookup"><span data-stu-id="c31a9-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="c31a9-107">Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="c31a9-107">Add Model Classes</span></span>

<span data-ttu-id="c31a9-108">In diesem Tutorial erstellen wir die Datenbank mithilfe des "Code First"-Ansatzes für Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="c31a9-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="c31a9-109">Mit Code First schreiben C# Sie Klassen, die Datenbanktabellen entsprechen, und EF erstellt die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c31a9-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="c31a9-110">(Weitere Informationen finden Sie unter [Entity Framework-Entwicklungsansätzen](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span><span class="sxs-lookup"><span data-stu-id="c31a9-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="c31a9-111">Zunächst definieren wir unsere Domänen Objekte als POCOS (Plain-Old CLR Objects).</span><span class="sxs-lookup"><span data-stu-id="c31a9-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="c31a9-112">Wir erstellen die folgenden POCOS:</span><span class="sxs-lookup"><span data-stu-id="c31a9-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="c31a9-113">Autor</span><span class="sxs-lookup"><span data-stu-id="c31a9-113">Author</span></span>
- <span data-ttu-id="c31a9-114">Buch</span><span class="sxs-lookup"><span data-stu-id="c31a9-114">Book</span></span>

<span data-ttu-id="c31a9-115">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Modelle.</span><span class="sxs-lookup"><span data-stu-id="c31a9-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="c31a9-116">Wählen Sie **Hinzufügen**und dann **Klasse**aus.</span><span class="sxs-lookup"><span data-stu-id="c31a9-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="c31a9-117">Geben Sie der Klassen den Namen `Author`.</span><span class="sxs-lookup"><span data-stu-id="c31a9-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="c31a9-118">Ersetzen Sie den gesamten Code in Author.cs durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="c31a9-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="c31a9-119">Fügen Sie eine weitere Klasse mit dem Namen `Book`mit dem folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="c31a9-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="c31a9-120">Entity Framework werden diese Modelle zum Erstellen von Datenbanktabellen verwenden.</span><span class="sxs-lookup"><span data-stu-id="c31a9-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="c31a9-121">Für jedes Modell wird die `Id`-Eigenschaft zur Primärschlüssel Spalte der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="c31a9-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="c31a9-122">In der Book-Klasse definiert der `AuthorId` einen Fremdschlüssel in der `Author` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="c31a9-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="c31a9-123">(Aus Gründen der Einfachheit gehe ich davon aus, dass jedes Buch über einen einzigen Autor verfügt.) Die Book-Klasse enthält auch eine Navigations Eigenschaft für die zugehörigen `Author`.</span><span class="sxs-lookup"><span data-stu-id="c31a9-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="c31a9-124">Mit der-Navigations Eigenschaft können Sie auf die zugehörigen `Author` im Code zugreifen.</span><span class="sxs-lookup"><span data-stu-id="c31a9-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="c31a9-125">Weitere Informationen zu Navigations Eigenschaften finden Sie in Teil 4, [Behandeln von Entitäts Beziehungen](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="c31a9-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="c31a9-126">Web-API-Controller hinzufügen</span><span class="sxs-lookup"><span data-stu-id="c31a9-126">Add Web API Controllers</span></span>

<span data-ttu-id="c31a9-127">In diesem Abschnitt fügen wir Web-API-Controller hinzu, die CRUD-Vorgänge unterstützen (erstellen, lesen, aktualisieren und löschen).</span><span class="sxs-lookup"><span data-stu-id="c31a9-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="c31a9-128">Die Controller verwenden Entity Framework, um mit der Datenbankebene zu kommunizieren.</span><span class="sxs-lookup"><span data-stu-id="c31a9-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="c31a9-129">Zuerst können Sie die Datei Controller/valuescontroller. cs löschen.</span><span class="sxs-lookup"><span data-stu-id="c31a9-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="c31a9-130">Diese Datei enthält einen Beispiel-Web-API-Controller, den Sie für dieses Tutorial jedoch nicht benötigen.</span><span class="sxs-lookup"><span data-stu-id="c31a9-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="c31a9-131">Erstellen Sie als nächstes das Projekt.</span><span class="sxs-lookup"><span data-stu-id="c31a9-131">Next, build the project.</span></span> <span data-ttu-id="c31a9-132">Das Web-API-Gerüst verwendet Reflektion, um nach den Modellklassen zu suchen. Daher muss die kompilierte Assembly verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c31a9-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="c31a9-133">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controller.</span><span class="sxs-lookup"><span data-stu-id="c31a9-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="c31a9-134">Wählen Sie **Hinzufügen**und dann **Controller**aus.</span><span class="sxs-lookup"><span data-stu-id="c31a9-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="c31a9-135">Wählen Sie im Dialogfeld **Gerüst hinzufügen** die Option "Web-API 2-Controller mit Aktionen unter Verwendung Entity Framework" aus.</span><span class="sxs-lookup"><span data-stu-id="c31a9-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="c31a9-136">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="c31a9-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="c31a9-137">Gehen Sie im Dialogfeld **Controller hinzufügen** wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="c31a9-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="c31a9-138">Wählen Sie in der Dropdown Liste **Modell Klasse** die `Author` Klasse aus.</span><span class="sxs-lookup"><span data-stu-id="c31a9-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="c31a9-139">(Wenn Sie in der Dropdown Liste nicht angezeigt wird, stellen Sie sicher, dass Sie das Projekt erstellt haben.)</span><span class="sxs-lookup"><span data-stu-id="c31a9-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="c31a9-140">Aktivieren Sie die Option "Async-Controller Aktionen verwenden".</span><span class="sxs-lookup"><span data-stu-id="c31a9-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="c31a9-141">Belassen Sie den Controller Namen als &quot;authorscontroller&quot;.</span><span class="sxs-lookup"><span data-stu-id="c31a9-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="c31a9-142">Klicken Sie neben **Datenkontext Klasse**auf die Schaltfläche Plus (+).</span><span class="sxs-lookup"><span data-stu-id="c31a9-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="c31a9-143">Überlassen Sie im Dialogfeld **neuer Datenkontext** den Standardnamen, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="c31a9-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="c31a9-144">Klicken Sie zum Vervollständigen des Dialog Felds **Controller hinzu** fügen auf **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="c31a9-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="c31a9-145">Im Dialogfeld werden zwei Klassen zu Ihrem Projekt hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="c31a9-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="c31a9-146">`AuthorsController` definiert einen Web-API-Controller.</span><span class="sxs-lookup"><span data-stu-id="c31a9-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="c31a9-147">Der Controller implementiert die Rest-API, die von Clients zum Durchführen von CRUD-Vorgängen für die Liste der Autoren verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c31a9-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="c31a9-148">`BookServiceContext` verwaltet Entitäts Objekte zur Laufzeit. dazu gehören das Auffüllen von Objekten mit Daten aus einer Datenbank, die Änderungs Nachverfolgung und das Beibehalten von Daten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c31a9-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="c31a9-149">Er erbt von `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c31a9-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="c31a9-150">Erstellen Sie an diesem Punkt das Projekt erneut.</span><span class="sxs-lookup"><span data-stu-id="c31a9-150">At this point, build the project again.</span></span> <span data-ttu-id="c31a9-151">Durchlaufen Sie nun die gleichen Schritte, um einen API-Controller für `Book` Entitäten hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="c31a9-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="c31a9-152">Wählen Sie diesmal `Book` für die Modell Klasse aus, und wählen Sie die vorhandene `BookServiceContext` Klasse für die Datenkontext Klasse aus.</span><span class="sxs-lookup"><span data-stu-id="c31a9-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="c31a9-153">(Erstellen Sie keinen neuen Datenkontext.) Klicken Sie auf **Hinzufügen** , um den Controller hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="c31a9-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> <span data-ttu-id="c31a9-154">[Zurück](part-1.md)
> [Weiter](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="c31a9-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
