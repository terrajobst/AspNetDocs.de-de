---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON- und XML-Serialisierung in der ASP.NET Web-API – ASP.NET 4.x
author: MikeWasson
description: Beschreibt die JSON- und XML-Formatierer in ASP.NET Web-API für ASP.NET 4.x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: a9e7ed63a55c146976e0221214e722f3a2292fee
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59408276"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="ea821-103">JSON- und XML-Serialisierung in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="ea821-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="ea821-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ea821-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ea821-105">Dieser Artikel beschreibt die JSON- und XML-Formatierer in ASP.NET Web-API.</span><span class="sxs-lookup"><span data-stu-id="ea821-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="ea821-106">In ASP.NET Web-API eine *medientypformatierer* ist ein Objekt, das können:</span><span class="sxs-lookup"><span data-stu-id="ea821-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="ea821-107">Read-CLR-Objekte aus einer HTTP-Nachrichtentext</span><span class="sxs-lookup"><span data-stu-id="ea821-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="ea821-108">Schreiben von CLR-Objekten in einen HTTP-Nachrichtentext</span><span class="sxs-lookup"><span data-stu-id="ea821-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="ea821-109">Web-API bietet die medientypformatierer für JSON- und XML.</span><span class="sxs-lookup"><span data-stu-id="ea821-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="ea821-110">Das Framework fügt dieser Formatierer in die Pipeline in der Standardeinstellung.</span><span class="sxs-lookup"><span data-stu-id="ea821-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="ea821-111">Clients können XML oder JSON im Accept-Header der HTTP-Anforderung anfordern.</span><span class="sxs-lookup"><span data-stu-id="ea821-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="ea821-112">Inhalt</span><span class="sxs-lookup"><span data-stu-id="ea821-112">Contents</span></span>

