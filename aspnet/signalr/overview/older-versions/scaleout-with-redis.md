---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Horizontale Skalierung von signalr mit redis (signalr 1. x) | Microsoft-Dokumentation
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 5079cf3fa773faeac911eddc75dc66e94ad98bb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431205"
---
# <a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="89f0f-102">Horizontale Skalierung in SignalR mit Redis (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="89f0f-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>

<span data-ttu-id="89f0f-103">von [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="89f0f-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="89f0f-104">In diesem Tutorial wird [redis](http://redis.io/) verwendet, um Nachrichten über eine signalr-Anwendung zu verteilen, die auf zwei separaten IIS-Instanzen bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="89f0f-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="89f0f-105">Redis ist ein Schlüssel-Wert-Speicher im Arbeitsspeicher.</span><span class="sxs-lookup"><span data-stu-id="89f0f-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="89f0f-106">Außerdem wird ein Messaging System mit einem veröffentlichen/abonnieren-Modell unterstützt.</span><span class="sxs-lookup"><span data-stu-id="89f0f-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="89f0f-107">Die signalr redis-Rückwand verwendet die Pub/Sub-Funktion, um Nachrichten an andere Server weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="89f0f-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="89f0f-108">In diesem Tutorial verwenden Sie drei Server:</span><span class="sxs-lookup"><span data-stu-id="89f0f-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="89f0f-109">Zwei Server, auf denen Windows ausgeführt wird und die Sie zum Bereitstellen einer signalr-Anwendung verwenden.</span><span class="sxs-lookup"><span data-stu-id="89f0f-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="89f0f-110">Ein Server, auf dem Linux ausgeführt wird und der zum Ausführen von redis verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="89f0f-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="89f0f-111">Für die Screenshots in diesem Tutorial habe ich Ubuntu 12,04 TLS verwendet.</span><span class="sxs-lookup"><span data-stu-id="89f0f-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="89f0f-112">Wenn Sie nicht über drei physische Server verfügen, können Sie VMS auf Hyper-V erstellen.</span><span class="sxs-lookup"><span data-stu-id="89f0f-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="89f0f-113">Eine weitere Möglichkeit ist das Erstellen von VMS in Azure.</span><span class="sxs-lookup"><span data-stu-id="89f0f-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="89f0f-114">Obwohl in diesem Tutorial die offizielle redis-Implementierung verwendet wird, gibt es auch einen [Windows-Port von redis](https://github.com/MSOpenTech/redis) von msopretech.</span><span class="sxs-lookup"><span data-stu-id="89f0f-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="89f0f-115">Setup und Konfiguration unterscheiden sich, aber andernfalls sind die Schritte identisch.</span><span class="sxs-lookup"><span data-stu-id="89f0f-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="89f0f-116">Die horizontale Skalierung von signalr mit redis bietet keine Unterstützung für redis-Cluster.</span><span class="sxs-lookup"><span data-stu-id="89f0f-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>

## <a name="overview"></a><span data-ttu-id="89f0f-117">Übersicht</span><span class="sxs-lookup"><span data-stu-id="89f0f-117">Overview</span></span>

<span data-ttu-id="89f0f-118">Bevor wir zum detaillierten Tutorial gelangen, finden Sie hier einen kurzen Überblick über die Vorgehensweise.</span><span class="sxs-lookup"><span data-stu-id="89f0f-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="89f0f-119">Installieren Sie redis, und starten Sie den redis-Server.</span><span class="sxs-lookup"><span data-stu-id="89f0f-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="89f0f-120">Fügen Sie die folgenden nuget-Pakete zu Ihrer Anwendung hinzu:</span><span class="sxs-lookup"><span data-stu-id="89f0f-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="89f0f-121">Microsoft. Aspnet. signalr</span><span class="sxs-lookup"><span data-stu-id="89f0f-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="89f0f-122">Microsoft. Aspnet. signalr. redis</span><span class="sxs-lookup"><span data-stu-id="89f0f-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="89f0f-123">Erstellen Sie eine signalr-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="89f0f-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="89f0f-124">Fügen Sie "Global. asax" den folgenden Code hinzu, um die Backplane zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="89f0f-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="89f0f-125">Ubuntu unter Hyper-V</span><span class="sxs-lookup"><span data-stu-id="89f0f-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="89f0f-126">Mithilfe von Windows Hyper-V können Sie problemlos einen virtuellen Ubuntu-Computer unter Windows Server erstellen.</span><span class="sxs-lookup"><span data-stu-id="89f0f-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="89f0f-127">Laden Sie Ubuntu ISO aus [http://www.ubuntu.com](http://www.ubuntu.com/)herunter.</span><span class="sxs-lookup"><span data-stu-id="89f0f-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="89f0f-128">Fügen Sie in Hyper-V einen neuen virtuellen Computer hinzu.</span><span class="sxs-lookup"><span data-stu-id="89f0f-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="89f0f-129">Wählen Sie im Schritt **virtuelle Festplatte verbinden** die Option **virtuelle Festplatte erstellen**aus.</span><span class="sxs-lookup"><span data-stu-id="89f0f-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="89f0f-130">Wählen Sie im Schritt **Installationsoptionen die Option** **Abbild Datei (. ISO)** aus, klicken Sie auf **Durchsuchen**, und navigieren Sie zur Ubuntu-Installations-ISO.</span><span class="sxs-lookup"><span data-stu-id="89f0f-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="89f0f-131">Installieren von redis</span><span class="sxs-lookup"><span data-stu-id="89f0f-131">Install Redis</span></span>

<span data-ttu-id="89f0f-132">Führen Sie die Schritte unter [http://redis.io/download](http://redis.io/download) aus, um redis herunterzuladen und zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="89f0f-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="89f0f-133">Dadurch werden die redis-Binärdateien im `src` Verzeichnis erstellt.</span><span class="sxs-lookup"><span data-stu-id="89f0f-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="89f0f-134">Standardmäßig ist für redis kein Kennwort erforderlich.</span><span class="sxs-lookup"><span data-stu-id="89f0f-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="89f0f-135">Um ein Kennwort festzulegen, bearbeiten Sie die Datei `redis.conf`, die sich im Stammverzeichnis des Quellcodes befindet.</span><span class="sxs-lookup"><span data-stu-id="89f0f-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="89f0f-136">(Erstellen Sie eine Sicherungskopie der Datei, bevor Sie Sie bearbeiten!) Fügen Sie die folgende Direktive `redis.conf`hinzu:</span><span class="sxs-lookup"><span data-stu-id="89f0f-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="89f0f-137">Starten Sie jetzt den redis-Server:</span><span class="sxs-lookup"><span data-stu-id="89f0f-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="89f0f-138">Öffnen Sie Port 6379. Dies ist der Standardport, über den redis lauscht.</span><span class="sxs-lookup"><span data-stu-id="89f0f-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="89f0f-139">(Sie können die Portnummer in der Konfigurationsdatei ändern.)</span><span class="sxs-lookup"><span data-stu-id="89f0f-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="89f0f-140">Erstellen der signalr-Anwendung</span><span class="sxs-lookup"><span data-stu-id="89f0f-140">Create the SignalR Application</span></span>

<span data-ttu-id="89f0f-141">Erstellen Sie eine signalr-Anwendung, indem Sie eines der folgenden Tutorials befolgen:</span><span class="sxs-lookup"><span data-stu-id="89f0f-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="89f0f-142">Einstieg in signalr</span><span class="sxs-lookup"><span data-stu-id="89f0f-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="89f0f-143">Einstieg in signalr und MVC 4</span><span class="sxs-lookup"><span data-stu-id="89f0f-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="89f0f-144">Als nächstes ändern wir die Chat-Anwendung, um die horizontale Skalierung mit redis zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="89f0f-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="89f0f-145">Fügen Sie zunächst dem Projekt das signalr. redis-nuget-Paket hinzu.</span><span class="sxs-lookup"><span data-stu-id="89f0f-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="89f0f-146">Klicken Sie in Visual Studio im **Menü Extras** auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="89f0f-146">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="89f0f-147">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="89f0f-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="89f0f-148">Öffnen Sie als nächstes die Datei "Global. asax".</span><span class="sxs-lookup"><span data-stu-id="89f0f-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="89f0f-149">Fügen Sie den folgenden Code zum **\_Start** -Methode der Anwendung hinzu:</span><span class="sxs-lookup"><span data-stu-id="89f0f-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="89f0f-150">"Server" ist der Name des Servers, auf dem redis ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="89f0f-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="89f0f-151">*Port* ist die Portnummer</span><span class="sxs-lookup"><span data-stu-id="89f0f-151">*port* is the port number</span></span>
- <span data-ttu-id="89f0f-152">"Password" ist das Kennwort, das Sie in der Datei "redis. conf" definiert haben.</span><span class="sxs-lookup"><span data-stu-id="89f0f-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="89f0f-153">"Appname" ist eine beliebige Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="89f0f-153">"AppName" is any string.</span></span> <span data-ttu-id="89f0f-154">Signalr erstellt einen redis-Pub/Sub-Channel mit diesem Namen.</span><span class="sxs-lookup"><span data-stu-id="89f0f-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="89f0f-155">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="89f0f-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="89f0f-156">Bereitstellen und Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="89f0f-156">Deploy and Run the Application</span></span>

<span data-ttu-id="89f0f-157">Bereiten Sie Ihre Windows Server-Instanzen für die Bereitstellung der signalr-Anwendung vor.</span><span class="sxs-lookup"><span data-stu-id="89f0f-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="89f0f-158">Fügen Sie die IIS-Rolle hinzu.</span><span class="sxs-lookup"><span data-stu-id="89f0f-158">Add the IIS role.</span></span> <span data-ttu-id="89f0f-159">Umfasst Funktionen für die Anwendungsentwicklung, einschließlich des WebSocket-Protokolls.</span><span class="sxs-lookup"><span data-stu-id="89f0f-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="89f0f-160">Schließen Sie auch den Verwaltungsdienst ein (unter "Verwaltungs Tools" aufgeführt).</span><span class="sxs-lookup"><span data-stu-id="89f0f-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="89f0f-161">**Installieren Sie Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="89f0f-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="89f0f-162">Wenn Sie IIS-Manager ausführen, werden Sie aufgefordert, die Microsoft-Webplattform zu installieren, oder Sie können [das Installationsprogramm herunterladen](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="89f0f-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="89f0f-163">Suchen Sie im Platt Form Installationsprogramm nach Web deploy, und installieren Sie Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="89f0f-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="89f0f-164">Überprüfen Sie, ob der Webverwaltungsdienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="89f0f-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="89f0f-165">Wenn dies nicht der Wert ist, starten Sie den Dienst.</span><span class="sxs-lookup"><span data-stu-id="89f0f-165">If not, start the service.</span></span> <span data-ttu-id="89f0f-166">(Wenn der Webverwaltungsdienst in der Liste der Windows-Dienste nicht angezeigt wird, stellen Sie sicher, dass Sie den-Verwaltungsdienst installiert haben, als Sie die IIS-Rolle hinzugefügt haben.)</span><span class="sxs-lookup"><span data-stu-id="89f0f-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="89f0f-167">Standardmäßig lauscht der Webverwaltungsdienst an TCP-Port 8172.</span><span class="sxs-lookup"><span data-stu-id="89f0f-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="89f0f-168">Erstellen Sie in der Windows-Firewall eine neue eingehende Regel, um TCP-Datenverkehr an Port 8172 zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="89f0f-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="89f0f-169">Weitere Informationen finden Sie unter [Konfigurieren von Firewallregeln](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="89f0f-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="89f0f-170">(Wenn Sie die VMs in Azure verwenden, können Sie dies direkt in der Azure-Portal tun.</span><span class="sxs-lookup"><span data-stu-id="89f0f-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="89f0f-171">Weitere Informationen finden Sie [unter Einrichten von Endpunkten für einen virtuellen Computer](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="89f0f-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="89f0f-172">Nun können Sie das Visual Studio-Projekt von Ihrem Entwicklungs Computer auf dem Server bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="89f0f-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="89f0f-173">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Lösung, und klicken Sie auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="89f0f-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="89f0f-174">Eine ausführlichere Dokumentation zur Webbereitstellung finden Sie unter [Webbereitstellungs-Inhalts Zuordnung für Visual Studio und ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="89f0f-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="89f0f-175">Wenn Sie die Anwendung auf zwei Servern bereitstellen, können Sie jede Instanz in einem separaten Browserfenster öffnen und sehen, dass Sie jeweils signalr-Nachrichten vom anderen empfangen.</span><span class="sxs-lookup"><span data-stu-id="89f0f-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="89f0f-176">(In einer Produktionsumgebung würden sich die beiden Server natürlich hinter einem Load Balancer befinden.)</span><span class="sxs-lookup"><span data-stu-id="89f0f-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="89f0f-177">Wenn Sie neugierig sind, welche Nachrichten an redis gesendet werden, können Sie den **redis-cli-** Client verwenden, der mit redis installiert wird.</span><span class="sxs-lookup"><span data-stu-id="89f0f-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
