---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Horizontale Skalierung von signalr mit SQL Server | Microsoft-Dokumentation
author: bradygaster
description: In den in diesem Thema verwendeten Software Versionen Visual Studio 2013 .NET 4,5 signalr Version 2 in früheren Versionen dieses Themas Informationen zu früheren Versionen von...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 709a9ebf8f3396842bee0d87e621c00ae1418ec1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467739"
---
# <a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="7f149-103">Horizontale Skalierung in SignalR mit SQL Server</span><span class="sxs-lookup"><span data-stu-id="7f149-103">SignalR Scaleout with SQL Server</span></span>

<span data-ttu-id="7f149-104">von [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="7f149-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="7f149-105">In diesem Thema verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="7f149-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="7f149-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7f149-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="7f149-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="7f149-107">.NET 4.5</span></span>
> - <span data-ttu-id="7f149-108">Signalr Version 2</span><span class="sxs-lookup"><span data-stu-id="7f149-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="7f149-109">Vorherige Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="7f149-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="7f149-110">Informationen zu früheren Versionen von signalr finden Sie unter [signalr ältere Versionen](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="7f149-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="7f149-111">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="7f149-111">Questions and comments</span></span>
>
> <span data-ttu-id="7f149-112">Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten.</span><span class="sxs-lookup"><span data-stu-id="7f149-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="7f149-113">Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="7f149-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="7f149-114">In diesem Tutorial verwenden Sie SQL Server zum Verteilen von Nachrichten über eine signalr-Anwendung, die in zwei separaten IIS-Instanzen bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="7f149-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="7f149-115">Sie können dieses Tutorial auch auf einem einzelnen Testcomputer ausführen, aber um die vollständige Wirkung zu erzielen, müssen Sie die signalr-Anwendung auf zwei oder mehr Servern bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="7f149-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="7f149-116">Sie müssen auch SQL Server auf einem der-Server oder auf einem separaten dedizierten Server installieren.</span><span class="sxs-lookup"><span data-stu-id="7f149-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="7f149-117">Eine weitere Möglichkeit besteht darin, das Tutorial mithilfe von VMS in Azure auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7f149-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="7f149-118">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="7f149-118">Prerequisites</span></span>

<span data-ttu-id="7f149-119">Microsoft SQL Server 2005 oder höher.</span><span class="sxs-lookup"><span data-stu-id="7f149-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="7f149-120">Die Rückwand unterstützt sowohl die Desktop-als auch die Server Edition von SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7f149-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="7f149-121">SQL Server Compact Edition oder Azure SQL-Datenbank wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="7f149-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="7f149-122">(Wenn Ihre Anwendung in Azure gehostet wird, sollten Sie stattdessen die Service Bus Rückwand in Erwägung gezogen.)</span><span class="sxs-lookup"><span data-stu-id="7f149-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="7f149-123">Übersicht</span><span class="sxs-lookup"><span data-stu-id="7f149-123">Overview</span></span>

<span data-ttu-id="7f149-124">Bevor wir zum detaillierten Tutorial gelangen, finden Sie hier einen kurzen Überblick über die Vorgehensweise.</span><span class="sxs-lookup"><span data-stu-id="7f149-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="7f149-125">Erstellen Sie eine neue leere Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7f149-125">Create a new empty database.</span></span> <span data-ttu-id="7f149-126">Die Rückwand erstellt die erforderlichen Tabellen in dieser Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7f149-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="7f149-127">Fügen Sie die folgenden nuget-Pakete zu Ihrer Anwendung hinzu:</span><span class="sxs-lookup"><span data-stu-id="7f149-127">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="7f149-128">Microsoft. Aspnet. signalr</span><span class="sxs-lookup"><span data-stu-id="7f149-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="7f149-129">Microsoft. Aspnet. signalr. SqlServer</span><span class="sxs-lookup"><span data-stu-id="7f149-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="7f149-130">Erstellen Sie eine signalr-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7f149-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="7f149-131">Fügen Sie Startup.cs den folgenden Code hinzu, um die Backplane zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="7f149-131">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="7f149-132">Dieser Code konfiguriert die Rückwand mit den Standardwerten für [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) und [maxqueuelength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="7f149-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="7f149-133">Weitere Informationen zum Ändern dieser Werte finden Sie unter [signalr Performance: ScaleOut Metrics](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="7f149-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

## <a name="configure-the-database"></a><span data-ttu-id="7f149-134">Konfigurieren der Datenbank</span><span class="sxs-lookup"><span data-stu-id="7f149-134">Configure the Database</span></span>

<span data-ttu-id="7f149-135">Entscheiden Sie, ob die Anwendung die Windows-Authentifizierung oder SQL Server Authentifizierung für den Zugriff auf die Datenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="7f149-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="7f149-136">Stellen Sie unabhängig davon sicher, dass der Datenbankbenutzer über Berechtigungen zum Anmelden, Erstellen von Schemas und Erstellen von Tabellen verfügt.</span><span class="sxs-lookup"><span data-stu-id="7f149-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="7f149-137">Erstellen Sie eine neue Datenbank, die von der Rückwand verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="7f149-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="7f149-138">Sie können der Datenbank einen beliebigen Namen übergeben.</span><span class="sxs-lookup"><span data-stu-id="7f149-138">You can give the database any name.</span></span> <span data-ttu-id="7f149-139">Sie müssen keine Tabellen in der Datenbank erstellen. die Rückwand erstellt die erforderlichen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="7f149-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="7f149-140">Aktivieren von Service Broker</span><span class="sxs-lookup"><span data-stu-id="7f149-140">Enable Service Broker</span></span>

<span data-ttu-id="7f149-141">Es wird empfohlen, Service Broker für die Rückwand-Datenbank zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="7f149-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="7f149-142">Service Broker bietet systemeigene Unterstützung für Messaging-und Warteschlangen Funktionen in SQL Server, sodass die Rückwand Updates effizienter empfangen können.</span><span class="sxs-lookup"><span data-stu-id="7f149-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="7f149-143">(Die Rückwand funktioniert jedoch auch ohne Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="7f149-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="7f149-144">Um zu überprüfen, ob Service Broker aktiviert ist, Fragen Sie die Spalte **is\_Broker\_aktivierte** Spalte in der **sys. Datenbanken** -Katalog Sicht ab.</span><span class="sxs-lookup"><span data-stu-id="7f149-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="7f149-145">Verwenden Sie die folgende SQL-Abfrage, um Service Broker zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="7f149-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="7f149-146">Wenn für diese Abfrage ein Deadlock auftritt, stellen Sie sicher, dass keine Anwendungen mit der Datenbank verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="7f149-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="7f149-147">Wenn Sie die Ablauf Verfolgung aktiviert haben, werden die Ablauf Verfolgungen auch anzeigen, ob Service Broker aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="7f149-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="7f149-148">Erstellen einer signalr-Anwendung</span><span class="sxs-lookup"><span data-stu-id="7f149-148">Create a SignalR Application</span></span>

<span data-ttu-id="7f149-149">Erstellen Sie eine signalr-Anwendung, indem Sie eines der folgenden Tutorials befolgen:</span><span class="sxs-lookup"><span data-stu-id="7f149-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="7f149-150">Einstieg in signalr 2,0</span><span class="sxs-lookup"><span data-stu-id="7f149-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="7f149-151">Einführung in signalr 2,0 und MVC 5</span><span class="sxs-lookup"><span data-stu-id="7f149-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="7f149-152">Als nächstes ändern wir die Chat-Anwendung, um das horizontale hochskalieren mit SQL Server zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="7f149-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="7f149-153">Fügen Sie zunächst das nuget-Paket signalr. SqlServer zu Ihrem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="7f149-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="7f149-154">Klicken Sie in Visual Studio im **Menü Extras** auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="7f149-154">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="7f149-155">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="7f149-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="7f149-156">Öffnen Sie als nächstes die Datei Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="7f149-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="7f149-157">Fügen Sie der **configure** -Methode den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="7f149-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="7f149-158">Bereitstellen und Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7f149-158">Deploy and Run the Application</span></span>

<span data-ttu-id="7f149-159">Bereiten Sie Ihre Windows Server-Instanzen für die Bereitstellung der signalr-Anwendung vor.</span><span class="sxs-lookup"><span data-stu-id="7f149-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="7f149-160">Fügen Sie die IIS-Rolle hinzu.</span><span class="sxs-lookup"><span data-stu-id="7f149-160">Add the IIS role.</span></span> <span data-ttu-id="7f149-161">Umfasst Funktionen für die Anwendungsentwicklung, einschließlich des WebSocket-Protokolls.</span><span class="sxs-lookup"><span data-stu-id="7f149-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="7f149-162">Schließen Sie auch den Verwaltungsdienst ein (unter "Verwaltungs Tools" aufgeführt).</span><span class="sxs-lookup"><span data-stu-id="7f149-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="7f149-163">**Installieren Sie Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="7f149-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="7f149-164">Wenn Sie IIS-Manager ausführen, werden Sie aufgefordert, die Microsoft-Webplattform zu installieren, oder Sie können [das Installationsprogramm herunterladen](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="7f149-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="7f149-165">Suchen Sie im Platt Form Installationsprogramm nach Web deploy, und installieren Sie Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="7f149-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="7f149-166">Überprüfen Sie, ob der Webverwaltungsdienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7f149-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="7f149-167">Wenn dies nicht der Wert ist, starten Sie den Dienst.</span><span class="sxs-lookup"><span data-stu-id="7f149-167">If not, start the service.</span></span> <span data-ttu-id="7f149-168">(Wenn der Webverwaltungsdienst in der Liste der Windows-Dienste nicht angezeigt wird, stellen Sie sicher, dass Sie den-Verwaltungsdienst installiert haben, als Sie die IIS-Rolle hinzugefügt haben.)</span><span class="sxs-lookup"><span data-stu-id="7f149-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="7f149-169">Öffnen Sie schließlich Port 8172 für TCP.</span><span class="sxs-lookup"><span data-stu-id="7f149-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="7f149-170">Dies ist der Port, der vom Web deploy Tool verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="7f149-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="7f149-171">Nun können Sie das Visual Studio-Projekt von Ihrem Entwicklungs Computer auf dem Server bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="7f149-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="7f149-172">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Lösung, und klicken Sie auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="7f149-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="7f149-173">Eine ausführlichere Dokumentation zur Webbereitstellung finden Sie unter [Webbereitstellungs-Inhalts Zuordnung für Visual Studio und ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="7f149-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="7f149-174">Wenn Sie die Anwendung auf zwei Servern bereitstellen, können Sie jede Instanz in einem separaten Browserfenster öffnen und sehen, dass Sie jeweils signalr-Nachrichten vom anderen empfangen.</span><span class="sxs-lookup"><span data-stu-id="7f149-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="7f149-175">(In einer Produktionsumgebung würden sich die beiden Server natürlich hinter einem Load Balancer befinden.)</span><span class="sxs-lookup"><span data-stu-id="7f149-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="7f149-176">Nachdem Sie die Anwendung ausgeführt haben, können Sie sehen, dass signalr automatisch Tabellen in der Datenbank erstellt hat:</span><span class="sxs-lookup"><span data-stu-id="7f149-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="7f149-177">Die Tabellen werden von signalr verwaltet.</span><span class="sxs-lookup"><span data-stu-id="7f149-177">SignalR manages the tables.</span></span> <span data-ttu-id="7f149-178">Solange Ihre Anwendung bereitgestellt ist, löschen Sie Zeilen nicht, ändern Sie die Tabelle usw.</span><span class="sxs-lookup"><span data-stu-id="7f149-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