- [<span data-ttu-id="ea821-113">JSON-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="ea821-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="ea821-114">Schreibgeschützte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="ea821-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="ea821-115">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="ea821-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="ea821-116">Einzug</span><span class="sxs-lookup"><span data-stu-id="ea821-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="ea821-117">Kamel-Schreibweise</span><span class="sxs-lookup"><span data-stu-id="ea821-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="ea821-118">Anonyme und schwach typisierte Objekte</span><span class="sxs-lookup"><span data-stu-id="ea821-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="ea821-119">XML-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="ea821-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="ea821-120">Schreibgeschützte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="ea821-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="ea821-121">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="ea821-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="ea821-122">Einzug</span><span class="sxs-lookup"><span data-stu-id="ea821-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="ea821-123">Festlegen von pro-Type-XML-Serialisierungsprogrammen</span><span class="sxs-lookup"><span data-stu-id="ea821-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="ea821-124">Entfernen die JSON- oder XML-Formatierungsprogramm</span><span class="sxs-lookup"><span data-stu-id="ea821-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="ea821-125">Behandeln von zyklischen Objektverweisen</span><span class="sxs-lookup"><span data-stu-id="ea821-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="ea821-126">Testen die Serialisierung von Objekten</span><span class="sxs-lookup"><span data-stu-id="ea821-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="ea821-127">JSON-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="ea821-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="ea821-128">JSON-Formatierung erfolgt über die **JsonMediaTypeFormatter** Klasse.</span><span class="sxs-lookup"><span data-stu-id="ea821-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="ea821-129">In der Standardeinstellung **JsonMediaTypeFormatter** verwendet die [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Bibliothek zur Serialisierung.</span><span class="sxs-lookup"><span data-stu-id="ea821-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="ea821-130">Json.NET wird eine Drittanbieter-open-Source-Projekt.</span><span class="sxs-lookup"><span data-stu-id="ea821-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="ea821-131">Wenn Sie es vorziehen, können Sie konfigurieren die **JsonMediaTypeFormatter** zu verwendende Klasse an die **DataContractJsonSerializer** anstelle von Json.NET.</span><span class="sxs-lookup"><span data-stu-id="ea821-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="ea821-132">Zu diesem Zweck legen Sie die **UseDataContractJsonSerializer** Eigenschaft **"true"**:</span><span class="sxs-lookup"><span data-stu-id="ea821-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="ea821-133">JSON-Serialisierung</span><span class="sxs-lookup"><span data-stu-id="ea821-133">JSON Serialization</span></span>

<span data-ttu-id="ea821-134">Dieser Abschnitt beschreibt einige bestimmten Verhaltensweisen von JSON-Formatierungsprogramm, über das standardmäßige [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Serialisierungsprogramm.</span><span class="sxs-lookup"><span data-stu-id="ea821-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="ea821-135">Dies soll keine umfassende Dokumentation der JSON.NET-Bibliothek werden; Weitere Informationen finden Sie unter den [JSON.NET-Dokumentation](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="ea821-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="ea821-136">Was serialisiert werden?</span><span class="sxs-lookup"><span data-stu-id="ea821-136">What Gets Serialized?</span></span>

<span data-ttu-id="ea821-137">Standardmäßig sind alle öffentlichen Eigenschaften und Felder in den serialisierten JSON-Code enthalten.</span><span class="sxs-lookup"><span data-stu-id="ea821-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="ea821-138">Um eine Eigenschaft oder ein Feld zu unterdrücken, versehen sie mit der **JsonIgnore** Attribut.</span><span class="sxs-lookup"><span data-stu-id="ea821-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="ea821-139">Falls gewünscht ein &quot;teilnehmen&quot; Ansatz, ergänzen die Klasse der **DataContract** Attribut.</span><span class="sxs-lookup"><span data-stu-id="ea821-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="ea821-140">Wenn dieses Attribut vorhanden ist, werden Elemente ignoriert, es sei denn, sie haben die **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="ea821-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="ea821-141">Sie können auch **DataMember** Private Member zu serialisieren.</span><span class="sxs-lookup"><span data-stu-id="ea821-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="ea821-142">Schreibgeschützte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="ea821-142">Read-Only Properties</span></span>

<span data-ttu-id="ea821-143">Standardmäßig werden die schreibgeschützten Eigenschaften serialisiert.</span><span class="sxs-lookup"><span data-stu-id="ea821-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="ea821-144">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="ea821-144">Dates</span></span>

<span data-ttu-id="ea821-145">Standardmäßig schreibt Json.NET Datumsangaben [ISO 8601](http://www.w3.org/TR/NOTE-datetime) Format.</span><span class="sxs-lookup"><span data-stu-id="ea821-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="ea821-146">Datumsangaben in UTC (Coordinated Universal Time) werden mit dem Suffix "Z" geschrieben.</span><span class="sxs-lookup"><span data-stu-id="ea821-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="ea821-147">Datumsangaben in Ortszeit enthalten einen Zeitzonenoffset.</span><span class="sxs-lookup"><span data-stu-id="ea821-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="ea821-148">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ea821-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="ea821-149">Standardmäßig behält Json.NET die Zeitzone an.</span><span class="sxs-lookup"><span data-stu-id="ea821-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="ea821-150">Sie können dies durch Festlegen der DateTimeZoneHandling-Eigenschaft außer Kraft setzen:</span><span class="sxs-lookup"><span data-stu-id="ea821-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="ea821-151">Wenn Sie lieber mit [Microsoft JSON-Datumsformat](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) anstelle von ISO 8601, legen Sie die **DateFormatHandling** Eigenschaft für die serialisierereinstellungen:</span><span class="sxs-lookup"><span data-stu-id="ea821-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="ea821-152">Einzug</span><span class="sxs-lookup"><span data-stu-id="ea821-152">Indenting</span></span>

<span data-ttu-id="ea821-153">Um eingezogen JSON zu schreiben, legen die **Formatierung** auf **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="ea821-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="ea821-154">Kamel-Schreibweise</span><span class="sxs-lookup"><span data-stu-id="ea821-154">Camel Casing</span></span>

<span data-ttu-id="ea821-155">Um JSON-Eigenschaftennamen mit Kamel-Schreibweise, schreiben, ohne Ihr Datenmodell ändern, legen die **CamelCasePropertyNamesContractResolver** auf das Serialisierungsprogramm:</span><span class="sxs-lookup"><span data-stu-id="ea821-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="ea821-156">Anonyme und schwach typisierte Objekte</span><span class="sxs-lookup"><span data-stu-id="ea821-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="ea821-157">Eine Aktionsmethode kann ein anonymes Objekt zurückgeben und deren Serialisierung in JSON.</span><span class="sxs-lookup"><span data-stu-id="ea821-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="ea821-158">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ea821-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="ea821-159">Text der Antwortnachricht enthält die folgenden JSON-Code:</span><span class="sxs-lookup"><span data-stu-id="ea821-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="ea821-160">Wenn Ihre Web-API lose empfängt JSON-Objekte von den Clients strukturiert sind, können Sie den Hauptteil der Anforderung zum Deserialisieren einer **Newtonsoft.Json.Linq.JObject** Typ.</span><span class="sxs-lookup"><span data-stu-id="ea821-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="ea821-161">Allerdings ist es normalerweise besser, stark typisierte Datenobjekte zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ea821-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="ea821-162">Klicken Sie dann Sie müssen nicht die Daten zu analysieren, und erhalten Sie die Vorteile der modellvalidierung.</span><span class="sxs-lookup"><span data-stu-id="ea821-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="ea821-163">Das XML-Serialisierungsprogramm unterstützt keine anonyme Typen oder **"jobject"** Instanzen.</span><span class="sxs-lookup"><span data-stu-id="ea821-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="ea821-164">Wenn Sie diese Funktionen für die JSON-Daten verwenden, sollten Sie das XML-Formatierungsprogramm aus der Pipeline entfernen, wie weiter unten in diesem Artikel beschrieben.</span><span class="sxs-lookup"><span data-stu-id="ea821-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="ea821-165">XML-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="ea821-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="ea821-166">XML-Formatierung erfolgt über die **XmlMediaTypeFormatter** Klasse.</span><span class="sxs-lookup"><span data-stu-id="ea821-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="ea821-167">In der Standardeinstellung **XmlMediaTypeFormatter** verwendet die **DataContractSerializer** Klasse zur Serialisierung.</span><span class="sxs-lookup"><span data-stu-id="ea821-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="ea821-168">Falls gewünscht, können Sie konfigurieren die **XmlMediaTypeFormatter** verwenden die **XmlSerializer** anstelle von der **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="ea821-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="ea821-169">Zu diesem Zweck legen Sie die **UseXmlSerializer** Eigenschaft **"true"**:</span><span class="sxs-lookup"><span data-stu-id="ea821-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="ea821-170">Die **XmlSerializer** Klasse unterstützt eine eingeschränktere Gruppe von Typen als **DataContractSerializer**, jedoch bietet mehr Kontrolle über den sich ergebenden XML-Code.</span><span class="sxs-lookup"><span data-stu-id="ea821-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="ea821-171">Erwägen Sie die Verwendung **XmlSerializer** Wenn Sie ein vorhandenes XML-Schema entsprechen müssen.</span><span class="sxs-lookup"><span data-stu-id="ea821-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="ea821-172">XML-Serialisierung</span><span class="sxs-lookup"><span data-stu-id="ea821-172">XML Serialization</span></span>

<span data-ttu-id="ea821-173">Dieser Abschnitt beschreibt einige bestimmten Verhaltensweisen von der XML-Formatierer, der über das standardmäßige **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="ea821-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="ea821-174">Standardmäßig verhält sich das DataContractSerializer-Element wie folgt:</span><span class="sxs-lookup"><span data-stu-id="ea821-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="ea821-175">Es werden alle öffentlichen Lese-/Schreibeigenschaften und Felder serialisiert.</span><span class="sxs-lookup"><span data-stu-id="ea821-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="ea821-176">Um eine Eigenschaft oder ein Feld zu unterdrücken, versehen sie mit der **IgnoreDataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="ea821-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="ea821-177">Private und geschützte Member werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="ea821-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="ea821-178">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="ea821-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="ea821-179">(Allerdings werden die Inhalte einer nur-Lese Auflistungseigenschaft serialisiert.)</span><span class="sxs-lookup"><span data-stu-id="ea821-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="ea821-180">Klassen- und Membernamen werden in der XML-Code geschrieben werden, genau wie in der Klassendeklaration.</span><span class="sxs-lookup"><span data-stu-id="ea821-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="ea821-181">Es wird ein XML-Standardnamespace verwendet.</span><span class="sxs-lookup"><span data-stu-id="ea821-181">A default XML namespace is used.</span></span>

<span data-ttu-id="ea821-182">Wenn Sie mehr Kontrolle über die Serialisierung benötigen, können Sie ergänzen die Klasse der **DataContract** Attribut.</span><span class="sxs-lookup"><span data-stu-id="ea821-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="ea821-183">Wenn dieses Attribut vorhanden ist, wird die Klasse wie folgt serialisiert:</span><span class="sxs-lookup"><span data-stu-id="ea821-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="ea821-184">&quot;Abonnieren von&quot; Ansatz: Eigenschaften und Felder werden standardmäßig nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="ea821-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="ea821-185">Um eine Eigenschaft oder ein Feld serialisieren, versehen sie mit der **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="ea821-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="ea821-186">Um ein privates oder geschütztes Member zu serialisieren, versehen sie mit der **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="ea821-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="ea821-187">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="ea821-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="ea821-188">Um ändern, wie der Klassenname in der XML-Code angezeigt wird, legen die *Namen* Parameter in der **DataContract** Attribut.</span><span class="sxs-lookup"><span data-stu-id="ea821-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="ea821-189">Um ändern, wie ein Elementnamen in der XML-Code angezeigt wird, legen die *Namen* Parameter in der **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="ea821-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="ea821-190">Um den XML-Namespace zu ändern, legen die *Namespace* Parameter in der **DataContract** Klasse.</span><span class="sxs-lookup"><span data-stu-id="ea821-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="ea821-191">Schreibgeschützte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="ea821-191">Read-Only Properties</span></span>

<span data-ttu-id="ea821-192">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="ea821-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="ea821-193">Verfügt eine nur-Lese Eigenschaft auf ein privates Unterstützungsfeld, können Sie mit das private Feld markieren die **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="ea821-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="ea821-194">Dieser Ansatz erfordert den **DataContract** -Attribut in der Klasse.</span><span class="sxs-lookup"><span data-stu-id="ea821-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="ea821-195">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="ea821-195">Dates</span></span>

<span data-ttu-id="ea821-196">Datumsangaben werden im ISO 8601-Format geschrieben.</span><span class="sxs-lookup"><span data-stu-id="ea821-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="ea821-197">Z. B. &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="ea821-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="ea821-198">Einzug</span><span class="sxs-lookup"><span data-stu-id="ea821-198">Indenting</span></span>

<span data-ttu-id="ea821-199">Um eingezogene XML zu schreiben, legen die **Einzug** Eigenschaft **"true"**:</span><span class="sxs-lookup"><span data-stu-id="ea821-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="ea821-200">Festlegen von pro-Type-XML-Serialisierungsprogrammen</span><span class="sxs-lookup"><span data-stu-id="ea821-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="ea821-201">Sie können verschiedene XML-Serialisierer für andere CLR-Typen festlegen.</span><span class="sxs-lookup"><span data-stu-id="ea821-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="ea821-202">Angenommen, Sie müssen möglicherweise ein bestimmtes Objekt, das erfordert **XmlSerializer** um Abwärtskompatibilität zu gewährleisten.</span><span class="sxs-lookup"><span data-stu-id="ea821-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="ea821-203">Sie können **XmlSerializer** für dieses Objekt aus, und verwenden weiterhin **DataContractSerializer** für andere Typen.</span><span class="sxs-lookup"><span data-stu-id="ea821-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="ea821-204">Rufen Sie zum Festlegen einer XML-Serialisierungsprogramm für einen bestimmten Typ **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="ea821-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="ea821-205">Sie können angeben, ein **XmlSerializer** oder ein beliebiges Objekt, das von abgeleitet ist **XmlObjectSerializer-Elements**.</span><span class="sxs-lookup"><span data-stu-id="ea821-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="ea821-206">Entfernen die JSON- oder XML-Formatierungsprogramm</span><span class="sxs-lookup"><span data-stu-id="ea821-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="ea821-207">Sie können das JSON-Formatierungsprogramm oder das XML-Formatierungsprogramm aus der Liste der Formatierer, entfernen, wenn Sie nicht, deren Verwendung möchten.</span><span class="sxs-lookup"><span data-stu-id="ea821-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="ea821-208">Zu diesem Zweck die wichtigsten Gründe sind:</span><span class="sxs-lookup"><span data-stu-id="ea821-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="ea821-209">Um Ihre Web-API-Antworten auf einen bestimmten Medientyp zu beschränken.</span><span class="sxs-lookup"><span data-stu-id="ea821-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="ea821-210">Sie könnten z. B. nur JSON-Antworten zu unterstützen, und entfernen das XML-Formatierungsprogramm.</span><span class="sxs-lookup"><span data-stu-id="ea821-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="ea821-211">Um das Standardformatierungsprogramm durch ein benutzerdefiniertes Formatierungsprogramm zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="ea821-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="ea821-212">Beispielsweise können Sie das JSON-Formatierungsprogramm durch Ihre eigene benutzerdefinierte Implementierung einer JSON-Formatierung ersetzen.</span><span class="sxs-lookup"><span data-stu-id="ea821-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="ea821-213">Der folgende Code zeigt, wie Sie die Standard-Formatierer zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="ea821-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="ea821-214">Rufen Sie diese aus Ihrem **Anwendung\_starten** Methode, die in "Global.asax" definiert.</span><span class="sxs-lookup"><span data-stu-id="ea821-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="ea821-215">Behandeln von zyklischen Objektverweisen</span><span class="sxs-lookup"><span data-stu-id="ea821-215">Handling Circular Object References</span></span>

<span data-ttu-id="ea821-216">Standardmäßig schreiben die JSON- und XML-Formatierer alle Objekte als Werte.</span><span class="sxs-lookup"><span data-stu-id="ea821-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="ea821-217">Wenn zwei Eigenschaften, die auf dasselbe Objekt verweisen oder wenn das gleiche Objekt zweimal in einer Auflistung angezeigt wird, das Formatierungsprogramm das Objekt zweimal serialisiert.</span><span class="sxs-lookup"><span data-stu-id="ea821-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="ea821-218">Dies ist ein bestimmtes Problem enthält Ihre Objektdiagramm Zyklen, da das Serialisierungsprogramm eine Ausnahme auslöst, wenn im Diagramm eine Schleife erkannt.</span><span class="sxs-lookup"><span data-stu-id="ea821-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="ea821-219">Erwägen Sie die folgenden Objektmodellen und den Controller.</span><span class="sxs-lookup"><span data-stu-id="ea821-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="ea821-220">Aufrufen von dieser Aktion wird den Formatierer, dazu führen, dass eine Ausnahme, die in einem Status Code 500 (Interner Serverfehler) Antwort an den Client übersetzt.</span><span class="sxs-lookup"><span data-stu-id="ea821-220">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="ea821-221">Um Objektverweise im JSON-Format zu erhalten, fügen Sie den folgenden Code **Anwendung\_starten** -Methode in der Datei "Global.asax":</span><span class="sxs-lookup"><span data-stu-id="ea821-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="ea821-222">Jetzt wird die Controlleraktion JSON zurück, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="ea821-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="ea821-223">Beachten Sie, die das Serialisierungsprogramm fügt eine &quot;$id&quot; Eigenschaft, um beide Objekte.</span><span class="sxs-lookup"><span data-stu-id="ea821-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="ea821-224">Darüber hinaus erkannt wird, dass die Employee.Department-Eigenschaft eine Schleife erstellt, sodass den Wert durch einen Objektverweis ersetzt: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="ea821-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="ea821-225">Objektverweise sind nicht in JSON-standard.</span><span class="sxs-lookup"><span data-stu-id="ea821-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="ea821-226">Beachten Sie, ob die Clients die Ergebnisse analysieren können, werden, bevor Sie mit dieser Funktion können.</span><span class="sxs-lookup"><span data-stu-id="ea821-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="ea821-227">Es möglicherweise besser einfach, Zyklen aus dem Diagramm zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="ea821-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="ea821-228">In diesem Beispiel ist die Verknüpfung aus der Mitarbeiter zur Abteilung z. B. wirklich nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ea821-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="ea821-229">Um Objektverweise in XML zu beizubehalten, müssen Sie zwei Optionen zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="ea821-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="ea821-230">Die einfachere Option besteht darin hinzufügen `[DataContract(IsReference=true)]` der Model-Klasse.</span><span class="sxs-lookup"><span data-stu-id="ea821-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="ea821-231">Die *IsReference* Parameter kann Objektverweise.</span><span class="sxs-lookup"><span data-stu-id="ea821-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="ea821-232">Beachten Sie, dass **DataContract** Serialisierung teilnehmen, macht, sodass Sie auch hinzufügen müssen **DataMember** -Attribute auf die Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="ea821-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="ea821-233">Jetzt XML-Code ähnelt das Formatierungsprogramm erzeugt, die folgenden:</span><span class="sxs-lookup"><span data-stu-id="ea821-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="ea821-234">Wenn Sie Attribute in Ihrer Modellklasse vermeiden möchten, besteht eine weitere Möglichkeit: Erstellen Sie eine neue typspezifische **DataContractSerializer** -Instanz, und legen Sie *PreserveObjectReferences* zu **"true"** im Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="ea821-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="ea821-235">Legen Sie dann diese Instanz als Serialisierungsprogramm pro Typ, für die XML-medientypformatierer.</span><span class="sxs-lookup"><span data-stu-id="ea821-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="ea821-236">Der folgende Code zeigt, wie Sie dies durchführen:</span><span class="sxs-lookup"><span data-stu-id="ea821-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="ea821-237">Testen die Serialisierung von Objekten</span><span class="sxs-lookup"><span data-stu-id="ea821-237">Testing Object Serialization</span></span>

<span data-ttu-id="ea821-238">Beim Entwerfen Ihrer Web-API ist es hilfreich, testen, wie Ihre Datenobjekte serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="ea821-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="ea821-239">Dies ist möglich, ohne einen Controller oder eine Controlleraktion aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="ea821-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
