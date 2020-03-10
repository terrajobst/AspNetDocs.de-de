---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Erstellen einer odata V4-Client-C#app () | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: a0016cf2cc7bffe6268664395ccb38e140090310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448119"
---
# <a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="002e0-102">Erstellen einer OData v4-Client-App (C#)</span><span class="sxs-lookup"><span data-stu-id="002e0-102">Create an OData v4 Client App (C#)</span></span>

<span data-ttu-id="002e0-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="002e0-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="002e0-104">Im vorherigen Tutorial haben Sie einen grundlegenden odata-Dienst erstellt, der CRUD-Vorgänge unterstützt.</span><span class="sxs-lookup"><span data-stu-id="002e0-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="002e0-105">Nun erstellen wir einen Client für den Dienst.</span><span class="sxs-lookup"><span data-stu-id="002e0-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="002e0-106">Starten Sie eine neue Instanz von Visual Studio, und erstellen Sie ein neues Konsolen Anwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="002e0-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="002e0-107">Wählen Sie im Dialogfeld **Neues Projekt** die Option **installierte** &gt; **Vorlagen** &gt;  **C# Visual** &gt; **Windows-Desktop**aus, und wählen Sie die Vorlage **Konsolenanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="002e0-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="002e0-108">Nennen Sie das Projekt &quot;productapp&quot;.</span><span class="sxs-lookup"><span data-stu-id="002e0-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="002e0-109">Sie können auch die Konsolen-App zur gleichen Visual Studio-Projekt Mappe hinzufügen, die den odata-Dienst enthält.</span><span class="sxs-lookup"><span data-stu-id="002e0-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>

## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="002e0-110">Installieren des odata-Client Code-Generators</span><span class="sxs-lookup"><span data-stu-id="002e0-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="002e0-111">Wählen Sie im Menü **Extras** die Option **Erweiterungen und Updates** aus.</span><span class="sxs-lookup"><span data-stu-id="002e0-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="002e0-112">Wählen Sie **Online** &gt; **Visual Studio Gallery**aus.</span><span class="sxs-lookup"><span data-stu-id="002e0-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="002e0-113">Suchen Sie im Suchfeld nach &quot;odata-Client Code-Generator&quot;.</span><span class="sxs-lookup"><span data-stu-id="002e0-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="002e0-114">Klicken Sie zum Installieren der VSIX auf **herunterladen** .</span><span class="sxs-lookup"><span data-stu-id="002e0-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="002e0-115">Möglicherweise werden Sie aufgefordert, Visual Studio neu zu starten.</span><span class="sxs-lookup"><span data-stu-id="002e0-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="002e0-116">Lokales Ausführen des odata-Diensts</span><span class="sxs-lookup"><span data-stu-id="002e0-116">Run the OData Service Locally</span></span>

<span data-ttu-id="002e0-117">Führen Sie das productservice-Projekt in Visual Studio aus.</span><span class="sxs-lookup"><span data-stu-id="002e0-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="002e0-118">Standardmäßig wird von Visual Studio ein Browser zum Stammverzeichnis der Anwendung gestartet.</span><span class="sxs-lookup"><span data-stu-id="002e0-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="002e0-119">Beachten Sie den URI. Dies wird im nächsten Schritt benötigt.</span><span class="sxs-lookup"><span data-stu-id="002e0-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="002e0-120">Unterbrechen Sie die Ausführung der Anwendung nicht.</span><span class="sxs-lookup"><span data-stu-id="002e0-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="002e0-121">Wenn Sie beide Projekte in dieselbe Projekt Mappe einfügen, stellen Sie sicher, dass das productservice-Projekt ohne Debuggen ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="002e0-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="002e0-122">Im nächsten Schritt müssen Sie den Dienst weiterhin ausführen, während Sie das Konsolen Anwendungsprojekt ändern.</span><span class="sxs-lookup"><span data-stu-id="002e0-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>

## <a name="generate-the-service-proxy"></a><span data-ttu-id="002e0-123">Generieren des Dienst Proxys</span><span class="sxs-lookup"><span data-stu-id="002e0-123">Generate the Service Proxy</span></span>

<span data-ttu-id="002e0-124">Der Dienst Proxy ist eine .NET-Klasse, die Methoden für den Zugriff auf den odata-Dienst definiert.</span><span class="sxs-lookup"><span data-stu-id="002e0-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="002e0-125">Der Proxy übersetzt Methodenaufrufe in HTTP-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="002e0-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="002e0-126">Sie erstellen die Proxy Klasse, indem Sie eine [T4-Vorlage](https://msdn.microsoft.com/library/bb126445.aspx)ausführen.</span><span class="sxs-lookup"><span data-stu-id="002e0-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="002e0-127">Klicken Sie mit der rechten Maustaste auf das Projekt.</span><span class="sxs-lookup"><span data-stu-id="002e0-127">Right-click the project.</span></span> <span data-ttu-id="002e0-128">Wählen Sie **Hinzufügen** &gt; **Neues Element** aus.</span><span class="sxs-lookup"><span data-stu-id="002e0-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="002e0-129">Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option  **C# visuelle Elemente** &gt; **Code** &gt; **odata-Client**aus.</span><span class="sxs-lookup"><span data-stu-id="002e0-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="002e0-130">Benennen Sie die Vorlage &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="002e0-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="002e0-131">Klicken Sie auf **Hinzufügen** , und klicken Sie auf die Sicherheitswarnung.</span><span class="sxs-lookup"><span data-stu-id="002e0-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="002e0-132">An diesem Punkt erhalten Sie eine Fehlermeldung, die Sie ignorieren können.</span><span class="sxs-lookup"><span data-stu-id="002e0-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="002e0-133">Visual Studio führt die Vorlage automatisch aus, aber die Vorlage benötigt zunächst einige Konfigurationseinstellungen.</span><span class="sxs-lookup"><span data-stu-id="002e0-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="002e0-134">Öffnen Sie die Datei "productclient. odata. config". Fügen Sie im `Parameter`-Element den URI aus dem productservice-Projekt (vorheriger Schritt) ein.</span><span class="sxs-lookup"><span data-stu-id="002e0-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="002e0-135">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="002e0-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="002e0-136">Führen Sie die Vorlage erneut aus.</span><span class="sxs-lookup"><span data-stu-id="002e0-136">Run the template again.</span></span> <span data-ttu-id="002e0-137">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Datei ProductClient.tt, und wählen Sie **benutzerdefiniertes Tool**</span><span class="sxs-lookup"><span data-stu-id="002e0-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="002e0-138">Die Vorlage erstellt eine Codedatei mit dem Namen ProductClient.cs, die den Proxy definiert.</span><span class="sxs-lookup"><span data-stu-id="002e0-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="002e0-139">Wenn Sie beim Entwickeln der APP den odata-Endpunkt ändern, führen Sie die Vorlage erneut aus, um den Proxy zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="002e0-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="002e0-140">Verwenden des Dienst Proxys, um den odata-Dienst aufzurufen</span><span class="sxs-lookup"><span data-stu-id="002e0-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="002e0-141">Öffnen Sie die Datei Program.cs, und ersetzen Sie den Code Bausteine durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="002e0-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="002e0-142">Ersetzen Sie den Wert von *ServiceUri* durch den Dienst-URI von früheren Versionen.</span><span class="sxs-lookup"><span data-stu-id="002e0-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="002e0-143">Wenn Sie die app ausführen, sollte Sie Folgendes ausgeben:</span><span class="sxs-lookup"><span data-stu-id="002e0-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
