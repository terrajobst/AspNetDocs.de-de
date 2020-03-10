---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: ASP.NET MVC 4-Abhängigkeitsinjektion | Microsoft-Dokumentation
author: rick-anderson
description: 'Hinweis: Diese praktische Übungseinheit setzt voraus, dass Sie über grundlegende Kenntnisse der Filter ASP.NET MVC und ASP.NET MVC 4 verfügen. Wenn Sie noch keine ASP.NET MVC 4-Filter verwendet haben, wird empfohlen...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 15c9d4dcb9e2c6b9f6adf54d65d15737b32cca3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451821"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="e0c01-104">ASP.NET MVC 4 – Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="e0c01-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="e0c01-105">vom [Web Camps-Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e0c01-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="e0c01-106">Webcamps-Trainingskit herunterladen</span><span class="sxs-lookup"><span data-stu-id="e0c01-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="e0c01-107">Diese praktische Übung geht davon aus, dass Sie über grundlegende Kenntnisse der Filter **ASP.NET MVC** und **ASP.NET MVC 4**verfügen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="e0c01-108">Wenn Sie noch keine **ASP.NET MVC 4-Filter** verwendet haben, empfiehlt es sich, das praktische Lab **ASP.NET MVC Custom Action Filters** zu überspringen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="e0c01-109">Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das in den [Releases Microsoft-Web/webcamptrainingkit](https://aka.ms/webcamps-training-kit)verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="e0c01-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="e0c01-110">Das für dieses Lab spezifische Projekt ist unter [ASP.NET MVC 4-Abhängigkeitsinjektion](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection)verfügbar.</span><span class="sxs-lookup"><span data-stu-id="e0c01-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="e0c01-111">Im **objektorientierten Programmier** Paradigma arbeiten Objekte in einem Zusammenarbeits Modell zusammen, in dem es Mitwirkende und Consumer gibt.</span><span class="sxs-lookup"><span data-stu-id="e0c01-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="e0c01-112">Natürlich generiert dieses Kommunikationsmodell Abhängigkeiten zwischen Objekten und Komponenten und wird bei zunehmender Komplexität schwierig zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="e0c01-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="e0c01-113">![Klassen Abhängigkeiten und Modell Komplexität](aspnet-mvc-4-dependency-injection/_static/image1.png "Klassen Abhängigkeiten und Modell Komplexität")</span><span class="sxs-lookup"><span data-stu-id="e0c01-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="e0c01-114">*Klassen Abhängigkeiten und Modell Komplexität*</span><span class="sxs-lookup"><span data-stu-id="e0c01-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="e0c01-115">Sie haben wahrscheinlich das **Factorymuster** und die Trennung zwischen der Schnittstelle und der Implementierung mithilfe von Diensten gehört, bei denen die Client Objekte oft für die Dienst Identifizierung verantwortlich sind.</span><span class="sxs-lookup"><span data-stu-id="e0c01-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="e0c01-116">Das Muster für die Abhängigkeitsinjektion ist eine bestimmte Implementierung der Inversion of Control.</span><span class="sxs-lookup"><span data-stu-id="e0c01-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="e0c01-117">Die **Inversion of Control (IOC)** bedeutet, dass Objekte keine anderen Objekte erstellen, auf die Sie sich verlassen, um Ihre Arbeit zu erledigen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="e0c01-118">Stattdessen erhalten Sie die Objekte, die Sie benötigen, von einer externen Quelle (z. b. einer XML-Konfigurationsdatei).</span><span class="sxs-lookup"><span data-stu-id="e0c01-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="e0c01-119">Die **Abhängigkeitsinjektion (di)** bedeutet, dass dies ohne den Objekt Eingriff erfolgt, normalerweise durch eine frameworkkomponente, die Konstruktorparameter übergibt und Eigenschaften festgelegt.</span><span class="sxs-lookup"><span data-stu-id="e0c01-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="e0c01-120">Das Entwurfsmuster für die Abhängigkeitsinjektion (di)</span><span class="sxs-lookup"><span data-stu-id="e0c01-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="e0c01-121">Auf hoher Ebene besteht das Ziel der Abhängigkeitsinjektion darin, dass eine Client Klasse (z. b. *der Golfer*) etwas benötigt, das eine Schnittstelle erfüllt (z. b. *iclub*).</span><span class="sxs-lookup"><span data-stu-id="e0c01-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="e0c01-122">Es ist nicht wichtig, dass der konkrete Typ (z. b. *woodclub, ironclub, wedgeclub* oder *putterclub*) von einem anderen Benutzer behandelt wird (z. b. ein guter *Caddy*).</span><span class="sxs-lookup"><span data-stu-id="e0c01-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="e0c01-123">Mit dem Abhängigkeits Konflikt Löser in ASP.NET MVC können Sie Ihre Abhängigkeits Logik an einer anderen Stelle registrieren (z. b. in einem Container oder einer Sammlung *von Schlägern*).</span><span class="sxs-lookup"><span data-stu-id="e0c01-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="e0c01-124">![Abhängigkeits Injection-Diagramm](aspnet-mvc-4-dependency-injection/_static/image2.png "Darstellung der Abhängigkeitsinjektion")</span><span class="sxs-lookup"><span data-stu-id="e0c01-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="e0c01-125">*Abhängigkeitsinjektion-Golf Analog*</span><span class="sxs-lookup"><span data-stu-id="e0c01-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="e0c01-126">Die Verwendung von Abhängigkeits Injektions Mustern und Inversion der Kontrolle bietet folgende Vorteile:</span><span class="sxs-lookup"><span data-stu-id="e0c01-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="e0c01-127">Reduziert die Klassen Kopplung</span><span class="sxs-lookup"><span data-stu-id="e0c01-127">Reduces class coupling</span></span>
- <span data-ttu-id="e0c01-128">Erhöht die Wiederverwendung von Code.</span><span class="sxs-lookup"><span data-stu-id="e0c01-128">Increases code reusing</span></span>
- <span data-ttu-id="e0c01-129">Verbesserte Code Wartbarkeit</span><span class="sxs-lookup"><span data-stu-id="e0c01-129">Improves code maintainability</span></span>
- <span data-ttu-id="e0c01-130">Verbessert das Testen von Anwendungen</span><span class="sxs-lookup"><span data-stu-id="e0c01-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="e0c01-131">Die Abhängigkeitsinjektion wird manchmal mit dem abstrakten Factoryentwurfsmuster verglichen, aber es gibt einen kleinen Unterschied zwischen beiden Ansätzen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="e0c01-132">Bei di gibt es ein Framework, mit dem Abhängigkeiten durch Aufrufen der Factorys und der registrierten Dienste gelöst werden können.</span><span class="sxs-lookup"><span data-stu-id="e0c01-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>

