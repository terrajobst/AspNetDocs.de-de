---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Erweiterte Unternehmensweb Bereitstellung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Tutorial wird gezeigt, wie Sie verschiedene Aufgaben durchführen, die in vielen Szenarien für die Unternehmens Bereitstellung erforderlich oder erwünscht sind. Für ein italienisches Übersetzungs...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: bf4c7d021763017592483df35cb717295c4924aa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474957"
---
# <a name="advanced-enterprise-web-deployment"></a>Erweiterte webbasierte Unternehmensbereitstellung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Tutorial wird gezeigt, wie Sie verschiedene Aufgaben durchführen, die in vielen Szenarien für die Unternehmens Bereitstellung erforderlich oder erwünscht sind.
> 
> Eine italienische Übersetzung dieser Tutorials finden Sie unter [http://www.lucamorelli.it](http://www.lucamorelli.it).

Dies ist Teil einer Reihe von Tutorials, die auf den Unternehmens Bereitstellungs Anforderungen eines fiktiven Unternehmens namens Fabrikam, Inc. basieren. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der&#x2014; [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) -Lösung verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Mittelpunkt dieser Tutorials basiert auf dem Untergrund Legendes [zum Buildprozess](../web-deployment-in-the-enterprise/understanding-the-build-process.md)beschriebenen Ansatz der Split Project-Datei, in dem der Buildprozess von zwei&#x2014;Projektdateien gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="scenario-overview"></a>Übersicht über das Szenario

Das allgemeine Szenario für diese Tutorials wird unter [Unternehmensweb Bereitstellung: Szenarioübersicht](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)beschrieben. Es wird empfohlen, dieses Thema zu lesen, bevor Sie mit diesem Tutorial beginnen.

## <a name="how-to-use-this-tutorial"></a>Verwendung dieses Tutorials

- Jedes der Themen in diesem Tutorial ist eigenständig und behandelt eine bestimmte Herausforderung oder ein Problem, das in Szenarios für die Unternehmens Bereitstellung auftritt. Sie müssen diese Themen nicht in einer bestimmten Reihenfolge durcharbeiten. In diesem Tutorial werden jedoch einige erweiterte Aufgaben behandelt. Daher sollten Sie sich mit den Konzepten und Techniken vertraut machen, die von der [Webbereitstellung im](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) Lernprogramm für Unternehmen abgedeckt werden, um von diesen Inhalten den größten Nutzen zu erzielen.
- Dieses Tutorial enthält die folgenden Themen:
- [Ausführen einer "What if"-Bereitstellung](performing-a-what-if-deployment.md). In vielen Szenarien sollten Sie die Auswirkung einer vorgeschlagenen Bereitstellung auf eine Zielumgebung oder auf vorhandene Inhalte ermitteln, bevor Sie tatsächlich Änderungen vornehmen. In diesem Thema wird beschrieben, wie Sie eine "Was-wäre-wenn"-Bereitstellung zum Generieren von Protokolldateien und Datenbankupdate Skripts ausführen können, als hätten Sie Inhalte in einer Zielumgebung bereitgestellt, ohne tatsächlich Änderungen vorzunehmen. Das Analysieren dieser Ressourcen kann Ihnen helfen, potenzielle Probleme im Vorfeld einer Live Bereitstellung zu erkennen.
- [Anpassen von Daten Bank Bereitstellungen für mehrere Umgebungen](customizing-database-deployments-for-multiple-environments.md). Wenn Sie ein Datenbankprojekt für mehrere Ziele bereitstellen, sollten Sie die Bereitstellungs Eigenschaften für jede Zielumgebung häufig anpassen. Beispielsweise würden Sie in Testumgebungen in der Regel die Datenbank bei jeder Bereitstellung neu erstellen, während Sie in der Stagingumgebung oder in der Produktionsumgebung viel wahrscheinlicher sind, inkrementelle Updates zu erstellen, um Ihre Daten beizubehalten. In diesem Thema wird beschrieben, wie Sie diese Eigenschafts Änderungen in die Bereitstellungs Logik einbinden können, indem Sie eine Umgebungs spezifische Bereitstellungs Konfigurationsdatei (sqldeployment) für jede Zielumgebung erstellen.
- Bereitstellen von Daten bankrollen Mitgliedschaften in Test Umgebungen. Wenn Sie z. b. eine Datenbank&#x2014;für jede Bereitstellung neu erstellen, z. b. als Teil einer Continuous Integration (CI)&#x2014;-Build und-Bereitstellung in einer Testumgebung, müssen Sie in der Regel jedes Mal Daten bankrollen Mitgliedschaften konfigurieren. Beispielsweise müssen Sie in der Regel der Anwendungs Pool Identität, die Ihrer Webanwendung zugeordnet ist, Berechtigungen erteilen. In diesem Thema wird beschrieben, wie Sie diesen Prozess automatisieren können, indem Sie der Bereitstellungs Logik ein SQL-Skript nach der Bereitstellung hinzufügen.
- Bereitstellen [von Mitgliedschafts Datenbanken in Unternehmensumgebungen](deploying-membership-databases-to-enterprise-environments.md). ASP.net-Mitgliedschafts Datenbanken haben verschiedene Merkmale, die den Bereitstellungs Prozess erschweren können. Beispielsweise wird bei einer reinen Schema Bereitstellung die Datenbank in einem nicht betriebsbereiten Zustand belassen. In den meisten Szenarien ist es vorzuziehen, eine Mitgliedschafts Datenbank direkt in jeder Zielumgebung zu erstellen. Wenn Sie jedoch eine Mitgliedschafts Datenbank bereitstellen müssen, werden in diesem Thema einige der Ansätze beschrieben, die Sie verwenden können, um die inhärenten Herausforderungen zu erfüllen.
- [Ausschließen von Dateien und Ordnern aus der Bereitstellung](excluding-files-and-folders-from-deployment.md). In einigen Szenarien möchten Sie den Inhalt Ihres Webpakets an bestimmte Ziel Umgebungen anpassen. Beispielsweise können Sie Vollversionen von JavaScript-Bibliotheken einschließen, wenn Sie in einer Testumgebung bereitstellen, um das Client seitige Debuggen zu unterstützen, aber Sie sollten bei der Bereitstellung in einer Staging-oder Produktionsumgebung die minimalen Versionen der Bibliotheken verwenden. In diesem Thema wird beschrieben, wie Sie bestimmte Dateien und Ordner aus dem Paket Erstellungs Prozess ausschließen können.
- [Offline schalten von Webanwendungen mit Web deploy](taking-web-applications-offline-with-web-deploy.md). Wenn Sie Lösungen in einer Staging-oder Produktionsumgebung bereitstellen, sollten Sie Ihre Webanwendungen für die Dauer des Bereitstellungs Prozesses häufig offline schalten. In diesem Thema wird beschrieben, wie Sie eine *App\_offline. htm* -Datei zu Beginn des Bereitstellungs Prozesses zu Ihrer Webanwendung hinzufügen und am Ende entfernen können. Während die *App\_offline. htm* -Datei vorhanden ist, werden alle Benutzer, die zur Webanwendung navigieren, automatisch an die *App\_offline. htm* -Datei umgeleitet.
- [Ausführen von Windows PowerShell-Skripts von MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Viele Bereitstellungs Szenarien erfordern komplexere Aktionen nach der Bereitstellung, z. b. das Hinzufügen benutzerdefinierter Ereignis Quellen zur Registrierung oder das Konfigurieren der Replikation zwischen SQL Server Instanzen. Diese Aktionen werden oft mithilfe von Windows PowerShell-Skripts durchgeführt. In diesem Thema wird beschrieben, wie Sie Windows PowerShell-Skripts aus einer Microsoft-Build-Engine-Projektdatei (MSBuild) als Teil des Build-und Bereitstellungs Prozesses ausführen.
- [Problembehandlung bei der Paket](troubleshooting-the-packaging-process.md)Erstellung. Die Webpublishing Pipeline (WPP) definiert eine MSBuild-Eigenschaft mit dem Namen **enablepackageprocessloggingandassert** , mit der Sie ausführliche Informationen zum Verpackungsprozess für Webanwendungs Projekte erstellen können. In diesem Thema wird beschrieben, wie die-Eigenschaft funktioniert und wie Sie verwendet wird.

## <a name="key-technologies"></a>Wichtige Technologien

Dieses Tutorial konzentriert sich auf die Verwendung dieser Produkte und Technologien, um die automatisierte Build-und Webbereitstellung zu unterstützen:

- Visual Studio 2010 und Team Foundation Server (TFS) 2010
- MSBuild und TFS-TeamBuild
- Internetinformationsdienste (IIS) 7,5
- IIS-Webbereitstellungs Tool (Web deploy) 2,1
- Das Hilfsprogramm zur Daten Bank Bereitstellung von VSDBCMD. exe

## <a name="other-tutorials-in-this-series"></a>Weitere Tutorials in dieser Reihe

Dies ist Teil einer Reihe von fünf Tutorials für die Webbereitstellung im Unternehmen. Dies sind andere Tutorials in der Reihe:

- Bereitstellen von [Webanwendungen in Unternehmens Szenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) Dieser einführende Inhalt enthält den kontextbezogenen Hintergrund der tutorialreihe. Es beschreibt das Tutorial-Szenario und veranschaulicht, wie die in der Reihe beschriebenen Aufgaben und exemplarischen Vorgehensweisen in einen umfassenderen Application Lifecycle Management (ALM)-Prozess passen.
- [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Tutorial enthält eine konzeptionelle Einführung in MSBuild-Projektdateien, WPP, Web deploy und andere verwandte Technologien. Es wird erläutert, wie Sie diese Tools zum Verwalten komplexer Bereitstellungs Prozesse verwenden können.
- [Konfigurieren von Server Umgebungen für die Webbereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie Sie Windows Server zur Unterstützung verschiedener Bereitstellungs Szenarien konfigurieren, einschließlich der Remoteweb Paket Bereitstellung mithilfe des Web Deployment Agent-Diensts (Remote-Agent) oder des Web deploy Handlers und der Remote Datenbank Es enthält Anleitungen zum Auswählen der geeigneten Bereitstellungs Methode für Ihre eigene Umgebung. Außerdem wird beschrieben, wie Sie das Web Farm Framework (WFF) verwenden, um bereitgestellte Webanwendungen auf allen Webservern in einer Server Farm zu replizieren.
- [Konfigurieren von Team Foundation Server für die Webbereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie Sie TFS zur Unterstützung verschiedener Bereitstellungs Szenarien konfigurieren, einschließlich automatisierter Bereitstellung als Teil eines CI-Prozesses und Manuelles Auslösen von bereit Stellungen spezifischer Builds.

> [!div class="step-by-step"]
> [Weiter](performing-a-what-if-deployment.md)
