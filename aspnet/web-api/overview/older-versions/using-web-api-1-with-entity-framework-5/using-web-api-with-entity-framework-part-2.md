---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Teil 2: Erstellen der Domänen Modelle | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c5ed1bdb4b390c94907b14e208231f16ad42d96
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600362"
---
# <a name="part-2-creating-the-domain-models"></a><span data-ttu-id="ab5cc-102">Teil 2: Erstellen der Domänen Modelle</span><span class="sxs-lookup"><span data-stu-id="ab5cc-102">Part 2: Creating the Domain Models</span></span>

<span data-ttu-id="ab5cc-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ab5cc-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ab5cc-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="ab5cc-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="ab5cc-105">Hinzufügen von Modellen</span><span class="sxs-lookup"><span data-stu-id="ab5cc-105">Add Models</span></span>

<span data-ttu-id="ab5cc-106">Es gibt drei Möglichkeiten, um Entity Framework zu umgehen:</span><span class="sxs-lookup"><span data-stu-id="ab5cc-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="ab5cc-107">Database-First: Sie beginnen mit einer Datenbank, und Entity Framework generiert den Code.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="ab5cc-108">Model-First: Sie beginnen mit einem visuellen Modell, und Entity Framework generiert sowohl die Datenbank als auch den Code.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="ab5cc-109">Code First: Sie beginnen mit Code, und Entity Framework generiert die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="ab5cc-110">Wir verwenden den Code First-Ansatz. daher definieren wir zunächst unsere Domänen Objekte als POCOS (Plain-Old CLR Objects).</span><span class="sxs-lookup"><span data-stu-id="ab5cc-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="ab5cc-111">Mit dem Code First-Ansatz benötigen Domänen Objekte keinen zusätzlichen Code zur Unterstützung der Datenbankebene, z. b. Transaktionen oder Persistenz.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="ab5cc-112">(Insbesondere müssen Sie nicht von der [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) -Klasse erben.) Sie können weiterhin Daten Anmerkungen verwenden, um zu steuern, wie Entity Framework das Datenbankschema erstellt.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="ab5cc-113">Da POCOS keine zusätzlichen Eigenschaften enthalten, die den [Daten Bank Status](https://msdn.microsoft.com/library/system.data.entitystate.aspx)beschreiben, können Sie problemlos in JSON oder XML serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="ab5cc-114">Dies bedeutet jedoch nicht, dass Sie Ihre Entity Framework Modelle immer direkt für Clients verfügbar machen sollten, wie später in diesem Tutorial zu sehen ist.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="ab5cc-115">Wir erstellen die folgenden POCOS:</span><span class="sxs-lookup"><span data-stu-id="ab5cc-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="ab5cc-116">Product</span><span class="sxs-lookup"><span data-stu-id="ab5cc-116">Product</span></span>
- <span data-ttu-id="ab5cc-117">Order</span><span class="sxs-lookup"><span data-stu-id="ab5cc-117">Order</span></span>
- <span data-ttu-id="ab5cc-118">Order Detail</span><span class="sxs-lookup"><span data-stu-id="ab5cc-118">OrderDetail</span></span>

<span data-ttu-id="ab5cc-119">Um jede Klasse zu erstellen, klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Modelle.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="ab5cc-120">Klicken Sie im Kontextmenü auf **Hinzufügen** , und wählen Sie dann **Klasse aus.**</span><span class="sxs-lookup"><span data-stu-id="ab5cc-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="ab5cc-121">Fügen Sie mit der folgenden Implementierung eine `Product` Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="ab5cc-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="ab5cc-122">Gemäß der Konvention verwendet Entity Framework die `Id`-Eigenschaft als Primärschlüssel und ordnet Sie einer Identitäts Spalte in der Datenbanktabelle zu.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="ab5cc-123">Wenn Sie eine neue `Product` Instanz erstellen, wird kein Wert für `Id`festgelegt, da die Datenbank den Wert generiert.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="ab5cc-124">Das **Gerüst Column** -Attribut weist ASP.NET MVC an, die `Id`-Eigenschaft zu überspringen, wenn ein Editor-Formular erzeugt wird.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="ab5cc-125">Das **erforderliche** -Attribut wird verwendet, um das Modell zu validieren.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="ab5cc-126">Gibt an, dass die `Name`-Eigenschaft eine nicht leere Zeichenfolge sein muss.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="ab5cc-127">Fügen Sie die `Order`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="ab5cc-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="ab5cc-128">Fügen Sie die `OrderDetail`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="ab5cc-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="ab5cc-129">Fremdschlüssel Beziehungen</span><span class="sxs-lookup"><span data-stu-id="ab5cc-129">Foreign Key Relations</span></span>

<span data-ttu-id="ab5cc-130">Eine Bestellung enthält viele Bestelldetails, und jedes Bestell Detail bezieht sich auf ein einzelnes Produkt.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="ab5cc-131">Um diese Beziehungen darzustellen, definiert die `OrderDetail`-Klasse Eigenschaften mit dem Namen `OrderId` und `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="ab5cc-132">Entity Framework wird ableiten, dass diese Eigenschaften Fremdschlüssel darstellen, und fügt der Datenbank Foreign Key-Einschränkungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="ab5cc-133">Die Klassen `Order` und `OrderDetail` enthalten außerdem Navigations Eigenschaften, die Verweise auf die verknüpften Objekte enthalten.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="ab5cc-134">Wenn Sie eine Bestellung haben, können Sie zu den Produkten in der Bestellung navigieren, indem Sie den Navigations Eigenschaften folgen.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="ab5cc-135">Kompilieren Sie das Projekt jetzt.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-135">Compile the project now.</span></span> <span data-ttu-id="ab5cc-136">Entity Framework verwendet Reflektion, um die Eigenschaften der Modelle zu ermitteln. Daher ist eine kompilierte Assembly erforderlich, um das Datenbankschema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="ab5cc-137">Konfigurieren der Medientyp-Formatierer</span><span class="sxs-lookup"><span data-stu-id="ab5cc-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="ab5cc-138">Ein [Medientyp-Formatierer](../../formats-and-model-binding/media-formatters.md) ist ein Objekt, das die Daten serialisiert, wenn die Web-API den HTTP-Antworttext schreibt.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="ab5cc-139">Die integrierten Formatierer unterstützen die JSON-und XML-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="ab5cc-140">Standardmäßig serialisieren beide Formatierer alle Objekte nach Wert.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="ab5cc-141">Bei der Serialisierung nach Wert wird ein Problem erstellt, wenn ein Objekt Diagramm zirkuläre Verweise enthält.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="ab5cc-142">Das ist genau der Fall bei den Klassen `Order` und `OrderDetail`, da jeder einen Verweis auf den anderen enthält.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="ab5cc-143">Der Formatierer befolgt die Verweise, schreibt jedes Objekt nach Wert und geht in Kreise.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="ab5cc-144">Daher müssen wir das Standardverhalten ändern.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="ab5cc-145">Erweitern Sie in Projektmappen-Explorer den Ordner App\_Start, und öffnen Sie die Datei mit dem Namen WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="ab5cc-146">Fügen Sie der `WebApiConfig`-Klasse folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="ab5cc-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="ab5cc-147">Mit diesem Code wird der JSON-Formatierer für die Beibehaltung von Objekt verweisen festgelegt, und der XML-Formatierer wird vollständig aus der Pipeline entfernt.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="ab5cc-148">(Sie können den XML-Formatierer so konfigurieren, dass er Objekt Verweise beibehält, aber es ist ein wenig mehr Arbeit, und wir benötigen nur JSON für diese Anwendung.</span><span class="sxs-lookup"><span data-stu-id="ab5cc-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="ab5cc-149">Weitere Informationen finden Sie unter [Behandeln von zirkulären Objekt verweisen](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="ab5cc-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ab5cc-150">[Zurück](using-web-api-with-entity-framework-part-1.md)
> [Weiter](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="ab5cc-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