<span data-ttu-id="e0c01-133">Nachdem Sie das Muster für die Abhängigkeitsinjektion verstanden haben, erfahren Sie in diesem Lab, wie Sie es in ASP.NET MVC 4 anwenden können.</span><span class="sxs-lookup"><span data-stu-id="e0c01-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="e0c01-134">Sie beginnen mit der Verwendung der Abhängigkeitsinjektion in den **Controllern** , um einen Datenbankzugriffs Dienst einzubeziehen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="e0c01-135">Als nächstes wenden Sie eine Abhängigkeitsinjektion auf die **Sichten** an, um einen Dienst zu nutzen und Informationen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="e0c01-136">Schließlich erweitern Sie den di-Filter auf ASP.NET MVC 4-Filter und in der Lösung einen benutzerdefinierten Aktionsfilter.</span><span class="sxs-lookup"><span data-stu-id="e0c01-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="e0c01-137">In dieser praktischen Übungseinheit erfahren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="e0c01-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="e0c01-138">Integrieren von ASP.NET MVC 4 in Unity für die Abhängigkeitsinjektion mithilfe von nuget-Paketen</span><span class="sxs-lookup"><span data-stu-id="e0c01-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="e0c01-139">Verwenden von Abhängigkeitsinjektion in einem ASP.NET-MVC-Controller</span><span class="sxs-lookup"><span data-stu-id="e0c01-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="e0c01-140">Abhängigkeitsinjektion innerhalb einer ASP.NET-MVC-Ansicht verwenden</span><span class="sxs-lookup"><span data-stu-id="e0c01-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="e0c01-141">Verwenden von Abhängigkeitsinjektion innerhalb eines ASP.NET MVC-Aktions Filters</span><span class="sxs-lookup"><span data-stu-id="e0c01-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="e0c01-142">Dieses Lab verwendet das Unity. Mvc3-nuget-Paket für die Abhängigkeitsauflösung, aber es ist möglich, jedes Framework für die Abhängigkeitsinjektion an ASP.NET MVC 4 anzupassen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e0c01-143">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="e0c01-143">Prerequisites</span></span>

<span data-ttu-id="e0c01-144">Zum Durchführen dieses Labs müssen Sie über Folgendes verfügen:</span><span class="sxs-lookup"><span data-stu-id="e0c01-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="e0c01-145">[Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder Superior (Informationen zur Installation finden Sie in [Anhang A](#AppendixA) ).</span><span class="sxs-lookup"><span data-stu-id="e0c01-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e0c01-146">Einrichten</span><span class="sxs-lookup"><span data-stu-id="e0c01-146">Setup</span></span>

<span data-ttu-id="e0c01-147">**Installieren von Code Ausschnitten**</span><span class="sxs-lookup"><span data-stu-id="e0c01-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="e0c01-148">Der Vorteil ist, dass ein Großteil des Codes, den Sie in diesem Lab verwalten, als Visual Studio-Code Ausschnitte verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="e0c01-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="e0c01-149">Um die Code Ausschnitte zu installieren, führen Sie **.\source\setup\codesnippeer-.vsi** -Datei aus.</span><span class="sxs-lookup"><span data-stu-id="e0c01-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="e0c01-150">Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, finden Sie den Anhang dieses Dokuments &quot;[Anhang B: Verwenden von Code Ausschnitten](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="e0c01-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e0c01-151">Exerzitien</span><span class="sxs-lookup"><span data-stu-id="e0c01-151">Exercises</span></span>

<span data-ttu-id="e0c01-152">Diese praktische Übungseinheit besteht aus den folgenden Übungen:</span><span class="sxs-lookup"><span data-stu-id="e0c01-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="e0c01-153">Übung 1: Einfügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="e0c01-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="e0c01-154">Übung 2: Einfügen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="e0c01-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="e0c01-155">Übung 3: Einfügen von Filtern</span><span class="sxs-lookup"><span data-stu-id="e0c01-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="e0c01-156">Jede Übung wird von einem **endordner** begleitet, der die sich ergebende Lösung enthält, die Sie nach Abschluss der Übungen erhalten.</span><span class="sxs-lookup"><span data-stu-id="e0c01-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="e0c01-157">Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Durcharbeiten der Übungen benötigen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="e0c01-158">Geschätzte Zeit bis zum Abschluss dieses Labs: **30 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="e0c01-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="e0c01-159">Übung 1: Einfügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="e0c01-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="e0c01-160">In dieser Übung erfahren Sie, wie Sie die Abhängigkeitsinjektion in ASP.NET-MVC-Controllern verwenden, indem Sie Unity mithilfe eines nuget-Pakets integrieren.</span><span class="sxs-lookup"><span data-stu-id="e0c01-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="e0c01-161">Aus diesem Grund werden Sie Dienste in Ihre mvcmusicstore-Controller einschließen, um die Logik vom Datenzugriff zu trennen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="e0c01-162">Die Dienste erstellen eine neue Abhängigkeit im controllerkonstruktor, die mithilfe von "Abhängigkeitsinjektion" mit der Hilfe von **Unity**aufgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="e0c01-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="e0c01-163">Dieser Ansatz zeigt Ihnen, wie Sie weniger gekoppelte Anwendungen generieren, die flexibler und leichter zu verwalten und zu testen sind.</span><span class="sxs-lookup"><span data-stu-id="e0c01-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="e0c01-164">Außerdem erfahren Sie, wie Sie ASP.NET MVC in Unity integrieren.</span><span class="sxs-lookup"><span data-stu-id="e0c01-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="e0c01-165">Informationen zum StoreManager-Dienst</span><span class="sxs-lookup"><span data-stu-id="e0c01-165">About StoreManager Service</span></span>

