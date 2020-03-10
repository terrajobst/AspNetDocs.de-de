---
uid: signalr/overview/performance/scaleout-with-redis
title: Horizontale Skalierung von signalr mit redis | Microsoft-Dokumentation
author: bradygaster
description: In den in diesem Thema verwendeten Software Versionen Visual Studio 2013 .NET 4,5 signalr Version 2 in früheren Versionen dieses Themas Informationen zu früheren Versionen von...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 58a7affa1769523955adc76455a1c33be6f49751
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467775"
---
# <a name="signalr-scaleout-with-redis"></a><span data-ttu-id="7d7d5-103">Horizontale Skalierung in SignalR mit Redis</span><span class="sxs-lookup"><span data-stu-id="7d7d5-103">SignalR Scaleout with Redis</span></span>

<span data-ttu-id="7d7d5-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7d7d5-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="7d7d5-105">In diesem Thema verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="7d7d5-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="7d7d5-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7d7d5-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="7d7d5-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="7d7d5-107">.NET 4.5</span></span>
> - <span data-ttu-id="7d7d5-108">Signalr-Version 2,4</span><span class="sxs-lookup"><span data-stu-id="7d7d5-108">SignalR version 2.4</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="7d7d5-109">Vorherige Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="7d7d5-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="7d7d5-110">Informationen zu früheren Versionen von signalr finden Sie unter [signalr ältere Versionen](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="7d7d5-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="7d7d5-111">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="7d7d5-111">Questions and comments</span></span>
>
> <span data-ttu-id="7d7d5-112">Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="7d7d5-113">Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="7d7d5-114">In diesem Tutorial wird [redis](http://redis.io/) verwendet, um Nachrichten über eine signalr-Anwendung zu verteilen, die auf zwei separaten IIS-Instanzen bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="7d7d5-115">Redis ist ein Schlüssel-Wert-Speicher im Arbeitsspeicher.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="7d7d5-116">Außerdem wird ein Messaging System mit einem veröffentlichen/abonnieren-Modell unterstützt.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="7d7d5-117">Die signalr redis-Rückwand verwendet die Pub/Sub-Funktion, um Nachrichten an andere Server weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="7d7d5-118">In diesem Tutorial verwenden Sie drei Server:</span><span class="sxs-lookup"><span data-stu-id="7d7d5-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="7d7d5-119">Zwei Server, auf denen Windows ausgeführt wird und die Sie zum Bereitstellen einer signalr-Anwendung verwenden.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="7d7d5-120">Ein Server, auf dem Linux ausgeführt wird und der zum Ausführen von redis verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="7d7d5-121">Für die Screenshots in diesem Tutorial habe ich Ubuntu 12,04 TLS verwendet.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="7d7d5-122">Wenn Sie nicht über drei physische Server verfügen, können Sie VMS auf Hyper-V erstellen.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="7d7d5-123">Eine weitere Möglichkeit ist das Erstellen von VMS in Azure.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="7d7d5-124">Obwohl in diesem Tutorial die offizielle redis-Implementierung verwendet wird, gibt es auch einen [Windows-Port von redis](https://github.com/MSOpenTech/redis) von msopretech.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="7d7d5-125">Setup und Konfiguration unterscheiden sich, aber andernfalls sind die Schritte identisch.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE]
>
> <span data-ttu-id="7d7d5-126">Die horizontale Skalierung von signalr mit redis bietet keine Unterstützung für redis-Cluster.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>

## <a name="overview"></a><span data-ttu-id="7d7d5-127">Übersicht</span><span class="sxs-lookup"><span data-stu-id="7d7d5-127">Overview</span></span>

<span data-ttu-id="7d7d5-128">Bevor wir zum detaillierten Tutorial gelangen, finden Sie hier einen kurzen Überblick über die Vorgehensweise.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="7d7d5-129">Installieren Sie redis, und starten Sie den redis-Server.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="7d7d5-130">Fügen Sie die folgenden nuget-Pakete zu Ihrer Anwendung hinzu:</span><span class="sxs-lookup"><span data-stu-id="7d7d5-130">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="7d7d5-131">Microsoft. Aspnet. signalr</span><span class="sxs-lookup"><span data-stu-id="7d7d5-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="7d7d5-132">Microsoft. Aspnet. signalr. stackexchangeredis</span><span class="sxs-lookup"><span data-stu-id="7d7d5-132">Microsoft.AspNet.SignalR.StackExchangeRedis</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.StackExchangeRedis)
    
3. <span data-ttu-id="7d7d5-133">Erstellen Sie eine signalr-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="7d7d5-134">Fügen Sie Startup.cs den folgenden Code hinzu, um die Backplane zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="7d7d5-134">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="7d7d5-135">Ubuntu unter Hyper-V</span><span class="sxs-lookup"><span data-stu-id="7d7d5-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="7d7d5-136">Mithilfe von Windows Hyper-V können Sie problemlos einen virtuellen Ubuntu-Computer unter Windows Server erstellen.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="7d7d5-137">Laden Sie Ubuntu ISO aus [http://www.ubuntu.com](http://www.ubuntu.com/)herunter.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="7d7d5-138">Fügen Sie in Hyper-V einen neuen virtuellen Computer hinzu.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="7d7d5-139">Wählen Sie im Schritt **virtuelle Festplatte verbinden** die Option **virtuelle Festplatte erstellen**aus.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="7d7d5-140">Wählen Sie im Schritt **Installationsoptionen die Option** **Abbild Datei (. ISO)** aus, klicken Sie auf **Durchsuchen**, und navigieren Sie zur Ubuntu-Installations-ISO.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="7d7d5-141">Installieren von redis</span><span class="sxs-lookup"><span data-stu-id="7d7d5-141">Install Redis</span></span>

<span data-ttu-id="7d7d5-142">Führen Sie die Schritte unter [http://redis.io/download](http://redis.io/download) aus, um redis herunterzuladen und zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="7d7d5-143">Dadurch werden die redis-Binärdateien im `src` Verzeichnis erstellt.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="7d7d5-144">Standardmäßig ist für redis kein Kennwort erforderlich.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="7d7d5-145">Um ein Kennwort festzulegen, bearbeiten Sie die Datei `redis.conf`, die sich im Stammverzeichnis des Quellcodes befindet.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="7d7d5-146">(Erstellen Sie eine Sicherungskopie der Datei, bevor Sie Sie bearbeiten!) Fügen Sie die folgende Direktive `redis.conf`hinzu:</span><span class="sxs-lookup"><span data-stu-id="7d7d5-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="7d7d5-147">Starten Sie jetzt den redis-Server:</span><span class="sxs-lookup"><span data-stu-id="7d7d5-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="7d7d5-148">Öffnen Sie Port 6379. Dies ist der Standardport, über den redis lauscht.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="7d7d5-149">(Sie können die Portnummer in der Konfigurationsdatei ändern.)</span><span class="sxs-lookup"><span data-stu-id="7d7d5-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="7d7d5-150">Erstellen der signalr-Anwendung</span><span class="sxs-lookup"><span data-stu-id="7d7d5-150">Create the SignalR Application</span></span>

<span data-ttu-id="7d7d5-151">Erstellen Sie eine signalr-Anwendung, indem Sie eines der folgenden Tutorials befolgen:</span><span class="sxs-lookup"><span data-stu-id="7d7d5-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="7d7d5-152">Einstieg in signalr 2,0</span><span class="sxs-lookup"><span data-stu-id="7d7d5-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="7d7d5-153">Einführung in signalr 2,0 und MVC 5</span><span class="sxs-lookup"><span data-stu-id="7d7d5-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="7d7d5-154">Als nächstes ändern wir die Chat-Anwendung, um die horizontale Skalierung mit redis zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="7d7d5-155">Fügen Sie zunächst das `Microsoft.AspNet.SignalR.StackExchangeRedis` nuget-Paket zu Ihrem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-155">First, add the `Microsoft.AspNet.SignalR.StackExchangeRedis` NuGet package to your project.</span></span> <span data-ttu-id="7d7d5-156">Klicken Sie in Visual Studio im **Menü Extras** auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-156">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="7d7d5-157">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="7d7d5-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="7d7d5-158">Öffnen Sie als nächstes die Datei Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="7d7d5-159">Fügen Sie der **Konfigurations** Methode den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="7d7d5-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="7d7d5-160">"Server" ist der Name des Servers, auf dem redis ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="7d7d5-161">*Port* ist die Portnummer</span><span class="sxs-lookup"><span data-stu-id="7d7d5-161">*port* is the port number</span></span>
- <span data-ttu-id="7d7d5-162">"Password" ist das Kennwort, das Sie in der Datei "redis. conf" definiert haben.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="7d7d5-163">"Appname" ist eine beliebige Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-163">"AppName" is any string.</span></span> <span data-ttu-id="7d7d5-164">Signalr erstellt einen redis-Pub/Sub-Channel mit diesem Namen.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="7d7d5-165">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7d7d5-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="7d7d5-166">Bereitstellen und Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="7d7d5-166">Deploy and Run the Application</span></span>

<span data-ttu-id="7d7d5-167">Bereiten Sie Ihre Windows Server-Instanzen für die Bereitstellung der signalr-Anwendung vor.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="7d7d5-168">Fügen Sie die IIS-Rolle hinzu.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-168">Add the IIS role.</span></span> <span data-ttu-id="7d7d5-169">Umfasst Funktionen für die Anwendungsentwicklung, einschließlich des WebSocket-Protokolls.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="7d7d5-170">Schließen Sie auch den Verwaltungsdienst ein (unter "Verwaltungs Tools" aufgeführt).</span><span class="sxs-lookup"><span data-stu-id="7d7d5-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="7d7d5-171">**Installieren Sie Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="7d7d5-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="7d7d5-172">Wenn Sie IIS-Manager ausführen, werden Sie aufgefordert, die Microsoft-Webplattform zu installieren, oder Sie können [das Installationsprogramm herunterladen](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="7d7d5-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="7d7d5-173">Suchen Sie im Platt Form Installationsprogramm nach Web deploy, und installieren Sie Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="7d7d5-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="7d7d5-174">Überprüfen Sie, ob der Webverwaltungsdienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="7d7d5-175">Wenn dies nicht der Wert ist, starten Sie den Dienst.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-175">If not, start the service.</span></span> <span data-ttu-id="7d7d5-176">(Wenn der Webverwaltungsdienst in der Liste der Windows-Dienste nicht angezeigt wird, stellen Sie sicher, dass Sie den-Verwaltungsdienst installiert haben, als Sie die IIS-Rolle hinzugefügt haben.)</span><span class="sxs-lookup"><span data-stu-id="7d7d5-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="7d7d5-177">Standardmäßig lauscht der Webverwaltungsdienst an TCP-Port 8172.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="7d7d5-178">Erstellen Sie in der Windows-Firewall eine neue eingehende Regel, um TCP-Datenverkehr an Port 8172 zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="7d7d5-179">Weitere Informationen finden Sie unter [Konfigurieren von Firewallregeln](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="7d7d5-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="7d7d5-180">(Wenn Sie die VMs in Azure verwenden, können Sie dies direkt in der Azure-Portal tun.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="7d7d5-181">Weitere Informationen finden Sie [unter Einrichten von Endpunkten für einen virtuellen Computer](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="7d7d5-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="7d7d5-182">Nun können Sie das Visual Studio-Projekt von Ihrem Entwicklungs Computer auf dem Server bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="7d7d5-183">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Lösung, und klicken Sie auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="7d7d5-184">Eine ausführlichere Dokumentation zur Webbereitstellung finden Sie unter [Webbereitstellungs-Inhalts Zuordnung für Visual Studio und ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="7d7d5-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="7d7d5-185">Wenn Sie die Anwendung auf zwei Servern bereitstellen, können Sie jede Instanz in einem separaten Browserfenster öffnen und sehen, dass Sie jeweils signalr-Nachrichten vom anderen empfangen.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="7d7d5-186">(In einer Produktionsumgebung würden sich die beiden Server natürlich hinter einem Load Balancer befinden.)</span><span class="sxs-lookup"><span data-stu-id="7d7d5-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="7d7d5-187">Wenn Sie neugierig sind, welche Nachrichten an redis gesendet werden, können Sie den **redis-cli-** Client verwenden, der mit redis installiert wird.</span><span class="sxs-lookup"><span data-stu-id="7d7d5-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
