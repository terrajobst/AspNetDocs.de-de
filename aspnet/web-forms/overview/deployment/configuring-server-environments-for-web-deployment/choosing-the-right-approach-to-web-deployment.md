---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Auswählen des richtigen Ansatzes für die Webbereitstellung | Microsoft-Dokumentation
author: jrjlee
description: Wenn Sie mit dem Internetinformationsdienste (IIS)-Webbereitstellungs Tool (Web deploy) 2,0 oder höher arbeiten, gibt es drei Hauptansätze, die Sie verwenden können...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13f784dd8e6404806104d56b026b3c41ca178892
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78441423"
---
# <a name="choosing-the-right-approach-to-web-deployment"></a>Auswählen der richtigen Vorgehensweise zur Webbereitstellung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Wenn Sie mit dem Internetinformationsdienste (IIS)-Webbereitstellungs Tool (Web deploy) 2,0 oder höher arbeiten, gibt es drei Hauptansätze, die Sie verwenden können, um Ihre gepackten Webanwendungen auf einen Webserver zu übernehmen. Sie haben folgende Möglichkeiten:
> 
> - Stellen Sie die Anwendung von einem Remote Standort aus bereit, indem Sie den *Web Deployment Agent-Dienst* (auch als "Remote-Agent" bezeichnet) auf dem Zielserver als Zielplattform einsetzen.
> - Stellen Sie die Anwendung über Web deploy on-Demand (auch als "Temp Agent" bezeichnet) an einem Remote Speicherort bereit.
> - Stellen Sie die Anwendung von einem Remote Standort aus bereit, indem Sie auf den *IIS-Web deploy Handler* auf dem Zielserver abzielen.
> - Stellen Sie die Anwendung bereit, indem Sie das Webpaket manuell auf den Zielserver kopieren und es über den IIS-Manager importieren.
> 
> Wie Sie Ihre Zielweb Server konfigurieren, hängt davon ab, welcher Ansatz für die Bereitstellung verwendet werden soll. Dieses Thema hilft Ihnen bei der Entscheidung, welche Herangehensweise an die Bereitstellung für Sie geeignet ist.

Diese Tabelle zeigt die Haupt vor-und Nachteile der einzelnen Bereitstellungs Ansätze sowie die Szenarien, die in der Regel für die einzelnen Ansätze geeignet sind.

| Vorgehensweise | Vorteile | Nachteile | Typische Szenarien |
| --- | --- | --- | --- |
| Remote-Agent | Die Einrichtung ist einfach. Sie eignet sich für reguläre Updates von Webanwendungen und Inhalten. | Der Benutzer muss ein Administrator auf dem Zielserver sein. der Benutzer kann keine alternativen Anmelde Informationen angeben. | Entwicklungsumgebungen. Test Umgebungen. |
| Temp-Agent | Es ist nicht erforderlich, Web deploy auf dem Bereitstellungs Zielcomputer zu installieren. Die neueste Version von Web deploy wird automatisch verwendet. | Der Benutzer muss ein Administrator auf dem Zielserver sein. der Benutzer kann keine alternativen Anmelde Informationen angeben. | Entwicklungsumgebungen. Test Umgebungen. |
| Web deploy Handler | Benutzer ohne Administratorrechte können Inhalte bereitstellen. Sie eignet sich für reguläre Updates von Webanwendungen und Inhalten. | Die Einrichtung ist viel komplexer. | Stagingumgebungen. Intranetproduktionsumgebungen. Gehostete Umgebungen. |
| Offline Bereitstellung | Die Einrichtung ist sehr einfach. Sie eignet sich für isolierte Umgebungen. | Der Server Administrator muss das Webpaket jedes Mal manuell kopieren und importieren. | Produktionsumgebungen mit Internet Zugriff. Isolierte Netzwerkumgebungen. |

## <a name="using-the-remote-agent"></a>Verwenden des Remote-Agents

Wenn Sie Web deploy mithilfe der Standardeinstellungen auf einem Zielserver installieren, wird der webDeployment Agent Dienst (der "Remote-Agent") automatisch installiert und gestartet. Standardmäßig macht der Remote-Agent einen HTTP-Endpunkt an dieser Adresse verfügbar:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]

> [!NOTE]
> Sie können [*Server*] durch den Computernamen des Webservers, eine IP-Adresse für Ihren Webserver oder einen Hostnamen ersetzen, der zu Ihrem Webserver aufgelöst wird.

Server Administratoren können Webpakete von einem Remote Standort aus bereitstellen, z. b. einem Entwickler Computer oder einem Buildserver, indem Sie diese Endpunkt Adresse angeben. Angenommen, Matt hink bei Fabrikam, Inc. hat beispielsweise das Webanwendungs Projekt ContactManager. MVC auf seinem Entwickler Computer erstellt. Der Buildprozess generiert ein Webpaket sowie eine *. cmd* -Datei, die die Web deploy Befehle enthält, die zum Installieren des Pakets erforderlich sind. Wenn Matt ein Server Administrator auf dem TESTWEB1-Server ist, kann er die Webanwendung auf dem Testweb Server bereitstellen, indem er den folgenden Befehl auf dem Entwickler Computer ausführen:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]

