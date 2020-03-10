---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Horizontale Skalierung von signalr mit Azure Service Bus | Microsoft-Dokumentation
author: bradygaster
description: Die in diesem Thema verwendeten Software Versionen Visual Studio 2013 .NET 4,5 signalr Version 2 vorherige Versionen dieses Themas für die signalr 1. x-Version dieses Themas,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 73ed95c5027f57c7e390069dcb36b18a3714973f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467733"
---
# <a name="signalr-scaleout-with-azure-service-bus"></a>Horizontale Skalierung in SignalR mit Azure Service Bus

von [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

In diesem Tutorial stellen Sie eine signalr-Anwendung für eine Windows Azure-webrolle bereit und verwenden die Service Bus Rückwand, um Nachrichten an jede Rollen Instanz zu verteilen. (Sie können die Service Bus-Rückwand auch mit [Web-Apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/)verwenden.)

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Erforderliche Komponenten:

- Ein Windows Azure-Konto.
- Das [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012 oder 2013.

Die Service Bus-Rückwand ist auch mit [Service Bus für Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), Version 1,1, kompatibel. Es ist jedoch nicht kompatibel mit Version 1,0 von Service Bus für Windows Server.

## <a name="pricing"></a>Preise

Die Service Bus Rückwand verwendet Themen zum Senden von Nachrichten. Die neuesten Preisinformationen finden Sie unter [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). Zum Zeitpunkt der Erstellung dieses Artikels können Sie 1 Million Nachrichten pro Monat für weniger als $1 senden. Die Rückwand sendet eine Service Bus-Nachricht für jeden Aufruf einer signalr-hubmethode. Außerdem gibt es einige Steuerungs Meldungen für Verbindungen, Trennungen, das beitreten zu Gruppen und so weiter. In den meisten Anwendungen ist der Großteil des Nachrichtenverkehrs hubmethoden Aufrufe.

## <a name="overview"></a>Übersicht

Bevor wir zum detaillierten Tutorial gelangen, finden Sie hier einen kurzen Überblick über die Vorgehensweise.

1. Verwenden Sie das Windows-Azure-Portal, um einen neuen Service Bus-Namespace zu erstellen.
2. Fügen Sie die folgenden nuget-Pakete zu Ihrer Anwendung hinzu: 

    - [Microsoft. Aspnet. signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. Aspnet. signalr. ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) oder [Microsoft. Aspnet. signalr. ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)
3. Erstellen Sie eine signalr-Anwendung.
4. Fügen Sie Startup.cs den folgenden Code hinzu, um die Backplane zu konfigurieren: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Dieser Code konfiguriert die Rückwand mit den Standardwerten für [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) und [maxqueuelength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Weitere Informationen zum Ändern dieser Werte finden Sie unter [signalr Performance: ScaleOut Metrics](signalr-performance.md#scaleout_metrics).

Wählen Sie für jede Anwendung einen anderen Wert für "yourappname" aus. Verwenden Sie nicht denselben Wert für mehrere Anwendungen.

## <a name="create-the-azure-services"></a>Erstellen der Azure-Dienste

Erstellen Sie einen clouddienst, wie unter [Erstellen und Bereitstellen eines clouddiensts](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)beschrieben. Befolgen Sie die Schritte im Abschnitt "Gewusst wie: Erstellen eines clouddiensts mit schneller Fassung". Für dieses Tutorial müssen Sie kein Zertifikat hochladen.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Erstellen Sie einen neuen Service Bus Namespace, wie unter Vorgehens [Weise beim Verwenden von Service Bus Themen/Abonnements](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)beschrieben. Befolgen Sie die Schritte im Abschnitt "Erstellen eines Dienst Namespace".

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Stellen Sie sicher, dass Sie die gleiche Region für den clouddienst und den Service Bus-Namespace auswählen.

## <a name="create-the-visual-studio-project"></a>Erstellen des Visual Studio-Projekts

Starten Sie Visual Studio. Klicken Sie im Menü **Datei** auf **Neues Projekt**.

Erweitern Sie im Dialogfeld **Neues Projekt** den **Eintrag C#Visual** . Wählen Sie unter **installierte Vorlagen**die Option **Cloud** , und wählen Sie dann **Windows Azure-clouddienst**aus. Behalten Sie die Standard .NET Framework 4,5 bei. Nennen Sie die Anwendung Chatservice, und klicken Sie auf **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

Wählen Sie im Dialogfeld **neuer Windows Azure-clouddienst** die Option ASP.net Web Role aus. Klicken Sie auf die Schaltfläche mit dem Pfeil nach rechts ( **&gt;** ), um die Rolle der Projekt Mappe hinzuzufügen.

Zeigen Sie mit der Maus auf die neue Rolle, damit das Stift Symbol sichtbar ist. Klicken Sie auf dieses Symbol, um die Rolle umzubenennen. Benennen Sie die Rolle "signalrchat", und klicken Sie auf **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die Option **MVC**aus, und klicken Sie auf OK.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Der Projekt-Assistent erstellt zwei Projekte:

- Chatservice: dieses Projekt ist die Windows Azure-Anwendung. Dabei werden die Azure-Rollen und andere Konfigurationsoptionen definiert.
- Signalrchat: dieses Projekt ist Ihr ASP.NET MVC 5-Projekt.

## <a name="create-the-signalr-chat-application"></a>Erstellen der signalr Chat-Anwendung

Um die Chat-Anwendung zu erstellen, führen Sie die Schritte im Tutorial erstellen [von signalr und MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)aus.

Verwenden Sie nuget, um die erforderlichen Bibliotheken zu installieren. Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus. Geben Sie im Fenster **Paket-Manager-Konsole** die folgenden Befehle ein:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Verwenden Sie die Option `-ProjectName`, um die Pakete für das MVC-Projekt ASP.net anstelle des Windows Azure-Projekts zu installieren.

## <a name="configure-the-backplane"></a>Konfigurieren der Backplane

Fügen Sie in der Startup.cs-Datei Ihrer Anwendung den folgenden Code hinzu:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Nun müssen Sie Ihre Service Bus-Verbindungs Zeichenfolge erhalten. Wählen Sie im Azure-Portal den Service Bus-Namespace aus, den Sie erstellt haben, und klicken Sie auf das Symbol Zugriffsschlüssel.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

Kopieren Sie die Verbindungs Zeichenfolge in die Zwischenablage, und fügen Sie Sie in die *ConnectionString* -Variable ein.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Bereitstellung in Azure

Erweitern Sie in Projektmappen-Explorer den Ordner **Rollen** im Projekt Chatservice.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Klicken Sie mit der rechten Maustaste auf die Rolle signalrchat, und wählen Sie **Eigenschaften**. Wählen Sie die Registerkarte **Konfiguration** aus. Wählen Sie unter **Instanzen** 2 aus. Sie können auch die Größe des virtuellen Computers auf " **Extra Small**" festlegen.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Speichern Sie die Änderungen.

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt Chatservice. Wählen Sie **Veröffentlichen**.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Wenn Sie das erste Mal in Windows Azure veröffentlichen, müssen Sie Ihre Anmelde Informationen herunterladen. Klicken Sie im **Veröffentlichungs** -Assistenten auf "anmelden, um Anmelde Informationen herunterzuladen". Dadurch werden Sie aufgefordert, sich bei der Windows-Azure-Portal anzumelden und eine Datei mit Veröffentlichungs Einstellungen herunterzuladen.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Klicken Sie auf **importieren** , und wählen Sie die heruntergeladene Datei mit den Veröffentlichungs Einstellungen aus.

Klicken Sie auf **Weiter**. Wählen Sie im Dialogfeld **Veröffentlichungs Einstellungen** unter **clouddienst**den clouddienst aus, den Sie zuvor erstellt haben.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Klicken Sie auf **Veröffentlichen**. Es kann einige Minuten dauern, bis die Anwendung bereitgestellt und die VMs gestartet werden.

Wenn Sie nun die Chatanwendung ausführen, kommunizieren die Rollen Instanzen über Azure Service Bus mithilfe eines Service Bus Themas. Ein Thema ist eine Nachrichten Warteschlange, die mehrere Abonnenten zulässt.

Die Rückwand erstellt automatisch das Thema und die Abonnements. Um die Abonnements und Nachrichten Aktivität anzuzeigen, öffnen Sie die Azure-Portal, wählen Sie den Service Bus-Namespace aus, und klicken Sie auf "Topics".

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

Es dauert einige Minuten, bis die Nachrichten Aktivität im Dashboard angezeigt wird.

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

Signalr verwaltet die Themen Lebensdauer. Wenn Ihre Anwendung bereitgestellt ist, versuchen Sie nicht, die Themen manuell zu löschen oder die Einstellungen für das Thema zu ändern.

## <a name="troubleshooting"></a>Problembehandlung

**System. InvalidOperationException "der einzige unterstützte IsolationLevel ist ' IsolationLevel. serialisierbar '."**

Dieser Fehler kann auftreten, wenn die Transaktionsebene für einen Vorgang auf einen anderen Wert als `Serializable`festgelegt ist. Vergewissern Sie sich, dass keine Vorgänge mit anderen Transaktions Ebenen ausgeführt werden.
