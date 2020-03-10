---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Entity Framework, wenn Komponententests ASP.net-Web-API 2 | Microsoft-Dokumentation
author: Rick-Anderson
description: Diese Anleitung und Anwendung veranschaulichen, wie Sie Komponententests für Ihre Web-API 2-Anwendung erstellen, die die Entity Framework verwendet. Es wird gezeigt, wie Sie das ändern...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 258450107ee7443c4efd43a3b8e4851249745227
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447087"
---
# <a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="403e1-104">Entity Framework, wenn Komponententests ASP.net-Web-API 2</span><span class="sxs-lookup"><span data-stu-id="403e1-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>

<span data-ttu-id="403e1-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="403e1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="403e1-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="403e1-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="403e1-107">Diese Anleitung und Anwendung veranschaulichen, wie Sie Komponententests für Ihre Web-API 2-Anwendung erstellen, die die Entity Framework verwendet.</span><span class="sxs-lookup"><span data-stu-id="403e1-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="403e1-108">Es zeigt, wie der Gerüst Controller geändert wird, um das Übergeben eines Kontext Objekts für Tests zu ermöglichen, und wie Testobjekte erstellt werden, die mit Entity Framework funktionieren.</span><span class="sxs-lookup"><span data-stu-id="403e1-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
>
> <span data-ttu-id="403e1-109">Eine Einführung in Komponententests mit ASP.net-Web-API finden Sie unter Komponenten [Tests mit ASP.net-Web-API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="403e1-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
>
> <span data-ttu-id="403e1-110">In diesem Tutorial wird davon ausgegangen, dass Sie mit den grundlegenden Konzepten von ASP.net-Web-API vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="403e1-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="403e1-111">Ein Einführ Endes Tutorial finden Sie unter [Getting Started with ASP.net-Web-API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="403e1-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="403e1-112">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="403e1-112">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="403e1-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="403e1-113">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="403e1-114">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="403e1-114">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="403e1-115">In diesem Thema</span><span class="sxs-lookup"><span data-stu-id="403e1-115">In this topic</span></span>

<span data-ttu-id="403e1-116">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="403e1-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="403e1-117">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="403e1-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="403e1-118">Code herunterladen</span><span class="sxs-lookup"><span data-stu-id="403e1-118">Download code</span></span>](#download)
- [<span data-ttu-id="403e1-119">Erstellen einer Anwendung mit einem Komponenten Testprojekt</span><span class="sxs-lookup"><span data-stu-id="403e1-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="403e1-120">Erstellen der Modell Klasse</span><span class="sxs-lookup"><span data-stu-id="403e1-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="403e1-121">Hinzufügen des Controllers</span><span class="sxs-lookup"><span data-stu-id="403e1-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="403e1-122">Abhängigkeitsinjektion hinzufügen</span><span class="sxs-lookup"><span data-stu-id="403e1-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="403e1-123">Installieren von nuget-Paketen im Testprojekt</span><span class="sxs-lookup"><span data-stu-id="403e1-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="403e1-124">Test Kontext erstellen</span><span class="sxs-lookup"><span data-stu-id="403e1-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="403e1-125">Erstellen von Tests</span><span class="sxs-lookup"><span data-stu-id="403e1-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="403e1-126">Tests ausführen</span><span class="sxs-lookup"><span data-stu-id="403e1-126">Run tests</span></span>](#runtests)

<span data-ttu-id="403e1-127">Wenn Sie die Schritte in Komponenten [Tests mit ASP.net-Web-API 2](unit-testing-with-aspnet-web-api.md)bereits ausgeführt haben, können Sie zum Abschnitt [Hinzufügen des Controllers](#controller)wechseln.</span><span class="sxs-lookup"><span data-stu-id="403e1-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="403e1-128">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="403e1-128">Prerequisites</span></span>

<span data-ttu-id="403e1-129">Visual Studio 2017 Community, Professional oder Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="403e1-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="403e1-130">Code herunterladen</span><span class="sxs-lookup"><span data-stu-id="403e1-130">Download code</span></span>

<span data-ttu-id="403e1-131">Laden Sie das [abgeschlossene Projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)herunter.</span><span class="sxs-lookup"><span data-stu-id="403e1-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="403e1-132">Das herunterladbare Projekt enthält Komponenten Testcode für dieses Thema und für das Thema Komponenten [Tests ASP.net-Web-API 2](unit-testing-with-aspnet-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="403e1-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="403e1-133">Erstellen einer Anwendung mit einem Komponenten Testprojekt</span><span class="sxs-lookup"><span data-stu-id="403e1-133">Create application with unit test project</span></span>

<span data-ttu-id="403e1-134">Sie können entweder ein Komponenten Testprojekt erstellen, wenn Sie die Anwendung erstellen oder einer vorhandenen Anwendung ein Komponenten Testprojekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="403e1-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="403e1-135">Dieses Tutorial zeigt, wie Sie ein Komponenten Testprojekt erstellen, wenn Sie die Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="403e1-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="403e1-136">Erstellen Sie eine neue ASP.NET-Webanwendung mit dem Namen **storeapp**.</span><span class="sxs-lookup"><span data-stu-id="403e1-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="403e1-137">Wählen Sie in den neuen ASP.net-Projekt Fenstern die **leere** Vorlage aus, und fügen Sie Ordner und Kern Verweise für die Web-API hinzu.</span><span class="sxs-lookup"><span data-stu-id="403e1-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="403e1-138">Wählen Sie die Option Komponenten **Tests hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="403e1-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="403e1-139">Das Komponenten Testprojekt wird automatisch mit dem Namen **storeapp. Tests**benannt.</span><span class="sxs-lookup"><span data-stu-id="403e1-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="403e1-140">Sie können diesen Namen beibehalten.</span><span class="sxs-lookup"><span data-stu-id="403e1-140">You can keep this name.</span></span>

![Komponenten Testprojekt erstellen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="403e1-142">Nachdem Sie die Anwendung erstellt haben, werden Sie sehen, dass Sie zwei Projekte enthält: **storeapp** und **storeapp. Tests**.</span><span class="sxs-lookup"><span data-stu-id="403e1-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="403e1-143">Erstellen der Modell Klasse</span><span class="sxs-lookup"><span data-stu-id="403e1-143">Create the model class</span></span>

<span data-ttu-id="403e1-144">Fügen Sie im Projekt storeapp dem Ordner **Models** eine Klassendatei mit dem Namen **Product.cs**hinzu.</span><span class="sxs-lookup"><span data-stu-id="403e1-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="403e1-145">Ersetzen Sie den Inhalt der Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="403e1-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="403e1-146">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="403e1-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="403e1-147">Hinzufügen des Controllers</span><span class="sxs-lookup"><span data-stu-id="403e1-147">Add the controller</span></span>

<span data-ttu-id="403e1-148">Klicken Sie mit der rechten Maustaste auf den Ordner Controller, und wählen Sie **Hinzufügen** und **Neues Gerüst Element**aus.</span><span class="sxs-lookup"><span data-stu-id="403e1-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="403e1-149">Wählen Sie Web-API 2-Controller mit Aktionen unter Verwendung von Entity Framework aus.</span><span class="sxs-lookup"><span data-stu-id="403e1-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![neuen Controller hinzufügen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="403e1-151">Legen Sie die folgenden Werte fest:</span><span class="sxs-lookup"><span data-stu-id="403e1-151">Set the following values:</span></span>

- <span data-ttu-id="403e1-152">Controller Name: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="403e1-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="403e1-153">Modell Klasse: **Produkt**</span><span class="sxs-lookup"><span data-stu-id="403e1-153">Model class: **Product**</span></span>
- <span data-ttu-id="403e1-154">Datenkontext Klasse: [Wählen Sie eine **neue Datenkontext** Schaltfläche aus, die die unten gezeigten Werte füllt.]</span><span class="sxs-lookup"><span data-stu-id="403e1-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![Controller angeben](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="403e1-156">Klicken Sie auf **Hinzufügen** , um den Controller mit automatisch generiertem Code zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="403e1-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="403e1-157">Der Code enthält Methoden zum Erstellen, abrufen, aktualisieren und Löschen von Instanzen der Product-Klasse.</span><span class="sxs-lookup"><span data-stu-id="403e1-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="403e1-158">Der folgende Code zeigt die-Methode für das Hinzufügen eines Produkts.</span><span class="sxs-lookup"><span data-stu-id="403e1-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="403e1-159">Beachten Sie, dass die-Methode eine Instanz von **ihttpactionresult**zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="403e1-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="403e1-160">Ihttpactionresult ist eines der neuen Features in der Web-API 2 und vereinfacht die Entwicklung von Komponententests.</span><span class="sxs-lookup"><span data-stu-id="403e1-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="403e1-161">Im nächsten Abschnitt wird der generierte Code angepasst, um das Übergeben von Testobjekten an den Controller zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="403e1-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="403e1-162">Abhängigkeitsinjektion hinzufügen</span><span class="sxs-lookup"><span data-stu-id="403e1-162">Add dependency injection</span></span>

<span data-ttu-id="403e1-163">Derzeit ist die ProductController-Klasse hart codiert, um eine Instanz der storeappcontext-Klasse zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="403e1-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="403e1-164">Sie verwenden ein Muster namens Abhängigkeitsinjektion, um die Anwendung zu ändern und diese hart codierte Abhängigkeit zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="403e1-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="403e1-165">Wenn Sie diese Abhängigkeit unterbrechen, können Sie beim Testen ein Mock-Objekt übergeben.</span><span class="sxs-lookup"><span data-stu-id="403e1-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="403e1-166">Klicken Sie mit der rechten Maustaste auf den Ordner **Modelle** , und fügen Sie eine neue Schnittstelle namens **istoreappcontext**hinzu.</span><span class="sxs-lookup"><span data-stu-id="403e1-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="403e1-167">Ersetzen Sie den Code durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="403e1-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="403e1-168">Öffnen Sie die Datei StoreAppContext.cs, und nehmen Sie die folgenden hervorgehobenen Änderungen vor.</span><span class="sxs-lookup"><span data-stu-id="403e1-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="403e1-169">Beachten Sie die folgenden wichtigen Änderungen:</span><span class="sxs-lookup"><span data-stu-id="403e1-169">The important changes to note are:</span></span>

- <span data-ttu-id="403e1-170">Storeappcontext-Klasse implementiert istoreappcontext-Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="403e1-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="403e1-171">Die markasmodified-Methode ist implementiert.</span><span class="sxs-lookup"><span data-stu-id="403e1-171">MarkAsModified method is implemented</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="403e1-172">Öffnen Sie die Datei ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="403e1-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="403e1-173">Ändern Sie den vorhandenen Code so, dass er dem markierten Code entspricht.</span><span class="sxs-lookup"><span data-stu-id="403e1-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="403e1-174">Diese Änderungen unterbrechen die Abhängigkeit von storeappcontext und ermöglichen es anderen Klassen, ein anderes Objekt für die Kontext Klasse zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="403e1-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="403e1-175">Diese Änderung ermöglicht es Ihnen, während der Komponententests einen Test Kontext zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="403e1-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="403e1-176">Es gibt eine weitere Änderung, die Sie in ProductController vornehmen müssen.</span><span class="sxs-lookup"><span data-stu-id="403e1-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="403e1-177">Ersetzen Sie in der **putproduct** -Methode die Zeile, die den Entitäts Status auf Modified festlegt, mithilfe eines Aufrufes der markasmodified-Methode.</span><span class="sxs-lookup"><span data-stu-id="403e1-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="403e1-178">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="403e1-178">Build the solution.</span></span>

<span data-ttu-id="403e1-179">Sie sind jetzt bereit, das Testprojekt einzurichten.</span><span class="sxs-lookup"><span data-stu-id="403e1-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="403e1-180">Installieren von nuget-Paketen im Testprojekt</span><span class="sxs-lookup"><span data-stu-id="403e1-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="403e1-181">Wenn Sie die leere Vorlage zum Erstellen einer Anwendung verwenden, enthält das Komponenten Testprojekt (storeapp. Tests) keine installierten nuget-Pakete.</span><span class="sxs-lookup"><span data-stu-id="403e1-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="403e1-182">Andere Vorlagen (z. b. die Web-API-Vorlage) enthalten einige nuget-Pakete in das Komponenten Testprojekt.</span><span class="sxs-lookup"><span data-stu-id="403e1-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="403e1-183">Für dieses Tutorial müssen Sie das Entity Framework-Paket und das Microsoft ASP.net Web-API 2-Kern Paket in das Testprojekt einschließen.</span><span class="sxs-lookup"><span data-stu-id="403e1-183">For this tutorial, you must include the Entity Framework package and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="403e1-184">Klicken Sie mit der rechten Maustaste auf das Projekt storeapp. Tests und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="403e1-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="403e1-185">Sie müssen das Projekt storeapp. Tests auswählen, um die Pakete dem Projekt hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="403e1-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Verwalten von Paketen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="403e1-187">Suchen Sie in den Online Paketen nach dem EntityFramework-Paket (Version 6,0 oder höher), und installieren Sie es.</span><span class="sxs-lookup"><span data-stu-id="403e1-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="403e1-188">Wenn das Paket "EntityFramework" bereits installiert ist, haben Sie möglicherweise das Projekt storeapp anstelle des Projekts storeapp. Tests ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="403e1-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Entity Framework hinzufügen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="403e1-190">Suchen und installieren Sie Microsoft ASP.net Web-API 2-Kernpaket.</span><span class="sxs-lookup"><span data-stu-id="403e1-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Installieren des Web-API-Kernpakets](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="403e1-192">Schließen Sie das Fenster nuget-Pakete verwalten.</span><span class="sxs-lookup"><span data-stu-id="403e1-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="403e1-193">Test Kontext erstellen</span><span class="sxs-lookup"><span data-stu-id="403e1-193">Create test context</span></span>

<span data-ttu-id="403e1-194">Fügen Sie dem Testprojekt eine Klasse mit dem Namen **testdbset** hinzu.</span><span class="sxs-lookup"><span data-stu-id="403e1-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="403e1-195">Diese Klasse dient als Basisklasse für das Test Dataset.</span><span class="sxs-lookup"><span data-stu-id="403e1-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="403e1-196">Ersetzen Sie den Code durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="403e1-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="403e1-197">Fügen Sie dem Testprojekt eine Klasse mit dem Namen **testproductdbset** hinzu, die den folgenden Code enthält.</span><span class="sxs-lookup"><span data-stu-id="403e1-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="403e1-198">Fügen Sie eine Klasse mit dem Namen **teststoreappcontext** hinzu, und ersetzen Sie den vorhandenen Code durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="403e1-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="403e1-199">Tests erstellen</span><span class="sxs-lookup"><span data-stu-id="403e1-199">Create tests</span></span>

<span data-ttu-id="403e1-200">Standardmäßig enthält das Testprojekt eine leere Testdatei mit dem Namen **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="403e1-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="403e1-201">Diese Datei zeigt die Attribute, die Sie zum Erstellen von Testmethoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="403e1-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="403e1-202">In diesem Tutorial können Sie diese Datei löschen, da Sie eine neue Testklasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="403e1-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="403e1-203">Fügen Sie dem Testprojekt eine Klasse mit dem Namen **testproductcontroller** hinzu.</span><span class="sxs-lookup"><span data-stu-id="403e1-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="403e1-204">Ersetzen Sie den Code durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="403e1-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="403e1-205">Tests durchführen</span><span class="sxs-lookup"><span data-stu-id="403e1-205">Run tests</span></span>

<span data-ttu-id="403e1-206">Sie sind jetzt bereit, die Tests auszuführen.</span><span class="sxs-lookup"><span data-stu-id="403e1-206">You are now ready to run the tests.</span></span> <span data-ttu-id="403e1-207">Alle Methoden, die mit dem **TestMethod** -Attribut markiert sind, werden getestet.</span><span class="sxs-lookup"><span data-stu-id="403e1-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="403e1-208">Führen Sie die Tests über das Menü Element **Test** aus.</span><span class="sxs-lookup"><span data-stu-id="403e1-208">From the **Test** menu item, run the tests.</span></span>

![Tests durchführen](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="403e1-210">Öffnen Sie das Fenster **Test-Explorer** , und sehen Sie sich die Ergebnisse der Tests an.</span><span class="sxs-lookup"><span data-stu-id="403e1-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![Testergebnisse](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