Tatsächlich kann die ausführbare Datei Web deploy die Endpunkt Adresse des Remote-Agents ableiten, wenn Sie den Computernamen angeben. Daher muss Matt nur Folgendes eingeben:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]

> [!NOTE]
> Weitere Informationen zu Web deploy Befehlszeilen Syntax und zum Bereitstellen von *cmd* -Dateien finden Sie unter Gewusst [wie: Installieren eines Bereitstellungs Pakets mit der Datei "Deployment. cmd](https://msdn.microsoft.com/library/ff356104.aspx)".

Der Remote-Agent bietet eine einfache Möglichkeit zum Bereitstellen von Inhalt von einem Remote Standort aus, und dieser Ansatz kann problemlos mit einem Mausklick oder einer automatisierten Bereitstellung funktionieren. Der Benutzer, der den Bereitstellungs Befehl ausführt, muss jedoch auch ein Domänen Administrator oder ein Mitglied der lokalen Administratoren Gruppe auf dem Zielserver sein. Außerdem unterstützt der Remote-Agent keine Standard Authentifizierung, sodass Sie keine alternativen Anmelde Informationen in der Befehlszeile übergeben können.

Der Remote-Agent bietet einen nützlichen Ansatz für die Bereitstellung in Entwicklungs-oder Testszenarien, bei denen es Entwicklern nicht ungewöhnlich ist, dass Sie eine vollständige Administrator Kontrolle über eine Testserver Umgebung haben und Anwendungen in der Regel neu erstellt und bereitgestellt werden. wieder. Dieser Ansatz ist jedoch in der Regel weniger akzeptabel für Staging-oder Produktionsumgebungen.

Ein End-to-End-Beispiel für ein Szenario, in dem der Remote-Agent-Ansatz verwendet wird, finden Sie unter [Szenario: Konfigurieren einer Test Umgebung für die Webbereitstellung](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Verwenden des Temp-Agents

Der Temp-Agent-Ansatz für die Bereitstellung ähnelt dem Remote-Agent-Ansatz. Im Gegensatz zum Remote-Agent-Ansatz müssen Sie Web deploy jedoch nicht auf dem Zielweb Server installieren. Stattdessen wird beim Ausführen der Bereitstellung von Web deploy eine temporäre Version des Webbereitstellungs-Agent-Diensts auf dem Zielserver installiert und zum Bereitstellen Ihrer Inhalte in IIS verwendet. Wenn die Bereitstellung vollständig ist, werden alle temporären Dateien entfernt.

Wenn Sie die Einstellung "Temp Agent Provider" verwenden möchten, fügen Sie dem Bereitstellungs Befehl das Flag **/g** hinzu:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]

> [!NOTE]
> Der Temp-Agent kann nicht verwendet werden, wenn der Webbereitstellungs-Agent-Dienst auf dem Zielcomputer installiert ist, auch wenn der Dienst nicht ausgeführt wird.

Der Vorteil dieses Ansatzes besteht darin, dass Sie keine Installationen von Web deploy auf ihren Ziel Servern aufbewahren müssen. Außerdem müssen Sie nicht sicherstellen, dass auf den Quell-und Ziel Computern dieselbe Version von Web deploy ausgeführt wird. Dieser Ansatz hat jedoch die gleichen Prinzipal Einschränkungen wie der Remote-Agent-Ansatz, d. h., Sie müssen ein lokaler Administrator auf dem Zielserver sein, um Inhalt bereitzustellen, und nur die NTLM-Authentifizierung wird unterstützt. Der Temp-Agent-Ansatz erfordert auch viel mehr anfängliche Konfiguration der Zielumgebung.

Weitere Informationen zur Verwendung des Temp-Agents finden Sie unter Gewusst [wie: Installieren eines Bereitstellungs Pakets mit der Datei "Deployment. cmd](https://msdn.microsoft.com/library/ff356104.aspx) " und [Web deploy bei Bedarf](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Verwenden des Web deploy Handlers

Ab IIS 7 bietet Web deploy einen alternativen Bereitstellungs Ansatz über den IIS-Web deploy Handler. Der Web deploy-Handler ist eng in den IIS-Webverwaltungsdienst (WmSvc) integriert, der Benutzern das Verwalten von IIS-Websites von Remote Standorten aus ermöglicht.

Standardmäßig macht der Remote-Agent einen HTTP-Endpunkt an dieser Adresse verfügbar:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]

> [!NOTE]
> Sie können [*Server*] durch den Computernamen des Webservers, eine IP-Adresse für Ihren Webserver oder einen Hostnamen ersetzen, der zu Ihrem Webserver aufgelöst wird.

Der große Vorteil des Web deploy Handlers über den Remote-Agent und den Temp-Agent besteht darin, dass Sie IIS so konfigurieren können, dass Benutzer ohne Administratorrechte Anwendungen und Inhalte für bestimmte IIS-Websites bereitstellen können. Der Web deploy Handler unterstützt auch die Standard Authentifizierung, sodass Sie alternative Anmelde Informationen als Parameter in Ihren Web deploy-Befehlen bereitstellen können. Der größte Nachteil besteht darin, dass die Einrichtung und Konfiguration des Web deploy Handlers anfänglich viel komplizierter ist.

Im Fall von Benutzern ohne Administratorrechte ermöglicht der Webverwaltungsdienst (WmSvc) nur dem Benutzer, eine Verbindung mit IIS herzustellen, anstatt eine Verbindung auf Serverebene herzustellen. Um auf eine bestimmte Website zuzugreifen, können Sie eine standortspezifische Abfrage Zeichenfolge in die Endpunkt Adresse einschließen:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]

Angenommen, ein Buildprozess ist so konfiguriert, dass nach jedem erfolgreichen Build automatisch eine Webanwendung in einer Stagingumgebung bereitgestellt wird. Wenn Sie den Remote-Agent-Ansatz verwendet haben, müssen Sie die buildprozessidentität als Administrator auf ihren Ziel Servern festlegen. Im Gegensatz dazu können Sie mithilfe des Web deploy handleransatzes&#x2014;**einem Benutzer,** der nicht Administrator ist, in diesem Fall&#x2014;nur eine Berechtigung für eine bestimmte IIS-Website erteilen, und der Buildprozess kann diese Anmelde Informationen zum Bereitstellen des Webpakets bereitstellen.

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]

