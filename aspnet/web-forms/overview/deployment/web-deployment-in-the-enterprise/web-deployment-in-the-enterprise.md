---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Webbereitstellung im Unternehmen | Microsoft-Dokumentation
author: jrjlee
description: In diesem Tutorial wird beschrieben, wie Sie viele der Herausforderungen erfüllen, die bei der Verwaltung der Bereitstellung von Webanwendungen auf Unternehmens Niveau für die deplizerstellung auftreten...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: bc7bb676a71af4ec3451aa3adf3c03ce3b5200d5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509715"
---
# <a name="web-deployment-in-the-enterprise"></a>Webbereitstellung im Unternehmen

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Tutorial wird beschrieben, wie Sie viele der Herausforderungen erfüllen, die beim Verwalten der Bereitstellung von Webanwendungen für Unternehmen in Entwicklungs-, Test-, Staging-und Produktionsumgebungen auftreten. Das Tutorial enthält eine Referenzlösung sowie eine Mischung aus konzeptionellen und aufgabenorientierten Inhalten, die Sie durch verschiedene gängige Aufgaben und Verfahren führen.
> 
> Eine italienische Übersetzung dieser Tutorials finden Sie unter [http://www.lucamorelli.it](http://www.lucamorelli.it).

## <a name="enterprise-deployment-challenges"></a>Unternehmen

Organisationen stoßen häufig auf diese Herausforderungen, wenn Sie die Bereitstellung komplexer, Unternehmens basierter Lösungen verwalten:

- Sie müssen Projekte in mehreren Umgebungen bereitstellen können, z. b. in Entwickler-oder Testumgebungen, Staging-und Produktionsservern. Die Lösung muss mit unterschiedlichen Konfigurationseinstellungen für jede Umgebung bereitgestellt werden.
- Sie müssen mehrere abhängige Projekte gleichzeitig im Rahmen eines einstufigen oder automatisierten Build-und Bereitstellungs Prozesses bereitstellen.
- Sie müssen in der Lage sein, die Bereitstellung aus einem automatisierten Prozess zu steuern. Sie möchten z. b. einen Continuous Integration (CI)-Prozess zum Bereitstellen von Webanwendungen in einer Testumgebung verwenden, wenn neuer Code eingeglichen wird.
- Sie müssen in der Lage sein, den Bereitstellungs Prozess zu steuern und Bereitstellungs Variablen von außerhalb von Visual Studio festzulegen, da es unwahrscheinlich ist, dass Entwickler die richtigen Konfigurationseinstellungen oder die erforderlichen Anmelde Informationen für jede Zielumgebung haben.
- Sie müssen Schema basierte Datenbankprojekte bereitstellen und vorhandene Daten in nachfolgenden bereit Stellungen beibehalten.
- Sie müssen Mitgliedschafts Datenbanken auf Ad-hoc-Basis bereitstellen, ohne Benutzerkontodaten bereitzustellen. Sie müssen möglicherweise auch das Schema der bereitgestellten Mitgliedschafts Datenbanken aktualisieren, ohne vorhandene Benutzerkontodaten zu verlieren.
- Sie müssen bestimmte Dateien oder Ordner ausschließen, wenn Sie Inhalte in verschiedenen Ziel Umgebungen bereitstellen.

## <a name="overview-of-approach"></a>Übersicht über den Ansatz

In diesem Tutorial wird zusammen mit den anderen Tutorials dieser Reihe dieser allgemeine Ansatz verwendet, um die oben beschriebenen Herausforderungen zu erfüllen.

- **Verwenden Sie benutzerdefinierte Microsoft-Build-Engine (MSBuild)-Projektdateien, um den gesamten Build-und Bereitstellungs Prozess zu steuern.**
- Auf diese Weise können Sie jedes Projekt in der Projekt Mappe als Teil eines einzelnen Skript fähigen Vorgangs erstellen und bereitstellen.
- Umgebungs spezifische Einstellungen werden mithilfe einfacher Umgebungs spezifischer Projektdateien konfiguriert. Im Gegensatz zum Visual Studio – zentrierten Ansatz der Verwendung von Projektmappenkonfigurationen und Veröffentlichungs Profilen zum Konfigurieren von bereit Stellungen für verschiedene Umgebungen können Sie mit diesem Ansatz den Bereitstellungs Prozess von außerhalb von Visual Studio konfigurieren und verwalten. Dies bedeutet, dass Entwickler keine voraus Kenntnisse über Verbindungs Zeichenfolgen, Dienst Endpunkte, Server Anmelde Informationen und andere Bereitstellungs Variablen für Ziel Umgebungen benötigen.
- Die benutzerdefinierten Projektdateien können von Team Build als Teil eines Team Foundation Server-Workflows (TFS) aufgerufen werden. Auf diese Weise können Sie die automatisierte Bereitstellung für CI-Szenarien konfigurieren.

**Verwenden Sie das Internetinformationsdienste (IIS)-Webbereitstellungs Tool (Web deploy), um Webanwendungs Projekte zu verpacken und bereitzustellen.**

- Web deploy bietet ein Framework, mit dem Sie Ihren Webanwendungs Inhalt Verpacken und auf einem IIS-Ziel Webserver sowie Abhängigkeiten, Konfigurationseinstellungen, Sicherheitseinstellungen und andere Anforderungen bereitstellen können.
- Sie können den gesamten Verpackungs-und Bereitstellungs Prozess innerhalb Ihrer benutzerdefinierten MSBuild-Projektdateien steuern. Sie können auch die Konfigurationseinstellungen ändern, die das Webbereitstellungs Paket begleiten, wie z. b. Verbindungs Zeichenfolgen, Dienst Endpunkte und Details zum IIS-Ziel.
- Web deploy bietet neben der Webpublishing Pipeline viele Erweiterbarkeits Punkte, mit denen Sie Ihre bereit Stellungen anpassen können. Beispielsweise ist es ganz einfach, unerwünschte Dateien und Ordner aus ihren Webbereitstellungs Paketen auszuschließen.

**Verwenden Sie das Hilfsprogramm "VSDBCMD. exe" zum Bereitstellen und Aktualisieren von Datenbankschemas.**

- VSDBCMD ermöglicht Ihnen die Bereitstellung von Datenbanken aus einer Datenbankschema Datei (. dbschema), die generiert wird, wenn Sie ein Visual Studio-Datenbankprojekt erstellen. Im Gegensatz dazu eignet sich die in Web deploy enthaltene Daten Bank Bereitstellungs Funktionalität besser zum Bereitstellen vorhandener Datenbanken aus einer lokalen SQL Server Instanz.
- Anders als die Funktionen von Visual Studio zum Bereitstellen von Datenbankprojekten können Sie mit VSDBCmd differenzielle Updates für eine vorhandene Zieldatenbank bereitstellen. Dies ermöglicht es Ihnen, vorhandene Daten beizubehalten, während Sie das Datenbankschema aktualisieren.
- Sie können VSDBCmd-Befehle in Ihren benutzerdefinierten MSBuild-Projektdateien ausführen.

## <a name="content-map"></a>Inhalts Karte

Dieses Tutorial enthält Themen, die in vier Hauptbereiche unterteilt sind.

In diesen Themen wird die Referenz&#x2014;Lösung der Contact Manager&#x2014;-Lösung vorgestellt, und es wird beschrieben, wie Sie heruntergeladen und auf Ihrem lokalen Computer konfiguriert werden:

- [Contact Manager-Lösung](the-contact-manager-solution.md)
- [Einrichten der Contact Manager-Lösung](setting-up-the-contact-manager-solution.md)

In diesen Themen werden MSBuild-Projektdateien vorgestellt, es wird beschrieben, wie Sie benutzerdefinierte Projektdateien erstellen und verwenden und wie Sie den Bereitstellungs Prozess für die Contact Manager-Lösung durchlaufen:

- [Grundlegendes zur Projektdatei](understanding-the-project-file.md)
- [Grundlegendes zum Buildprozess](understanding-the-build-process.md)

Diese Themen beschreiben die Bereitstellung von Webanwendungen, einschließlich der Funktionsweise des Build-und Verpackungsprozesses, der Integration des Buildprozesses in die Webpublishing Pipeline, das Ändern von Bereitstellungs Parametern und das Bereitstellen von Webpaketen für das Ziel. Umgebungen

- [Erstellen von Webanwendungsprojekten und Paketerstellung](building-and-packaging-web-application-projects.md)
- [Konfigurieren von Parametern für die Bereitstellung von Webpaketen](configuring-parameters-for-web-package-deployment.md)
- [Bereitstellen von Webpaketen](deploying-web-packages.md)

- Beim Bereitstellen von [Datenbankprojekten](deploying-database-projects.md) werden die verschiedenen Techniken beschrieben, mit denen Sie Visual Studio-Datenbankprojekte bereitstellen können, sowie die vor-und Nachteile der einzelnen Ansätze. Beim [Erstellen und Ausführen einer Befehlsdatei für die Bereitstellung](creating-and-running-a-deployment-command-file.md) wird beschrieben, wie Sie eine einfache Befehlsdatei erstellen, die Ihre Bereitstellungs Logik kapselt, und Sie können komplexe Lösungen als einen Schritt bereitstellen.
- Schließlich wird das Tutorial durch [Manuelles Installieren von Webpaketen](manually-installing-web-packages.md) beendet, indem Sie das Importieren von Webpaketen in IIS veranschaulichen.

## <a name="key-technologies"></a>Wichtige Technologien

Die Themen in diesem Tutorial verwenden diese Technologien hauptsächlich zur Verwaltung von Build und Bereitstellung:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- Das Hilfsprogramm zur Daten Bank Bereitstellung von VSDBCMD. exe

## <a name="other-tutorials-in-this-series"></a>Weitere Tutorials in dieser Reihe

Dies ist Teil einer Reihe von fünf Tutorials für die Webbereitstellung im Unternehmen. Dies sind die anderen Tutorials in der Reihe:

- Bereitstellen von [Webanwendungen in Unternehmens Szenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) Dieser einführende Inhalt enthält den kontextbezogenen Hintergrund der tutorialreihe. Es beschreibt das Tutorial-Szenario und veranschaulicht, wie die in der Reihe beschriebenen Aufgaben und exemplarischen Vorgehensweisen in einen umfassenderen Application Lifecycle Management (ALM)-Prozess passen.
- [Konfigurieren von Server Umgebungen für die Webbereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie Sie Windows Server zur Unterstützung verschiedener Bereitstellungs Szenarien konfigurieren, einschließlich der Remoteweb Paket Bereitstellung mithilfe des Web Deployment Agent-Diensts (Remote-Agent) oder des Web deploy Handlers und der Remote Datenbank Es enthält Anleitungen zum Auswählen der geeigneten Bereitstellungs Methode für Ihre eigene Umgebung. Außerdem wird beschrieben, wie Sie das Web Farm Framework (WFF) verwenden, um bereitgestellte Webanwendungen auf allen Webservern in einer Server Farm zu replizieren.
- [Konfigurieren von Team Foundation Server für die Webbereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie Sie TFS zur Unterstützung verschiedener Bereitstellungs Szenarien konfigurieren, einschließlich automatisierter Bereitstellung als Teil eines CI-Prozesses und Manuelles Auslösen von bereit Stellungen spezifischer Builds.
- [Erweiterte Unternehmensweb Bereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In diesem Tutorial wird beschrieben, wie Sie verschiedene erweiterte Bereitstellungs Aufgaben durchführen können, wie z. b. das Anpassen von Daten Bank Bereitstellungen für mehrere Umgebungen, das Ausschließen von Dateien und Ordnern aus der Bereitstellung und das offline schalten von Webanwendungen .

> [!div class="step-by-step"]
> [Weiter](the-contact-manager-solution.md)
