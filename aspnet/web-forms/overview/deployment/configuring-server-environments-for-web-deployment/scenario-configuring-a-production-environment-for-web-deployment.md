---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung | Microsoft-Dokumentation'
author: jrjlee
description: In diesem Thema wird ein typisches Webbereitstellungs Szenario für eine Produktionsumgebung beschrieben. Außerdem werden die Aufgaben erläutert, die Sie ausführen müssen, um eine ähnliche...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2d76e715cdbf6ec484fa0ff98b3b3d1d8dfd3961
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515931"
---
# <a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird ein typisches Webbereitstellungs Szenario für eine Produktionsumgebung beschrieben. Außerdem werden die Aufgaben erläutert, die Sie ausführen müssen, um eine ähnliche Umgebung einzurichten.

Die Produktionsumgebung ist das endgültige Ziel für eine Webanwendung oder eine Website. An diesem Punkt hat ihre Anwendung Tests durchlaufen, wurde in einer Stagingumgebung bereitgestellt und ist für die "Live-Bereitstellung" bereit. Die Merkmale einer Produktionsumgebung können sich je nach Art und Zweck Ihres Webinhalts, der Größe Ihrer Organisation, der Zielgruppe und vielen anderen Faktoren stark unterscheiden. In einem Unternehmens Szenario kann die Produktionsumgebung folgende Merkmale aufweisen:

- Die Umgebung besteht aus mehreren Webservern mit Lastenausgleich und einem oder mehreren Datenbankservern, häufig mit Failoverclustering und Daten Bank Spiegelung.
- Wenn die Umgebung mit dem Internet erreichbar ist, wird Sie wahrscheinlich vom internen Netzwerk getrennt. Sie kann sich in einem anderen Subnetz in einem Umkreis Netzwerk befinden, kann sich in einer anderen Domäne befinden und kann sich in einer ganz anderen Netzwerkinfrastruktur befinden.
- Entwicklern und Buildserver-Prozess Konten ist sehr unwahrscheinlich, dass Sie über Administratorrechte auf den Produktionsservern verfügen.
- Änderungen an Anwendungen werden weniger häufig als Test-oder Stagingbereitstellungen bereitgestellt.

> [!NOTE]
> Das horizontale hochskalieren einer Daten Bank Bereitstellung auf mehreren Servern geht über den Rahmen dieses Tutorials hinaus. Weitere Informationen zu diesem Bereich finden Sie unter [SQL Server-Onlinedokumentation](https://technet.microsoft.com/library/ms130214.aspx).

In unserem [Tutorial-Szenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)enthält ein Team Build Server z. b. Builddefinitionen, mit denen Benutzer die Contact Manager-Lösung erstellen und in einem einzigen Schritt in einer Stagingumgebung bereitstellen können. Wenn die Anwendung für die Bereitstellung in der Produktionsumgebung bereit ist, muss der Administrator der Produktionsumgebung aufgrund der Einschränkungen, die von den Sicherheitsanforderungen und der Netzwerkinfrastruktur auferlegt werden, das Webpaket manuell auf einen produktionsweb Server kopieren und importieren. dies über Internetinformationsdienste (IIS)-Manager.

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Übersicht über die Lösungen

In diesem Szenario können Sie diese Fakten aus einer Analyse der Bereitstellungs Anforderungen ableiten:

- Aufgrund von Sicherheitseinschränkungen und der Netzwerkkonfiguration ist es nicht möglich, die Produktionsumgebung für die Unterstützung der One-Click-oder automatisierten Bereitstellung zu konfigurieren. Die Offline Bereitstellung ist der einzige geeignete Ansatz in diesem Szenario.
- Die Produktionsumgebung umfasst mehrere Webserver, sodass Sie das Web Farm Framework (WFF) zum Erstellen einer Serverfarm verwenden können. Bei dieser Vorgehensweise muss der Administrator die Anwendung nur auf einen Webserver (den primären Server) importieren, und die Bereitstellung wird von WFF auf allen anderen Webservern in der Produktionsumgebung repliziert.

Diese Themen enthalten alle Informationen, die Sie benötigen, um die folgenden Aufgaben auszuführen:

- [Erstellen Sie eine Server Farm mit dem Webfarm-Framework](configuring-a-database-server-for-web-deploy-publishing.md). In diesem Thema wird beschrieben, wie eine Serverfarm mithilfe von WFF erstellt und konfiguriert wird, damit Webplattform-Produkte und-Komponenten, Konfigurationseinstellungen und Websites und Anwendungen auf mehreren Webservern mit Lastenausgleich repliziert werden.
- [Konfigurieren eines Webservers für die Web deploy Veröffentlichung (Offline Bereitstellung)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). In diesem Thema wird beschrieben, wie Sie einen Webserver erstellen, mit dem Administratoren Webpakete manuell importieren und bereitstellen können, beginnend mit einem sauberen Windows Server 2008 R2-Build.
- [Konfigurieren Sie einen Daten Bank Server für die Web deploy Veröffentlichung](configuring-a-database-server-for-web-deploy-publishing.md). In diesem Thema wird beschrieben, wie ein Datenbankserver für die Unterstützung von Remote Zugriff und-Bereitstellung gestartet wird, beginnend mit einer Standardinstallation von SQL Server 2008 R2.

## <a name="further-reading"></a>Weitere nützliche Informationen

Anleitungen zum Konfigurieren einer typischen Testumgebung für Entwickler finden Sie unter [Szenario: Konfigurieren einer Testumgebung für die Webbereitstellung](scenario-configuring-a-test-environment-for-web-deployment.md). Anleitungen zum Konfigurieren einer typischen Stagingumgebung finden Sie unter [Szenario: Konfigurieren einer Stagingumgebung für die Webbereitstellung](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Zurück](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [Weiter](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