<span data-ttu-id="e0c01-166">Der in der Lösung "BEGIN" bereitgestellte MVC Music Store enthält jetzt einen Dienst, der die Store Controller-Daten mit dem Namen " **storeservice**" verwaltet.</span><span class="sxs-lookup"><span data-stu-id="e0c01-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="e0c01-167">Unten finden Sie die Store Service-Implementierung.</span><span class="sxs-lookup"><span data-stu-id="e0c01-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="e0c01-168">Beachten Sie, dass alle Methoden Modell Entitäten zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="e0c01-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="e0c01-169">**StoreController** von der BEGIN-Lösung verwendet nun **storeservice**.</span><span class="sxs-lookup"><span data-stu-id="e0c01-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="e0c01-170">Alle Daten Verweise wurden aus **StoreController**entfernt und können jetzt den aktuellen Datenzugriffs Anbieter ändern, ohne eine Methode zu ändern, die **storeservice**beansprucht.</span><span class="sxs-lookup"><span data-stu-id="e0c01-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="e0c01-171">Unten sehen Sie, dass die **StoreController** -Implementierung über eine Abhängigkeit mit **storeservice** innerhalb des Klassenkonstruktors verfügt.</span><span class="sxs-lookup"><span data-stu-id="e0c01-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="e0c01-172">Die in dieser Übung eingeführte Abhängigkeit bezieht sich auf die **Inversion of Control** (IOC).</span><span class="sxs-lookup"><span data-stu-id="e0c01-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="e0c01-173">Der **StoreController** -Klassenkonstruktor empfängt einen **istoreservice** -Typparameter, der für die Durchführung von Dienst aufrufen aus der-Klasse erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="e0c01-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="e0c01-174">**StoreController** implementiert jedoch nicht den Standardkonstruktor (ohne Parameter), den jeder Controller für die Arbeit mit ASP.NET MVC benötigen muss.</span><span class="sxs-lookup"><span data-stu-id="e0c01-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="e0c01-175">Um die Abhängigkeit aufzulösen, muss der Controller von einer abstrakten Factory (einer Klasse, die ein beliebiges Objekt des angegebenen Typs zurückgibt) erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="e0c01-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="e0c01-176">Sie erhalten eine Fehlermeldung, wenn die Klasse versucht, StoreController zu erstellen, ohne das Dienst Objekt zu senden, da kein Parameter loser Konstruktor deklariert wurde.</span><span class="sxs-lookup"><span data-stu-id="e0c01-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="e0c01-177">Aufgabe 1: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e0c01-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="e0c01-178">In dieser Aufgabe führen Sie die BEGIN-Anwendung aus, die den-Dienst in den Speicher Controller einschließt, der den Datenzugriff von der Anwendungslogik trennt.</span><span class="sxs-lookup"><span data-stu-id="e0c01-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="e0c01-179">Wenn Sie die Anwendung ausführen, erhalten Sie eine Ausnahme, da der Controller Dienst nicht standardmäßig als Parameter übergeben wird:</span><span class="sxs-lookup"><span data-stu-id="e0c01-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="e0c01-180">Öffnen Sie die Projekt Mappe " **Begin** " in **source\ex01-Input controller\begin**.</span><span class="sxs-lookup"><span data-stu-id="e0c01-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="e0c01-181">Sie müssen einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e0c01-182">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="e0c01-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e0c01-183">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e0c01-184">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="e0c01-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e0c01-185">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="e0c01-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e0c01-186">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e0c01-187">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="e0c01-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e0c01-188">Drücken Sie **STRG + F5** , um die Anwendung ohne Debuggen auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="e0c01-189">Sie erhalten die Fehlermeldung, &quot;**kein Parameter loser Konstruktor für dieses Objekt&quot;definiert** ist:</span><span class="sxs-lookup"><span data-stu-id="e0c01-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="e0c01-190">![Fehler beim Ausführen der ASP.NET MVC-BEGIN-Anwendung.](aspnet-mvc-4-dependency-injection/_static/image3.png "Fehler beim Ausführen der ASP.NET MVC-BEGIN-Anwendung.")</span><span class="sxs-lookup"><span data-stu-id="e0c01-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="e0c01-191">*Fehler beim Ausführen der ASP.NET MVC-BEGIN-Anwendung.*</span><span class="sxs-lookup"><span data-stu-id="e0c01-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="e0c01-192">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="e0c01-192">Close the browser.</span></span>

<span data-ttu-id="e0c01-193">In den folgenden Schritten arbeiten Sie mit der Music Store-Projekt Mappe zusammen, um die Abhängigkeit, die dieser Controller benötigt, einzufügen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="e0c01-194">Aufgabe 2: einschließen von Unity in eine mvcmusicstore-Lösung</span><span class="sxs-lookup"><span data-stu-id="e0c01-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="e0c01-195">In dieser Aufgabe fügen Sie das nuget-Paket " **Unity. Mvc3** " in die Projekt Mappe ein.</span><span class="sxs-lookup"><span data-stu-id="e0c01-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="e0c01-196">Das Unity. Mvc3-Paket wurde für ASP.NET MVC 3 entwickelt, ist aber vollständig mit ASP.NET MVC 4 kompatibel.</span><span class="sxs-lookup"><span data-stu-id="e0c01-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="e0c01-197">Unity ist ein schlanker, erweiterbarer Abhängigkeits Injection-Container mit optionaler Unterstützung für Instanzen und typabfang.</span><span class="sxs-lookup"><span data-stu-id="e0c01-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="e0c01-198">Dabei handelt es sich um einen allgemeinen Container zur Verwendung in beliebigen .NET-Anwendungs Typen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="e0c01-199">Es bietet alle allgemeinen Features, die in Mechanismen für die Abhängigkeitsinjektion enthalten sind: Objekt Erstellung, Abstraktion von Anforderungen durch Angeben von Abhängigkeiten zur Laufzeit und Flexibilität, indem die Komponenten Konfiguration auf den Container verschoben wird.</span><span class="sxs-lookup"><span data-stu-id="e0c01-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>

