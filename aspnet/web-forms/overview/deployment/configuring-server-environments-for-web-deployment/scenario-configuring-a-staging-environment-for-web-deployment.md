---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Szenario: Konfigurieren einer Stagingumgebung für die Webbereitstellung | Microsoft-Dokumentation'
author: jrjlee
description: In diesem Thema wird ein typisches Webbereitstellungs Szenario für eine Stagingumgebung beschrieben, und es werden die Aufgaben erläutert, die Sie ausführen müssen, um eine ähnliche env einzurichten...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: eaa61ca850817f8dd98955b59e94be93389bf256
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518007"
---
# <a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Szenario: Konfigurieren einer Stagingumgebung für die Webbereitstellung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird ein typisches Webbereitstellungs Szenario für eine Stagingumgebung beschrieben. Außerdem werden die Aufgaben erläutert, die Sie ausführen müssen, um eine ähnliche Umgebung einzurichten.

Viele Organisationen verwenden Stagingumgebungen zum Anzeigen einer Vorschau von Aktualisierungen für Webanwendungen oder Websites. Dadurch haben Personen innerhalb der Organisation die Möglichkeit, neue Funktionen oder Inhalte zu untersuchen und zu überprüfen, bevor die Website "online geschaltet" wird, oder mit anderen Worten, Sie werden in einer Produktionsumgebung bereitgestellt. Die Stagingumgebung ist so konzipiert, dass die Produktionsumgebung so genau wie möglich repliziert wird, um eine realistische Vorschau bereitzustellen. Diese Art von Stagingumgebung weist in der Regel die folgenden Merkmale auf:

- Die Umgebung besteht aus mehreren Webservern mit Lastenausgleich und einem oder mehreren Datenbankservern, häufig mit Failoverclustering und Daten Bank Spiegelung.
- Anwendungen können manuell von einem Entwicklungsteam oder automatisch von einem Team Build Server bereitgestellt werden.
- Die Benutzer oder Prozess Konten, die Anwendungen bereitstellen, verfügen wahrscheinlich nicht über Administratorrechte auf den Stagingservern.
- Änderungen an Anwendungen werden in regelmäßigen Abständen bereitgestellt, sodass die Umgebung eine Einzelschritt-oder automatisierte Bereitstellung unterstützen muss.

> [!NOTE]
> Das horizontale hochskalieren einer Daten Bank Bereitstellung auf mehreren Servern geht über den Rahmen dieses Tutorials hinaus. Weitere Informationen zu diesem Bereich finden Sie unter [SQL Server-Onlinedokumentation](https://technet.microsoft.com/library/ms130214.aspx).

In unserem [Tutorial-Szenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)verwaltet Team Foundation Server (TFS) z. b. die Contact Manager-Lösung. Der TFS-Administrator Rob Walters hat eine Builddefinition erstellt, mit der Entwickler nach Bedarf eine Bereitstellung in der Stagingumgebung auslöst.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Beachten Sie, dass Sie in den meisten Fällen nicht unbedingt den neuesten Build in der Stagingumgebung bereitstellen möchten. Stattdessen ist es viel wahrscheinlicher, dass Sie einen bestimmten Build bereitstellen möchten, der bereits in der Testumgebung validiert und überprüft wurde.

## <a name="solution-overview"></a>Übersicht über die Lösungen

In diesem Szenario können Sie diese Fakten aus einer Analyse der Bereitstellungs Anforderungen ableiten:

- Das Benutzer-oder Prozess Konto, das die Bereitstellung ausführt, verfügt nicht über Administratorrechte auf den Stagingservern, sodass die Stagingweb Server die Bereitstellung ohne Administratorrechte unterstützen müssen. Daher müssen Sie die Stagingweb Server so konfigurieren, dass Sie den Web deploy Handler anstelle des Remote-Agents verwenden.
- In der Stagingumgebung sind mehrere Webserver enthalten, aber die Bereitstellung mit nur einem Klick oder einer automatisierten Bereitstellung muss unterstützt werden. Daher müssen Sie das Web Farm Framework (WFF) zum Erstellen einer Serverfarm verwenden. Mit dieser Vorgehensweise können Sie eine Anwendung auf einem Webserver (dem primären Server) bereitstellen, und die Bereitstellung wird von WFF auf allen anderen Webservern in der Stagingumgebung repliziert.
- Das Benutzer-oder Prozess Konto, das die Bereitstellung ausführt, muss über Berechtigungen zum Erstellen von Datenbanken verfügen. Daher müssen Sie das Konto der **dbcreator** -Server Rolle auf dem Datenbankserver hinzufügen, zusätzlich zum Konfigurieren des Datenbankservers, um den Remote Zugriff und die Bereitstellung zu unterstützen.

Diese Themen enthalten alle Informationen, die Sie benötigen, um die folgenden Aufgaben auszuführen:

- [Erstellen Sie eine Server Farm mit dem Webfarm-Framework](creating-a-server-farm-with-the-web-farm-framework.md). In diesem Thema wird beschrieben, wie eine Serverfarm mithilfe von WFF erstellt und konfiguriert wird, damit Webplattform-Produkte und-Komponenten, Konfigurationseinstellungen und Websites und Anwendungen auf mehreren Webservern mit Lastenausgleich repliziert werden.
- [Konfigurieren eines Webservers für die Web deploy Veröffentlichung (Web deploy Handler)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). In diesem Thema wird beschrieben, wie ein Webserver, der Web deploy Veröffentlichung unterstützt, mithilfe des Remote-Agent-Ansatzes, beginnend mit einem sauberen Windows Server 2008 R2-Build, erstellt wird.
- [Konfigurieren Sie einen Daten Bank Server für die Web deploy Veröffentlichung](configuring-a-database-server-for-web-deploy-publishing.md). In diesem Thema wird beschrieben, wie ein Datenbankserver für die Unterstützung von Remote Zugriff und-Bereitstellung gestartet wird, beginnend mit einer Standardinstallation von SQL Server 2008 R2.

## <a name="further-reading"></a>Weitere nützliche Informationen

Anleitungen zum Konfigurieren einer typischen Testumgebung für Entwickler finden Sie unter [Szenario: Konfigurieren einer Testumgebung für die Webbereitstellung](scenario-configuring-a-test-environment-for-web-deployment.md). Anleitungen zum Konfigurieren einer typischen Produktionsumgebung finden Sie unter [Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Zurück](scenario-configuring-a-test-environment-for-web-deployment.md)
> [Weiter](scenario-configuring-a-production-environment-for-web-deployment.md)
