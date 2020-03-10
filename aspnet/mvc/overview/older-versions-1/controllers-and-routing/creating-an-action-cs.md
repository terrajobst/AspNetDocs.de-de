---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Erstellen einer Aktion (C#) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie Sie einem ASP.NET-MVC-Controller eine neue Aktion hinzufügen. Erfahren Sie mehr über die Anforderungen für eine Methode, die eine Aktion sein soll.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: ebba935383819935ad85c95245666f4eaf6a0dca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78470199"
---
# <a name="creating-an-action-c"></a><span data-ttu-id="7eb27-104">Erstellen einer Aktion (C#)</span><span class="sxs-lookup"><span data-stu-id="7eb27-104">Creating an Action (C#)</span></span>

<span data-ttu-id="7eb27-105">von [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7eb27-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="7eb27-106">Erfahren Sie, wie Sie einem ASP.NET-MVC-Controller eine neue Aktion hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7eb27-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="7eb27-107">Erfahren Sie mehr über die Anforderungen für eine Methode, die eine Aktion sein soll.</span><span class="sxs-lookup"><span data-stu-id="7eb27-107">Learn about the requirements for a method to be an action.</span></span>

<span data-ttu-id="7eb27-108">In diesem Tutorial wird erläutert, wie Sie eine neue Controller Aktion erstellen können.</span><span class="sxs-lookup"><span data-stu-id="7eb27-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="7eb27-109">Sie erfahren mehr über die Anforderungen einer Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="7eb27-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="7eb27-110">Außerdem erfahren Sie, wie Sie verhindern können, dass eine Methode als Aktion verfügbar gemacht wird.</span><span class="sxs-lookup"><span data-stu-id="7eb27-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="7eb27-111">Hinzufügen einer Aktion zu einem Controller</span><span class="sxs-lookup"><span data-stu-id="7eb27-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="7eb27-112">Sie fügen einem Controller eine neue Aktion hinzu, indem Sie dem Controller eine neue Methode hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7eb27-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="7eb27-113">Der Controller in der Liste 1 enthält z. b. eine Aktion mit dem Namen Index () und eine Aktion mit dem Namen "SayHello ()".</span><span class="sxs-lookup"><span data-stu-id="7eb27-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="7eb27-114">Beide Methoden werden als Aktionen verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="7eb27-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="7eb27-115">**Codebeispiel 1-controllers\homecontroller.cs**</span><span class="sxs-lookup"><span data-stu-id="7eb27-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="7eb27-116">Um dem Universum als Aktion zur Verfügung gestellt zu werden, muss eine Methode bestimmte Anforderungen erfüllen:</span><span class="sxs-lookup"><span data-stu-id="7eb27-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="7eb27-117">Die Methode muss öffentlich sein.</span><span class="sxs-lookup"><span data-stu-id="7eb27-117">The method must be public.</span></span>
- <span data-ttu-id="7eb27-118">Die Methode kann keine statische Methode sein.</span><span class="sxs-lookup"><span data-stu-id="7eb27-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="7eb27-119">Die Methode kann keine Erweiterungsmethode sein.</span><span class="sxs-lookup"><span data-stu-id="7eb27-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="7eb27-120">Bei der Methode kann es sich nicht um einen Konstruktor, einen Getter oder einen Setter handeln.</span><span class="sxs-lookup"><span data-stu-id="7eb27-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="7eb27-121">Die-Methode kann keine offenen generischen Typen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="7eb27-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="7eb27-122">Die-Methode ist keine Methode der Controller Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="7eb27-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="7eb27-123">Die-Methode kann keine **ref** -oder **out** -Parameter enthalten.</span><span class="sxs-lookup"><span data-stu-id="7eb27-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="7eb27-124">Beachten Sie, dass es für den Rückgabetyp einer Controller Aktion keine Einschränkungen gibt.</span><span class="sxs-lookup"><span data-stu-id="7eb27-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="7eb27-125">Eine Controller Aktion kann eine Zeichenfolge, einen DateTime-Wert, eine Instanz der Zufalls Klasse oder "void" zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="7eb27-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="7eb27-126">Das ASP.NET-MVC-Framework konvertiert jeden Rückgabetyp, der kein Aktions Ergebnis ist, in eine Zeichenfolge und stellt die Zeichenfolge im Browser dar.</span><span class="sxs-lookup"><span data-stu-id="7eb27-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="7eb27-127">Wenn Sie einem Controller eine Methode hinzufügen, die diese Anforderungen nicht verletzt, wird die Methode als Controller Aktion verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="7eb27-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="7eb27-128">Seien Sie vorsichtig.</span><span class="sxs-lookup"><span data-stu-id="7eb27-128">Be careful here.</span></span> <span data-ttu-id="7eb27-129">Eine Controller Aktion kann von allen Personen aufgerufen werden, die mit dem Internet verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="7eb27-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="7eb27-130">Erstellen Sie z. b. eine deletemywebsite ()-Controller Aktion nicht.</span><span class="sxs-lookup"><span data-stu-id="7eb27-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="7eb27-131">Verhindern, dass eine öffentliche Methode aufgerufen wird</span><span class="sxs-lookup"><span data-stu-id="7eb27-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="7eb27-132">Wenn Sie eine öffentliche Methode in einer Controller Klasse erstellen müssen und die Methode nicht als Controller Aktion verfügbar machen möchten, können Sie verhindern, dass die Methode mithilfe des [NonAction]-Attributs aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="7eb27-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="7eb27-133">Der Controller in der Liste 2 enthält z. b. eine öffentliche Methode mit dem Namen "companysecrets ()", die mit dem Attribut [NonAction] ergänzt wird.</span><span class="sxs-lookup"><span data-stu-id="7eb27-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="7eb27-134">**Codebeispiel 2: controllers\workcontroller.cs**</span><span class="sxs-lookup"><span data-stu-id="7eb27-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="7eb27-135">Wenn Sie versuchen, die controanysecrets ()-Controller Aktion aufzurufen, indem Sie/Work/CompanySecrets in die Adressleiste Ihres Browsers eingeben, erhalten Sie die Fehlermeldung in Abbildung 1.</span><span class="sxs-lookup"><span data-stu-id="7eb27-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>

<span data-ttu-id="7eb27-136">[![Aufrufen einer NonAction-Methode](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7eb27-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="7eb27-137">**Abbildung 01**: Aufrufen einer nicht Aktionsmethode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="7eb27-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7eb27-138">[Zurück](creating-a-controller-cs.md)
> [Weiter](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7eb27-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>
