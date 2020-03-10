---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON-und XML-Serialisierung in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: Beschreibt die JSON-und XML-Formatierer in ASP.net-Web-API für ASP.NET 4. x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 00fa07f00eabf7e6c883c5e9ceaf9a38a8f49605
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449127"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="f1352-103">JSON-und XML-Serialisierung in ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="f1352-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="f1352-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f1352-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f1352-105">In diesem Artikel werden die JSON-und XML-Formatierer in ASP.net-Web-API beschrieben.</span><span class="sxs-lookup"><span data-stu-id="f1352-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="f1352-106">In ASP.net-Web-API ist ein *Medientyp-Formatierer* ein Objekt, das Folgendes kann:</span><span class="sxs-lookup"><span data-stu-id="f1352-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="f1352-107">Lesen von CLR-Objekten aus einem HTTP-Nachrichtentext</span><span class="sxs-lookup"><span data-stu-id="f1352-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="f1352-108">Schreiben von CLR-Objekten in einen HTTP-Nachrichtentext</span><span class="sxs-lookup"><span data-stu-id="f1352-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="f1352-109">Die Web-API stellt Medientyp-Formatierer für JSON und XML bereit.</span><span class="sxs-lookup"><span data-stu-id="f1352-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="f1352-110">Das Framework fügt diese Formatierer standardmäßig in die Pipeline ein.</span><span class="sxs-lookup"><span data-stu-id="f1352-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="f1352-111">Clients können entweder JSON oder XML im Accept-Header der HTTP-Anforderung anfordern.</span><span class="sxs-lookup"><span data-stu-id="f1352-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="f1352-112">Inhalt</span><span class="sxs-lookup"><span data-stu-id="f1352-112">Contents</span></span>

