---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Sicherheits Leit Faden für ASP.net-Web-API 2 odata-ASP.NET 4. x
author: MikeWasson
description: Beschreibt Sicherheitsaspekte, die bei der Bereitstellung eines Datasets über odata für ASP.net-Web-API 2 auf ASP.NET 4. x zu beachten sind.
ms.author: riande
ms.date: 02/06/2013
ms.custom: seoapril2019
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 8194a368cb0629c30e32ec05bf4bed150d442ad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448293"
---
# <a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="83572-103">Sicherheits Leit Faden für ASP.net-Web-API 2 odata</span><span class="sxs-lookup"><span data-stu-id="83572-103">Security Guidance for ASP.NET Web API 2 OData</span></span>

<span data-ttu-id="83572-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="83572-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="83572-105">In diesem Thema werden einige Sicherheitsaspekte beschrieben, die Sie beim verfügbar machen eines Datasets über odata für ASP.net-Web-API 2 auf ASP.NET 4. x berücksichtigen sollten.</span><span class="sxs-lookup"><span data-stu-id="83572-105">This topic describes some of the security issues that you should consider when exposing a dataset through OData for ASP.NET Web API 2 on ASP.NET 4.x.</span></span>

## <a name="edm-security"></a><span data-ttu-id="83572-106">EDM-Sicherheit</span><span class="sxs-lookup"><span data-stu-id="83572-106">EDM Security</span></span>

<span data-ttu-id="83572-107">Die Abfrage Semantik basiert auf dem Entity Data Model (EDM), nicht auf den zugrunde liegenden Modelltypen.</span><span class="sxs-lookup"><span data-stu-id="83572-107">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="83572-108">Sie können eine Eigenschaft aus dem EDM ausschließen und sind für die Abfrage nicht sichtbar.</span><span class="sxs-lookup"><span data-stu-id="83572-108">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="83572-109">Angenommen, das Modell enthält einen Employee-Typ mit einer Gehalt-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="83572-109">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="83572-110">Möglicherweise möchten Sie diese Eigenschaft aus dem EDM ausschließen, um Sie von Clients auszublenden.</span><span class="sxs-lookup"><span data-stu-id="83572-110">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="83572-111">Es gibt zwei Möglichkeiten, eine Eigenschaft aus dem EDM auszuschließen.</span><span class="sxs-lookup"><span data-stu-id="83572-111">There are two ways to exclude a property from the EDM.</span></span> <span data-ttu-id="83572-112">Sie können das Attribut **[ignoredatamember]** für die Eigenschaft in der Modell Klasse festlegen:</span><span class="sxs-lookup"><span data-stu-id="83572-112">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="83572-113">Sie können die-Eigenschaft auch Programm gesteuert aus dem EDM entfernen:</span><span class="sxs-lookup"><span data-stu-id="83572-113">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="83572-114">Abfrage Sicherheit</span><span class="sxs-lookup"><span data-stu-id="83572-114">Query Security</span></span>

