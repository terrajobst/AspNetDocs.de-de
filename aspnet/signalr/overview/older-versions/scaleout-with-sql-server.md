---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Horizontale Skalierung von signalr mit SQL Server (signalr 1. x) | Microsoft-Dokumentation
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 8674b06a2a751c9a7b1c6c9b19782974d0602a62
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431127"
---
# <a name="signalr-scaleout-with-sql-server-signalr-1x"></a>Horizontale Skalierung in SignalR mit SQL Server (SignalR 1.x)

von [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

In diesem Tutorial verwenden Sie SQL Server zum Verteilen von Nachrichten über eine signalr-Anwendung, die in zwei separaten IIS-Instanzen bereitgestellt wird. Sie können dieses Tutorial auch auf einem einzelnen Testcomputer ausführen, aber um die vollständige Wirkung zu erzielen, müssen Sie die signalr-Anwendung auf zwei oder mehr Servern bereitstellen. Sie müssen auch SQL Server auf einem der-Server oder auf einem separaten dedizierten Server installieren. Eine weitere Möglichkeit besteht darin, das Tutorial mithilfe von VMS in Azure auszuführen.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Voraussetzungen

Microsoft SQL Server 2005 oder höher. Die Rückwand unterstützt sowohl die Desktop-als auch die Server Edition von SQL Server. SQL Server Compact Edition oder Azure SQL-Datenbank wird nicht unterstützt. (Wenn Ihre Anwendung in Azure gehostet wird, sollten Sie stattdessen die Service Bus Rückwand in Erwägung gezogen.)

## <a name="overview"></a>Übersicht

Bevor wir zum detaillierten Tutorial gelangen, finden Sie hier einen kurzen Überblick über die Vorgehensweise.

1. Erstellen Sie eine neue leere Datenbank. Die Rückwand erstellt die erforderlichen Tabellen in dieser Datenbank.
2. Fügen Sie die folgenden nuget-Pakete zu Ihrer Anwendung hinzu: 

    - [Microsoft. Aspnet. signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. Aspnet. signalr. SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Erstellen Sie eine signalr-Anwendung.
4. Fügen Sie "Global. asax" den folgenden Code hinzu, um die Backplane zu konfigurieren: 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a>Konfigurieren der Datenbank

Entscheiden Sie, ob die Anwendung die Windows-Authentifizierung oder SQL Server Authentifizierung für den Zugriff auf die Datenbank verwendet. Stellen Sie unabhängig davon sicher, dass der Datenbankbenutzer über Berechtigungen zum Anmelden, Erstellen von Schemas und Erstellen von Tabellen verfügt.

Erstellen Sie eine neue Datenbank, die von der Rückwand verwendet werden soll. Sie können der Datenbank einen beliebigen Namen übergeben. Sie müssen keine Tabellen in der Datenbank erstellen. die Rückwand erstellt die erforderlichen Tabellen.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Aktivieren von Service Broker

Es wird empfohlen, Service Broker für die Rückwand-Datenbank zu aktivieren. Service Broker bietet systemeigene Unterstützung für Messaging-und Warteschlangen Funktionen in SQL Server, sodass die Rückwand Updates effizienter empfangen können. (Die Rückwand funktioniert jedoch auch ohne Service Broker.)

Um zu überprüfen, ob Service Broker aktiviert ist, Fragen Sie die Spalte **is\_Broker\_aktivierte** Spalte in der **sys. Datenbanken** -Katalog Sicht ab.

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Verwenden Sie die folgende SQL-Abfrage, um Service Broker zu aktivieren:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Wenn für diese Abfrage ein Deadlock auftritt, stellen Sie sicher, dass keine Anwendungen mit der Datenbank verbunden sind.

Wenn Sie die Ablauf Verfolgung aktiviert haben, werden die Ablauf Verfolgungen auch anzeigen, ob Service Broker aktiviert ist.

## <a name="create-a-signalr-application"></a>Erstellen einer signalr-Anwendung

Erstellen Sie eine signalr-Anwendung, indem Sie eines der folgenden Tutorials befolgen:

- [Einstieg in signalr](../getting-started/tutorial-getting-started-with-signalr.md)
- [Einstieg in signalr und MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

Als nächstes ändern wir die Chat-Anwendung, um das horizontale hochskalieren mit SQL Server zu unterstützen. Fügen Sie zunächst das nuget-Paket signalr. SqlServer zu Ihrem Projekt hinzu. Klicken Sie in Visual Studio im **Menü Extras** auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Öffnen Sie als nächstes die Datei "Global. asax". Fügen Sie den folgenden Code zum **\_Start** -Methode der Anwendung hinzu:

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Bereitstellen und Ausführen der Anwendung

Bereiten Sie Ihre Windows Server-Instanzen für die Bereitstellung der signalr-Anwendung vor.

Fügen Sie die IIS-Rolle hinzu. Umfasst Funktionen für die Anwendungsentwicklung, einschließlich des WebSocket-Protokolls.

![](scaleout-with-sql-server/_static/image4.png)

Schließen Sie auch den Verwaltungsdienst ein (unter "Verwaltungs Tools" aufgeführt).

![](scaleout-with-sql-server/_static/image5.png)

**Installieren Sie Web Deploy 3,0.** Wenn Sie IIS-Manager ausführen, werden Sie aufgefordert, die Microsoft-Webplattform zu installieren, oder Sie können [das Installationsprogramm herunterladen](https://go.microsoft.com/fwlink/?LinkId=255386). Suchen Sie im Platt Form Installationsprogramm nach Web deploy, und installieren Sie Web Deploy 3,0

![](scaleout-with-sql-server/_static/image6.png)

Überprüfen Sie, ob der Webverwaltungsdienst ausgeführt wird. Wenn dies nicht der Wert ist, starten Sie den Dienst. (Wenn der Webverwaltungsdienst in der Liste der Windows-Dienste nicht angezeigt wird, stellen Sie sicher, dass Sie den-Verwaltungsdienst installiert haben, als Sie die IIS-Rolle hinzugefügt haben.)

Öffnen Sie schließlich Port 8172 für TCP. Dies ist der Port, der vom Web deploy Tool verwendet wird.

Nun können Sie das Visual Studio-Projekt von Ihrem Entwicklungs Computer auf dem Server bereitstellen. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Lösung, und klicken Sie auf **veröffentlichen**.

Eine ausführlichere Dokumentation zur Webbereitstellung finden Sie unter [Webbereitstellungs-Inhalts Zuordnung für Visual Studio und ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).

Wenn Sie die Anwendung auf zwei Servern bereitstellen, können Sie jede Instanz in einem separaten Browserfenster öffnen und sehen, dass Sie jeweils signalr-Nachrichten vom anderen empfangen. (In einer Produktionsumgebung würden sich die beiden Server natürlich hinter einem Load Balancer befinden.)

![](scaleout-with-sql-server/_static/image7.png)

Nachdem Sie die Anwendung ausgeführt haben, können Sie sehen, dass signalr automatisch Tabellen in der Datenbank erstellt hat:

![](scaleout-with-sql-server/_static/image8.png)

Die Tabellen werden von signalr verwaltet. Solange Ihre Anwendung bereitgestellt ist, löschen Sie Zeilen nicht, ändern Sie die Tabelle usw.
