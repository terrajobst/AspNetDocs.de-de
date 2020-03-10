---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Komponententests ASP.net-Web-API 2 | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieser Leitfaden und diese Anwendung veranschaulichen, wie Sie einfache Komponententests für Ihre Web-API 2-Anwendung erstellen. In diesem Tutorial wird gezeigt, wie Sie einen Komponenten Test-proj einschließen...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f2d60b977475e048a3a74aabff4adc768ee22baf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446985"
---
# <a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="2e1f0-104">Komponententests ASP.net-Web-API 2</span><span class="sxs-lookup"><span data-stu-id="2e1f0-104">Unit Testing ASP.NET Web API 2</span></span>

<span data-ttu-id="2e1f0-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2e1f0-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="2e1f0-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="2e1f0-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="2e1f0-107">Dieser Leitfaden und diese Anwendung veranschaulichen, wie Sie einfache Komponententests für Ihre Web-API 2-Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="2e1f0-108">In diesem Tutorial wird gezeigt, wie Sie ein Komponenten Testprojekt in die Projekt Mappe einschließen und Testmethoden schreiben, mit denen die zurückgegebenen Werte von einer Controller Methode überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
>
> <span data-ttu-id="2e1f0-109">In diesem Tutorial wird davon ausgegangen, dass Sie mit den grundlegenden Konzepten von ASP.net-Web-API vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="2e1f0-110">Ein Einführ Endes Tutorial finden Sie unter [Getting Started with ASP.net-Web-API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="2e1f0-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> <span data-ttu-id="2e1f0-111">Die Komponententests in diesem Thema sind absichtlich auf einfache Daten Szenarien beschränkt.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="2e1f0-112">Weitere Informationen zu Komponententests für erweiterte Daten Szenarios finden Sie unter [Entity Framework bei Unittests ASP.net-Web-API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="2e1f0-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2e1f0-113">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="2e1f0-113">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="2e1f0-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2e1f0-114">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="2e1f0-115">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="2e1f0-115">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="2e1f0-116">In diesem Thema</span><span class="sxs-lookup"><span data-stu-id="2e1f0-116">In this topic</span></span>

<span data-ttu-id="2e1f0-117">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="2e1f0-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="2e1f0-118">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="2e1f0-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="2e1f0-119">Code herunterladen</span><span class="sxs-lookup"><span data-stu-id="2e1f0-119">Download code</span></span>](#download)
- [<span data-ttu-id="2e1f0-120">Erstellen einer Anwendung mit einem Komponenten Testprojekt</span><span class="sxs-lookup"><span data-stu-id="2e1f0-120">Create application with unit test project</span></span>](#appwithunittest)
    - [<span data-ttu-id="2e1f0-121">Komponenten Testprojekt beim Erstellen der Anwendung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="2e1f0-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="2e1f0-122">Hinzufügen eines Komponenten Testprojekts zu einer vorhandenen Anwendung</span><span class="sxs-lookup"><span data-stu-id="2e1f0-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="2e1f0-123">Einrichten der Web-API 2-Anwendung</span><span class="sxs-lookup"><span data-stu-id="2e1f0-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="2e1f0-124">Installieren von nuget-Paketen im Testprojekt</span><span class="sxs-lookup"><span data-stu-id="2e1f0-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="2e1f0-125">Erstellen von Tests</span><span class="sxs-lookup"><span data-stu-id="2e1f0-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="2e1f0-126">Tests ausführen</span><span class="sxs-lookup"><span data-stu-id="2e1f0-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="2e1f0-127">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="2e1f0-127">Prerequisites</span></span>

<span data-ttu-id="2e1f0-128">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional oder Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="2e1f0-128">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="2e1f0-129">Code herunterladen</span><span class="sxs-lookup"><span data-stu-id="2e1f0-129">Download code</span></span>

<span data-ttu-id="2e1f0-130">Laden Sie das [abgeschlossene Projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)herunter.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="2e1f0-131">Das herunterladbare Projekt enthält Komponenten Testcode für dieses Thema und für den [Entity Framework bei Komponententests ASP.net-Web-API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) .</span><span class="sxs-lookup"><span data-stu-id="2e1f0-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="2e1f0-132">Erstellen einer Anwendung mit einem Komponenten Testprojekt</span><span class="sxs-lookup"><span data-stu-id="2e1f0-132">Create application with unit test project</span></span>

<span data-ttu-id="2e1f0-133">Sie können entweder ein Komponenten Testprojekt erstellen, wenn Sie die Anwendung erstellen oder einer vorhandenen Anwendung ein Komponenten Testprojekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="2e1f0-134">Dieses Tutorial zeigt beide Methoden zum Erstellen eines Komponenten Testprojekts.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="2e1f0-135">Um diesem Tutorial zu folgen, können Sie beide Ansätze verwenden.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="2e1f0-136">Komponenten Testprojekt beim Erstellen der Anwendung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="2e1f0-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="2e1f0-137">Erstellen Sie eine neue ASP.NET-Webanwendung mit dem Namen **storeapp**.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![Erstellen des Projekts](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="2e1f0-139">Wählen Sie in den neuen ASP.net-Projekt Fenstern die **leere** Vorlage aus, und fügen Sie Ordner und Kern Verweise für die Web-API hinzu.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="2e1f0-140">Wählen Sie die Option Komponenten **Tests hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="2e1f0-141">Das Komponenten Testprojekt wird automatisch mit dem Namen **storeapp. Tests**benannt.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="2e1f0-142">Sie können diesen Namen beibehalten.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-142">You can keep this name.</span></span>

![Komponenten Testprojekt erstellen](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="2e1f0-144">Nachdem Sie die Anwendung erstellt haben, werden Sie sehen, dass Sie zwei Projekte enthält.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-144">After creating the application, you will see it contains two projects.</span></span>

![zwei Projekte](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="2e1f0-146">Hinzufügen eines Komponenten Testprojekts zu einer vorhandenen Anwendung</span><span class="sxs-lookup"><span data-stu-id="2e1f0-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="2e1f0-147">Wenn Sie das Komponenten Testprojekt nicht erstellt haben, als Sie die Anwendung erstellt haben, können Sie es jederzeit hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="2e1f0-148">Nehmen wir beispielsweise an, dass Sie bereits eine Anwendung mit dem Namen storeapp haben und Komponententests hinzufügen möchten.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="2e1f0-149">Um ein Komponenten Testprojekt hinzuzufügen, klicken Sie mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **Hinzufügen** und **Neues Projekt**</span><span class="sxs-lookup"><span data-stu-id="2e1f0-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Neues Projekt zur Projekt Mappe hinzufügen](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="2e1f0-151">Wählen Sie im linken Bereich **Test** aus, und wählen Sie Komponenten **Test Projekt** für den Projekttyp aus.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="2e1f0-152">Nennen Sie das Projekt **storeapp. Tests**.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-152">Name the project **StoreApp.Tests**.</span></span>

![Komponenten Testprojekt hinzufügen](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="2e1f0-154">Das Komponenten Testprojekt wird in der Projekt Mappe angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="2e1f0-155">Fügen Sie im Komponenten Testprojekt einen Projekt Verweis zum ursprünglichen Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="2e1f0-156">Einrichten der Web-API 2-Anwendung</span><span class="sxs-lookup"><span data-stu-id="2e1f0-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="2e1f0-157">Fügen Sie im Projekt storeapp dem Ordner **Models** eine Klassendatei mit dem Namen **Product.cs**hinzu.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="2e1f0-158">Ersetzen Sie den Inhalt der Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="2e1f0-159">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-159">Build the solution.</span></span>

<span data-ttu-id="2e1f0-160">Klicken Sie mit der rechten Maustaste auf den Ordner Controller, und wählen Sie **Hinzufügen** und **Neues Gerüst Element**aus.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="2e1f0-161">Wählen Sie **Web-API 2-Controller-leer**aus.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-161">Select **Web API 2 Controller - Empty**.</span></span>

![neuen Controller hinzufügen](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="2e1f0-163">Legen Sie den Controller Namen auf **simpleproductcontroller**fest, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![Controller angeben](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="2e1f0-165">Ersetzen Sie den vorhandenen Code durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2e1f0-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="2e1f0-166">Um dieses Beispiel zu vereinfachen, werden die Daten in einer Liste und nicht in einer Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="2e1f0-167">Die in dieser Klasse definierte Liste stellt die Produktionsdaten dar.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="2e1f0-168">Beachten Sie, dass der Controller einen Konstruktor enthält, der als Parameter eine Liste von Produkt Objekten annimmt.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="2e1f0-169">Dieser Konstruktor ermöglicht Ihnen das Übergeben von Testdaten bei Unittests.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="2e1f0-170">Der Controller enthält auch zwei **Async** -Methoden, um Komponententests für asynchrone Methoden zu veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="2e1f0-171">Diese asynchronen Methoden wurden durch Aufrufen von **Task. fromresult** implementiert, um den überflüssigen Code zu minimieren, normalerweise enthalten die Methoden jedoch ressourcenintensive Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="2e1f0-172">Die GetProduct-Methode gibt eine Instanz der **ihttpactionresult** -Schnittstelle zurück.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="2e1f0-173">Ihttpactionresult ist eines der neuen Features in der Web-API 2 und vereinfacht die Entwicklung von Komponententests.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="2e1f0-174">Klassen, die die ihttpactionresult-Schnittstelle implementieren, finden Sie im [System. Web. http. results](https://msdn.microsoft.com/library/system.web.http.results.aspx) -Namespace.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="2e1f0-175">Diese Klassen stellen mögliche Antworten aus einer Aktions Anforderung dar und entsprechen HTTP-Statuscodes.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="2e1f0-176">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-176">Build the solution.</span></span>

<span data-ttu-id="2e1f0-177">Sie sind jetzt bereit, das Testprojekt einzurichten.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="2e1f0-178">Installieren von nuget-Paketen im Testprojekt</span><span class="sxs-lookup"><span data-stu-id="2e1f0-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="2e1f0-179">Wenn Sie die leere Vorlage zum Erstellen einer Anwendung verwenden, enthält das Komponenten Testprojekt (storeapp. Tests) keine installierten nuget-Pakete.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="2e1f0-180">Andere Vorlagen (z. b. die Web-API-Vorlage) enthalten einige nuget-Pakete in das Komponenten Testprojekt.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="2e1f0-181">Für dieses Tutorial müssen Sie das Microsoft ASP.net Web-API 2-Kern Paket in das Testprojekt einschließen.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="2e1f0-182">Klicken Sie mit der rechten Maustaste auf das Projekt storeapp. Tests und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="2e1f0-183">Sie müssen das Projekt storeapp. Tests auswählen, um die Pakete dem Projekt hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Verwalten von Paketen](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="2e1f0-185">Suchen und installieren Sie Microsoft ASP.net Web-API 2-Kernpaket.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Installieren des Web-API-Kernpakets](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="2e1f0-187">Schließen Sie das Fenster nuget-Pakete verwalten.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="2e1f0-188">Tests erstellen</span><span class="sxs-lookup"><span data-stu-id="2e1f0-188">Create tests</span></span>

<span data-ttu-id="2e1f0-189">Standardmäßig enthält das Testprojekt eine leere Testdatei mit dem Namen UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="2e1f0-190">Diese Datei zeigt die Attribute, die Sie zum Erstellen von Testmethoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="2e1f0-191">Für die Komponententests können Sie diese Datei verwenden oder eine eigene Datei erstellen.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="2e1f0-193">In diesem Tutorial erstellen Sie eine eigene Testklasse.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="2e1f0-194">Sie können die Datei UnitTest1.cs löschen.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="2e1f0-195">Fügen Sie eine Klasse mit dem Namen **TestSimpleProductController.cs**hinzu, und ersetzen Sie den Code durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="2e1f0-196">Tests durchführen</span><span class="sxs-lookup"><span data-stu-id="2e1f0-196">Run tests</span></span>

<span data-ttu-id="2e1f0-197">Sie sind jetzt bereit, die Tests auszuführen.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-197">You are now ready to run the tests.</span></span> <span data-ttu-id="2e1f0-198">Alle Methoden, die mit dem **TestMethod** -Attribut markiert sind, werden getestet.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="2e1f0-199">Führen Sie die Tests über das Menü Element **Test** aus.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-199">From the **Test** menu item, run the tests.</span></span>

![Tests durchführen](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="2e1f0-201">Öffnen Sie das Fenster **Test-Explorer** , und sehen Sie sich die Ergebnisse der Tests an.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![Testergebnisse](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="2e1f0-203">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="2e1f0-203">Summary</span></span>

<span data-ttu-id="2e1f0-204">Sie haben dieses Lernprogramm abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-204">You have completed this tutorial.</span></span> <span data-ttu-id="2e1f0-205">Die Daten in diesem Tutorial wurden absichtlich vereinfacht, um sich auf Komponenten Testbedingungen zu konzentrieren.</span><span class="sxs-lookup"><span data-stu-id="2e1f0-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="2e1f0-206">Weitere Informationen zu Komponententests für erweiterte Daten Szenarios finden Sie unter [Entity Framework bei Unittests ASP.net-Web-API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="2e1f0-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
