---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Modell Validierung in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: Übersicht über die Modell Validierung in ASP.net-Web-API für ASP.NET 4. x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 531a66b7ab642bd012663517640f2766f1917f25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448929"
---
# <a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="48d6d-103">Modell Validierung in ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="48d6d-103">Model Validation in ASP.NET Web API</span></span>

<span data-ttu-id="48d6d-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="48d6d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="48d6d-105">In diesem Artikel wird gezeigt, wie Sie Ihre Modelle mit Anmerkungen versehen, die Anmerkungen für die Datenvalidierung verwenden und Validierungs Fehler in Ihrer Web-API behandeln.</span><span class="sxs-lookup"><span data-stu-id="48d6d-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span> <span data-ttu-id="48d6d-106">Wenn ein Client Daten an Ihre Web-API sendet, empfiehlt es sich, die Daten vor der Verarbeitung zu validieren.</span><span class="sxs-lookup"><span data-stu-id="48d6d-106">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> 

## <a name="data-annotations"></a><span data-ttu-id="48d6d-107">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="48d6d-107">Data Annotations</span></span>

<span data-ttu-id="48d6d-108">In ASP.net-Web-API können Sie Attribute aus dem [System. ComponentModel. DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) -Namespace verwenden, um Validierungsregeln für Eigenschaften in Ihrem Modell festzulegen.</span><span class="sxs-lookup"><span data-stu-id="48d6d-108">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="48d6d-109">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="48d6d-109">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="48d6d-110">Wenn Sie die Modell Validierung in ASP.NET MVC verwendet haben, sollte dies Ihnen bekannt vorkommen.</span><span class="sxs-lookup"><span data-stu-id="48d6d-110">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="48d6d-111">Das **erforderliche** Attribut besagt, dass die `Name`-Eigenschaft nicht NULL sein darf.</span><span class="sxs-lookup"><span data-stu-id="48d6d-111">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="48d6d-112">Das **Range** -Attribut gibt an, dass `Weight` zwischen 0 und 999 liegen muss.</span><span class="sxs-lookup"><span data-stu-id="48d6d-112">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="48d6d-113">Angenommen, ein Client sendet eine Post-Anforderung mit der folgenden JSON-Darstellung:</span><span class="sxs-lookup"><span data-stu-id="48d6d-113">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="48d6d-114">Sie können sehen, dass der Client nicht die `Name`-Eigenschaft enthielt, die als erforderlich markiert ist.</span><span class="sxs-lookup"><span data-stu-id="48d6d-114">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="48d6d-115">Wenn die Web-API den JSON-Code in eine `Product` Instanz konvertiert, überprüft er die `Product` anhand der Validierungs Attribute.</span><span class="sxs-lookup"><span data-stu-id="48d6d-115">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="48d6d-116">In ihrer Controller Aktion können Sie überprüfen, ob das Modell gültig ist:</span><span class="sxs-lookup"><span data-stu-id="48d6d-116">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="48d6d-117">Die Modell Validierung garantiert nicht, dass Client Daten sicher sind.</span><span class="sxs-lookup"><span data-stu-id="48d6d-117">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="48d6d-118">In anderen Ebenen der Anwendung ist möglicherweise eine zusätzliche Überprüfung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="48d6d-118">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="48d6d-119">(Beispielsweise kann die Datenschicht Foreign Key-Einschränkungen erzwingen.) Im Tutorial [Verwenden der Web-API mit Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) werden einige dieser Probleme untersucht.</span><span class="sxs-lookup"><span data-stu-id="48d6d-119">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="48d6d-120">**"Bei**der Bereitstellung": eine unter Veröffentlichung erfolgt, wenn der Client einige Eigenschaften verlässt.</span><span class="sxs-lookup"><span data-stu-id="48d6d-120">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="48d6d-121">Nehmen wir beispielsweise an, der Client sendet Folgendes:</span><span class="sxs-lookup"><span data-stu-id="48d6d-121">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="48d6d-122">Hier hat der Client keine Werte für `Price` oder `Weight`angegeben.</span><span class="sxs-lookup"><span data-stu-id="48d6d-122">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="48d6d-123">Der JSON-Formatierer weist den fehlenden Eigenschaften den Standardwert 0 (null) zu.</span><span class="sxs-lookup"><span data-stu-id="48d6d-123">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="48d6d-124">Der Modell Zustand ist gültig, da NULL ein gültiger Wert für diese Eigenschaften ist.</span><span class="sxs-lookup"><span data-stu-id="48d6d-124">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="48d6d-125">Ob dies ein Problem ist, hängt von Ihrem Szenario ab.</span><span class="sxs-lookup"><span data-stu-id="48d6d-125">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="48d6d-126">Beispielsweise können Sie bei einem Aktualisierungs Vorgang zwischen "0" und "nicht festgelegt" unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="48d6d-126">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="48d6d-127">Legen Sie die Eigenschaft auf NULL fest, und legen Sie das **erforderliche** Attribut fest, um zu erzwingen, dass Clients einen Wert festlegen:</span><span class="sxs-lookup"><span data-stu-id="48d6d-127">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="48d6d-128">**"Überschreiben"** : ein Client kann auch *mehr* Daten als erwartet senden.</span><span class="sxs-lookup"><span data-stu-id="48d6d-128">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="48d6d-129">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="48d6d-129">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="48d6d-130">Hier enthält die JSON-Eigenschaft eine Eigenschaft ("Color"), die im `Product` Modell nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="48d6d-130">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="48d6d-131">In diesem Fall ignoriert der JSON-Formatierer diesen Wert einfach.</span><span class="sxs-lookup"><span data-stu-id="48d6d-131">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="48d6d-132">(Das XML-Formatierer führt dasselbe aus.) Eine Übertragung verursacht Probleme, wenn Ihr Modell über Eigenschaften verfügt, die Sie als schreibgeschützt feststellen möchten.</span><span class="sxs-lookup"><span data-stu-id="48d6d-132">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="48d6d-133">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="48d6d-133">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="48d6d-134">Sie möchten nicht, dass Benutzer die `IsAdmin`-Eigenschaft aktualisieren und sich nicht auf Administratoren erhöhen!</span><span class="sxs-lookup"><span data-stu-id="48d6d-134">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="48d6d-135">Die sicherste Strategie besteht darin, eine Modell Klasse zu verwenden, die genau mit dem übereinstimmt, was der Client senden darf:</span><span class="sxs-lookup"><span data-stu-id="48d6d-135">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="48d6d-136">Der Blogbeitrag von Brad Wilson (Eingabe Überprüfung und[Modell Überprüfung in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)) enthält eine gute Erörterung der unter-und über-und über-und Übertragung.</span><span class="sxs-lookup"><span data-stu-id="48d6d-136">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="48d6d-137">Obwohl es sich bei dem Beitrag um ASP.NET MVC 2 handelt, sind die Probleme weiterhin für die Web-API relevant.</span><span class="sxs-lookup"><span data-stu-id="48d6d-137">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>