1. <span data-ttu-id="e0c01-200">Installieren Sie das nuget-Paket " **Unity. Mvc3** " im **mvcmusicstore** -Projekt.</span><span class="sxs-lookup"><span data-stu-id="e0c01-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="e0c01-201">Öffnen Sie hierzu die Paket- **Manager-Konsole** in der **Ansicht** | **anderen Fenstern**.</span><span class="sxs-lookup"><span data-stu-id="e0c01-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="e0c01-202">Führen Sie den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="e0c01-202">Run the following command.</span></span>

    <span data-ttu-id="e0c01-203">PMC</span><span class="sxs-lookup"><span data-stu-id="e0c01-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="e0c01-204">![Installieren des Unity. Mvc3-nuget-Pakets](aspnet-mvc-4-dependency-injection/_static/image4.png "Installieren des Unity. Mvc3-nuget-Pakets")</span><span class="sxs-lookup"><span data-stu-id="e0c01-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="e0c01-205">*Installieren des Unity. Mvc3-nuget-Pakets*</span><span class="sxs-lookup"><span data-stu-id="e0c01-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="e0c01-206">Nachdem das **Unity. Mvc3** -Paket installiert wurde, untersuchen Sie die Dateien und Ordner, die automatisch hinzugefügt werden, um die Unity-Konfiguration zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="e0c01-207">![Unity. Mvc3-Paket installiert](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity. Mvc3-Paket installiert")</span><span class="sxs-lookup"><span data-stu-id="e0c01-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="e0c01-208">*Unity. Mvc3-Paket installiert*</span><span class="sxs-lookup"><span data-stu-id="e0c01-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-application_start"></a><span data-ttu-id="e0c01-209">Aufgabe 3: Registrieren von Unity in der Global.asax.cs-Anwendung\_Start</span><span class="sxs-lookup"><span data-stu-id="e0c01-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="e0c01-210">In dieser Aufgabe aktualisieren Sie die **Anwendung\_Start** -Methode in **Global.asax.cs** , um den Unity-bootstrapperinitialisierer aufzurufen, und aktualisieren anschließend die Bootstrapperdatei, die den Dienst und den Controller registriert, den Sie für die Abhängigkeitsinjektion verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="e0c01-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="e0c01-211">Nun verbinden Sie den Boots Trapper, der die Datei ist, die den Unity-Container und den Abhängigkeits Konflikt Löser initialisiert.</span><span class="sxs-lookup"><span data-stu-id="e0c01-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="e0c01-212">Öffnen Sie hierzu **Global.asax.cs** , und fügen Sie den folgenden hervorgehobenen Code in der **Anwendung\_Start** -Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="e0c01-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="e0c01-213">(Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex01-Initialize Unity*)</span><span class="sxs-lookup"><span data-stu-id="e0c01-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="e0c01-214">Öffnen Sie die Datei **Bootstrapper.cs** .</span><span class="sxs-lookup"><span data-stu-id="e0c01-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="e0c01-215">Fügen Sie die folgenden Namespaces ein: **mvcmusicstore. Services** und **Musicstore. Controllers**.</span><span class="sxs-lookup"><span data-stu-id="e0c01-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="e0c01-216">(Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex01-Bootstrapper Hinzufügen von Namespaces*)</span><span class="sxs-lookup"><span data-stu-id="e0c01-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="e0c01-217">Ersetzen Sie den Inhalt der **buildunitycontainer** -Methode durch den folgenden Code, in dem Store Controller und Store Service registriert sind.</span><span class="sxs-lookup"><span data-stu-id="e0c01-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="e0c01-218">(Code Ausschnitt- *ASP.net Abhängigkeitsinjektion Lab-Ex01-Store Controller und Dienst registrieren*)</span><span class="sxs-lookup"><span data-stu-id="e0c01-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="e0c01-219">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e0c01-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="e0c01-220">In dieser Aufgabe führen Sie die Anwendung aus, um zu überprüfen, ob Sie jetzt nach dem einschließen von Unity geladen werden kann.</span><span class="sxs-lookup"><span data-stu-id="e0c01-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="e0c01-221">Drücken Sie **F5** , um die Anwendung auszuführen. die Anwendung sollte nun geladen werden, ohne dass eine Fehlermeldung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="e0c01-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="e0c01-222">![Ausführen einer Anwendung mit Abhängigkeitsinjektion](aspnet-mvc-4-dependency-injection/_static/image6.png "Ausführen einer Anwendung mit Abhängigkeitsinjektion")</span><span class="sxs-lookup"><span data-stu-id="e0c01-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="e0c01-223">*Ausführen einer Anwendung mit Abhängigkeitsinjektion*</span><span class="sxs-lookup"><span data-stu-id="e0c01-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="e0c01-224">Navigieren Sie zu **/Store**.</span><span class="sxs-lookup"><span data-stu-id="e0c01-224">Browse to **/Store**.</span></span> <span data-ttu-id="e0c01-225">Dadurch wird **StoreController**aufgerufen, das nun mithilfe von **Unity**erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="e0c01-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="e0c01-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span><span class="sxs-lookup"><span data-stu-id="e0c01-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="e0c01-227">*MVC Music Store*</span><span class="sxs-lookup"><span data-stu-id="e0c01-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="e0c01-228">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="e0c01-228">Close the browser.</span></span>

