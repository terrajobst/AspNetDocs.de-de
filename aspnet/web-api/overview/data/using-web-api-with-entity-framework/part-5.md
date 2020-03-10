---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Create Datenübertragung Objects (DTOs) | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: fc0463420207eba764014b8ec7123c5150e38247
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449025"
---
# <a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="ea3d9-102">Erstellen von Datentransferobjekten (DTOs)</span><span class="sxs-lookup"><span data-stu-id="ea3d9-102">Create Data Transfer Objects (DTOs)</span></span>

<span data-ttu-id="ea3d9-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ea3d9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ea3d9-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="ea3d9-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="ea3d9-105">Derzeit macht unsere Web-API die Daten Bank Entitäten für den Client verfügbar.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="ea3d9-106">Der Client empfängt Daten, die direkt den Datenbanktabellen zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="ea3d9-107">Das ist jedoch nicht immer eine gute Idee.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-107">However, that's not always a good idea.</span></span> <span data-ttu-id="ea3d9-108">Manchmal möchten Sie die Form der Daten ändern, die Sie an den Client senden.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="ea3d9-109">Auf diese Weise können Sie beispielsweise folgende Vorgänge durchführen:</span><span class="sxs-lookup"><span data-stu-id="ea3d9-109">For example, you might want to:</span></span>

- <span data-ttu-id="ea3d9-110">Entfernen Sie zirkuläre Verweise (siehe vorheriger Abschnitt).</span><span class="sxs-lookup"><span data-stu-id="ea3d9-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="ea3d9-111">Blenden Sie bestimmte Eigenschaften aus, die Clients nicht anzeigen sollen.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="ea3d9-112">Lassen Sie einige Eigenschaften Weg, um die Nutzlastgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="ea3d9-113">Vereinfachen Sie Objekt Diagramme, die die verglichenen Objekte enthalten, damit Sie für Clients bequemer werden.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="ea3d9-114">Vermeiden Sie "overposting"-Sicherheitsrisiken.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="ea3d9-115">(Eine Erörterung der Übertragung finden Sie unter [Modell Validierung](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) .)</span><span class="sxs-lookup"><span data-stu-id="ea3d9-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="ea3d9-116">Entkoppeln Sie die Dienst Ebene von der Datenbankebene.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="ea3d9-117">Hierzu können Sie ein *Datenübertragungs Objekt (Data Transfer Object* , dto) definieren.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="ea3d9-118">Ein DTO ist ein Objekt, das definiert, wie die Daten über das Netzwerk gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="ea3d9-119">Sehen wir uns an, wie das mit der Buch-Entität funktioniert.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="ea3d9-120">Fügen Sie im Ordner Models zwei DTO-Klassen hinzu:</span><span class="sxs-lookup"><span data-stu-id="ea3d9-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="ea3d9-121">Die `BookDetailDto`-Klasse enthält alle Eigenschaften aus dem Buch Modell, mit dem Unterschied, dass `AuthorName` eine Zeichenfolge ist, die den Namen des Autors enthält.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-121">The `BookDetailDto` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="ea3d9-122">Die `BookDto`-Klasse enthält eine Teilmenge der Eigenschaften von `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-122">The `BookDto` class contains a subset of properties from `BookDetailDto`.</span></span>

<span data-ttu-id="ea3d9-123">Ersetzen Sie als nächstes die beiden Get-Methoden in der `BooksController`-Klasse mit den Versionen, die DTOs zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="ea3d9-124">Wir verwenden die LINQ **Select** -Anweisung, um von Book-Entitäten in DTOs zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="ea3d9-125">Hier ist das SQL, das von der neuen `GetBooks`-Methode generiert wird.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="ea3d9-126">Sie können sehen, dass EF die LINQ **Select** -Anweisung in eine SQL SELECT-Anweisung übersetzt.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="ea3d9-127">Ändern Sie abschließend die `PostBook`-Methode, um einen DTO-Wert zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="ea3d9-128">In diesem Tutorial werden Sie manuell in DTOs in den Code umwandelt.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="ea3d9-129">Eine andere Möglichkeit besteht darin, eine Bibliothek wie [AutoMapper](http://automapper.org/) zu verwenden, die die Konvertierung automatisch verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="ea3d9-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="ea3d9-130">[Zurück](part-4.md)
> [Weiter](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="ea3d9-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
