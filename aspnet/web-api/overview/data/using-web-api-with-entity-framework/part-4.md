---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Behandeln von Entitäts Beziehungen | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: be4948e5443a5eb4e1824c63dd0c445a7ee1928e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449121"
---
# <a name="handling-entity-relations"></a><span data-ttu-id="6fff1-102">Verarbeiten von Entitätsbeziehungen</span><span class="sxs-lookup"><span data-stu-id="6fff1-102">Handling Entity Relations</span></span>

<span data-ttu-id="6fff1-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6fff1-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="6fff1-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="6fff1-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="6fff1-105">In diesem Abschnitt werden einige Details erläutert, wie EF Verwandte Entitäten lädt und wie zirkuläre Navigations Eigenschaften in den Modellklassen behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="6fff1-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="6fff1-106">(Dieser Abschnitt enthält Hintergrundinformationen und ist nicht erforderlich, um das Tutorial abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="6fff1-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="6fff1-107">Wenn Sie möchten, fahren Sie mit [Teil 5 fort.](part-5.md))</span><span class="sxs-lookup"><span data-stu-id="6fff1-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="6fff1-108">Unverzügliches Laden und Lazy Load</span><span class="sxs-lookup"><span data-stu-id="6fff1-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="6fff1-109">Wenn EF mit einer relationalen Datenbank verwendet wird, ist es wichtig zu verstehen, wie EF verwandte Daten lädt.</span><span class="sxs-lookup"><span data-stu-id="6fff1-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="6fff1-110">Außerdem ist es hilfreich, die SQL-Abfragen anzuzeigen, die von EF generiert werden.</span><span class="sxs-lookup"><span data-stu-id="6fff1-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="6fff1-111">Fügen Sie dem `BookServiceContext`-Konstruktor die folgende Codezeile hinzu, um die SQL-Ablauf Verfolgung zu verfolgen:</span><span class="sxs-lookup"><span data-stu-id="6fff1-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="6fff1-112">Wenn Sie eine GET-Anforderung an/API/Books senden, wird JSON wie folgt zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="6fff1-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="6fff1-113">Sie sehen, dass die Author-Eigenschaft NULL ist, auch wenn das Buch eine gültige AutorID enthält.</span><span class="sxs-lookup"><span data-stu-id="6fff1-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="6fff1-114">Das liegt daran, dass EF die zugehörigen Autor Entitäten nicht lädt.</span><span class="sxs-lookup"><span data-stu-id="6fff1-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="6fff1-115">Das Ablauf Verfolgungs Protokoll der SQL-Abfrage bestätigt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="6fff1-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="6fff1-116">Die SELECT-Anweisung nimmt aus der Bücher Tabelle und verweist nicht auf die Autoren Tabelle.</span><span class="sxs-lookup"><span data-stu-id="6fff1-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="6fff1-117">Im folgenden finden Sie die-Methode in der `BooksController`-Klasse, die die Liste der Bücher zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="6fff1-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="6fff1-118">Sehen wir uns an, wie wir den Autor als Teil der JSON-Daten zurückgeben können.</span><span class="sxs-lookup"><span data-stu-id="6fff1-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="6fff1-119">Es gibt drei Möglichkeiten, verwandte Daten in Entity Framework zu laden: Eager Loading, Lazy Loading und explizites laden.</span><span class="sxs-lookup"><span data-stu-id="6fff1-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="6fff1-120">Es gibt Kompromisse bei den einzelnen Techniken, daher ist es wichtig zu verstehen, wie Sie funktionieren.</span><span class="sxs-lookup"><span data-stu-id="6fff1-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="6fff1-121">Eager Loading</span><span class="sxs-lookup"><span data-stu-id="6fff1-121">Eager Loading</span></span>

<span data-ttu-id="6fff1-122">Mit *Eager Loading*lädt EF Verwandte Entitäten als Teil der anfänglichen Datenbankabfrage.</span><span class="sxs-lookup"><span data-stu-id="6fff1-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="6fff1-123">Verwenden Sie die " **System. Data. Entity. include** "-Erweiterungsmethode, um Eager Loading auszuführen.</span><span class="sxs-lookup"><span data-stu-id="6fff1-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="6fff1-124">Dies weist EF an, die Autoren Daten in die Abfrage einzubeziehen.</span><span class="sxs-lookup"><span data-stu-id="6fff1-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="6fff1-125">Wenn Sie diese Änderung vornehmen und die app ausführen, sehen die JSON-Daten nun wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="6fff1-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="6fff1-126">Das Ablauf Verfolgungs Protokoll zeigt, dass EF einen Join mit den Tabellen "Book" und "Author" ausgeführt hat.</span><span class="sxs-lookup"><span data-stu-id="6fff1-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="6fff1-127">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="6fff1-127">Lazy Loading</span></span>

<span data-ttu-id="6fff1-128">Mit Lazy Loading lädt EF automatisch eine verknüpfte Entität, wenn die Navigations Eigenschaft für diese Entität dereferenziert wird.</span><span class="sxs-lookup"><span data-stu-id="6fff1-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="6fff1-129">Um Lazy Loading zu aktivieren, stellen Sie die Navigations Eigenschaft als virtuell dar.</span><span class="sxs-lookup"><span data-stu-id="6fff1-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="6fff1-130">Beispielsweise in der Book-Klasse:</span><span class="sxs-lookup"><span data-stu-id="6fff1-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="6fff1-131">Sehen Sie sich jetzt den folgenden Code an:</span><span class="sxs-lookup"><span data-stu-id="6fff1-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="6fff1-132">Wenn Lazy Loading aktiviert ist, wird der Zugriff auf die `Author`-Eigenschaft auf `books[0]` bewirkt, dass EF die Datenbank für den Autor abfragt.</span><span class="sxs-lookup"><span data-stu-id="6fff1-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="6fff1-133">Lazy Load erfordert mehrere Daten Bank Fahrten, da EF jedes Mal eine Abfrage sendet, wenn eine verknüpfte Entität abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="6fff1-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="6fff1-134">Im Allgemeinen möchten Sie für Objekte, die Sie serialisieren, Lazy Loading deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="6fff1-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="6fff1-135">Das Serialisierungsprogramm muss alle Eigenschaften des Modells lesen, das das Laden der verknüpften Entitäten auslöst.</span><span class="sxs-lookup"><span data-stu-id="6fff1-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="6fff1-136">Hier sind z. b. die SQL-Abfragen, bei denen EF die Liste der Bücher mit aktiviertem Lazy Loading serialisiert.</span><span class="sxs-lookup"><span data-stu-id="6fff1-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="6fff1-137">Sie können sehen, dass EF drei separate Abfragen für die drei Autoren durchführt.</span><span class="sxs-lookup"><span data-stu-id="6fff1-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="6fff1-138">Es gibt immer noch Zeiten, in denen Sie Lazy Loading verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="6fff1-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="6fff1-139">Das unverzügliches Laden kann bewirken, dass EF einen sehr komplexen Join generiert.</span><span class="sxs-lookup"><span data-stu-id="6fff1-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="6fff1-140">Möglicherweise benötigen Sie jedoch auch Verwandte Entitäten für eine kleine Teilmenge der Daten, und Lazy Loading wäre effizienter.</span><span class="sxs-lookup"><span data-stu-id="6fff1-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="6fff1-141">Eine Möglichkeit, um Serialisierungsprobleme zu vermeiden, ist das Serialisieren von DTOs (Data Transfer Objects) anstelle von Entitäts Objekten.</span><span class="sxs-lookup"><span data-stu-id="6fff1-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="6fff1-142">Diesen Ansatz zeige ich später in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="6fff1-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="6fff1-143">Explizites Laden</span><span class="sxs-lookup"><span data-stu-id="6fff1-143">Explicit Loading</span></span>

<span data-ttu-id="6fff1-144">Explizites Laden ähnelt Lazy Loading, mit dem Unterschied, dass Sie die zugehörigen Daten explizit im Code erhalten. Dies geschieht nicht automatisch, wenn Sie auf eine Navigations Eigenschaft zugreifen.</span><span class="sxs-lookup"><span data-stu-id="6fff1-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="6fff1-145">Explizites Laden ermöglicht Ihnen mehr Kontrolle darüber, wann verwandte Daten geladen werden müssen, erfordert jedoch zusätzlichen Code.</span><span class="sxs-lookup"><span data-stu-id="6fff1-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="6fff1-146">Weitere Informationen zum expliziten Laden finden Sie unter [Laden verwandter Entitäten](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="6fff1-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="6fff1-147">Navigations Eigenschaften und Zirkel Verweise</span><span class="sxs-lookup"><span data-stu-id="6fff1-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="6fff1-148">Beim Definieren der Buch-und Autoren Modelle habe ich eine Navigations Eigenschaft für die `Book`-Klasse für die Book-Author-Beziehung definiert, aber ich habe keine Navigations Eigenschaft in der anderen Richtung definiert.</span><span class="sxs-lookup"><span data-stu-id="6fff1-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="6fff1-149">Was geschieht, wenn Sie der `Author` Klasse die entsprechende Navigations Eigenschaft hinzufügen?</span><span class="sxs-lookup"><span data-stu-id="6fff1-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="6fff1-150">Dies führt leider zu einem Problem, wenn Sie die Modelle serialisieren.</span><span class="sxs-lookup"><span data-stu-id="6fff1-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="6fff1-151">Wenn Sie die zugehörigen Daten laden, wird ein Zirkel Objekt Diagramm erstellt.</span><span class="sxs-lookup"><span data-stu-id="6fff1-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="6fff1-152">Wenn der JSON-oder XML-Formatierer versucht, das Diagramm zu serialisieren, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="6fff1-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="6fff1-153">Die zwei Formatierer lösen verschiedene Ausnahme Meldungen aus.</span><span class="sxs-lookup"><span data-stu-id="6fff1-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="6fff1-154">Im folgenden finden Sie ein Beispiel für den JSON-Formatierer:</span><span class="sxs-lookup"><span data-stu-id="6fff1-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="6fff1-155">Hier ist das XML-Formatierer:</span><span class="sxs-lookup"><span data-stu-id="6fff1-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="6fff1-156">Eine Lösung besteht darin, DTOs zu verwenden, die im nächsten Abschnitt beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="6fff1-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="6fff1-157">Alternativ können Sie die JSON-und XML-Formatierer so konfigurieren, dass Sie Diagramm Zyklen verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="6fff1-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="6fff1-158">Weitere Informationen finden Sie unter [Handling Zirkel Objekt Verweise](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="6fff1-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="6fff1-159">Für dieses Tutorial benötigen Sie die `Author.Book`-Navigations Eigenschaft nicht, sodass Sie sie weglassen können.</span><span class="sxs-lookup"><span data-stu-id="6fff1-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6fff1-160">[Zurück](part-3.md)
> [Weiter](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="6fff1-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
