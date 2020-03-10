---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Validieren mit einer Dienst Ebene (C#) | Microsoft-Dokumentation
author: StephenWalther
description: Erfahren Sie, wie Sie Ihre Validierungs Logik aus Ihren Controller Aktionen und in eine separate Dienst Schicht verschieben. In diesem Tutorial erläutert Stephen Walther, wie Sie...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: da1a1c9cc79a452eb0d7597810e86f7bcf6cd179
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78436605"
---
# <a name="validating-with-a-service-layer-c"></a><span data-ttu-id="8185c-104">Überprüfen mit einer Dienstschicht (C#)</span><span class="sxs-lookup"><span data-stu-id="8185c-104">Validating with a Service Layer (C#)</span></span>

<span data-ttu-id="8185c-105">von [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="8185c-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="8185c-106">Erfahren Sie, wie Sie Ihre Validierungs Logik aus Ihren Controller Aktionen und in eine separate Dienst Schicht verschieben.</span><span class="sxs-lookup"><span data-stu-id="8185c-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="8185c-107">In diesem Tutorial erläutert Stephen Walther, wie Sie eine deutliche Trennung der Belange gewährleisten können, indem Sie die Dienst Ebene von ihrer Controller Schicht isolieren.</span><span class="sxs-lookup"><span data-stu-id="8185c-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>

<span data-ttu-id="8185c-108">Ziel dieses Tutorials ist es, eine Methode zur Durchführung der Validierung in einer ASP.NET MVC-Anwendung zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="8185c-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="8185c-109">In diesem Tutorial erfahren Sie, wie Sie Ihre Validierungs Logik aus ihren Controllern und in eine separate Dienst Schicht verschieben.</span><span class="sxs-lookup"><span data-stu-id="8185c-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="8185c-110">Trennung von Belangen</span><span class="sxs-lookup"><span data-stu-id="8185c-110">Separating Concerns</span></span>

<span data-ttu-id="8185c-111">Wenn Sie eine ASP.NET MVC-Anwendung erstellen, sollten Sie Ihre Daten Bank Logik nicht in den Controller Aktionen platzieren.</span><span class="sxs-lookup"><span data-stu-id="8185c-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="8185c-112">Durch die Kombination von Datenbank und Controller Logik wird die Verwaltung Ihrer Anwendung im Laufe der Zeit erschwert.</span><span class="sxs-lookup"><span data-stu-id="8185c-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="8185c-113">Es wird empfohlen, dass Sie die gesamte Daten Bank Logik in einer separaten Repository-Schicht platzieren.</span><span class="sxs-lookup"><span data-stu-id="8185c-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="8185c-114">Beispielsweise enthält die Auflistung 1 ein einfaches Repository namens productrepository.</span><span class="sxs-lookup"><span data-stu-id="8185c-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="8185c-115">Das produktrepository enthält den gesamten Datenzugriffs Code für die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="8185c-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="8185c-116">Die Auflistung enthält auch die iproductrepository-Schnittstelle, die das produktrepository implementiert.</span><span class="sxs-lookup"><span data-stu-id="8185c-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="8185c-117">**Codebeispiel 1: models\productrepositoriy.cs**</span><span class="sxs-lookup"><span data-stu-id="8185c-117">**Listing 1 -- Models\ProductRepository.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

<span data-ttu-id="8185c-118">Der Controller in der Liste 2 verwendet die Repository-Ebene sowohl in der Index ()-als auch der Create ()-Aktion.</span><span class="sxs-lookup"><span data-stu-id="8185c-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="8185c-119">Beachten Sie, dass dieser Controller keine Daten Bank Logik enthält.</span><span class="sxs-lookup"><span data-stu-id="8185c-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="8185c-120">Durch das Erstellen einer Repository-Ebene können Sie eine saubere Trennung der Belange gewährleisten.</span><span class="sxs-lookup"><span data-stu-id="8185c-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="8185c-121">Controller sind für Anwendungs Fluss-Steuerungslogik zuständig, und das Repository ist für die Datenzugriffs Logik verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="8185c-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="8185c-122">**Codebeispiel 2: controllers\productcontroller.cs**</span><span class="sxs-lookup"><span data-stu-id="8185c-122">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="8185c-123">Erstellen einer Dienst Ebene</span><span class="sxs-lookup"><span data-stu-id="8185c-123">Creating a Service Layer</span></span>

<span data-ttu-id="8185c-124">Daher gehört die Logik der Anwendungs Fluss Steuerung zu einem Controller, und die Datenzugriffs Logik gehört zu einem Repository.</span><span class="sxs-lookup"><span data-stu-id="8185c-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="8185c-125">Wo platzieren Sie die Validierungs Logik in diesem Fall?</span><span class="sxs-lookup"><span data-stu-id="8185c-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="8185c-126">Eine Möglichkeit besteht darin, die Validierungs Logik in eine *Dienst Ebene*zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="8185c-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="8185c-127">Eine Dienst Ebene ist eine zusätzliche Ebene in einer ASP.NET MVC-Anwendung, die die Kommunikation zwischen einem Controller und einer Repository-Schicht vermittelt.</span><span class="sxs-lookup"><span data-stu-id="8185c-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="8185c-128">Die Dienst Ebene enthält Geschäftslogik.</span><span class="sxs-lookup"><span data-stu-id="8185c-128">The service layer contains business logic.</span></span> <span data-ttu-id="8185c-129">Insbesondere enthält Sie Validierungs Logik.</span><span class="sxs-lookup"><span data-stu-id="8185c-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="8185c-130">Beispielsweise verfügt die Produkt Dienst Schicht in der Liste 3 über eine Methode "kreateproduct ()".</span><span class="sxs-lookup"><span data-stu-id="8185c-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="8185c-131">Die Methode "Methode ()" Ruft die Methode "validateproduct ()" auf, um ein neues Produkt zu validieren, bevor das Produkt an das produktrepository übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="8185c-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="8185c-132">**Codebeispiel 3: models\productservice.cs**</span><span class="sxs-lookup"><span data-stu-id="8185c-132">**Listing 3 - Models\ProductService.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

<span data-ttu-id="8185c-133">Der Produkt Controller wurde in der Liste 4 aktualisiert, sodass anstelle der Repository-Ebene die Dienst Ebene verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8185c-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="8185c-134">Die Controller Schicht spricht mit der Dienst Ebene.</span><span class="sxs-lookup"><span data-stu-id="8185c-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="8185c-135">Die Dienst Ebene spricht mit der Repository-Ebene.</span><span class="sxs-lookup"><span data-stu-id="8185c-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="8185c-136">Jede Ebene hat eine separate Verantwortung.</span><span class="sxs-lookup"><span data-stu-id="8185c-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="8185c-137">**Codebeispiel 4-controllers\productcontroller.cs**</span><span class="sxs-lookup"><span data-stu-id="8185c-137">**Listing 4 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

<span data-ttu-id="8185c-138">Beachten Sie, dass der Product Service im Product Controller-Konstruktor erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="8185c-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="8185c-139">Wenn der Product Service erstellt wird, wird das Modell Zustands Wörterbuch an den Dienst übermittelt.</span><span class="sxs-lookup"><span data-stu-id="8185c-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="8185c-140">Der Product Service verwendet den Modell Status, um Validierungs Fehlermeldungen an den Controller zurückzuleiten.</span><span class="sxs-lookup"><span data-stu-id="8185c-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="8185c-141">Entkoppeln der Dienst Ebene</span><span class="sxs-lookup"><span data-stu-id="8185c-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="8185c-142">Die Controller-und Dienst Ebenen konnten nicht in einer Hinsicht isoliert werden.</span><span class="sxs-lookup"><span data-stu-id="8185c-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="8185c-143">Die Controller-und Dienst Ebenen kommunizieren über den Modell Zustand.</span><span class="sxs-lookup"><span data-stu-id="8185c-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="8185c-144">Anders ausgedrückt: die Dienst Schicht hat eine Abhängigkeit von einer bestimmten Funktion des ASP.NET-MVC-Frameworks.</span><span class="sxs-lookup"><span data-stu-id="8185c-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="8185c-145">Wir möchten die Dienst Ebene so weit wie möglich von unserer Controller Schicht isolieren.</span><span class="sxs-lookup"><span data-stu-id="8185c-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="8185c-146">Theoretisch sollten wir die Dienst Schicht mit allen Anwendungs Typen und nicht nur mit einer ASP.NET MVC-Anwendung verwenden können.</span><span class="sxs-lookup"><span data-stu-id="8185c-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="8185c-147">Beispielsweise möchten wir in Zukunft möglicherweise ein WPF-Front-End für die Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="8185c-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="8185c-148">Wir sollten eine Möglichkeit finden, die Abhängigkeit vom ASP.NET MVC-Modell Status von unserer Dienst Schicht zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="8185c-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="8185c-149">In der Liste 5 wurde die Dienst Schicht so aktualisiert, dass Sie nicht mehr den Modell Status verwendet.</span><span class="sxs-lookup"><span data-stu-id="8185c-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="8185c-150">Stattdessen wird eine beliebige Klasse verwendet, die die ivalidationdictionary-Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="8185c-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="8185c-151">**Auflistung 5-models\productservice.cs (entkoppelt)**</span><span class="sxs-lookup"><span data-stu-id="8185c-151">**Listing 5 - Models\ProductService.cs (decoupled)**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

<span data-ttu-id="8185c-152">Die ivalidationdictionary-Schnittstelle ist in der Liste 6 definiert.</span><span class="sxs-lookup"><span data-stu-id="8185c-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="8185c-153">Diese einfache Schnittstelle verfügt über eine einzelne-Methode und eine einzelne-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="8185c-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="8185c-154">**Codebeispiel 6: models\ivalidationditionary.cs**</span><span class="sxs-lookup"><span data-stu-id="8185c-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

<span data-ttu-id="8185c-155">Die Klasse in der Auflistung 7 namens der modelstatewrapper-Klasse implementiert die ivalidationdictionary-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="8185c-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="8185c-156">Sie können die modelstatewrapper-Klasse instanziieren, indem Sie ein Modell Zustands Wörterbuch an den-Konstruktor übergeben.</span><span class="sxs-lookup"><span data-stu-id="8185c-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="8185c-157">**Codebeispiel 7: models\modelstatewrapper.cs**</span><span class="sxs-lookup"><span data-stu-id="8185c-157">**Listing 7 - Models\ModelStateWrapper.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

<span data-ttu-id="8185c-158">Schließlich verwendet der aktualisierte Controller in der Auflistung 8 den modelstatewrapper, wenn die Dienst Ebene im Konstruktor erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="8185c-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="8185c-159">**Auflisten von 8-controllers\productcontroller.cs**</span><span class="sxs-lookup"><span data-stu-id="8185c-159">**Listing 8 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

<span data-ttu-id="8185c-160">Mithilfe der ivalidationdictionary-Schnittstelle und der modelstatewrapper-Klasse können wir unsere Dienst Schicht vollständig von unserer Controller Schicht isolieren.</span><span class="sxs-lookup"><span data-stu-id="8185c-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="8185c-161">Die Dienst Ebene ist nicht mehr vom Modell Zustand abhängig.</span><span class="sxs-lookup"><span data-stu-id="8185c-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="8185c-162">Sie können eine beliebige Klasse übergeben, die die ivalidationdictionary-Schnittstelle implementiert, an die Dienst Ebene.</span><span class="sxs-lookup"><span data-stu-id="8185c-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="8185c-163">Eine WPF-Anwendung kann z. b. die ivalidationdictionary-Schnittstelle mit einer einfachen Auflistungs Klasse implementieren.</span><span class="sxs-lookup"><span data-stu-id="8185c-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="8185c-164">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="8185c-164">Summary</span></span>

<span data-ttu-id="8185c-165">Ziel dieses Tutorials war es, einen Ansatz für die Durchführung der Validierung in einer ASP.NET MVC-Anwendung zu erörtern.</span><span class="sxs-lookup"><span data-stu-id="8185c-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="8185c-166">In diesem Tutorial haben Sie erfahren, wie Sie Ihre gesamte Validierungs Logik aus ihren Controllern und in eine separate Dienst Schicht verschieben.</span><span class="sxs-lookup"><span data-stu-id="8185c-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="8185c-167">Außerdem haben Sie erfahren, wie Sie die Dienst Ebene von ihrer Controller Schicht isolieren, indem Sie eine modelstatewrapper-Klasse erstellen.</span><span class="sxs-lookup"><span data-stu-id="8185c-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8185c-168">[Zurück](validating-with-the-idataerrorinfo-interface-cs.md)
> [Weiter](validation-with-the-data-annotation-validators-cs.md)</span><span class="sxs-lookup"><span data-stu-id="8185c-168">[Previous](validating-with-the-idataerrorinfo-interface-cs.md)
[Next](validation-with-the-data-annotation-validators-cs.md)</span></span>
