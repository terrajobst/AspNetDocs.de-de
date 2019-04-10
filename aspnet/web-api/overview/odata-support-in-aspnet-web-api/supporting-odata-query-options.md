---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Unterstützung von OData-in der ASP.NET Web API 2 - ASP.NET Abfrageoptionen 4.x
author: MikeWasson
description: Übersicht über die mit Codebeispielen zeigt unterstützenden OData-Abfrageoptionen in ASP.NET Web API 2 für ASP.NET 4.x.
ms.author: riande
ms.date: 02/04/2013
ms.custom: seoapril2019
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 428e4942e42436585049c1e84cd7b07a4a79c0d1
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59411565"
---
# <a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="c6be8-103">Unterstützung von OData-Abfrageoptionen in der ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="c6be8-103">Supporting OData Query Options in ASP.NET Web API 2</span></span>

<span data-ttu-id="c6be8-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c6be8-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c6be8-105">In dieser Übersicht mit Codebeispielen veranschaulicht unterstützenden OData-Abfrageoptionen in ASP.NET Web API 2 für ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="c6be8-105">This overview with code examples demonstrates the supporting OData Query Options in ASP.NET Web API 2 for ASP.NET 4.x.</span></span> 

<span data-ttu-id="c6be8-106">OData definiert die Parameter, die zum Ändern einer OData-Abfrage verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="c6be8-106">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="c6be8-107">Der Client sendet diese Parameter in der Abfragezeichenfolge der Anforderungs-URI.</span><span class="sxs-lookup"><span data-stu-id="c6be8-107">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="c6be8-108">Um die Ergebnisse zu sortieren, verwendet z. B. ein Client den $orderby-Parameter an:</span><span class="sxs-lookup"><span data-stu-id="c6be8-108">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="c6be8-109">Die OData-Spezifikation ruft diese Parameter *Abfrageoptionen*.</span><span class="sxs-lookup"><span data-stu-id="c6be8-109">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="c6be8-110">Sie können OData-Abfrageoptionen für alle Web-API-Controller in Ihrem Projekt &#8212; Controller muss nicht auf einen OData-Endpunkt handeln.</span><span class="sxs-lookup"><span data-stu-id="c6be8-110">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="c6be8-111">Dies bietet Ihnen eine bequeme Möglichkeit zum Hinzufügen von Features wie beispielsweise Filterung und Sortierung für jede Web-API-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="c6be8-111">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="c6be8-112">Vor dem Aktivieren der Abfrageoptionen, lesen Sie bitte das Thema [OData-Sicherheitsleitfaden](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="c6be8-112">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="c6be8-113">Aktivieren der OData-Abfrageoptionen</span><span class="sxs-lookup"><span data-stu-id="c6be8-113">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="c6be8-114">Beispiele für Abfragen</span><span class="sxs-lookup"><span data-stu-id="c6be8-114">Example Queries</span></span>](#examples)
- [<span data-ttu-id="c6be8-115">Servergesteuertes Paging</span><span class="sxs-lookup"><span data-stu-id="c6be8-115">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="c6be8-116">Beschränken die Abfrageoptionen</span><span class="sxs-lookup"><span data-stu-id="c6be8-116">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="c6be8-117">Abfrageoptionen direkt aufrufen</span><span class="sxs-lookup"><span data-stu-id="c6be8-117">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="c6be8-118">Abfragevalidierung</span><span class="sxs-lookup"><span data-stu-id="c6be8-118">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="c6be8-119">Aktivieren der OData-Abfrageoptionen</span><span class="sxs-lookup"><span data-stu-id="c6be8-119">Enabling OData Query Options</span></span>

<span data-ttu-id="c6be8-120">Web-API unterstützt die folgenden OData-Abfrageoptionen:</span><span class="sxs-lookup"><span data-stu-id="c6be8-120">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="c6be8-121">Option</span><span class="sxs-lookup"><span data-stu-id="c6be8-121">Option</span></span> | <span data-ttu-id="c6be8-122">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="c6be8-122">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c6be8-123">der $expand-</span><span class="sxs-lookup"><span data-stu-id="c6be8-123">$expand</span></span> | <span data-ttu-id="c6be8-124">Wird die verknüpfte Entitäten Inline erweitert.</span><span class="sxs-lookup"><span data-stu-id="c6be8-124">Expands related entities inline.</span></span> |
| <span data-ttu-id="c6be8-125">$filter</span><span class="sxs-lookup"><span data-stu-id="c6be8-125">$filter</span></span> | <span data-ttu-id="c6be8-126">Filtert die Ergebnisse, die auf Basis einer booleschen Bedingung.</span><span class="sxs-lookup"><span data-stu-id="c6be8-126">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="c6be8-127">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="c6be8-127">$inlinecount</span></span> | <span data-ttu-id="c6be8-128">Weist den Server, der die Gesamtzahl der übereinstimmenden Entitäten in der Antwort enthalten.</span><span class="sxs-lookup"><span data-stu-id="c6be8-128">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="c6be8-129">(Hilfreich für die serverseitige Auslagerung.)</span><span class="sxs-lookup"><span data-stu-id="c6be8-129">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="c6be8-130">$orderby</span><span class="sxs-lookup"><span data-stu-id="c6be8-130">$orderby</span></span> | <span data-ttu-id="c6be8-131">Sortiert die Ergebnisse.</span><span class="sxs-lookup"><span data-stu-id="c6be8-131">Sorts the results.</span></span> |
| <span data-ttu-id="c6be8-132">$select</span><span class="sxs-lookup"><span data-stu-id="c6be8-132">$select</span></span> | <span data-ttu-id="c6be8-133">Wählt die Eigenschaften an, in der Antwort enthalten.</span><span class="sxs-lookup"><span data-stu-id="c6be8-133">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="c6be8-134">$skip</span><span class="sxs-lookup"><span data-stu-id="c6be8-134">$skip</span></span> | <span data-ttu-id="c6be8-135">Überspringt die ersten n Ergebnisse.</span><span class="sxs-lookup"><span data-stu-id="c6be8-135">Skips the first n results.</span></span> |
| <span data-ttu-id="c6be8-136">$top</span><span class="sxs-lookup"><span data-stu-id="c6be8-136">$top</span></span> | <span data-ttu-id="c6be8-137">Gibt nur der ersten n Ergebnisse zurück.</span><span class="sxs-lookup"><span data-stu-id="c6be8-137">Returns only the first n the results.</span></span> |

<span data-ttu-id="c6be8-138">Um die OData-Abfrageoptionen verwenden zu können, müssen Sie sie explizit aktivieren.</span><span class="sxs-lookup"><span data-stu-id="c6be8-138">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="c6be8-139">Sie können global für die gesamte Anwendung zu aktivieren, oder für bestimmte Domänencontroller oder bestimmte Aktionen zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="c6be8-139">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="c6be8-140">Um OData-Abfrageoptionen global zu aktivieren, rufen **EnableQuerySupport** auf die **HttpConfiguration** Klasse beim Start:</span><span class="sxs-lookup"><span data-stu-id="c6be8-140">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="c6be8-141">Die **EnableQuerySupport** Methode können Sie Abfrageoptionen global für alle Controlleraktion, die zurückgibt ein **"IQueryable"** Typ.</span><span class="sxs-lookup"><span data-stu-id="c6be8-141">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="c6be8-142">Wenn Sie nicht, dass die Abfrageoptionen für die gesamte Anwendung aktiviert möchten, können Sie sie für bestimmte Controlleraktionen aktivieren, durch das Hinzufügen der **[Queryable]** Attribut zur Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="c6be8-142">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="c6be8-143">Beispiele für Abfragen</span><span class="sxs-lookup"><span data-stu-id="c6be8-143">Example Queries</span></span>

<span data-ttu-id="c6be8-144">Dieser Abschnitt zeigt die Typen von Abfragen, die mit der OData-Abfrageoptionen möglich sind.</span><span class="sxs-lookup"><span data-stu-id="c6be8-144">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="c6be8-145">Spezifische Details zu den Abfrageoptionen, finden Sie in der OData-Dokumentation unter [www.odata.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="c6be8-145">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="c6be8-146">Informationen zu $erweitern und $select, finden Sie unter [verwenden $select, $expand, und $value in OData der ASP.NET Web API](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="c6be8-146">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

**<span data-ttu-id="c6be8-147">Client-gesteuerte Paging</span><span class="sxs-lookup"><span data-stu-id="c6be8-147">Client-Driven Paging</span></span>**

<span data-ttu-id="c6be8-148">Bei großen Entitätssätzen sollten der Client die Anzahl der Ergebnisse zu beschränken.</span><span class="sxs-lookup"><span data-stu-id="c6be8-148">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="c6be8-149">Beispielsweise kann ein Client 10 Einträge jeweils mit "Weiter" Links zum Abrufen der nächsten Seite der Ergebnisse zeigen.</span><span class="sxs-lookup"><span data-stu-id="c6be8-149">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="c6be8-150">Hierzu verwendet der Client die $top und $skip-Optionen.</span><span class="sxs-lookup"><span data-stu-id="c6be8-150">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="c6be8-151">$Top können die maximale Anzahl von Einträgen zurückgegeben, und die Option $skip gibt die Anzahl der zu überspringenden Einträge.</span><span class="sxs-lookup"><span data-stu-id="c6be8-151">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="c6be8-152">Im vorherige Beispiel ruft Einträge 21 bis 30.</span><span class="sxs-lookup"><span data-stu-id="c6be8-152">The previous example fetches entries 21 through 30.</span></span>

**<span data-ttu-id="c6be8-153">Filtern</span><span class="sxs-lookup"><span data-stu-id="c6be8-153">Filtering</span></span>**

<span data-ttu-id="c6be8-154">Die $filter-Option ermöglicht einen Client, der die Ergebnisse zu filtern, indem Sie einen booleschen Ausdruck anwenden.</span><span class="sxs-lookup"><span data-stu-id="c6be8-154">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="c6be8-155">Die Filterausdrücke sind sehr leistungsstark; Sie enthalten, logische und arithmetische Operatoren, Zeichenfolgenfunktionen und Date-Funktionen.</span><span class="sxs-lookup"><span data-stu-id="c6be8-155">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="c6be8-156">Zurückgeben Sie aller Produkte, mit der Kategorie "Toys" gleich.</span><span class="sxs-lookup"><span data-stu-id="c6be8-156">Return all products with category equal to "Toys".</span></span> | `http://localhost/Products?$filter=Category` <span data-ttu-id="c6be8-157">EQ "Toys"</span><span class="sxs-lookup"><span data-stu-id="c6be8-157">eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="c6be8-158">Geben Sie alle Produkte mit weniger als 10 Preis zurück.</span><span class="sxs-lookup"><span data-stu-id="c6be8-158">Return all products with price less than 10.</span></span> | `http://localhost/Products?$filter=Price` <span data-ttu-id="c6be8-159">Lt 10</span><span class="sxs-lookup"><span data-stu-id="c6be8-159">lt 10</span></span> |
| <span data-ttu-id="c6be8-160">Logische Operatoren: Alle Produkte zurück, in dem Preis > = 5 und Preis < = 15.</span><span class="sxs-lookup"><span data-stu-id="c6be8-160">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | `http://localhost/Products?$filter=Price` <span data-ttu-id="c6be8-161">ge 5 und Preis le 15</span><span class="sxs-lookup"><span data-stu-id="c6be8-161">ge 5 and Price le 15</span></span> |
| <span data-ttu-id="c6be8-162">Zeichenfolgenfunktionen: Gibt alle Produkte mit "Zz" in den Namen zurück.</span><span class="sxs-lookup"><span data-stu-id="c6be8-162">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="c6be8-163">Datumsfunktionen: Gibt alle Produkte mit ReleaseDate nach 2005 zurück.</span><span class="sxs-lookup"><span data-stu-id="c6be8-163">Date functions: Return all products with ReleaseDate after 2005.</span></span> | `http://localhost/Products?$filter=year(ReleaseDate)` <span data-ttu-id="c6be8-164">Gt 2005</span><span class="sxs-lookup"><span data-stu-id="c6be8-164">gt 2005</span></span> |

**<span data-ttu-id="c6be8-165">Sortieren</span><span class="sxs-lookup"><span data-stu-id="c6be8-165">Sorting</span></span>**

<span data-ttu-id="c6be8-166">Verwenden Sie zum Sortieren der Ergebnisse der $orderby Filter ein.</span><span class="sxs-lookup"><span data-stu-id="c6be8-166">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="c6be8-167">Sortieren Sie nach Preis.</span><span class="sxs-lookup"><span data-stu-id="c6be8-167">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="c6be8-168">Sortieren Sie nach Preis in absteigender Reihenfolge (höchster zu niedrigster Priorität).</span><span class="sxs-lookup"><span data-stu-id="c6be8-168">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="c6be8-169">Sortieren Sie nach Kategorie und dann nach Preis in absteigender Reihenfolge in Kategorien zu sortieren.</span><span class="sxs-lookup"><span data-stu-id="c6be8-169">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="c6be8-170">Servergesteuertes Paging</span><span class="sxs-lookup"><span data-stu-id="c6be8-170">Server-Driven Paging</span></span>

<span data-ttu-id="c6be8-171">Wenn die Datenbank von Millionen von Zeilen enthält, möchten Sie nicht senden sie alle in einer Nutzlast.</span><span class="sxs-lookup"><span data-stu-id="c6be8-171">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="c6be8-172">Um dies zu verhindern, kann der Server die Anzahl der Einträge begrenzen, den sie in einer einzelnen Antwort sendet.</span><span class="sxs-lookup"><span data-stu-id="c6be8-172">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="c6be8-173">Legen Sie zum Aktivieren von Serverpaging der **PageSize** -Eigenschaft in der **Queryable** Attribut.</span><span class="sxs-lookup"><span data-stu-id="c6be8-173">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="c6be8-174">Der Wert ist die maximale Anzahl von Einträgen zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="c6be8-174">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="c6be8-175">Wenn Ihr Controller OData-Format zurückgibt, enthält der Antworttext einen Link zur nächsten Seite der Daten:</span><span class="sxs-lookup"><span data-stu-id="c6be8-175">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="c6be8-176">Der Client kann auf diesen Link verwenden, zum Abrufen der nächsten Seite.</span><span class="sxs-lookup"><span data-stu-id="c6be8-176">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="c6be8-177">Die Gesamtanzahl von Einträgen im Resultset zu finden, kann der Client die Abfrageoption $inlinecount, mit dem Wert "Allpages" festlegen.</span><span class="sxs-lookup"><span data-stu-id="c6be8-177">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="c6be8-178">Der Wert weist "Allpages" den Server an der Gesamtanzahl in der Antwort enthalten:</span><span class="sxs-lookup"><span data-stu-id="c6be8-178">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="c6be8-179">Nächste Seite Links und Inlineanzahl jeweils der OData-Format erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c6be8-179">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="c6be8-180">Der Grund ist, dass OData spezielle Felder im Antworttext zurückgeben, um dem Link und die Anzahl von aufzunehmen definiert.</span><span class="sxs-lookup"><span data-stu-id="c6be8-180">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>


<span data-ttu-id="c6be8-181">Für nicht-OData-Formate, ist es weiterhin möglich, Anzahl der Wechsel zur nächsten Seite Links und Inline, zu unterstützen, indem Sie umschließen die Ergebnisse der Abfrage in einem **PageResult&lt;T&gt;**  Objekt.</span><span class="sxs-lookup"><span data-stu-id="c6be8-181">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="c6be8-182">Allerdings ist es ein wenig mehr Code erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c6be8-182">However, it requires a bit more code.</span></span> <span data-ttu-id="c6be8-183">Im Folgenden ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c6be8-183">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="c6be8-184">Hier ist ein Beispiel für JSON-Antwort:</span><span class="sxs-lookup"><span data-stu-id="c6be8-184">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="c6be8-185">Beschränken die Abfrageoptionen</span><span class="sxs-lookup"><span data-stu-id="c6be8-185">Limiting the Query Options</span></span>

<span data-ttu-id="c6be8-186">Die Abfrageoptionen, den der Client einen Großteil der Kontrolle über die Abfrage, die auf dem Server ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c6be8-186">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="c6be8-187">In einigen Fällen empfiehlt es sich um die verfügbaren Optionen aus Gründen der Sicherheit oder Leistung zu beschränken.</span><span class="sxs-lookup"><span data-stu-id="c6be8-187">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="c6be8-188">Die **[Queryable]** Attribut verfügt über einige integrierte Eigenschaften für diese.</span><span class="sxs-lookup"><span data-stu-id="c6be8-188">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="c6be8-189">Im Folgenden finden Sie einige Beispiele.</span><span class="sxs-lookup"><span data-stu-id="c6be8-189">Here are some examples.</span></span>

<span data-ttu-id="c6be8-190">Zulassen von nur $skip und $top, zur Unterstützung von Paging und nichts anderes:</span><span class="sxs-lookup"><span data-stu-id="c6be8-190">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="c6be8-191">Lassen Sie die Reihenfolge nur durch bestimmte Eigenschaften, um zu verhindern, Sortieren nach Eigenschaften, die in der Datenbank nicht indiziert werden:</span><span class="sxs-lookup"><span data-stu-id="c6be8-191">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="c6be8-192">Zulassen der logischen "Eq"-Funktion, aber keine anderen logischen Funktionen:</span><span class="sxs-lookup"><span data-stu-id="c6be8-192">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="c6be8-193">Lassen Sie keine arithmetischen Operatoren:</span><span class="sxs-lookup"><span data-stu-id="c6be8-193">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="c6be8-194">Sie können Optionen global einschränken, durch das Erstellen einer **queryableattribute überprüft** -Instanz und die Übergabe an die **EnableQuerySupport** Funktion:</span><span class="sxs-lookup"><span data-stu-id="c6be8-194">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="c6be8-195">Abfrageoptionen direkt aufrufen</span><span class="sxs-lookup"><span data-stu-id="c6be8-195">Invoking Query Options Directly</span></span>

<span data-ttu-id="c6be8-196">Anstatt die **[Queryable]** -Attribut, Sie können die Abfrageoptionen direkt in Ihrem Controller aufrufen.</span><span class="sxs-lookup"><span data-stu-id="c6be8-196">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="c6be8-197">Fügen Sie zu diesem Zweck eine **ODataQueryOptions** Parameter für die Controllermethode.</span><span class="sxs-lookup"><span data-stu-id="c6be8-197">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="c6be8-198">In diesem Fall müssen nicht die **[Queryable]** Attribut.</span><span class="sxs-lookup"><span data-stu-id="c6be8-198">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="c6be8-199">Web-API füllt die **ODataQueryOptions** aus dem URI-Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="c6be8-199">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="c6be8-200">Übergeben Sie zum Anwenden der Abfrage eine **"IQueryable"** auf die **ApplyTo** Methode.</span><span class="sxs-lookup"><span data-stu-id="c6be8-200">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="c6be8-201">Die Methode wird ein anderes **"IQueryable"**.</span><span class="sxs-lookup"><span data-stu-id="c6be8-201">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="c6be8-202">Für erweiterte Szenarien, wenn Sie kein **"IQueryable"** -Abfrageanbieter, die Sie untersuchen die **ODataQueryOptions** und die Abfrageoptionen in eine andere Form übersetzen.</span><span class="sxs-lookup"><span data-stu-id="c6be8-202">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="c6be8-203">(Z. B. finden Sie in RaghuRam Nadimintis Blogbeitrag [Übersetzen von OData-Abfragen in HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), darunter auch eine [Beispiel](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="c6be8-203">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="c6be8-204">Abfragevalidierung</span><span class="sxs-lookup"><span data-stu-id="c6be8-204">Query Validation</span></span>

<span data-ttu-id="c6be8-205">Die **[Queryable]** Attribut überprüft, ob die Abfrage vor der Ausführung.</span><span class="sxs-lookup"><span data-stu-id="c6be8-205">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="c6be8-206">Der Überprüfungsschritt erfolgt in der **QueryableAttribute.ValidateQuery** Methode.</span><span class="sxs-lookup"><span data-stu-id="c6be8-206">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="c6be8-207">Sie können auch den Überprüfungsprozess anpassen.</span><span class="sxs-lookup"><span data-stu-id="c6be8-207">You can also customize the validation process.</span></span>

<span data-ttu-id="c6be8-208">Siehe auch [OData-Sicherheitsleitfaden](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="c6be8-208">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="c6be8-209">Zunächst außer Kraft setzen, die eine für das Validierungssteuerelement, Klassen definiert, der **Web.Http.OData.Query.Validators** Namespace.</span><span class="sxs-lookup"><span data-stu-id="c6be8-209">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="c6be8-210">Beispielsweise werden die folgenden Validator-Klasse die Option 'Desc' für die $orderby-Option deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="c6be8-210">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="c6be8-211">Unterklasse der **[Queryable]** Attribut zum Überschreiben der **ValidateQuery** Methode.</span><span class="sxs-lookup"><span data-stu-id="c6be8-211">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="c6be8-212">Legen Sie dann das benutzerdefinierte Attribut entweder global oder pro Controller:</span><span class="sxs-lookup"><span data-stu-id="c6be8-212">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="c6be8-213">Bei Verwendung von **ODataQueryOptions** direkt festgelegt werden, das Validierungssteuerelement zu den Optionen:</span><span class="sxs-lookup"><span data-stu-id="c6be8-213">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
