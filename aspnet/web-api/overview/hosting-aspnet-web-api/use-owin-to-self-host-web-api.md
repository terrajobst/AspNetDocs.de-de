---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Verwenden von owin für die Self-Host-ASP.net-Web-API-ASP.NET 4. x
author: rick-anderson
description: Tutorial mit Code, in dem gezeigt wird, wie ASP.net-Web-API in einer Konsolenanwendung gehostet wird.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 872b931391a63ef82b96e5b264c070c0b5e9605d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448329"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a><span data-ttu-id="0adf8-103">Verwenden von owin für Self-Host-ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="0adf8-103">Use OWIN to Self-Host ASP.NET Web API</span></span> 

> <span data-ttu-id="0adf8-104">In diesem Tutorial wird gezeigt, wie Sie ASP.net-Web-API in einer Konsolenanwendung hosten, indem Sie owin zum Selbsthosten des Web-API-Frameworks verwenden.</span><span class="sxs-lookup"><span data-stu-id="0adf8-104">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="0adf8-105">[Open Web Interface for .net](http://owin.org) (owin) definiert eine Abstraktion zwischen .net-Webservern und Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="0adf8-105">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="0adf8-106">Owin entkoppelt die Webanwendung vom Server, wodurch owin ideal für das selbst Hosting einer Webanwendung in Ihrem eigenen Prozess außerhalb von IIS geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="0adf8-106">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0adf8-107">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="0adf8-107">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="0adf8-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="0adf8-108">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/) 
> - <span data-ttu-id="0adf8-109">Web-API 5.2.7</span><span class="sxs-lookup"><span data-stu-id="0adf8-109">Web API 5.2.7</span></span>

> [!NOTE]
> <span data-ttu-id="0adf8-110">Den gesamten Quellcode für dieses Tutorial finden Sie unter [GitHub.com/ASPNET/Samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span><span class="sxs-lookup"><span data-stu-id="0adf8-110">You can find the complete source code for this tutorial at [github.com/aspnet/samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="0adf8-111">Erstellen einer Konsolenanwendung</span><span class="sxs-lookup"><span data-stu-id="0adf8-111">Create a console application</span></span>

<span data-ttu-id="0adf8-112">Klicken Sie im Menü **Datei** auf **neu**, und wählen Sie dann **Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="0adf8-112">On the **File** menu,  **New**, then select **Project**.</span></span> <span data-ttu-id="0adf8-113">Wählen **Sie unter** **C#Visual**den **Windows-Desktop** aus, und wählen Sie dann **Konsolen-app (.NET Framework)** aus.</span><span class="sxs-lookup"><span data-stu-id="0adf8-113">From **Installed**, under **Visual C#**, select **Windows Desktop** and then select **Console App (.Net Framework)**.</span></span> <span data-ttu-id="0adf8-114">Nennen Sie das Projekt "owinselfhostsample", und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="0adf8-114">Name the project "OwinSelfhostSample" and select **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="0adf8-115">Hinzufügen der Web-API und der owin-Pakete</span><span class="sxs-lookup"><span data-stu-id="0adf8-115">Add the Web API and OWIN packages</span></span>

<span data-ttu-id="0adf8-116">Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="0adf8-116">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="0adf8-117">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="0adf8-117">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="0adf8-118">Dadurch wird das WebAPI owin SelfHost-Paket und alle erforderlichen owin-Pakete installiert.</span><span class="sxs-lookup"><span data-stu-id="0adf8-118">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="0adf8-119">Web-API für Self-Host konfigurieren</span><span class="sxs-lookup"><span data-stu-id="0adf8-119">Configure Web API for self-host</span></span>

<span data-ttu-id="0adf8-120">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie / **Klasse** **Hinzufügen** , um eine neue Klasse hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="0adf8-120">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="0adf8-121">Geben Sie der Klassen den Namen `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0adf8-121">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="0adf8-122">Ersetzen Sie den gesamten Code in dieser Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0adf8-122">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="0adf8-123">Hinzufügen eines Web-API-Controllers</span><span class="sxs-lookup"><span data-stu-id="0adf8-123">Add a Web API controller</span></span>

<span data-ttu-id="0adf8-124">Fügen Sie als nächstes eine Web-API-Controller Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="0adf8-124">Next, add a Web API controller class.</span></span> <span data-ttu-id="0adf8-125">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie / **Klasse** **Hinzufügen** , um eine neue Klasse hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="0adf8-125">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="0adf8-126">Geben Sie der Klassen den Namen `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="0adf8-126">Name the class `ValuesController`.</span></span>

<span data-ttu-id="0adf8-127">Ersetzen Sie den gesamten Code in dieser Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0adf8-127">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a><span data-ttu-id="0adf8-128">Starten Sie den owin-Host, und stellen Sie eine Anforderung mit httpclient her.</span><span class="sxs-lookup"><span data-stu-id="0adf8-128">Start the OWIN Host and make a request with HttpClient</span></span>

<span data-ttu-id="0adf8-129">Ersetzen Sie den gesamten Code in der Program.cs-Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0adf8-129">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a><span data-ttu-id="0adf8-130">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0adf8-130">Run the application</span></span>

<span data-ttu-id="0adf8-131">Drücken Sie in Visual Studio F5, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0adf8-131">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="0adf8-132">Die Ausgabe sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="0adf8-132">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="0adf8-133">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="0adf8-133">Additional resources</span></span>

[<span data-ttu-id="0adf8-134">Übersicht über das Katana-Projekt</span><span class="sxs-lookup"><span data-stu-id="0adf8-134">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="0adf8-135">Hosten von ASP.net-Web-API in einer Azure-workerrolle</span><span class="sxs-lookup"><span data-stu-id="0adf8-135">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