<span data-ttu-id="e0c01-229">In den folgenden Übungen erfahren Sie, wie Sie den Bereich für die Abhängigkeitsinjektion erweitern, um ihn in ASP.NET MVC-Ansichten und-Aktions Filtern zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="e0c01-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="e0c01-230">Übung 2: Einfügen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="e0c01-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="e0c01-231">In dieser Übung erfahren Sie, wie Sie die Abhängigkeitsinjektion in einer Ansicht mit den neuen Features von ASP.NET MVC 4 für die Unity-Integration verwenden.</span><span class="sxs-lookup"><span data-stu-id="e0c01-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="e0c01-232">Dazu rufen Sie einen benutzerdefinierten Dienst in der Such Ansicht des Stores auf, in der eine Meldung und ein Bild unten angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="e0c01-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="e0c01-233">Anschließend integrieren Sie das Projekt in Unity und erstellen einen benutzerdefinierten Abhängigkeits Konflikt Löser zum Einfügen der Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="e0c01-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="e0c01-234">Aufgabe 1: Erstellen einer Ansicht, die einen Dienst nutzt</span><span class="sxs-lookup"><span data-stu-id="e0c01-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="e0c01-235">In dieser Aufgabe erstellen Sie eine Ansicht, die einen Dienst aufzurufen, um eine neue Abhängigkeit zu generieren.</span><span class="sxs-lookup"><span data-stu-id="e0c01-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="e0c01-236">Der Dienst besteht aus einem einfachen Messaging Dienst, der in dieser Lösung enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="e0c01-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="e0c01-237">Öffnen Sie die Projekt Mappe " **Anfang** ", die sich im Ordner " **source\ex02-Input view\begin** " befindet.</span><span class="sxs-lookup"><span data-stu-id="e0c01-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="e0c01-238">Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.</span><span class="sxs-lookup"><span data-stu-id="e0c01-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="e0c01-239">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e0c01-240">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="e0c01-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e0c01-241">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e0c01-242">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="e0c01-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e0c01-243">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="e0c01-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e0c01-244">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e0c01-245">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="e0c01-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="e0c01-246">Weitere Informationen finden Sie in diesem Artikel: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="e0c01-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="e0c01-247">Schließen Sie die **MessageService.cs** -und **IMessageService.cs** -Klassen ein, die sich im Ordner " **Source \assets** " in **/Services**befinden.</span><span class="sxs-lookup"><span data-stu-id="e0c01-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="e0c01-248">Klicken Sie hierzu mit der rechten Maustaste auf den Ordner **Dienste** , und wählen Sie **Vorhandenes Element hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="e0c01-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="e0c01-249">Navigieren Sie zum Speicherort der Dateien, und fügen Sie Sie ein.</span><span class="sxs-lookup"><span data-stu-id="e0c01-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="e0c01-250">![Hinzufügen von Message Service und Dienst Schnittstelle](aspnet-mvc-4-dependency-injection/_static/image8.png "Hinzufügen von Message Service und Dienst Schnittstelle")</span><span class="sxs-lookup"><span data-stu-id="e0c01-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="e0c01-251">*Hinzufügen von Message Service und Dienst Schnittstelle*</span><span class="sxs-lookup"><span data-stu-id="e0c01-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e0c01-252">Die **imessageservice** -Schnittstelle definiert zwei Eigenschaften, die von der **MessageService** -Klasse implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="e0c01-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="e0c01-253">Diese Eigenschaften-**Message** und **ImageUrl**: Speichern Sie die Nachricht und die URL des anzuzeigenden Bilds.</span><span class="sxs-lookup"><span data-stu-id="e0c01-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="e0c01-254">Erstellen Sie den Ordner **/pages** im Stamm Ordner des Projekts, und fügen Sie dann die vorhandene Klasse **MyBasePage.cs** aus **source\assets**hinzu.</span><span class="sxs-lookup"><span data-stu-id="e0c01-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="e0c01-255">Die Basis Seite, von der Sie erben, weist die folgende Struktur auf.</span><span class="sxs-lookup"><span data-stu-id="e0c01-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="e0c01-256">![Ordner "Pages"](aspnet-mvc-4-dependency-injection/_static/image9.png "Ordner „Seiten“")</span><span class="sxs-lookup"><span data-stu-id="e0c01-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="e0c01-257">Öffnen Sie die Ansicht **Browse. cshtml** aus dem Ordner **/views/Store** , und legen Sie die Vererbung von **MyBasePage.cs**ab.</span><span class="sxs-lookup"><span data-stu-id="e0c01-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="e0c01-258">Fügen Sie in der Ansicht **Durchsuchen** einen " **MessageService** "-Befehl hinzu, um ein Bild und eine vom Dienst abgerufene Nachricht anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="e0c01-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="e0c01-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="e0c01-260">Aufgabe 2: Einschließen eines benutzerdefinierten Abhängigkeits Konflikt Lösers und einer benutzerdefinierten Ansichts Seiten Aktivierung</span><span class="sxs-lookup"><span data-stu-id="e0c01-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="e0c01-261">In der vorherigen Aufgabe haben Sie eine neue Abhängigkeit in eine Ansicht eingefügt, um darin einen Dienst aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="e0c01-262">Nun lösen Sie diese Abhängigkeit durch Implementieren der ASP.NET MVC-Abhängigkeits Injektions Schnittstellen **iviewpageactivator** und **idetzdencyresolver**.</span><span class="sxs-lookup"><span data-stu-id="e0c01-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="e0c01-263">Sie fügen in die Lösung eine Implementierung von **idepdencyresolver** ein, die den Dienst Abruf mithilfe von Unity behandelt.</span><span class="sxs-lookup"><span data-stu-id="e0c01-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="e0c01-264">Anschließend fügen Sie eine weitere benutzerdefinierte Implementierung der **iviewpageactivator** -Schnittstelle ein, die die Erstellung der Sichten löst.</span><span class="sxs-lookup"><span data-stu-id="e0c01-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="e0c01-265">Seit ASP.NET MVC 3 hat die Implementierung für die Abhängigkeitsinjektion die Schnittstellen für die Registrierung von Diensten vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="e0c01-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="e0c01-266">**Idetzdencyresolver** und **iviewpageactivator** sind Teil der ASP.NET MVC 3-Funktionen für die Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="e0c01-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="e0c01-267">Die **idepdencyresolver-** Schnittstelle ersetzt den vorherigen imvcservicelocator.</span><span class="sxs-lookup"><span data-stu-id="e0c01-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="e0c01-268">Implementierer von idepdencyresolver müssen eine Instanz des Dienstanbieter oder einer Dienst Auflistung zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="e0c01-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="e0c01-269">Die **iviewpageactivator-** Schnittstelle bietet eine präzisere Steuerung der instanziierten Ansichts Seiten mithilfe der Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="e0c01-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="e0c01-270">Die Klassen, die die **iviewpageactivator** -Schnittstelle implementieren, können Ansichts Instanzen mithilfe von Kontextinformationen erstellen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]

