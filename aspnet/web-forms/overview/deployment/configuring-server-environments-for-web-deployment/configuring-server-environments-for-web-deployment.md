---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Konfigurieren von Server Umgebungen für die Webbereitstellung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Tutorial erfahren Sie, wie Sie Serverumgebungen einrichten, um die Bereitstellung und Veröffentlichung von One-Click-oder automatisierten Websites in verschiedenen unterschiedlichen Scen zu unterstützen...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 073161ce4faa3b7ba6749d7dfbaa5309eeca4f74
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515115"
---
# <a name="configuring-server-environments-for-web-deployment"></a>Konfigurieren von Serverumgebungen für die Webbereitstellung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Tutorial erfahren Sie, wie Sie Serverumgebungen einrichten, um die Bereitstellung und Veröffentlichung mit einem Mausklick in verschiedenen Szenarios zu unterstützen. Das Lernprogramm enthält Themen, in denen Sie durch die Ausführung verschiedener Aufgaben geführt werden, wie z. b. das Konfigurieren eines Webservers für die Unterstützung spezifischer Bereitstellungs Ansätze und das Einrichten einer Web Farm Framework (WFF)-Server Farm sowie szenariobasierte Übersichten, die End-to-End-Anleitungen auf höherer Ebene.
> 
> Das Tutorial verwendet das in [Unternehmensweb Bereitstellung: Szenarioübersicht](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) beschriebene Bereitstellungs Szenario Fabrikam, Inc. als Bezugspunkt für Beispiele und eine Netzwerkinfrastruktur.
> 
> Eine italienische Übersetzung dieser Tutorials finden Sie unter [http://www.lucamorelli.it](http://www.lucamorelli.it).

Dieses Tutorial enthält die folgenden Themen:

- [Auswählen der richtigen Vorgehensweise zur Webbereitstellung](choosing-the-right-approach-to-web-deployment.md)
- [Szenario: Konfigurieren einer Testumgebung für die Webbereitstellung](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Szenario: Konfigurieren einer Stagingumgebung für die Webbereitstellung](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Konfigurieren eines Webservers für die Web Deploy-Veröffentlichung (Remote-Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Konfigurieren eines Webservers für die Web Deploy-Veröffentlichung (Web Deploy-Handler)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Konfigurieren eines Webservers für die Web Deploy-Veröffentlichung (Offlinebereitstellung)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Konfigurieren eines Datenbankservers für die Web Deploy-Veröffentlichung](configuring-a-database-server-for-web-deploy-publishing.md)
- [Erstellen einer Serverfarm mit dem Webfarmframework](creating-a-server-farm-with-the-web-farm-framework.md)
- [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](configuring-deployment-properties-for-a-target-environment.md)

Das erste Thema, [das Auswählen des richtigen Ansatzes für die Webbereitstellung](choosing-the-right-approach-to-web-deployment.md), beschreibt die Hauptansätze, die Sie zum Veröffentlichen von Webanwendungen mit dem Internetinformationsdienste (IIS)-Webbereitstellungs Tool (Web deploy) 2,0 verwenden können. Außerdem werden die Szenarien identifiziert, die den einzelnen Ansätzen zugeordnet werden. Von hier aus enthält jedes szenariothema einen Überblick über die Aufgaben, die Sie ausführen müssen, und gibt Aufschluss über die Themen, die Sie für die Durchführung dieser Aufgaben benötigen.

Wenn Sie den in Grundlegendes [zum Buildprozess](../web-deployment-in-the-enterprise/understanding-the-build-process.md) zum Erstellen und Bereitstellen der Projekt Mappe beschriebenen Ansatz "Split Project file" verwenden, wird im letzten Thema Konfigurieren von [Bereitstellungs Eigenschaften für eine Zielumgebung](configuring-deployment-properties-for-a-target-environment.md)beschrieben, wie Sie Umgebungs spezifische Projektdateien für die Bereitstellung in verschiedenen Ziel Umgebungen konfigurieren.

## <a name="key-technologies"></a>Wichtige Technologien

Dieses Tutorial konzentriert sich auf die Verwendung dieser Produkte und Technologien, um die Webbereitstellung zu unterstützen:

- IIS 7.5
- Web deploy 2. x
- WFF 2.x
- IIS-Webverwaltungsdienst (WmSvc)

Das Tutorial berührt auch die Verwendung von Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4,0 und ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Weitere Tutorials in dieser Reihe

Dies ist Teil einer Reihe von fünf Tutorials für die Webbereitstellung im Unternehmen. Dies sind die anderen Tutorials in der Reihe:

- Bereitstellen von [Webanwendungen in Unternehmens Szenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) Dieser einführende Inhalt enthält den kontextbezogenen Hintergrund der tutorialreihe. Es beschreibt das Tutorial-Szenario und veranschaulicht, wie die in der Reihe beschriebenen Aufgaben und exemplarischen Vorgehensweisen in einen umfassenderen Application Lifecycle Management (ALM)-Prozess passen.
- [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Tutorial enthält eine konzeptionelle Einführung in Microsoft-Build-Engine (MSBuild)-Projektdateien, die Webpublishing Pipeline, Web deploy und andere verwandte Technologien. Es wird erläutert, wie Sie diese Tools zum Verwalten komplexer Bereitstellungs Prozesse verwenden können.
- [Konfigurieren von Team Foundation Server für die Webbereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie Sie Team Foundation Server (TFS) zur Unterstützung verschiedener Bereitstellungs Szenarien konfigurieren, einschließlich automatisierter Bereitstellung als Teil eines Continuous Integration (CI)-Prozesses und manueller Auslösung von bereit Stellungen spezifischer Builds.
- [Erweiterte Unternehmensweb Bereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In diesem Tutorial wird beschrieben, wie Sie verschiedene erweiterte Bereitstellungs Aufgaben durchführen können, wie z. b. das Anpassen von Daten Bank Bereitstellungen für mehrere Umgebungen, das Ausschließen von Dateien und Ordnern aus der Bereitstellung und das offline schalten von Webanwendungen .

> [!div class="step-by-step"]
> [Weiter](choosing-the-right-approach-to-web-deployment.md)