> [!NOTE]
> Weitere Informationen zu Web deploy Befehlszeilen Vorgängen und zur Syntax finden Sie unter [Web deploy Befehlszeilen Referenz](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Weitere Informationen zur Verwendung der Datei " *. Deployment. cmd* " finden Sie unter Gewusst [wie: Installieren eines Bereitstellungs Pakets mit der Datei "Deployment. cmd](https://msdn.microsoft.com/library/ff356104.aspx)".

Der Web deploy-Handler bietet einen nützlichen Ansatz für die Bereitstellung in Stagingumgebungen, gehosteten Umgebungen und intranetbasierten Produktionsumgebungen, in denen der Remote Zugriff auf den Server zwar verfügbar ist, die Administrator Anmelde Informationen jedoch nicht.

Ein End-to-End-Beispiel für ein Szenario, in dem der Web deploy handleransatz verwendet wird, finden Sie unter [Szenario: Konfigurieren einer Stagingumgebung für die Webbereitstellung](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Verwenden der Offline Bereitstellung

In einigen Fällen ist es nicht möglich oder praktisch, Anwendungen und Inhalte von einem Remote Standort aus auf einer IIS-Website bereitzustellen. Beispielsweise können sich Quell-und Zielcomputer in isolierten Netzwerken oder Netzwerksegmenten befinden, oder die Firewall-Richtlinie lässt keinen Remote Zugriff zu.

In solchen Szenarien können Sie weiterhin die Verpackungs-und Veröffentlichungs Funktionen von Web deploy verwenden; Sie können nicht von einem Remote Standort aus verwendet werden. Stattdessen muss ein Administrator auf dem Zielserver das Webpaket auf den Server kopieren und über den IIS-Manager importieren.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

Der Offline Bereitstellungs Ansatz ist in der Regel nützlich in Produktionsumgebungen mit Internet Zugriff, bei denen Server in einem Umkreis Netzwerk möglicherweise eingeschränkte Konnektivität mit Computern im internen Netzwerk haben.

Ein End-to-End-Beispiel für ein Szenario, in dem der Offline Bereitstellungs Ansatz verwendet wird, finden Sie unter [Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu Web deploy Befehlszeilen Vorgängen und zur Syntax finden Sie unter [Web deploy Befehlszeilen Referenz](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Weitere Informationen zur Verwendung der Datei " *. Deployment. cmd* " finden Sie unter Gewusst [wie: Installieren eines Bereitstellungs Pakets mit der Datei "Deployment. cmd](https://msdn.microsoft.com/library/ff356104.aspx)".

Weitere allgemeine Anleitungen zu den verschiedenen Methoden, mit denen Sie Webpakete von einem Remote Computer bereitstellen können, finden Sie unter [Verwenden von Web deploy](https://technet.microsoft.com/library/ee461175(WS.10).aspx)Remote. Weitere Informationen zur bedarfsgesteuerten Verwendung von Web deploy finden Sie unter [Web deploy on Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Zurück](configuring-server-environments-for-web-deployment.md)
> [Weiter](scenario-configuring-a-test-environment-for-web-deployment.md)
