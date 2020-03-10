---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Verwenden von signalr mit Web-Apps in Azure App Service | Microsoft-Dokumentation
author: bradygaster
description: In diesem Dokument wird beschrieben, wie Sie eine signalr-Anwendung konfigurieren, die auf Microsoft Azure ausgeführt wird. Im Tutorial verwendete Software Versionen Visual Studio 2013 oder VIS...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 0e6a18bdbe9cc47e89b5a458753845afb53595f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450183"
---
# <a name="using-signalr-with-web-apps-in-azure-app-service"></a>Verwenden von SignalR mit Web-Apps in Azure App Service

von [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Dokument wird beschrieben, wie Sie eine signalr-Anwendung konfigurieren, die auf Microsoft Azure ausgeführt wird.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) oder Visual Studio 2012
> - .NET 4.5
> - Signalr Version 2
> - Azure SDK 2,3 für Visual Studio 2013 oder 2012
>
>
>
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
>
> Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten. Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), in [StackOverflow.com](http://stackoverflow.com/)oder in den [Microsoft Azure Foren](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform)veröffentlichen.

## <a name="table-of-contents"></a>Inhaltsverzeichnis

- [Einführung](#introduction)
- [Bereitstellen einer signalr-Web-App für Azure App Service](#deploying)
- [Aktivieren von websockets auf Azure App Service](#websocket)
- [Verwenden der Azure redis Cache Backplane](#backplane)
- [Next Steps](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Einführung

ASP.net signalr kann verwendet werden, um eine neue Ebene der Interaktivität zwischen Servern und Web-oder .NET-Clients zu bieten. Wenn Sie in Azure gehostet werden, können signalr-Anwendungen die hoch verfügbare, skalierbare und leistungsfähige Umgebung nutzen, die in der Cloud ausgeführt wird.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Bereitstellen einer signalr-Web-App für Azure App Service

Signalr fügt keine speziellen Komplikationen zum Bereitstellen einer Anwendung in Azure und zum Bereitstellen auf einem lokalen Server hinzu. Eine Anwendung, die signalr verwendet, kann in Azure ohne Änderungen an der Konfiguration oder anderen Einstellungen gehostet werden (für die websocketunterstützung finden Sie unter [Aktivieren von websockets auf Azure App Service weiter](#websocket) unten). In diesem Tutorial stellen Sie die Anwendung bereit, die Sie im [Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) zu den ersten Schritten in Azure erstellt haben.

**Erforderliche Komponenten**

- Visual Studio 2013. Wenn Sie nicht über Visual Studio verfügen, ist Visual Studio 2013 Express für Web in der Installation des Azure SDK enthalten.
- [Azure SDK 2,3 für Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) oder [Azure SDK 2,3 für Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Sie benötigen ein Azure-Abonnement, um dieses Lernprogramm auszuführen. Sie können [Ihre MSDN-Abonnenten Vorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)oder [sich für ein Testabonnement registrieren](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Bereitstellen einer signalr-Web-App in Azure

1. Vervollständigen Sie das [Tutorial "Getting Started](../getting-started/tutorial-getting-started-with-signalr.md)", oder laden Sie das fertige Projekt aus der [Code Galerie](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)herunter.
2. Wählen Sie in Visual Studio die Option **Erstellen**und dann **signalr Chat veröffentlichen**aus.
3. Wählen Sie im Dialogfeld "Web veröffentlichen" die Option "Windows Azure-Websites" aus.

    ![Auswählen von Azure-Websites](using-signalr-with-azure-web-sites/_static/image1.png)
4. Wenn Sie nicht bei Ihrem Microsoft-Konto angemeldet sind, klicken Sie im Dialogfeld "vorhandene Website auswählen" auf **anmelden...** , und melden Sie sich an.

    ![Vorhandene Website auswählen](using-signalr-with-azure-web-sites/_static/image2.png)    ![Anmelden bei Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. Klicken Sie im Dialogfeld "vorhandene Website auswählen" auf **neu**.

    ![Neue Website](using-signalr-with-azure-web-sites/_static/image4.png)
6. Geben Sie im Dialogfeld "Website in Windows Azure erstellen" einen eindeutigen APP-Namen ein. Wählen Sie in der Dropdown Liste Region die nächstgelegene Region aus. Klicken Sie auf **Erstellen**.

    ![Site auf Azure erstellen](using-signalr-with-azure-web-sites/_static/image5.png)
7. Klicken Sie im Dialogfeld "Web veröffentlichen" auf **veröffentlichen**.

    ![Website veröffentlichen](using-signalr-with-azure-web-sites/_static/image6.png)
8. Wenn die Veröffentlichung der APP abgeschlossen ist, wird die signalr Chat-Anwendung, die in Azure App Service-Web-Apps gehostet wird, in einem Browser geöffnet.

    ![Öffnen von Websites in einem Browser](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Aktivieren von websockets für Azure App Service-Web-Apps

Websockets müssen in Ihrer Web-App explizit aktiviert sein, damit Sie in einer signalr-Anwendung verwendet werden können. Andernfalls werden andere Protokolle verwendet (Weitere Informationen finden Sie unter [Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports) ).

Um websockets für Azure App Service Web-Apps zu verwenden, aktivieren Sie diese im Konfigurations Abschnitt der Web-App. Öffnen Sie hierzu Ihre Web-App im Azure- [Verwaltungsportal](https://manage.windowsazure.com/), und wählen Sie konfigurieren aus.

![Registerkarte „Konfigurieren“](using-signalr-with-azure-web-sites/_static/image8.png)

Stellen Sie oben auf der Konfigurationsseite sicher, dass .NET 4,5 für Ihre Web-App verwendet wird.

![Einstellung der .NET Framework-Version 4,5](using-signalr-with-azure-web-sites/_static/image9.png)

Wählen Sie auf der Seite Konfiguration in der Einstellung **websockets** die Option **ein aus.**

![Websockets-Einstellung: ein](using-signalr-with-azure-web-sites/_static/image10.png)

Wählen Sie unten auf der Seite Konfiguration die Option **Speichern** aus, um die Änderungen zu speichern.

![Einstellungen speichern](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Verwenden der Azure redis Cache Backplane

Wenn Sie für Ihre Web-App mehrere Instanzen verwenden und die Benutzer dieser Instanzen miteinander interagieren müssen (damit beispielsweise Chat-Nachrichten, die in einer Instanz erstellt werden, die mit anderen Instanzen verbundenen Benutzer erreichen können), muss die [Azure redis Cache Rückwand](../performance/scaleout-with-redis.md) in der Anwendung implementiert werden.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Web-Apps in Azure App Service finden Sie unter [Übersicht über Web-Apps](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