- [<span data-ttu-id="f1352-113">JSON Media-Type-Formatierer</span><span class="sxs-lookup"><span data-stu-id="f1352-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="f1352-114">Schreibgeschützte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="f1352-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="f1352-115">Voraus</span><span class="sxs-lookup"><span data-stu-id="f1352-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="f1352-116">Einzug</span><span class="sxs-lookup"><span data-stu-id="f1352-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="f1352-117">Kamel Schreibweise</span><span class="sxs-lookup"><span data-stu-id="f1352-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="f1352-118">Anonyme und schwach typisierte Objekte</span><span class="sxs-lookup"><span data-stu-id="f1352-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="f1352-119">XML-Medientyp Formatierer</span><span class="sxs-lookup"><span data-stu-id="f1352-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="f1352-120">Schreibgeschützte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="f1352-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="f1352-121">Voraus</span><span class="sxs-lookup"><span data-stu-id="f1352-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="f1352-122">Einzug</span><span class="sxs-lookup"><span data-stu-id="f1352-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="f1352-123">Festlegen von XML-Serialisierern pro Typ</span><span class="sxs-lookup"><span data-stu-id="f1352-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="f1352-124">Entfernen der JSON-oder XML-Formatierer</span><span class="sxs-lookup"><span data-stu-id="f1352-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="f1352-125">Behandeln von zirkulären Objekt verweisen</span><span class="sxs-lookup"><span data-stu-id="f1352-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="f1352-126">Testen der Objektserialisierung</span><span class="sxs-lookup"><span data-stu-id="f1352-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="f1352-127">JSON Media-Type-Formatierer</span><span class="sxs-lookup"><span data-stu-id="f1352-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="f1352-128">Die JSON-Formatierung wird von der **jsonmediatypeer Formatter** -Klasse bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="f1352-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="f1352-129">Standardmäßig verwendet **jsonmediatypeer Formatter** die [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) -Bibliothek, um die Serialisierung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="f1352-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="f1352-130">JSON.net ist ein Open Source-Projekt von Drittanbietern.</span><span class="sxs-lookup"><span data-stu-id="f1352-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="f1352-131">Wenn Sie möchten, können Sie die **jsonmediatypformatter** -Klasse so konfigurieren, dass **DataContractJsonSerializer** anstelle von JSON.NET verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="f1352-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="f1352-132">Legen Sie zu diesem Zweck die **usedatacontractjsonserializer** -Eigenschaft auf " **true**" fest:</span><span class="sxs-lookup"><span data-stu-id="f1352-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="f1352-133">JSON-Serialisierung</span><span class="sxs-lookup"><span data-stu-id="f1352-133">JSON Serialization</span></span>

<span data-ttu-id="f1352-134">In diesem Abschnitt werden einige spezifische Verhalten des JSON-Formatierers mithilfe des standardmäßigen [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) -Serialisierungsprogramms beschrieben.</span><span class="sxs-lookup"><span data-stu-id="f1352-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="f1352-135">Dies soll nicht als umfassende Dokumentation der JSON.NET-Bibliothek dienen. Weitere Informationen finden Sie in der [JSON.NET-Dokumentation](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="f1352-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="f1352-136">Was wird serialisiert?</span><span class="sxs-lookup"><span data-stu-id="f1352-136">What Gets Serialized?</span></span>

<span data-ttu-id="f1352-137">Standardmäßig sind alle öffentlichen Eigenschaften und Felder im serialisierten JSON-Code enthalten.</span><span class="sxs-lookup"><span data-stu-id="f1352-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="f1352-138">Um eine Eigenschaft oder ein Feld auszulassen, versehen Sie Sie mit dem **jsonignore** -Attribut.</span><span class="sxs-lookup"><span data-stu-id="f1352-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="f1352-139">Wenn Sie einen &quot;Opt-in-&quot; Ansatz bevorzugen, müssen Sie die-Klasse mit dem **DataContract** -Attribut ergänzen.</span><span class="sxs-lookup"><span data-stu-id="f1352-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="f1352-140">Wenn dieses Attribut vorhanden ist, werden Member ignoriert, es sei denn, Sie haben den **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="f1352-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="f1352-141">Sie können auch **DataMember** verwenden, um private Member zu serialisieren.</span><span class="sxs-lookup"><span data-stu-id="f1352-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="f1352-142">Schreibgeschützte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="f1352-142">Read-Only Properties</span></span>

<span data-ttu-id="f1352-143">Schreibgeschützte Eigenschaften werden standardmäßig serialisiert.</span><span class="sxs-lookup"><span data-stu-id="f1352-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="f1352-144">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="f1352-144">Dates</span></span>

<span data-ttu-id="f1352-145">Standardmäßig schreibt JSON.net Datumsangaben im [ISO 8601](http://www.w3.org/TR/NOTE-datetime) -Format.</span><span class="sxs-lookup"><span data-stu-id="f1352-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="f1352-146">Datumsangaben in UTC (koordinierte Weltzeit) werden mit dem Suffix "Z" geschrieben.</span><span class="sxs-lookup"><span data-stu-id="f1352-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="f1352-147">Datumsangaben in der lokalen Zeit umfassen einen Zeit Zonen Offset.</span><span class="sxs-lookup"><span data-stu-id="f1352-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="f1352-148">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f1352-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="f1352-149">Standardmäßig behält JSON.net die Zeitzone bei.</span><span class="sxs-lookup"><span data-stu-id="f1352-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="f1352-150">Sie können dies überschreiben, indem Sie die datetimezonehanding-Eigenschaft festlegen:</span><span class="sxs-lookup"><span data-stu-id="f1352-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="f1352-151">Wenn Sie lieber das [Microsoft JSON-Datumsformat](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) anstelle von ISO 8601 verwenden möchten, legen Sie die **dateformathandge** -Eigenschaft in den serialisierungsprotokeneinstellungen fest:</span><span class="sxs-lookup"><span data-stu-id="f1352-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="f1352-152">Einzug</span><span class="sxs-lookup"><span data-stu-id="f1352-152">Indenting</span></span>

<span data-ttu-id="f1352-153">Legen Sie zum Schreiben von eingerückt-JSON die **Formatierungs** Einstellung auf " **Formatierung. eingezogen**" fest:</span><span class="sxs-lookup"><span data-stu-id="f1352-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="f1352-154">Kamel Schreibweise</span><span class="sxs-lookup"><span data-stu-id="f1352-154">Camel Casing</span></span>

<span data-ttu-id="f1352-155">Wenn sie JSON-Eigenschaftsnamen mit Kamel Schreibweise schreiben möchten, ohne das Datenmodell zu ändern, legen Sie den " **camelcasepropertynamesverhütungs tresolver** " für das Serialisierungsprogramm fest:</span><span class="sxs-lookup"><span data-stu-id="f1352-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="f1352-156">Anonyme und schwach typisierte Objekte</span><span class="sxs-lookup"><span data-stu-id="f1352-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="f1352-157">Eine Aktionsmethode kann ein anonymes Objekt zurückgeben und in JSON Serialisieren.</span><span class="sxs-lookup"><span data-stu-id="f1352-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="f1352-158">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f1352-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="f1352-159">Der Text der Antwortnachricht enthält den folgenden JSON-Code:</span><span class="sxs-lookup"><span data-stu-id="f1352-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="f1352-160">Wenn Ihre Web-API lose strukturierte JSON-Objekte von Clients empfängt, können Sie den Anforderungs Text in den Typ " **newtonsoft. JSON. Linq. jobject** " deserialisieren.</span><span class="sxs-lookup"><span data-stu-id="f1352-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="f1352-161">Es ist jedoch in der Regel besser, stark typisierte Datenobjekte zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="f1352-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="f1352-162">Dann müssen Sie die Daten nicht selbst analysieren, und Sie erhalten die Vorteile der Modell Validierung.</span><span class="sxs-lookup"><span data-stu-id="f1352-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="f1352-163">Der XML-Serialisierer unterstützt keine anonymen Typen oder **jobject** -Instanzen.</span><span class="sxs-lookup"><span data-stu-id="f1352-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="f1352-164">Wenn Sie diese Features für Ihre JSON-Daten verwenden, sollten Sie den XML-Formatierer aus der Pipeline entfernen, wie weiter unten in diesem Artikel beschrieben.</span><span class="sxs-lookup"><span data-stu-id="f1352-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="f1352-165">XML-Medientyp Formatierer</span><span class="sxs-lookup"><span data-stu-id="f1352-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="f1352-166">Die XML-Formatierung wird von der **xmlmediatypeer Formatter** -Klasse bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="f1352-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="f1352-167">Standardmäßig verwendet **xmlmediatypformatter** die **DataContractSerializer** -Klasse, um die Serialisierung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="f1352-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="f1352-168">Wenn Sie möchten, können Sie den **xmlmediatypformatter** so konfigurieren, dass er anstelle von **DataContractSerializer**den **XmlSerializer** verwendet.</span><span class="sxs-lookup"><span data-stu-id="f1352-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="f1352-169">Legen Sie zu diesem Zweck die **usexmlserializer** -Eigenschaft auf **true**fest:</span><span class="sxs-lookup"><span data-stu-id="f1352-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="f1352-170">Die **XmlSerializer** -Klasse unterstützt einen engeren Satz von Typen als **DataContractSerializer**, bietet jedoch mehr Kontrolle über den resultierenden XML-Code.</span><span class="sxs-lookup"><span data-stu-id="f1352-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="f1352-171">Verwenden Sie ggf. **XmlSerializer** , wenn Sie ein vorhandenes XML-Schema zuordnen müssen.</span><span class="sxs-lookup"><span data-stu-id="f1352-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="f1352-172">XML-Serialisierung</span><span class="sxs-lookup"><span data-stu-id="f1352-172">XML Serialization</span></span>

<span data-ttu-id="f1352-173">In diesem Abschnitt werden einige spezifische Verhalten des XML-Formatierers mithilfe des **standarddatacontractserializers**beschrieben.</span><span class="sxs-lookup"><span data-stu-id="f1352-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="f1352-174">Standardmäßig verhält sich DataContractSerializer wie folgt:</span><span class="sxs-lookup"><span data-stu-id="f1352-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="f1352-175">Alle öffentlichen Lese-/Schreibeigenschaften und-Felder werden serialisiert.</span><span class="sxs-lookup"><span data-stu-id="f1352-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="f1352-176">Um eine Eigenschaft oder ein Feld auszulassen, versehen Sie Sie mit dem **ignoredatamember** -Attribut.</span><span class="sxs-lookup"><span data-stu-id="f1352-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="f1352-177">Private und geschützte Member werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="f1352-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="f1352-178">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="f1352-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="f1352-179">(Der Inhalt einer schreibgeschützten Auflistungs Eigenschaft wird jedoch serialisiert.)</span><span class="sxs-lookup"><span data-stu-id="f1352-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="f1352-180">Klassen-und Elementnamen werden in der XML-Datei genau so geschrieben, wie Sie in der Klassen Deklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="f1352-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="f1352-181">Ein XML-Standard Namespace wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="f1352-181">A default XML namespace is used.</span></span>

<span data-ttu-id="f1352-182">Wenn Sie mehr Kontrolle über die Serialisierung benötigen, können Sie die Klasse mit dem **DataContract** -Attribut ergänzen.</span><span class="sxs-lookup"><span data-stu-id="f1352-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="f1352-183">Wenn dieses Attribut vorhanden ist, wird die-Klasse wie folgt serialisiert:</span><span class="sxs-lookup"><span data-stu-id="f1352-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="f1352-184">&quot;&quot; Ansatz: Eigenschaften und Felder werden standardmäßig nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="f1352-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="f1352-185">Um eine Eigenschaft oder ein Feld zu serialisieren, müssen Sie es mit dem **DataMember** -Attribut ergänzen.</span><span class="sxs-lookup"><span data-stu-id="f1352-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="f1352-186">Um einen privaten oder geschützten Member zu serialisieren, versehen Sie ihn mit dem **DataMember** -Attribut.</span><span class="sxs-lookup"><span data-stu-id="f1352-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="f1352-187">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="f1352-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="f1352-188">Legen Sie den *Name* -Parameter im **DataContract** -Attribut fest, um zu ändern, wie der Klassenname im XML-Code angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="f1352-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="f1352-189">Legen Sie den *Name* -Parameter im **DataMember** -Attribut fest, um zu ändern, wie ein Elementname im XML-Code angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="f1352-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="f1352-190">Um den XML-Namespace zu ändern, legen Sie den *Namespace* -Parameter in der **DataContract** -Klasse fest.</span><span class="sxs-lookup"><span data-stu-id="f1352-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="f1352-191">Schreibgeschützte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="f1352-191">Read-Only Properties</span></span>

<span data-ttu-id="f1352-192">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="f1352-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="f1352-193">Wenn eine schreibgeschützte Eigenschaft ein privates Feld unterstützt, können Sie das private Feld mit dem **DataMember** -Attribut markieren.</span><span class="sxs-lookup"><span data-stu-id="f1352-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="f1352-194">Dieser Ansatz erfordert das **DataContract** -Attribut für die-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f1352-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="f1352-195">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="f1352-195">Dates</span></span>

<span data-ttu-id="f1352-196">Datumsangaben werden im ISO 8601-Format geschrieben.</span><span class="sxs-lookup"><span data-stu-id="f1352-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="f1352-197">Beispielsweise &quot;2012-05-23t20:21:37.9116538 z&quot;.</span><span class="sxs-lookup"><span data-stu-id="f1352-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="f1352-198">Einzug</span><span class="sxs-lookup"><span data-stu-id="f1352-198">Indenting</span></span>

<span data-ttu-id="f1352-199">Legen Sie für die **Indent** -Eigenschaft den Wert " **true**" fest</span><span class="sxs-lookup"><span data-stu-id="f1352-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="f1352-200">Festlegen von XML-Serialisierern pro Typ</span><span class="sxs-lookup"><span data-stu-id="f1352-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="f1352-201">Sie können verschiedene XML-Serialisierungstypen für verschiedene CLR-Typen festlegen.</span><span class="sxs-lookup"><span data-stu-id="f1352-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="f1352-202">Beispielsweise könnten Sie über ein bestimmtes Datenobjekt verfügen, das **XmlSerializer** für die Abwärtskompatibilität benötigt.</span><span class="sxs-lookup"><span data-stu-id="f1352-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="f1352-203">Sie können **XmlSerializer** für dieses Objekt verwenden und die Verwendung von **DataContractSerializer** für andere Typen fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="f1352-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="f1352-204">Um ein XML-Serialisierungsprogramm für einen bestimmten Typ festzulegen, nennen Sie **setserializer**.</span><span class="sxs-lookup"><span data-stu-id="f1352-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="f1352-205">Sie können ein **XmlSerializer** -Objekt oder ein beliebiges Objekt angeben, das von **XmlObjectSerializer**abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="f1352-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="f1352-206">Entfernen der JSON-oder XML-Formatierer</span><span class="sxs-lookup"><span data-stu-id="f1352-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="f1352-207">Sie können das JSON-Formatierer oder den XML-Formatierer aus der Liste der Formatierer entfernen, wenn Sie Sie nicht verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="f1352-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="f1352-208">Die Hauptgründe hierfür sind:</span><span class="sxs-lookup"><span data-stu-id="f1352-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="f1352-209">Um Ihre Web-API-Antworten auf einen bestimmten Medientyp zu beschränken.</span><span class="sxs-lookup"><span data-stu-id="f1352-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="f1352-210">Beispielsweise können Sie sich entscheiden, nur JSON-Antworten zu unterstützen und das XML-Formatierer zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="f1352-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="f1352-211">, Um das Standardformatierer durch einen benutzerdefinierten Formatierer zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="f1352-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="f1352-212">Beispielsweise können Sie den JSON-Formatierer durch ihre eigene benutzerdefinierte Implementierung eines JSON-Formatierers ersetzen.</span><span class="sxs-lookup"><span data-stu-id="f1352-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="f1352-213">Der folgende Code zeigt, wie die Standard Formatierer entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="f1352-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="f1352-214">Nennen Sie dies in der **Anwendung\_Start** -Methode, die in "Global. asax" definiert ist.</span><span class="sxs-lookup"><span data-stu-id="f1352-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="f1352-215">Behandeln von zirkulären Objekt verweisen</span><span class="sxs-lookup"><span data-stu-id="f1352-215">Handling Circular Object References</span></span>

<span data-ttu-id="f1352-216">Standardmäßig schreiben die JSON-und XML-Formatierer alle-Objekte als-Werte.</span><span class="sxs-lookup"><span data-stu-id="f1352-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="f1352-217">Wenn zwei Eigenschaften auf das gleiche Objekt verweisen oder das gleiche Objekt zweimal in einer Auflistung angezeigt wird, serialisiert das formatierungprogramm das Objekt zweimal.</span><span class="sxs-lookup"><span data-stu-id="f1352-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="f1352-218">Dies ist ein besonderes Problem, wenn das Objekt Diagramm Zyklen enthält, da das Serialisierungsprogramm eine Ausnahme auslöst, wenn eine Schleife im Diagramm erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="f1352-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="f1352-219">Beachten Sie die folgenden Objekt Modelle und den Controller.</span><span class="sxs-lookup"><span data-stu-id="f1352-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="f1352-220">Wenn diese Aktion aufgerufen wird, löst der Formatierer eine Ausnahme aus, die in die Antwortstatus Code 500 (interner Server Fehler) an den Client übersetzt.</span><span class="sxs-lookup"><span data-stu-id="f1352-220">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="f1352-221">Fügen Sie der **Anwendungs\_Start** -Methode in der Datei "Global. asax" den folgenden Code hinzu, um Objekt Verweise in JSON beizubehalten:</span><span class="sxs-lookup"><span data-stu-id="f1352-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="f1352-222">Nun gibt die Controller Aktion JSON zurück, das wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="f1352-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="f1352-223">Beachten Sie, dass das Serialisierungsprogramm beiden Objekten eine &quot;$ID&quot;-Eigenschaft hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="f1352-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="f1352-224">Außerdem wird erkannt, dass die Employee. Department-Eigenschaft eine-Schleife erstellt, sodass der Wert durch einen Objekt Verweis ersetzt wird: {&quot;$Ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="f1352-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="f1352-225">Objekt Verweise sind in JSON nicht standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="f1352-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="f1352-226">Bevor Sie dieses Feature verwenden, sollten Sie berücksichtigen, ob die Ergebnisse von den Clients analysiert werden können.</span><span class="sxs-lookup"><span data-stu-id="f1352-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="f1352-227">Es kann besser sein, nur Zyklen aus dem Diagramm zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="f1352-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="f1352-228">Beispielsweise ist die Verknüpfung von Employee zurück zur Abteilung in diesem Beispiel nicht unbedingt erforderlich.</span><span class="sxs-lookup"><span data-stu-id="f1352-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>

<span data-ttu-id="f1352-229">Um Objekt Verweise in XML beizubehalten, haben Sie zwei Möglichkeiten.</span><span class="sxs-lookup"><span data-stu-id="f1352-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="f1352-230">Die einfachere Option besteht darin, der Modell Klasse `[DataContract(IsReference=true)]` hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="f1352-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="f1352-231">Der *IsReference* -Parameter aktiviert Objekt Verweise.</span><span class="sxs-lookup"><span data-stu-id="f1352-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="f1352-232">Beachten Sie, dass **DataContract** die Serialisierung deaktiviert, sodass Sie den Eigenschaften auch **DataMember** -Attribute hinzufügen müssen:</span><span class="sxs-lookup"><span data-stu-id="f1352-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="f1352-233">Nun erzeugt das Formatierer XML ähnlich wie folgt:</span><span class="sxs-lookup"><span data-stu-id="f1352-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="f1352-234">Wenn Sie Attribute in der Modell Klasse vermeiden möchten, gibt es eine weitere Option: Erstellen Sie eine neue typspezifische **DataContractSerializer** -Instanz, und legen Sie *preserveObjectReferences* im Konstruktor auf **true** fest.</span><span class="sxs-lookup"><span data-stu-id="f1352-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="f1352-235">Legen Sie diese Instanz dann als Serialisierungsprogramm pro Typ für den XML-Medientyp-Formatierer fest.</span><span class="sxs-lookup"><span data-stu-id="f1352-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="f1352-236">Der folgende Code zeigt, wie Sie dies tun:</span><span class="sxs-lookup"><span data-stu-id="f1352-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="f1352-237">Testen der Objektserialisierung</span><span class="sxs-lookup"><span data-stu-id="f1352-237">Testing Object Serialization</span></span>

<span data-ttu-id="f1352-238">Wenn Sie Ihre Web-API entwerfen, ist es hilfreich, zu testen, wie Ihre Datenobjekte serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="f1352-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="f1352-239">Dies ist möglich, ohne einen Controller zu erstellen oder eine Controller Aktion aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="f1352-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