## <a name="handling-validation-errors"></a><span data-ttu-id="48d6d-138">Behandeln von Validierungs Fehlern</span><span class="sxs-lookup"><span data-stu-id="48d6d-138">Handling Validation Errors</span></span>

<span data-ttu-id="48d6d-139">Die Web-API gibt beim Fehlschlagen der Validierung nicht automatisch einen Fehler an den Client zurück.</span><span class="sxs-lookup"><span data-stu-id="48d6d-139">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="48d6d-140">Es liegt an der Controller Aktion, den Modell Status zu überprüfen und entsprechend zu reagieren.</span><span class="sxs-lookup"><span data-stu-id="48d6d-140">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="48d6d-141">Sie können auch einen Aktionsfilter erstellen, um den Modell Status zu überprüfen, bevor die Controller Aktion aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="48d6d-141">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="48d6d-142">Der folgende Code zeigt ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="48d6d-142">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="48d6d-143">Wenn die Modell Validierung fehlschlägt, gibt dieser Filter eine HTTP-Antwort zurück, die die Validierungs Fehler enthält.</span><span class="sxs-lookup"><span data-stu-id="48d6d-143">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="48d6d-144">In diesem Fall wird die Controller Aktion nicht aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="48d6d-144">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="48d6d-145">Wenn Sie diesen Filter auf alle Web-API-Controller anwenden möchten, fügen Sie der **httpconfiguration. Filters** -Auflistung während der Konfiguration eine Instanz des Filters hinzu:</span><span class="sxs-lookup"><span data-stu-id="48d6d-145">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="48d6d-146">Eine weitere Möglichkeit besteht darin, den Filter als Attribut für einzelne Controller oder Controller Aktionen festzulegen:</span><span class="sxs-lookup"><span data-stu-id="48d6d-146">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
