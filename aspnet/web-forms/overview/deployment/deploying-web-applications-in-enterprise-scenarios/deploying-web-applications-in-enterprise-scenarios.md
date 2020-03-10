---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Bereitstellen von Webanwendungen in Unternehmens Szenarien mithilfe von Visual Studio 2010 | Microsoft-Dokumentation
author: jrjlee
description: In diesen Tutorials werden Tools und Techniken beschrieben, die Sie zum Bereitstellen von Webanwendungen in verschiedenen Unternehmens Szenarios verwenden können. Es wird erläutert, wie Sie die bestmögliche Verwendung erzielen...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: eb7b82d16079d4d086af1919d092fb28db60d8b3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463431"
---
# <a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Bereitstellen von Webanwendungen in Unternehmensszenarien mit Visual Studio 2010

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesen Tutorials werden Tools und Techniken beschrieben, die Sie zum Bereitstellen von Webanwendungen in verschiedenen Unternehmens Szenarios verwenden können. Es wird erläutert, wie Sie Technologien wie Visual Studio 2010, das Microsoft-Build-Engine (MSBuild), Internetinformationsdienste (IIS) 7,5, das IIS-Webbereitstellungs Tool (Web deploy), das Web Farm Framework (WFF) und Hilfsprogramme wie "VSDBCMD. exe" optimal nutzen können. vereinfachen und verwalten Sie den Bereitstellungs Prozess. Es enthält konzeptionelle Übersichten und aufgabenorientierte Anleitungen, die Sie bei folgenden Aufgaben unterstützen:
> 
> - Informieren Sie sich über die Bereitstellungs Anforderungen für eine Webanwendung auf Unternehmens Niveau.
> - Konfigurieren Sie Test-, Staging-und Produktions-Webserver Umgebungen, um die Webbereitstellung zu unterstützen.
> - Konfigurieren Sie Team Foundation Server (TFS) Continuous Integration (CI)-Prozesse, um die automatisierte Webbereitstellung zu unterstützen.
> - Stellen Sie Webanwendungen auf Unternehmens Niveau in verschiedenen Serverumgebungen mit unterschiedlichen Anforderungen und Einschränkungen bereit.
> - Stellen Sie Änderungen an Webanwendungen bereit, die in verschiedenen Serverumgebungen ausgeführt werden.
> 
> > [!NOTE]
> > Diese Tutorials beschreiben zwar die Verwendung von TFS als CI-Server, die Anleitungen sind aber problemlos an beliebige CI-Server angepasst. Sie benötigen keine detaillierten Kenntnisse über TFS, um die Tutorials zu verstehen und zu nutzen.
> 
> 
> Eine italienische Übersetzung dieser Tutorials finden Sie unter [http://www.lucamorelli.it](http://www.lucamorelli.it).

## <a name="about-the-authors"></a>Über die Autoren

Jason Lee ist ein Prinzipal Techniker mit dem [Inhalts Master](http://www.contentmaster.com/) , in dem er seit mehreren Jahren mit Microsoft-Produkten und-Technologien, insbesondere SharePoint und ASP.net, arbeitet. Jason hat bei der computingverwaltung promotet und ist zurzeit MCPD und MCTS-zertifiziert. Weitere Informationen finden Sie unter [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin Curry ist ein Prinzipal Techniker mit [Inhalts Master](http://www.contentmaster.com/) , der Whitepaper, SDK-Dokumentation, PowerPoint-Präsentationen und Schulungskurse für Dozenten und Online Schulungen in seiner Karriere verfasst hat. Ein ursprüngliches Mitglied des ASP.net-Dokumentationsteams hat seit über einem Jahrzehnt mit den Webtechnologien von Microsoft gearbeitet.

## <a name="target-audience"></a>Zielgruppe

Diese Tutorials richten sich an ASP.NET-Webanwendungs Entwickler und Lösungs Architekten, die Visual Studio 2010 zum Erstellen von Webanwendungen für Unternehmen verwenden. Um den größten Nutzen aus dem Inhalt zu erzielen, sollten Sie sich mit Visual Studio 2010 vertraut machen und eine grundlegende Vertrautheit mit TFS sowie ein Bewusstsein für Microsoft-Webplattform-Technologien wie ASP.NET MVC 3, Windows Communication Foundation (WCF), IIS, SQL verwenden. Server-und Visual Studio-Datenbankprojekte. Sie müssen jedoch nicht mit Bereitstellungs Tools und-Technologien vertraut sein oder wissen, wie Sie CI-Systeme einrichten müssen.

## <a name="requirements"></a>Anforderungen

Um den exemplarischen Vorgehensweisen zu folgen und die Aufgaben auszuführen, die in diesen Tutorials beschrieben werden, müssen Sie diese Software auf Ihrem Entwicklungs Computer installieren:

- Visual Studio 2010 Premium oder Ultimate Edition mit Service Pack 1
- .NET Framework 4.0
- .NET Framework 3,5 mit Service Pack 1
- ASP.NET MVC 3.0
- IIS 7,5 Express
- SQL Server Express 2008 R2

Um die in diesen exemplarischen Vorgehensweisen beschriebenen Bereitstellungs Schritte auszuführen, benötigen Sie Zugriff auf Beispiel Umgebungen für die Webanwendungs Bereitstellung. Um optimale Ergebnisse zu erzielen, sollten diese Umgebungen das Unternehmens Bereitstellungs Muster Ihrer Organisation widerspiegeln. Anschließend können Sie die in dieser Dokumentation bereitgestellten exemplarischen Vorgehensweisen ändern, um die Bereitstellungs Umgebungen und Anforderungen Ihrer eigenen Organisation widerzuspiegeln.

## <a name="series-contents"></a>Reihen Inhalt

Dieser einführende Abschnitt besteht aus zwei weiteren Themen. Diese sind so konzipiert, dass Sie einen breiteren Kontext für die folgenden Tutorials bereitstellen:

- [Unternehmensweb Bereitstellung: Szenarioübersicht](enterprise-web-deployment-scenario-overview.md). In diesem Thema wird das Szenario beschrieben, in dem die einzelnen Tutorials in dieser Reihe unterstützt werden. Das Szenario konzentriert sich auf die Application Lifecycle Management (ALM)-Anforderungen eines fiktiven Unternehmens namens Fabrikam, Inc., da es eine Webanwendung auf Unternehmens Niveau entwickelt.
- [Application Lifecycle Management: von der Entwicklung bis zur Produktion](application-lifecycle-management-from-development-to-production.md). Dieses Thema bietet einen umfassenden Überblick über einen Bereitstellungs Prozess. Es veranschaulicht, wie Fabrikam, Inc. eine ASP.NET-Webanwendung im Unternehmen durch Test-, Staging-und Produktionsumgebungen im Rahmen eines kontinuierlichen Entwicklungsprozesses verschiebt.

Die Reihe umfasst vier Lernprogramm Sätze. Jede konzentriert sich auf verschiedene Aspekte der Webbereitstellung:

- [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Tutorial enthält eine konzeptionelle Einführung in MSBuild-Projektdateien, die Webpublishing Pipeline, Web deploy und andere verwandte Technologien. Es wird erläutert, wie Sie diese Tools zum Verwalten komplexer Bereitstellungs Prozesse verwenden können.
- [Konfigurieren von Server Umgebungen für die Webbereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie Sie Windows Server zur Unterstützung verschiedener Bereitstellungs Szenarien konfigurieren, einschließlich der Remoteweb Paket Bereitstellung mithilfe des Web Deployment Agent-Diensts ("Remote-Agent") oder des Web deploy Handlers und der Remote Datenbank Es enthält Anleitungen zum Auswählen der geeigneten Bereitstellungs Methode für Ihre eigene Umgebung. Außerdem wird beschrieben, wie Sie die WFF zum Replizieren bereitgestellter Webanwendungen auf allen Webservern in einer Serverfarm verwenden.
- [Konfigurieren von Team Foundation Server für die Webbereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie Sie TFS zur Unterstützung verschiedener Bereitstellungs Szenarien konfigurieren, einschließlich automatisierter Bereitstellung als Teil eines CI-Prozesses und Manuelles Auslösen von bereit Stellungen spezifischer Builds.
- [Erweiterte Unternehmensweb Bereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In diesem Tutorial wird beschrieben, wie Sie verschiedene erweiterte Bereitstellungs Aufgaben durchführen können, wie z. b. das Anpassen von Daten Bank Bereitstellungen für mehrere Umgebungen, das Ausschließen von Dateien und Ordnern aus der Bereitstellung und das offline schalten von Webanwendungen .

## <a name="where-to-start"></a>Start Seite

In diesen Tutorials wird eine Beispiellösung mit einer realistischen Komplexität und einem fiktiven Szenario für die Unternehmens Bereitstellung verwendet, um eine Referenz Implementierung bereitzustellen und um den Aufgaben und exemplarischen Vorgehensweisen einen allgemeinen Kontext zu bieten. Im nächsten Thema, [Enterprise Web Deployment: Szenarioübersicht](enterprise-web-deployment-scenario-overview.md), werden das Szenario und die Beispiellösung vorgestellt. Von dort aus können Sie die Tutorials und Themen durcharbeiten, die Ihren Anforderungen am ehesten entsprechen.

> [!div class="step-by-step"]
> [Weiter](enterprise-web-deployment-scenario-overview.md)
