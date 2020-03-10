---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Szenario: Konfigurieren einer Test Umgebung für die Webbereitstellung | Microsoft-Dokumentation'
author: jrjlee
description: In diesem Thema wird ein typisches Webbereitstellungs Szenario für Entwickler-oder Testumgebungen beschrieben. Außerdem werden die Aufgaben erläutert, die Sie ausführen müssen, um eine Si...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: d580e550f2461837f0e8a4e477273348b49cb53e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78517983"
---
# <a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Szenario: Konfigurieren einer Test Umgebung für die Webbereitstellung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird ein typisches Webbereitstellungs Szenario für Entwickler-oder Testumgebungen beschrieben. Außerdem werden die Aufgaben erläutert, die Sie ausführen müssen, um eine ähnliche Umgebung einzurichten.

Wenn Entwickler an Webanwendungen arbeiten, erhalten Sie häufig Zugriff auf eine Serverumgebung, die Sie verwenden können, um Änderungen an Ihren Anwendungen in einer realistischen Einstellung zu testen. Diese Art von Entwicklungs-oder Testumgebung weist in der Regel die folgenden Merkmale auf:

- Die Umgebung besteht aus einem einzelnen Webserver und einem einzelnen Datenbankserver.
- Die Entwickler verfügen in der Regel über Administratorrechte auf den Servern, damit Sie die Umgebung für die Anforderungen Ihrer Anwendungen konfigurieren können.
- Änderungen an Anwendungen werden in regelmäßigen Abständen bereitgestellt, sodass die Umgebung eine Einzelschritt-oder automatisierte Bereitstellung unterstützen muss.

In unserem [Tutorial-Szenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)ist Matt hink z. b. ein Entwickler bei Fabrikam, Inc. Matt arbeitet an der Contact Manager-Lösung und muss regelmäßig Änderungen an einer Testumgebung bereitstellen. Matt ist ein Administrator auf dem Testweb Server und dem Test-Datenbankserver. Zunächst muss Matt die Lösung direkt in der Testumgebung bereitstellen können.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

Wenn die Arbeit fortschreitet und mehr Entwickler dem Team beitreten, wird die Contact Manager-Lösung für Continuous Integration (CI) in Team Foundation Server (TFS) konfiguriert. Wenn ein Entwickler Inhalt eincheckt, sollte Team Build die Projekt Mappe erstellen, alle Komponententests ausführen und die Lösung automatisch in der Testumgebung bereitstellen.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Übersicht über die Lösungen

Die Testumgebung muss eine Einzel-oder automatisierte Bereitstellung von einem Remote Computer aus unterstützen, sodass Sie zwei Hauptansätze wählen können. Sie haben folgende Möglichkeiten:

- Konfigurieren Sie den Testweb Server für die Unterstützung der Bereitstellung mithilfe des Web Deployment Agent-Diensts ("Remote-Agent").
- Konfigurieren Sie den Testweb Server für die Unterstützung der Bereitstellung mithilfe des Web deploy Handlers.

> [!NOTE]
> Sie können auch [Web deploy on Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("Temp Agent") verwenden. Dies ähnelt dem Remote-Agent-Ansatz hinsichtlich der Anforderungen und Einschränkungen.

In diesem Fall verfügen die Entwickler über Administratorrechte auf den Ziel Servern, und die Testumgebung unterliegt nicht strengen Sicherheitseinschränkungen. Daher besteht die logische Wahl darin, den Testweb Server für die Unterstützung der Bereitstellung mit dem Remote-Agent zu konfigurieren. Dies ist weniger komplex und erfordert weniger anfängliche Konfigurationen als bei der Web deploy Handlermethode. Außerdem müssen Sie den Datenbankserver für die Unterstützung von Remote Zugriff und Bereitstellung konfigurieren.

Diese Themen enthalten alle Informationen, die Sie benötigen, um die folgenden Aufgaben auszuführen:

- [Konfigurieren eines Webservers für die Web deploy Veröffentlichung (Remote-Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). In diesem Thema wird beschrieben, wie ein Webserver, der Web deploy Veröffentlichung unterstützt, mithilfe des Remote-Agent-Ansatzes, beginnend mit einem sauberen Windows Server 2008 R2-Build, erstellt wird.
- [Konfigurieren Sie einen Daten Bank Server für die Web deploy Veröffentlichung](configuring-a-database-server-for-web-deploy-publishing.md). In diesem Thema wird beschrieben, wie ein Datenbankserver für die Unterstützung von Remote Zugriff und-Bereitstellung gestartet wird, beginnend mit einer Standardinstallation von SQL Server 2008 R2.

## <a name="further-reading"></a>Weitere nützliche Informationen

Anleitungen zum Konfigurieren einer typischen Stagingumgebung finden Sie unter [Szenario: Konfigurieren einer Stagingumgebung für die Webbereitstellung](scenario-configuring-a-staging-environment-for-web-deployment.md). Anleitungen zum Konfigurieren einer typischen Produktionsumgebung finden Sie unter [Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Zurück](choosing-the-right-approach-to-web-deployment.md)
> [Weiter](scenario-configuring-a-staging-environment-for-web-deployment.md)