1. <span data-ttu-id="e0c01-271">Erstellen Sie den**Ordner/** Factorys im Stamm Ordner des Projekts.</span><span class="sxs-lookup"><span data-stu-id="e0c01-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="e0c01-272">Fügen Sie **CustomViewPageActivator.cs** aus dem Ordner **/Sources/Assets/** in Factorys **in Ihre** Lösung ein.</span><span class="sxs-lookup"><span data-stu-id="e0c01-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="e0c01-273">Klicken Sie dazu mit der rechten Maustaste auf den Ordner **/Factories** , und wählen Sie **hinzufügen aus. Vorhandenes Element** , und wählen Sie dann **CustomViewPageActivator.cs**aus.</span><span class="sxs-lookup"><span data-stu-id="e0c01-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="e0c01-274">Diese Klasse implementiert die **iviewpageactivator** -Schnittstelle für den Unity-Container.</span><span class="sxs-lookup"><span data-stu-id="e0c01-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="e0c01-275">**Customviewpageactivator** ist für die Verwaltung der Erstellung einer Ansicht mit einem Unity-Container zuständig.</span><span class="sxs-lookup"><span data-stu-id="e0c01-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="e0c01-276">Fügen Sie die **UnityDependencyResolver.cs** -Datei aus **/Sources/Assets** in den Ordner **/Factories** ein.</span><span class="sxs-lookup"><span data-stu-id="e0c01-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="e0c01-277">Klicken Sie dazu mit der rechten Maustaste auf den Ordner **/Factories** , und wählen Sie **hinzufügen aus. Vorhandenes Element** , und wählen Sie dann **UnityDependencyResolver.cs** File aus.</span><span class="sxs-lookup"><span data-stu-id="e0c01-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="e0c01-278">Die **unitydependencyresolver** -Klasse ist ein benutzerdefinierter DependencyResolver für Unity.</span><span class="sxs-lookup"><span data-stu-id="e0c01-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="e0c01-279">Wenn ein Dienst im Unity-Container nicht gefunden werden kann, wird der basisresolver aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="e0c01-280">In der folgenden Aufgabe werden beide Implementierungen registriert, damit das Modell den Speicherort der Dienste und Sichten kennt.</span><span class="sxs-lookup"><span data-stu-id="e0c01-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="e0c01-281">Aufgabe 3: Registrieren für die Abhängigkeitsinjektion innerhalb des Unity-Containers</span><span class="sxs-lookup"><span data-stu-id="e0c01-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="e0c01-282">In dieser Aufgabe werden alle vorherigen Elemente zusammengefasst, um die Abhängigkeitsinjektion zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="e0c01-283">Bis jetzt verfügt Ihre Lösung über die folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="e0c01-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="e0c01-284">Eine **browseinsicht** , die von **MyBaseClass** erbt und **MessageService**verwendet.</span><span class="sxs-lookup"><span data-stu-id="e0c01-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="e0c01-285">Eine Zwischenklasse (**MyBaseClass**), für die eine Abhängigkeitsinjektion für die Dienst Schnittstelle deklariert wurde.</span><span class="sxs-lookup"><span data-stu-id="e0c01-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="e0c01-286">Einen Service- **MessageService** -und seine Schnittstelle **imessageservice**.</span><span class="sxs-lookup"><span data-stu-id="e0c01-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="e0c01-287">Ein benutzerdefinierter Abhängigkeits Konflikt Löser für Unity- **unitydependencyresolver** , der den Dienst Abruf behandelt.</span><span class="sxs-lookup"><span data-stu-id="e0c01-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="e0c01-288">Eine Ansichts Seiten-Activator- **customviewpageactivator** -, die die Seite erstellt.</span><span class="sxs-lookup"><span data-stu-id="e0c01-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="e0c01-289">Zum Einfügen der **browseinsicht** registrieren Sie nun den benutzerdefinierten Abhängigkeits Konflikt Löser im Unity-Container.</span><span class="sxs-lookup"><span data-stu-id="e0c01-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="e0c01-290">Öffnen Sie die Datei **Bootstrapper.cs** .</span><span class="sxs-lookup"><span data-stu-id="e0c01-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="e0c01-291">Registrieren Sie eine Instanz von **MessageService** im Unity-Container, um den Dienst zu initialisieren:</span><span class="sxs-lookup"><span data-stu-id="e0c01-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="e0c01-292">(Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex02-Register Message Service*)</span><span class="sxs-lookup"><span data-stu-id="e0c01-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="e0c01-293">Fügen Sie einen Verweis auf den **mvcmusicstore.** factorynamespace hinzu.</span><span class="sxs-lookup"><span data-stu-id="e0c01-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="e0c01-294">(Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex02-factorynamespace*)</span><span class="sxs-lookup"><span data-stu-id="e0c01-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="e0c01-295">Registrieren Sie **customviewpageactivator** als Ansichts Seiten Aktivierung im Unity-Container:</span><span class="sxs-lookup"><span data-stu-id="e0c01-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="e0c01-296">(Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex02-Register customviewpageactivator*)</span><span class="sxs-lookup"><span data-stu-id="e0c01-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="e0c01-297">Ersetzen Sie ASP.NET MVC 4 Default-Abhängigkeits Konflikt Löser durch eine Instanz von **unitydependencyresolver**.</span><span class="sxs-lookup"><span data-stu-id="e0c01-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="e0c01-298">Ersetzen Sie hierzu den **Initialisierungs** Methoden Inhalt durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="e0c01-298">To do this, replace **Initialize** method content with the following code:</span></span>

    <span data-ttu-id="e0c01-299">(Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex02-Abhängigkeits*Konflikt Löser aktualisieren)</span><span class="sxs-lookup"><span data-stu-id="e0c01-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="e0c01-300">ASP.NET MVC stellt eine standardabhängigkeitsresolver-Klasse bereit.</span><span class="sxs-lookup"><span data-stu-id="e0c01-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="e0c01-301">Um mit benutzerdefinierten Abhängigkeits Konflikt Löser zu arbeiten, die wir für Unity erstellt haben, muss dieser Konflikt Löser ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="e0c01-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="e0c01-302">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e0c01-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="e0c01-303">In dieser Aufgabe führen Sie die Anwendung aus, um zu überprüfen, ob der Store-Browser den Dienst verwendet und das Abbild und die abgerufene Nachricht angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="e0c01-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="e0c01-304">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="e0c01-305">Klicken Sie im Menü Genres auf **Rock** , und sehen Sie sich an, wie der **MessageService** in die Ansicht eingefügt und die Willkommensnachricht und das Bild geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="e0c01-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="e0c01-306">In diesem Beispiel wechseln wir zu &quot;**Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="e0c01-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="e0c01-307">![MVC Music Store-Einschleusung anzeigen](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store-Einschleusung anzeigen")</span><span class="sxs-lookup"><span data-stu-id="e0c01-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="e0c01-308">*MVC Music Store-Einschleusung anzeigen*</span><span class="sxs-lookup"><span data-stu-id="e0c01-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="e0c01-309">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="e0c01-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="e0c01-310">Übung 3: Einfügen von Aktions Filtern</span><span class="sxs-lookup"><span data-stu-id="e0c01-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="e0c01-311">In den vorherigen praktischen Übungseinheiten für praktische Übungseinheiten **haben Sie mit** der Anpassung und Injektion von Filtern gearbeitet.</span><span class="sxs-lookup"><span data-stu-id="e0c01-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="e0c01-312">In dieser Übung erfahren Sie, wie Sie mithilfe des Unity-Containers Filter mit Abhängigkeitsinjektion einfügen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="e0c01-313">Zu diesem Zweck fügen Sie der Music Store-Projekt Mappe einen benutzerdefinierten Aktionsfilter hinzu, der die Aktivität der Site verfolgt.</span><span class="sxs-lookup"><span data-stu-id="e0c01-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="e0c01-314">Aufgabe 1: einschließen des nach Verfolgungs Filters in der Lösung</span><span class="sxs-lookup"><span data-stu-id="e0c01-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="e0c01-315">In dieser Aufgabe fügen Sie in den Music Store einen benutzerdefinierten Aktionsfilter ein, um Ereignisse zu verfolgen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="e0c01-316">Da benutzerdefinierte Aktionsfilter Konzepte bereits in der vorherigen Lab-&quot;&quot;benutzerdefinierte Aktionsfilter behandelt werden, fügen Sie einfach die Filter-Klasse aus dem Ordner "Assets" dieses Labs ein, und erstellen Sie dann einen Filter Anbieter für Unity:</span><span class="sxs-lookup"><span data-stu-id="e0c01-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="e0c01-317">Öffnen Sie **die Projekt** Mappe "Start", die sich im Ordner " **source\ex03-einschleusung\begin** " befindet.</span><span class="sxs-lookup"><span data-stu-id="e0c01-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="e0c01-318">Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.</span><span class="sxs-lookup"><span data-stu-id="e0c01-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="e0c01-319">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e0c01-320">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="e0c01-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e0c01-321">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e0c01-322">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="e0c01-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e0c01-323">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="e0c01-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e0c01-324">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e0c01-325">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="e0c01-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="e0c01-326">Weitere Informationen finden Sie in diesem Artikel: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="e0c01-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="e0c01-327">Fügen Sie die **TraceActionFilter.cs** -Datei aus **/Sources/Assets** in den Ordner **/Filters** ein.</span><span class="sxs-lookup"><span data-stu-id="e0c01-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="e0c01-328">Dieser benutzerdefinierte Aktionsfilter führt ASP.net-Ablauf Verfolgung aus.</span><span class="sxs-lookup"><span data-stu-id="e0c01-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="e0c01-329">Sie können &quot;ASP.NET MVC 4-Filter für lokale und dynamische Aktionen&quot; Lab überprüfen, um weitere Informationen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="e0c01-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="e0c01-330">Fügen Sie dem Projekt im Ordner **/Filters.** die leere **FilterProvider.cs** -Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="e0c01-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="e0c01-331">Fügen Sie die Namespaces **System. Web. MVC** und **Microsoft. Practices. unity** in **FilterProvider.cs**hinzu.</span><span class="sxs-lookup"><span data-stu-id="e0c01-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="e0c01-332">(Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex03-Filter Anbieter hinzufügen von Namespaces*)</span><span class="sxs-lookup"><span data-stu-id="e0c01-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="e0c01-333">Erbt die Klasse von der **ifilterprovider** -Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="e0c01-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="e0c01-334">Fügen Sie in der Klasse **filterprovider** eine **IUnityContainer** -Eigenschaft hinzu, und erstellen Sie dann einen Klassenkonstruktor, um den Container zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="e0c01-335">(Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex03-Filter anbieterkonstruktor*)</span><span class="sxs-lookup"><span data-stu-id="e0c01-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="e0c01-336">Der filteranbieterklassenkonstruktor erstellt kein **Neues** Objekt in.</span><span class="sxs-lookup"><span data-stu-id="e0c01-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="e0c01-337">Der Container wird als Parameter übergeben, und die Abhängigkeit wird von Unity gelöst.</span><span class="sxs-lookup"><span data-stu-id="e0c01-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="e0c01-338">Implementieren Sie in der **filterprovider** -Klasse die **getFilters** -Methode von der **ifilterprovider** -Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="e0c01-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="e0c01-339">(Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex03-Filter Anbieter getFilters*)</span><span class="sxs-lookup"><span data-stu-id="e0c01-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="e0c01-340">Aufgabe 2: registrieren und Aktivieren des Filters</span><span class="sxs-lookup"><span data-stu-id="e0c01-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="e0c01-341">In dieser Aufgabe aktivieren Sie die Standort Nachverfolgung.</span><span class="sxs-lookup"><span data-stu-id="e0c01-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="e0c01-342">Zu diesem Zweck registrieren Sie den Filter in der **Bootstrapper.cs buildunitycontainer** -Methode, um die Ablauf Verfolgung zu starten:</span><span class="sxs-lookup"><span data-stu-id="e0c01-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="e0c01-343">Öffnen Sie die Datei " **Web. config** " im Stammverzeichnis des Projekts, und aktivieren Sie die Ablauf Verfolgungs Verfolgung in der Gruppe "System</span><span class="sxs-lookup"><span data-stu-id="e0c01-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="e0c01-344">Öffnen Sie **Bootstrapper.cs** im Projektstamm.</span><span class="sxs-lookup"><span data-stu-id="e0c01-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="e0c01-345">Fügen Sie einen Verweis auf den **mvcmusicstore. Filters** -Namespace hinzu.</span><span class="sxs-lookup"><span data-stu-id="e0c01-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="e0c01-346">(Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex03-Bootstrapper Hinzufügen von Namespaces*)</span><span class="sxs-lookup"><span data-stu-id="e0c01-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="e0c01-347">Wählen Sie die **buildunitycontainer** -Methode aus, und registrieren Sie den Filter im Unity-Container.</span><span class="sxs-lookup"><span data-stu-id="e0c01-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="e0c01-348">Sie müssen den Filter Anbieter und den Aktionsfilter registrieren.</span><span class="sxs-lookup"><span data-stu-id="e0c01-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="e0c01-349">(Code Ausschnitt- *ASP.net-Abhängigkeitsinjektion Lab-Ex03-Register filterprovider und Action Filter*)</span><span class="sxs-lookup"><span data-stu-id="e0c01-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="e0c01-350">Aufgabe 3: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e0c01-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="e0c01-351">In dieser Aufgabe führen Sie die Anwendung aus und testen, ob der benutzerdefinierte Aktionsfilter die Aktivität verfolgt:</span><span class="sxs-lookup"><span data-stu-id="e0c01-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="e0c01-352">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="e0c01-353">Klicken Sie im Menü Genres auf **Rock** .</span><span class="sxs-lookup"><span data-stu-id="e0c01-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="e0c01-354">Wenn Sie möchten, können Sie nach weiteren Genres suchen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="e0c01-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span><span class="sxs-lookup"><span data-stu-id="e0c01-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="e0c01-356">*Music Store*</span><span class="sxs-lookup"><span data-stu-id="e0c01-356">*Music Store*</span></span>
3. <span data-ttu-id="e0c01-357">Navigieren Sie zu **/Trace.axd** , um die Seite für die Anwendungs Ablauf Verfolgung anzuzeigen, und klicken Sie dann auf **Details anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="e0c01-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="e0c01-358">![Anwendungs Ablauf Verfolgungs Protokoll](aspnet-mvc-4-dependency-injection/_static/image12.png "Anwendungs Ablauf Verfolgungs Protokoll")</span><span class="sxs-lookup"><span data-stu-id="e0c01-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="e0c01-359">*Anwendungs Ablauf Verfolgungs Protokoll*</span><span class="sxs-lookup"><span data-stu-id="e0c01-359">*Application Trace Log*</span></span>

    <span data-ttu-id="e0c01-360">![Anwendungs Ablauf Verfolgung: Anforderungs Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Anwendungs Ablauf Verfolgung: Anforderungs Details")</span><span class="sxs-lookup"><span data-stu-id="e0c01-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="e0c01-361">*Anwendungs Ablauf Verfolgung: Anforderungs Details*</span><span class="sxs-lookup"><span data-stu-id="e0c01-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="e0c01-362">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="e0c01-362">Close the browser.</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e0c01-363">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="e0c01-363">Summary</span></span>

<span data-ttu-id="e0c01-364">Wenn Sie diese praktische Übungseinheit durcharbeiten, haben Sie gelernt, wie Sie die Abhängigkeitsinjektion in ASP.NET MVC 4 verwenden, indem Sie Unity mithilfe eines nuget-Pakets integrieren.</span><span class="sxs-lookup"><span data-stu-id="e0c01-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="e0c01-365">Um dies zu erreichen, haben Sie Abhängigkeitsinjektion innerhalb von Controllern, Ansichten und Aktions Filtern verwendet.</span><span class="sxs-lookup"><span data-stu-id="e0c01-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="e0c01-366">Die folgenden Konzepte wurden behandelt:</span><span class="sxs-lookup"><span data-stu-id="e0c01-366">The following concepts were covered:</span></span>

- <span data-ttu-id="e0c01-367">ASP.NET MVC 4-Abhängigkeits einschleusungs Features</span><span class="sxs-lookup"><span data-stu-id="e0c01-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="e0c01-368">Unity-Integration mit dem Unity. Mvc3-nuget-Paket</span><span class="sxs-lookup"><span data-stu-id="e0c01-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="e0c01-369">Abhängigkeitsinjektion in Controller</span><span class="sxs-lookup"><span data-stu-id="e0c01-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="e0c01-370">Abhängigkeitsinjektion in Sichten</span><span class="sxs-lookup"><span data-stu-id="e0c01-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="e0c01-371">Abhängigkeitsinjektion von Aktions Filtern</span><span class="sxs-lookup"><span data-stu-id="e0c01-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="e0c01-372">Anhang A: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="e0c01-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="e0c01-373">Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren.</span><span class="sxs-lookup"><span data-stu-id="e0c01-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="e0c01-374">Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="e0c01-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="e0c01-375">Wechseln Sie zu [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="e0c01-375">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="e0c01-376">Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Windows Azure SDK</em>&quot;suchen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="e0c01-377">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="e0c01-377">Click on **Install Now**.</span></span> <span data-ttu-id="e0c01-378">Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.</span><span class="sxs-lookup"><span data-stu-id="e0c01-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="e0c01-379">Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="e0c01-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="e0c01-380">![Installieren von Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Installieren von Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="e0c01-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="e0c01-381">*Installieren von Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="e0c01-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="e0c01-382">Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="e0c01-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="e0c01-384">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="e0c01-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="e0c01-385">Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="e0c01-385">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="e0c01-387">*Installationsfortschritt*</span><span class="sxs-lookup"><span data-stu-id="e0c01-387">*Installation progress*</span></span>
6. <span data-ttu-id="e0c01-388">Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-388">When the installation completes, click **Finish**.</span></span>

    ![Installation abgeschlossen](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="e0c01-390">*Installation abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="e0c01-390">*Installation completed*</span></span>
7. <span data-ttu-id="e0c01-391">Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .</span><span class="sxs-lookup"><span data-stu-id="e0c01-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="e0c01-392">Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .</span><span class="sxs-lookup"><span data-stu-id="e0c01-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express für Web-Kachel](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="e0c01-394">*VS Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="e0c01-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="e0c01-395">Anhang B: Verwenden von Code Ausschnitten</span><span class="sxs-lookup"><span data-stu-id="e0c01-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="e0c01-396">Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="e0c01-397">Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="e0c01-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="e0c01-398">![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-dependency-injection/_static/image19.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")</span><span class="sxs-lookup"><span data-stu-id="e0c01-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="e0c01-399">*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="e0c01-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="e0c01-400">***So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)***</span><span class="sxs-lookup"><span data-stu-id="e0c01-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="e0c01-401">Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="e0c01-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="e0c01-402">Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).</span><span class="sxs-lookup"><span data-stu-id="e0c01-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="e0c01-403">Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.</span><span class="sxs-lookup"><span data-stu-id="e0c01-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="e0c01-404">Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="e0c01-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="e0c01-405">Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.</span><span class="sxs-lookup"><span data-stu-id="e0c01-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="e0c01-406">![Beginnen Sie mit der Eingabe des Ausschnitt namens.](aspnet-mvc-4-dependency-injection/_static/image20.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")</span><span class="sxs-lookup"><span data-stu-id="e0c01-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="e0c01-407">*Beginnen Sie mit der Eingabe des Ausschnitt namens.*</span><span class="sxs-lookup"><span data-stu-id="e0c01-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="e0c01-408">![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](aspnet-mvc-4-dependency-injection/_static/image21.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")</span><span class="sxs-lookup"><span data-stu-id="e0c01-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="e0c01-409">*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*</span><span class="sxs-lookup"><span data-stu-id="e0c01-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="e0c01-410">![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](aspnet-mvc-4-dependency-injection/_static/image22.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")</span><span class="sxs-lookup"><span data-stu-id="e0c01-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="e0c01-411">*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*</span><span class="sxs-lookup"><span data-stu-id="e0c01-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="e0c01-412">So ***fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="e0c01-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="e0c01-413">Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="e0c01-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="e0c01-414">Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.</span><span class="sxs-lookup"><span data-stu-id="e0c01-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="e0c01-415">Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="e0c01-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="e0c01-416">![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](aspnet-mvc-4-dependency-injection/_static/image23.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")</span><span class="sxs-lookup"><span data-stu-id="e0c01-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="e0c01-417">*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*</span><span class="sxs-lookup"><span data-stu-id="e0c01-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="e0c01-418">![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](aspnet-mvc-4-dependency-injection/_static/image24.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")</span><span class="sxs-lookup"><span data-stu-id="e0c01-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="e0c01-419">*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*</span><span class="sxs-lookup"><span data-stu-id="e0c01-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
