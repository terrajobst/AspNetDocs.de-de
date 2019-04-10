---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Sicherheitsempfehlungen für ASP.NET Web-API 2-OData - ASP.NET 4.x
author: MikeWasson
description: Beschreibt Sicherheitsaspekte zu berücksichtigen, wenn Sie ein Dataset über OData für ASP.NET Web API 2 auf ASP.NET verfügbar machen 4.x.
ms.author: riande
ms.date: 02/06/2013
ms.custom: seoapril2019
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 8194a368cb0629c30e32ec05bf4bed150d442ad8
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59393508"
---
# <a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="775d6-103">Sicherheitsempfehlungen für ASP.NET Web-API 2 OData</span><span class="sxs-lookup"><span data-stu-id="775d6-103">Security Guidance for ASP.NET Web API 2 OData</span></span>

<span data-ttu-id="775d6-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="775d6-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="775d6-105">In diesem Thema wird beschrieben, einige der Sicherheitsprobleme, die Sie berücksichtigen sollten, wenn Sie ein Dataset über OData für ASP.NET Web API 2 auf ASP.NET verfügbar machen 4.x.</span><span class="sxs-lookup"><span data-stu-id="775d6-105">This topic describes some of the security issues that you should consider when exposing a dataset through OData for ASP.NET Web API 2 on ASP.NET 4.x.</span></span>

## <a name="edm-security"></a><span data-ttu-id="775d6-106">EDM-Sicherheit</span><span class="sxs-lookup"><span data-stu-id="775d6-106">EDM Security</span></span>

<span data-ttu-id="775d6-107">Die Semantik der Abfrage basiert auf Entity Data Model (EDM) gibt nicht die zugrunde liegenden Modelltypen.</span><span class="sxs-lookup"><span data-stu-id="775d6-107">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="775d6-108">Sie können eine Eigenschaft ausschließen, aus dem EDM und wird nicht für die Abfrage sichtbar sein.</span><span class="sxs-lookup"><span data-stu-id="775d6-108">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="775d6-109">Nehmen wir beispielsweise an, dass Ihr Modell einen Employee-Typ mit einem Gehalt-Eigenschaft enthält.</span><span class="sxs-lookup"><span data-stu-id="775d6-109">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="775d6-110">Sie sollten diese Eigenschaft des EDM, um es von Clients ausblenden ausgeschlossen werden.</span><span class="sxs-lookup"><span data-stu-id="775d6-110">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="775d6-111">Es gibt zwei Möglichkeiten, eine Eigenschaft vom EDM ausschließen.</span><span class="sxs-lookup"><span data-stu-id="775d6-111">There are two ways to exclude a property from the EDM.</span></span> <span data-ttu-id="775d6-112">Sie können festlegen, die **[IgnoreDataMember]** Attribut für die Eigenschaft in der Modellklasse:</span><span class="sxs-lookup"><span data-stu-id="775d6-112">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="775d6-113">Sie können auch programmgesteuert die-Eigenschaft von EDM entfernen:</span><span class="sxs-lookup"><span data-stu-id="775d6-113">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="775d6-114">Abfragesicherheit</span><span class="sxs-lookup"><span data-stu-id="775d6-114">Query Security</span></span>

