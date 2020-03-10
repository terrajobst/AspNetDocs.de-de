---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Konfigurieren von Team Foundation Server für die Webbereitstellung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Tutorial wird gezeigt, wie Sie Team Foundation Server (TFS) 2010 konfigurieren, um Lösungen zu erstellen und Webinhalte in verschiedenen Ziel Umgebungen bereitzustellen. ...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 638d696abbc5f05957c0ed2eb7ebb65fce7813ea
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78424203"
---
# <a name="configuring-team-foundation-server-for-web-deployment"></a>Konfigurieren von Team Foundation Server für die Webbereitstellung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Tutorial wird gezeigt, wie Sie Team Foundation Server (TFS) 2010 konfigurieren, um Lösungen zu erstellen und Webinhalte in verschiedenen Ziel Umgebungen bereitzustellen. Dies umfasst Continuous Integration (CI)-Szenarios, in denen Sie Inhalte automatisch bereitstellen, wenn ein Entwickler eine Änderung vornimmt. Es können auch manuelle auslöserszenarien enthalten sein, in denen ein Administrator die Bereitstellung eines bestimmten Builds in einer Stagingumgebung möglicherweise auslöst, sobald der Build überprüft und in der Testumgebung validiert wurde. Die Themen in diesem Tutorial führen Sie durch den gesamten Konfigurationsprozess, einschließlich der folgenden:
> 
> - Erstellen eines neuen Teamprojekts in TFS.
> - Vorgehensweise beim Hinzufügen von Inhalt zur Quell Code Verwaltung.
> - Konfigurieren eines Buildservers für die Unterstützung von CI und Bereitstellung.
> - Erstellen einer Builddefinition, die Bereitstellungs Logik einschließt.
> - Vorgehensweise beim Konfigurieren der Berechtigungen für die automatisierte Bereitstellung.
> 
> Eine italienische Übersetzung dieser Tutorials finden Sie unter [http://www.lucamorelli.it](http://www.lucamorelli.it).

In diesem Tutorial wird davon ausgegangen, dass Sie TFS 2010 installiert und im Rahmen des erst Konfigurations Vorgangs eine Teamprojekt Sammlung erstellt haben. Das [Team Foundation-Installationshandbuch für Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) bietet umfassende Anleitungen zu diesen Aufgaben.

## <a name="context"></a>Kontext

Dies ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der&#x2014; [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) -Lösung verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Mittelpunkt dieser Tutorials basiert auf dem Untergrund Legendes [zum Buildprozess](../web-deployment-in-the-enterprise/understanding-the-build-process.md)beschriebenen Ansatz der Split Project-Datei, in dem der Buildprozess von zwei&#x2014;Projektdateien gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="scenario-overview"></a>Übersicht über das Szenario

Das allgemeine Szenario für diese Tutorials wird unter [Unternehmensweb Bereitstellung: Szenarioübersicht](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)beschrieben. Es wird empfohlen, dieses Thema zu lesen, bevor Sie mit diesem Tutorial beginnen.

## <a name="how-to-use-this-tutorial"></a>Verwendung dieses Tutorials

Wenn Sie die in diesem Tutorial beschriebenen Aufgaben zum ersten Mal ausgeführt haben oder wenn Sie die Beispiele mit der Beispiellösung befolgen möchten, sollten Sie die Lernprogramm Themen in der richtigen Reihenfolge durcharbeiten. Alternativ können Sie einzelne Themen als Leitfaden für bestimmte Aufgaben verwenden. Dieses Tutorial enthält die folgenden Themen:

- [Erstellen eines Team Projekts in TFS](creating-a-team-project-in-tfs.md). Ein Teamprojekt ist die Kerneinheit für die Quell Code Verwaltung, Prozess Verwaltung und Erstellung in TFS. Sie müssen ein Teamprojekt erstellen, bevor Sie der Quell Code Verwaltung Inhalt hinzufügen oder Builddefinitionen erstellen können.
- [Hinzufügen von Inhalt zur Quell](adding-content-to-source-control.md)Code Verwaltung. Nachdem Sie ein Teamprojekt erstellt haben, können Sie mit dem Hinzufügen von Inhalt zur Quell Code Verwaltung beginnen. Sie müssen Ihre Projekte und Projektmappen sowie alle externen Abhängigkeiten hinzufügen, bevor Sie mit der Konfiguration von Builds beginnen können.
- [Konfigurieren eines TFS-Buildservers für die Webbereitstellung](configuring-a-tfs-build-server-for-web-deployment.md). Wenn Sie den Teamprojekt Inhalt erstellen möchten, müssen Sie einen Buildserver konfigurieren. In den meisten Fällen sollte dies auf einem separaten Computer von ihrer TFS-Installation erfolgen. Zum Konfigurieren eines Buildservers müssen Sie den TFS-Builddienst installieren und konfigurieren, Visual Studio 2010 installieren, BuildController und Build-Agents erstellen, alle Produkte oder Komponenten installieren, die Ihr Code benötigt, um erfolgreich zu erstellen, und installieren Sie das Internetinformationsdienste (IIS)-Webbereitstellungs Tool (Web deploy).
- [Erstellen einer Builddefinition, die Bereitstellung unterstützt](creating-a-build-definition-that-supports-deployment.md). Bevor Sie mit der Warteschlange oder dem Auslösen von Builds in TFS beginnen können, müssen Sie mindestens eine Builddefinition für das Teamprojekt erstellen. Die Builddefinition definiert jeden Aspekt des Builds, einschließlich der Elemente, die in den Build eingeschlossen werden sollen, was den Build auslöst, und wo die Buildausgaben von Team Build gesendet werden sollen. Sie können eine Builddefinition konfigurieren, um benutzerdefinierte Microsoft-Build-Engine (MSBuild)-Projektdateien auszuführen, sodass Sie die Bereitstellungs Logik in die automatisierten Builds einschließen können.
- Bereitstellen [eines bestimmten Builds](deploying-a-specific-build.md). In vielen Szenarien sollten Sie einen bestimmten Build anstelle des aktuellen Builds in einer Zielumgebung bereitstellen. In diesem Fall können Sie eine Builddefinition konfigurieren, mit der Inhalt aus einem bestimmten Ablage Ordner bereitgestellt wird.
- [Konfigurieren von Berechtigungen für die Team Build-Bereitstellung](configuring-permissions-for-team-build-deployment.md). Wenn der Builddienst Inhalte als Teil eines automatisierten Buildprozesses bereitstellen soll, müssen Sie dem Builddienstkonto für alle Zielweb Server und Datenbankserver verschiedene Berechtigungen erteilen.

## <a name="key-technologies"></a>Wichtige Technologien

Dieses Tutorial konzentriert sich auf die Verwendung dieser Produkte und Technologien, um die automatisierte Build-und Webbereitstellung zu unterstützen:

- Visual Studio Team Foundation Server 2010
- Team Build und MSBuild
- Web Deploy

Das Tutorial berührt außerdem die Verwendung von Windows Server 2008 R2, IIS 7,5, SQL Server 2008 R2, ASP.NET 4,0 und ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Weitere Tutorials in dieser Reihe

Dies ist Teil einer Reihe von fünf Tutorials für die Webbereitstellung im Unternehmen. Dies sind die anderen Tutorials in der Reihe:

- Bereitstellen von [Webanwendungen in Unternehmens Szenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) Dieser einführende Inhalt enthält den kontextbezogenen Hintergrund der tutorialreihe. Es beschreibt das Tutorial-Szenario und veranschaulicht, wie die in der Reihe beschriebenen Aufgaben und exemplarischen Vorgehensweisen in einen umfassenderen Application Lifecycle Management (ALM)-Prozess passen.
- [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Tutorial enthält eine konzeptionelle Einführung in MSBuild-Projektdateien, die Web Publishing Pipeline (WPP), Web deploy und andere verwandte Technologien. Es wird erläutert, wie Sie diese Tools zum Verwalten komplexer Bereitstellungs Prozesse verwenden können.
- [Konfigurieren von Server Umgebungen für die Webbereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie Sie Windows Server zur Unterstützung verschiedener Bereitstellungs Szenarien konfigurieren, einschließlich der Remoteweb Paket Bereitstellung mithilfe des Web Deployment Agent-Diensts (Remote-Agent) oder des Web deploy Handlers und der Remote Datenbank Es enthält Anleitungen zum Auswählen der geeigneten Bereitstellungs Methode für Ihre eigene Umgebung. Außerdem wird beschrieben, wie Sie das Web Farm Framework (WFF) verwenden, um bereitgestellte Webanwendungen auf allen Webservern in einer Server Farm zu replizieren.
- [Erweiterte Unternehmensweb Bereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In diesem Tutorial wird beschrieben, wie Sie verschiedene erweiterte Bereitstellungs Aufgaben durchführen können, wie z. b. das Anpassen von Daten Bank Bereitstellungen für mehrere Umgebungen, das Ausschließen von Dateien und Ordnern aus der Bereitstellung und das offline schalten von Webanwendungen .

> [!div class="step-by-step"]
> [Weiter](creating-a-team-project-in-tfs.md)
