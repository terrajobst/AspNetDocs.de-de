---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Komponententests von signalr-Anwendungen | Microsoft-Dokumentation
author: bradygaster
description: In diesem Artikel wird beschrieben, wie die Komponenten Testfunktionen von signalr 2,0 verwendet werden.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: 2cf2e88f141d89971439dc1fc4979849f8dded47
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467301"
---
# <a name="unit-testing-signalr-applications"></a><span data-ttu-id="7145d-103">Komponententests für SignalR-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="7145d-103">Unit Testing SignalR Applications</span></span>

<span data-ttu-id="7145d-104">von [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="7145d-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="7145d-105">In diesem Artikel wird die Verwendung der Komponenten Testfunktionen von signalr 2 beschrieben.</span><span class="sxs-lookup"><span data-stu-id="7145d-105">This article describes using the Unit Testing features of SignalR 2.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="7145d-106">In diesem Thema verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="7145d-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="7145d-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7145d-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="7145d-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="7145d-108">.NET 4.5</span></span>
> - <span data-ttu-id="7145d-109">Signalr Version 2</span><span class="sxs-lookup"><span data-stu-id="7145d-109">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="7145d-110">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="7145d-110">Questions and comments</span></span>
>
> <span data-ttu-id="7145d-111">Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten.</span><span class="sxs-lookup"><span data-stu-id="7145d-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="7145d-112">Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="7145d-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="7145d-113">Komponententests von signalr-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="7145d-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="7145d-114">Sie können die Komponenten Testfunktionen in signalr 2 verwenden, um Komponententests für Ihre signalr-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7145d-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="7145d-115">Signalr 2 enthält die [ihubcallerconnectioncontext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) -Schnittstelle, die verwendet werden kann, um ein Mock-Objekt zum Simulieren der hubmethoden für Tests zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7145d-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="7145d-116">In diesem Abschnitt fügen Sie Komponententests für die Anwendung hinzu, die Sie im [Tutorial "Getting Started](../getting-started/tutorial-getting-started-with-signalr.md) " mithilfe von [XUnit.net](https://github.com/xunit/xunit) [und der](https://github.com/Moq/moq4)-Version erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="7145d-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="7145d-117">XUnit.net wird verwendet, um den Test zu steuern. MOQ wird verwendet, um ein [Mock](http://en.wikipedia.org/wiki/Mock_object) -Objekt für Tests zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7145d-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="7145d-118">Andere Frameworks können bei Bedarf verwendet werden. [Nersatz](http://nsubstitute.github.io/) ist auch eine gute Wahl.</span><span class="sxs-lookup"><span data-stu-id="7145d-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="7145d-119">In diesem Tutorial wird veranschaulicht, wie Sie das Mock-Objekt auf zwei Arten einrichten: zuerst mithilfe eines `dynamic` Objekts (eingeführt in .NET Framework 4) und zweitens über eine Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="7145d-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="7145d-120">Inhalt</span><span class="sxs-lookup"><span data-stu-id="7145d-120">Contents</span></span>

<span data-ttu-id="7145d-121">Dieses Tutorial enthält die folgenden Abschnitte.</span><span class="sxs-lookup"><span data-stu-id="7145d-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="7145d-122">Unittests mit dynamischer</span><span class="sxs-lookup"><span data-stu-id="7145d-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="7145d-123">Komponententests nach Typ</span><span class="sxs-lookup"><span data-stu-id="7145d-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="7145d-124">Unittests mit dynamischer</span><span class="sxs-lookup"><span data-stu-id="7145d-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="7145d-125">In diesem Abschnitt fügen Sie einen Komponenten Test für die Anwendung hinzu, die im [Tutorial "Getting Started](../getting-started/tutorial-getting-started-with-signalr.md) " mithilfe eines dynamischen Objekts erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="7145d-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="7145d-126">Installieren Sie die [xUnit Runner-Erweiterung](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) für Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="7145d-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="7145d-127">Führen Sie entweder das [Tutorial zu](../getting-started/tutorial-getting-started-with-signalr.md)den ersten Schritten aus, oder laden Sie die fertige Anwendung aus der [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)herunter.</span><span class="sxs-lookup"><span data-stu-id="7145d-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="7145d-128">Wenn Sie die Download Version der Anwendung für die ersten Schritte verwenden, öffnen Sie die **Paket-Manager-Konsole** , und klicken Sie auf **Wiederherstellen** , um dem Projekt das signalr-Paket hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="7145d-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Pakete wiederherstellen](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="7145d-130">Fügen Sie der Projekt Mappe ein Projekt für den Komponenten Test hinzu.</span><span class="sxs-lookup"><span data-stu-id="7145d-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="7145d-131">Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf Ihre Projekt Mappe, und wählen Sie **Hinzufügen**, **Neues Projekt**aus. Wählen Sie **C#** unter dem Knoten den Knoten **Windows** aus.</span><span class="sxs-lookup"><span data-stu-id="7145d-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="7145d-132">Wählen Sie **Klassenbibliothek**aus.</span><span class="sxs-lookup"><span data-stu-id="7145d-132">Select **Class Library**.</span></span> <span data-ttu-id="7145d-133">Nennen Sie das neue Projekt **testlibrary** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7145d-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Test Bibliothek erstellen](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="7145d-135">Fügen Sie dem Projekt signalrchat einen Verweis im Test Bibliotheksprojekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="7145d-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="7145d-136">Klicken Sie mit der rechten Maustaste auf das Projekt **testlibrary** , und wählen Sie **Hinzufügen**, **Verweis...** . Wählen Sie **unter dem Knoten** Projekt Mappe den Knoten **Projekte** aus, und überprüfen Sie **signalrchat**.</span><span class="sxs-lookup"><span data-stu-id="7145d-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="7145d-137">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7145d-137">Click **OK**.</span></span>

    ![Projekt Verweis hinzufügen](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="7145d-139">Fügen Sie die Module signalr, muq und xUnit dem Projekt **testlibrary** hinzu.</span><span class="sxs-lookup"><span data-stu-id="7145d-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="7145d-140">Legen Sie in der **Paket-Manager-Konsole**die Dropdown Liste **Standard Projekt** auf **testlibrary**fest.</span><span class="sxs-lookup"><span data-stu-id="7145d-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="7145d-141">Führen Sie im Konsolenfenster die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="7145d-141">Run the following commands in the console window:</span></span>

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Installieren von Paketen](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="7145d-143">Erstellen Sie die Testdatei.</span><span class="sxs-lookup"><span data-stu-id="7145d-143">Create the test file.</span></span> <span data-ttu-id="7145d-144">Klicken Sie mit der rechten Maustaste auf das Projekt **testlibrary** **, und**klicken Sie auf **hinzufügen.**</span><span class="sxs-lookup"><span data-stu-id="7145d-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="7145d-145">Nennen Sie die neue Klasse **Tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="7145d-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="7145d-146">Ersetzen Sie den Inhalt von Tests.cs durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="7145d-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="7145d-147">Im obigen Code wird ein-Test Client mit dem `Mock`-Objekt aus der- [Bibliothek der](https://github.com/Moq/moq4) Bibliothek vom Typ [ihubcallerconnectioncontext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in signalr 2,1 `dynamic` für den Typparameter zugewiesen) erstellt. Die `IHubCallerConnectionContext`-Schnittstelle ist das Proxy Objekt, mit dem Sie Methoden auf dem Client aufrufen.</span><span class="sxs-lookup"><span data-stu-id="7145d-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="7145d-148">Die `broadcastMessage`-Funktion wird dann für den mockclient definiert, sodass Sie von der `ChatHub`-Klasse aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="7145d-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="7145d-149">Die Test-Engine ruft dann die `Send`-Methode der `ChatHub`-Klasse auf, die wiederum die simulierte `broadcastMessage`-Funktion aufruft.</span><span class="sxs-lookup"><span data-stu-id="7145d-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="7145d-150">Erstellen Sie die Projekt Mappe, indem Sie **F6**drücken.</span><span class="sxs-lookup"><span data-stu-id="7145d-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="7145d-151">Führen Sie den Komponententest aus.</span><span class="sxs-lookup"><span data-stu-id="7145d-151">Run the unit test.</span></span> <span data-ttu-id="7145d-152">Wählen Sie in Visual Studio **Test**, **Windows**, **Test-Explorer**aus.</span><span class="sxs-lookup"><span data-stu-id="7145d-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="7145d-153">Klicken Sie im Test-Explorer-Fenster mit der rechten Maustaste auf **hubsaremockableviadynamic** , und wählen Sie **Ausgewählte Tests ausführen**aus.</span><span class="sxs-lookup"><span data-stu-id="7145d-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Test-Explorer](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="7145d-155">Überprüfen Sie, ob der Test erfolgreich war, indem Sie den unteren Bereich im Fenster Test-Explorer aktivieren.</span><span class="sxs-lookup"><span data-stu-id="7145d-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="7145d-156">Im Fenster wird angezeigt, dass der Test erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="7145d-156">The window will show that the test passed.</span></span>

    ![Bestandene Tests](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="7145d-158">Komponententests nach Typ</span><span class="sxs-lookup"><span data-stu-id="7145d-158">Unit testing by type</span></span>

<span data-ttu-id="7145d-159">In diesem Abschnitt fügen Sie einen Test für die im [Tutorial "Getting Started](../getting-started/tutorial-getting-started-with-signalr.md) " erstellte Anwendung mithilfe einer Schnittstelle hinzu, die die zu testende Methode enthält.</span><span class="sxs-lookup"><span data-stu-id="7145d-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="7145d-160">Führen Sie die Schritte 1-7 im obigen Tutorial für Komponenten [Tests mit dynamischem](#dynamic) Lernprogramm aus.</span><span class="sxs-lookup"><span data-stu-id="7145d-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="7145d-161">Ersetzen Sie den Inhalt von Tests.cs durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="7145d-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="7145d-162">Im obigen Code wird eine Schnittstelle erstellt, die die Signatur der `broadcastMessage` Methode definiert, für die die Test-Engine einen Mock-Client erstellt.</span><span class="sxs-lookup"><span data-stu-id="7145d-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="7145d-163">Anschließend wird ein Mock-Client mithilfe des `Mock` Objekts vom Typ " [ihubcallerconnectioncontext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) " (in signalr 2,1 `dynamic` für den Typparameter zugewiesen) erstellt. Die `IHubCallerConnectionContext`-Schnittstelle ist das Proxy Objekt, mit dem Sie Methoden auf dem Client aufrufen.</span><span class="sxs-lookup"><span data-stu-id="7145d-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="7145d-164">Der Test erstellt dann eine Instanz von `ChatHub`und erstellt dann eine Pseudo Version der `broadcastMessage`-Methode, die wiederum durch Aufrufen der `Send`-Methode auf dem Hub aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="7145d-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="7145d-165">Erstellen Sie die Projekt Mappe, indem Sie **F6**drücken.</span><span class="sxs-lookup"><span data-stu-id="7145d-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="7145d-166">Führen Sie den Komponententest aus.</span><span class="sxs-lookup"><span data-stu-id="7145d-166">Run the unit test.</span></span> <span data-ttu-id="7145d-167">Wählen Sie in Visual Studio **Test**, **Windows**, **Test-Explorer**aus.</span><span class="sxs-lookup"><span data-stu-id="7145d-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="7145d-168">Klicken Sie im Test-Explorer-Fenster mit der rechten Maustaste auf **hubsaremockableviadynamic** , und wählen Sie **Ausgewählte Tests ausführen**aus.</span><span class="sxs-lookup"><span data-stu-id="7145d-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Test-Explorer](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="7145d-170">Überprüfen Sie, ob der Test erfolgreich war, indem Sie den unteren Bereich im Fenster Test-Explorer aktivieren.</span><span class="sxs-lookup"><span data-stu-id="7145d-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="7145d-171">Im Fenster wird angezeigt, dass der Test erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="7145d-171">The window will show that the test passed.</span></span>

    ![Bestandene Tests](unit-testing-signalr-applications/_static/image8.png)