<span data-ttu-id="775d6-115">Ein böswilliger oder naive-Client möglicherweise zum Erstellen einer Abfrage, die zum Ausführen sehr lange dauert.</span><span class="sxs-lookup"><span data-stu-id="775d6-115">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="775d6-116">Im schlimmsten Fall kann dies den Zugriff auf den Dienst unterbrechen.</span><span class="sxs-lookup"><span data-stu-id="775d6-116">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="775d6-117">Die **[Queryable]** -Attribut ist ein Aktionsfilter, der analysiert, überprüft und wendet die Abfrage.</span><span class="sxs-lookup"><span data-stu-id="775d6-117">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="775d6-118">Der Filter konvertiert die Abfrageoptionen in einem LINQ-Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="775d6-118">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="775d6-119">Wenn der OData-Controller gibt eine **"IQueryable"** Typ, der **"IQueryable"** LINQ-Anbieter konvertiert den LINQ-Ausdruck in einer Abfrage.</span><span class="sxs-lookup"><span data-stu-id="775d6-119">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="775d6-120">Aus diesem Grund hängt die Leistung auf der LINQ-Anbieter, der verwendet wird, sowie auf den bestimmten Merkmalen Ihres Schemas Dataset oder eine Datenbank.</span><span class="sxs-lookup"><span data-stu-id="775d6-120">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="775d6-121">Weitere Informationen zur Verwendung von OData-Abfrageoptionen in ASP.NET Web-API finden Sie unter [unterstützt OData-Abfrageoptionen](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="775d6-121">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="775d6-122">Wenn Sie wissen, dass alle Clients (z. B. in einer unternehmensumgebung) als vertrauenswürdig eingestuft werden, oder wenn Ihr Dataset klein ist, kann die abfrageleistung kein Problem dar.</span><span class="sxs-lookup"><span data-stu-id="775d6-122">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="775d6-123">Andernfalls sollten Sie die folgenden Empfehlungen.</span><span class="sxs-lookup"><span data-stu-id="775d6-123">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="775d6-124">Testen Sie des Diensts mit verschiedenen Abfragen aus, und Stufen Sie die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="775d6-124">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="775d6-125">Aktivieren Sie servergesteuertes Paging, um zu vermeiden, ein großes Dataset in einer Abfrage zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="775d6-125">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="775d6-126">Weitere Informationen finden Sie unter [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="775d6-126">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="775d6-127">Erforderlich $filter und $orderby?</span><span class="sxs-lookup"><span data-stu-id="775d6-127">Do you need $filter and $orderby?</span></span> <span data-ttu-id="775d6-128">Bei einigen Anwendungen können paging, mithilfe von $top und $skip zulassen, aber der anderen Abfrageoptionen deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="775d6-128">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="775d6-129">Beachten Sie, $orderby auf Eigenschaften in einem gruppierten Index zu beschränken.</span><span class="sxs-lookup"><span data-stu-id="775d6-129">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="775d6-130">Sortieren von großen Daten ohne gruppierten Index ist langsam.</span><span class="sxs-lookup"><span data-stu-id="775d6-130">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="775d6-131">Maximale Knotenanzahl: Die **MaxNodeCount** Eigenschaft **[Queryable]** legt die maximale Anzahl Knoten, die in der Syntaxstruktur $filter erlaubt.</span><span class="sxs-lookup"><span data-stu-id="775d6-131">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="775d6-132">Der Standardwert ist 100, aber Sie möchten ein niedrigerer Wert, da eine große Anzahl von Knoten zum Kompilieren langsam sein kann.</span><span class="sxs-lookup"><span data-stu-id="775d6-132">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="775d6-133">Dies gilt insbesondere bei Verwendung von LINQ to Objects (z. B. LINQ-Abfragen in einer Auflistung im Arbeitsspeicher, ohne einen zwischengeschalteten LINQ-Anbieter).</span><span class="sxs-lookup"><span data-stu-id="775d6-133">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="775d6-134">Deaktivieren Sie ggf. die Funktionen any() und all(), da diese langsam sein können.</span><span class="sxs-lookup"><span data-stu-id="775d6-134">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="775d6-135">Wenn alle Zeichenfolgeneigenschaften große Zeichenfolgen enthalten&#8212;z. B. eine produktbeschreibung oder einem Blogeintrag&#8212;sollten Sie die Zeichenfolgenfunktionen deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="775d6-135">If any string properties contain large strings&#8212;for example, a product description or a blog entry&#8212;consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="775d6-136">Beachten Sie, untersagen von Filtern auf Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="775d6-136">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="775d6-137">Filterung für Navigationseigenschaften kann in einem Join führen, die je nach Schema Ihrer Datenbank langsam sein kann.</span><span class="sxs-lookup"><span data-stu-id="775d6-137">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="775d6-138">Der folgende Code zeigt ein Abfrage-Validierungssteuerelement, das verhindert, dass für Navigationseigenschaften des filtern.</span><span class="sxs-lookup"><span data-stu-id="775d6-138">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="775d6-139">Weitere Informationen zu Abfrage-Validierungssteuerelemente, finden Sie unter [Abfragevalidierung](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="775d6-139">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="775d6-140">Beachten Sie, Einschränken von $filter Abfragen durch Schreiben ein Validierungssteuerelement, das für Ihre Datenbank angepasst wird.</span><span class="sxs-lookup"><span data-stu-id="775d6-140">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="775d6-141">Betrachten Sie beispielsweise diese beiden Abfragen aus:</span><span class="sxs-lookup"><span data-stu-id="775d6-141">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="775d6-142">Alle Filme mit Actors, deren Nachname mit "A" beginnt.</span><span class="sxs-lookup"><span data-stu-id="775d6-142">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="775d6-143">Alle Filme im 1994 veröffentlicht wurde.</span><span class="sxs-lookup"><span data-stu-id="775d6-143">All movies released in 1994.</span></span>

    <span data-ttu-id="775d6-144">Es sei denn, Filme Akteure indiziert sind, müssen möglicherweise die erste Abfrage der Datenbank-Engine, um die gesamte Liste von Filmen zu scannen.</span><span class="sxs-lookup"><span data-stu-id="775d6-144">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="775d6-145">Während die zweite Abfrage zulässig ist, werden die Annahme, dass Filme von Veröffentlichungsjahr indiziert.</span><span class="sxs-lookup"><span data-stu-id="775d6-145">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="775d6-146">Der folgende Code zeigt ein Validierungssteuerelement, das ermöglicht das Filtern auf die Eigenschaften "ReleaseYear" und "Title", aber keine anderen Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="775d6-146">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="775d6-147">Im Allgemeinen sollten Sie die $filter-Funktionen, die Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="775d6-147">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="775d6-148">Wenn Ihre Clients die vollständige ausdrucksfähigkeit der $filter nicht benötigen, können Sie die zulässigen Funktionen begrenzen.</span><span class="sxs-lookup"><span data-stu-id="775d6-148">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
