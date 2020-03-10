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
# <a name="signalr-scaleout-with-redis-signalr-1x"></a>Horizontale Skalierung in SignalR mit Redis (SignalR 1.x)

von [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

In diesem Tutorial wird [redis](http://redis.io/) verwendet, um Nachrichten über eine signalr-Anwendung zu verteilen, die auf zwei separaten IIS-Instanzen bereitgestellt wird.

Redis ist ein Schlüssel-Wert-Speicher im Arbeitsspeicher. Außerdem wird ein Messaging System mit einem veröffentlichen/abonnieren-Modell unterstützt. Die signalr redis-Rückwand verwendet die Pub/Sub-Funktion, um Nachrichten an andere Server weiterzuleiten.

![](scaleout-with-redis/_static/image1.png)

In diesem Tutorial verwenden Sie drei Server:

- Zwei Server, auf denen Windows ausgeführt wird und die Sie zum Bereitstellen einer signalr-Anwendung verwenden.
- Ein Server, auf dem Linux ausgeführt wird und der zum Ausführen von redis verwendet wird. Für die Screenshots in diesem Tutorial habe ich Ubuntu 12,04 TLS verwendet.

Wenn Sie nicht über drei physische Server verfügen, können Sie VMS auf Hyper-V erstellen. Eine weitere Möglichkeit ist das Erstellen von VMS in Azure.

Obwohl in diesem Tutorial die offizielle redis-Implementierung verwendet wird, gibt es auch einen [Windows-Port von redis](https://github.com/MSOpenTech/redis) von msopretech. Setup und Konfiguration unterscheiden sich, aber andernfalls sind die Schritte identisch.

> [!NOTE] 
> 
> Die horizontale Skalierung von signalr mit redis bietet keine Unterstützung für redis-Cluster.

## <a name="overview"></a>Übersicht

Bevor wir zum detaillierten Tutorial gelangen, finden Sie hier einen kurzen Überblick über die Vorgehensweise.

1. Installieren Sie redis, und starten Sie den redis-Server.
2. Fügen Sie die folgenden nuget-Pakete zu Ihrer Anwendung hinzu: 

    - [Microsoft. Aspnet. signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. Aspnet. signalr. redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Erstellen Sie eine signalr-Anwendung.
4. Fügen Sie "Global. asax" den folgenden Code hinzu, um die Backplane zu konfigurieren: 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu unter Hyper-V

Mithilfe von Windows Hyper-V können Sie problemlos einen virtuellen Ubuntu-Computer unter Windows Server erstellen.

Laden Sie Ubuntu ISO aus [http://www.ubuntu.com](http://www.ubuntu.com/)herunter.

Fügen Sie in Hyper-V einen neuen virtuellen Computer hinzu. Wählen Sie im Schritt **virtuelle Festplatte verbinden** die Option **virtuelle Festplatte erstellen**aus.

![](scaleout-with-redis/_static/image2.png)

Wählen Sie im Schritt **Installationsoptionen die Option** **Abbild Datei (. ISO)** aus, klicken Sie auf **Durchsuchen**, und navigieren Sie zur Ubuntu-Installations-ISO.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Installieren von redis

Führen Sie die Schritte unter [http://redis.io/download](http://redis.io/download) aus, um redis herunterzuladen und zu erstellen.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Dadurch werden die redis-Binärdateien im `src` Verzeichnis erstellt.

Standardmäßig ist für redis kein Kennwort erforderlich. Um ein Kennwort festzulegen, bearbeiten Sie die Datei `redis.conf`, die sich im Stammverzeichnis des Quellcodes befindet. (Erstellen Sie eine Sicherungskopie der Datei, bevor Sie Sie bearbeiten!) Fügen Sie die folgende Direktive `redis.conf`hinzu:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Starten Sie jetzt den redis-Server:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Öffnen Sie Port 6379. Dies ist der Standardport, über den redis lauscht. (Sie können die Portnummer in der Konfigurationsdatei ändern.)

## <a name="create-the-signalr-application"></a>Erstellen der signalr-Anwendung

Erstellen Sie eine signalr-Anwendung, indem Sie eines der folgenden Tutorials befolgen:

- [Einstieg in signalr](../getting-started/tutorial-getting-started-with-signalr.md)
- [Einstieg in signalr und MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

Als nächstes ändern wir die Chat-Anwendung, um die horizontale Skalierung mit redis zu unterstützen. Fügen Sie zunächst dem Projekt das signalr. redis-nuget-Paket hinzu. Klicken Sie in Visual Studio im **Menü Extras** auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Öffnen Sie als nächstes die Datei "Global. asax". Fügen Sie den folgenden Code zum **\_Start** -Methode der Anwendung hinzu:

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "Server" ist der Name des Servers, auf dem redis ausgeführt wird.
- *Port* ist die Portnummer
- "Password" ist das Kennwort, das Sie in der Datei "redis. conf" definiert haben.
- "Appname" ist eine beliebige Zeichenfolge. Signalr erstellt einen redis-Pub/Sub-Channel mit diesem Namen.

Beispiel:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Bereitstellen und Ausführen der Anwendung

Bereiten Sie Ihre Windows Server-Instanzen für die Bereitstellung der signalr-Anwendung vor.

Fügen Sie die IIS-Rolle hinzu. Umfasst Funktionen für die Anwendungsentwicklung, einschließlich des WebSocket-Protokolls.

![](scaleout-with-redis/_static/image5.png)

Schließen Sie auch den Verwaltungsdienst ein (unter "Verwaltungs Tools" aufgeführt).

![](scaleout-with-redis/_static/image6.png)

**Installieren Sie Web Deploy 3,0.** Wenn Sie IIS-Manager ausführen, werden Sie aufgefordert, die Microsoft-Webplattform zu installieren, oder Sie können [das Installationsprogramm herunterladen](https://go.microsoft.com/fwlink/?LinkId=255386). Suchen Sie im Platt Form Installationsprogramm nach Web deploy, und installieren Sie Web Deploy 3,0

![](scaleout-with-redis/_static/image7.png)

Überprüfen Sie, ob der Webverwaltungsdienst ausgeführt wird. Wenn dies nicht der Wert ist, starten Sie den Dienst. (Wenn der Webverwaltungsdienst in der Liste der Windows-Dienste nicht angezeigt wird, stellen Sie sicher, dass Sie den-Verwaltungsdienst installiert haben, als Sie die IIS-Rolle hinzugefügt haben.)

Standardmäßig lauscht der Webverwaltungsdienst an TCP-Port 8172. Erstellen Sie in der Windows-Firewall eine neue eingehende Regel, um TCP-Datenverkehr an Port 8172 zuzulassen. Weitere Informationen finden Sie unter [Konfigurieren von Firewallregeln](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Wenn Sie die VMs in Azure verwenden, können Sie dies direkt in der Azure-Portal tun. Weitere Informationen finden Sie [unter Einrichten von Endpunkten für einen virtuellen Computer](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)

Nun können Sie das Visual Studio-Projekt von Ihrem Entwicklungs Computer auf dem Server bereitstellen. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Lösung, und klicken Sie auf **veröffentlichen**.

Eine ausführlichere Dokumentation zur Webbereitstellung finden Sie unter [Webbereitstellungs-Inhalts Zuordnung für Visual Studio und ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).

Wenn Sie die Anwendung auf zwei Servern bereitstellen, können Sie jede Instanz in einem separaten Browserfenster öffnen und sehen, dass Sie jeweils signalr-Nachrichten vom anderen empfangen. (In einer Produktionsumgebung würden sich die beiden Server natürlich hinter einem Load Balancer befinden.)

![](scaleout-with-redis/_static/image8.png)

Wenn Sie neugierig sind, welche Nachrichten an redis gesendet werden, können Sie den **redis-cli-** Client verwenden, der mit redis installiert wird.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