<span data-ttu-id="83572-115">Ein böswilliger oder naive Client kann möglicherweise eine Abfrage erstellen, die für die Ausführung sehr viel Zeit in Anspruch nimmt.</span><span class="sxs-lookup"><span data-stu-id="83572-115">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="83572-116">Im schlimmsten Fall kann dies den Zugriff auf Ihren Dienst stören.</span><span class="sxs-lookup"><span data-stu-id="83572-116">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="83572-117">Das **[querable]** -Attribut ist ein Aktionsfilter, der die Abfrage analysiert, überprüft und anwendet.</span><span class="sxs-lookup"><span data-stu-id="83572-117">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="83572-118">Der Filter konvertiert die Abfrage Optionen in einen LINQ-Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="83572-118">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="83572-119">Wenn der odata-Controller einen **iquerable** -Typ zurückgibt, konvertiert der **iquerable** LINQ-Anbieter den LINQ-Ausdruck in eine Abfrage.</span><span class="sxs-lookup"><span data-stu-id="83572-119">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="83572-120">Daher ist die Leistung abhängig von dem verwendeten LINQ-Anbieter und den besonderen Merkmalen Ihres Datasets oder Datenbankschemas.</span><span class="sxs-lookup"><span data-stu-id="83572-120">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="83572-121">Weitere Informationen zur Verwendung von odata-Abfrage Optionen in ASP.net-Web-API finden Sie [unter unterstützen von odata-Abfrage Optionen](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="83572-121">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="83572-122">Wenn Sie wissen, dass alle Clients vertrauenswürdig sind (z. b. in einer Unternehmensumgebung) oder wenn das DataSet klein ist, ist die Abfrageleistung möglicherweise kein Problem.</span><span class="sxs-lookup"><span data-stu-id="83572-122">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="83572-123">Andernfalls sollten Sie die folgenden Empfehlungen in Erwägung gezogen werden.</span><span class="sxs-lookup"><span data-stu-id="83572-123">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="83572-124">Testen Sie den Dienst mit verschiedenen Abfragen, und erstellen Sie ein Profil der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="83572-124">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="83572-125">Aktivieren Sie das Server gesteuerte Paging, um zu vermeiden, dass ein großes Dataset in einer Abfrage zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="83572-125">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="83572-126">Weitere Informationen finden Sie unter [Server gesteuerte Auslagerung](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="83572-126">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="83572-127">Benötigen Sie $Filter und $OrderBy?</span><span class="sxs-lookup"><span data-stu-id="83572-127">Do you need $filter and $orderby?</span></span> <span data-ttu-id="83572-128">Einige Anwendungen ermöglichen möglicherweise das Paging von Clients, indem $Top und $Skip verwendet werden, aber die anderen Abfrage Optionen werden deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="83572-128">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="83572-129">Beschränken Sie $OrderBy auf Eigenschaften in einem gruppierten Index.</span><span class="sxs-lookup"><span data-stu-id="83572-129">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="83572-130">Das Sortieren von großen Daten ohne gruppierten Index ist langsam.</span><span class="sxs-lookup"><span data-stu-id="83572-130">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="83572-131">Maximale Anzahl von Knoten: die **maxnodecount** -Eigenschaft für **[querable]** legt die maximale Anzahl von Knoten fest, die in der $Filter Syntax Struktur zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="83572-131">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="83572-132">Der Standardwert ist 100, Sie möchten jedoch möglicherweise einen niedrigeren Wert festlegen, da eine große Anzahl von Knoten langsam kompiliert werden kann.</span><span class="sxs-lookup"><span data-stu-id="83572-132">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="83572-133">Dies trifft vor allem dann zu, wenn Sie LINQ to Objects verwenden (d. h. LINQ-Abfragen für eine Auflistung im Arbeitsspeicher, ohne einen LINQ-zwischen Anbieter).</span><span class="sxs-lookup"><span data-stu-id="83572-133">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="83572-134">Deaktivieren Sie ggf. die Funktionen any () und all (), da diese langsam sein können.</span><span class="sxs-lookup"><span data-stu-id="83572-134">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="83572-135">Wenn Zeichen folgen Eigenschaften z. b&#8212;. große Zeichen folgen enthalten, empfiehlt eine Produktbeschreibung&#8212;oder ein Blogeintrag die Deaktivierung der Zeichen folgen Funktionen.</span><span class="sxs-lookup"><span data-stu-id="83572-135">If any string properties contain large strings&#8212;for example, a product description or a blog entry&#8212;consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="83572-136">Sie sollten das Filtern auf Navigations Eigenschaften nicht zulassen.</span><span class="sxs-lookup"><span data-stu-id="83572-136">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="83572-137">Das Filtern nach Navigations Eigenschaften kann zu einer Verknüpfung führen, die je nach Datenbankschema langsam verläuft.</span><span class="sxs-lookup"><span data-stu-id="83572-137">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="83572-138">Der folgende Code zeigt ein Abfrage Validierungs Steuerelement, das das Filtern von Navigations Eigenschaften verhindert.</span><span class="sxs-lookup"><span data-stu-id="83572-138">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="83572-139">Weitere Informationen zu Abfrage Validierungs Steuerelementen finden Sie unter [Abfrage Validierung](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="83572-139">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="83572-140">Beschränken Sie $Filter Abfragen, indem Sie ein Validierungs Steuerelement schreiben, das für Ihre Datenbank angepasst ist.</span><span class="sxs-lookup"><span data-stu-id="83572-140">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="83572-141">Beachten Sie z. b. die folgenden beiden Abfragen:</span><span class="sxs-lookup"><span data-stu-id="83572-141">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="83572-142">Alle Filme mit Actors, deren Nachname mit "A" beginnt.</span><span class="sxs-lookup"><span data-stu-id="83572-142">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="83572-143">Alle in 1994 veröffentlichten Filme.</span><span class="sxs-lookup"><span data-stu-id="83572-143">All movies released in 1994.</span></span>

    <span data-ttu-id="83572-144">Wenn Filme nicht von Actors indiziert werden, kann es für die erste Abfrage erforderlich sein, dass die Datenbank-Engine die gesamte Liste der Filme scannt.</span><span class="sxs-lookup"><span data-stu-id="83572-144">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="83572-145">Die zweite Abfrage ist zwar möglicherweise akzeptabel, angenommen, die Filme werden nach dem releasejahr indiziert.</span><span class="sxs-lookup"><span data-stu-id="83572-145">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="83572-146">Der folgende Code zeigt ein Validierungs Steuerelement, das das Filtern nach den Eigenschaften "releaseyear" und "Title" ermöglicht, aber keine anderen Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="83572-146">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="83572-147">Im Allgemeinen sollten Sie überprüfen, welche $Filter Funktionen Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="83572-147">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="83572-148">Wenn Ihre Clients die vollständige Ausdruckskraft $Filter nicht benötigen, können Sie die zulässigen Funktionen einschränken.</span><span class="sxs-lookup"><span data-stu-id="83572-148">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
