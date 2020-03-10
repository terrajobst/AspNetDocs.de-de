---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Unternehmensweb Bereitstellung: Szenarioübersicht | Microsoft-Dokumentation'
author: jrjlee
description: In diesen Tutorials wird eine Beispiellösung mit realistischer Komplexität und einem fiktiven Szenario für die Unternehmens Bereitstellung verwendet, um einen Verweis bereitzustellen...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 9786879844da13c21e6a953b1ab24b29ca8121e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463407"
---
# <a name="enterprise-web-deployment-scenario-overview"></a>Unternehmensweb Bereitstellung: Szenarioübersicht

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesen Tutorials wird eine Beispiellösung mit einer realistischen Komplexität und einem fiktiven Szenario für die Unternehmens Bereitstellung verwendet, um eine Referenz Implementierung bereitzustellen und um den Aufgaben und exemplarischen Vorgehensweisen einen allgemeinen Kontext zu bieten. In diesem Thema wird das Tutorial-Szenario beschrieben und die Beispiellösung eingeführt.

## <a name="scenario-description"></a>Beschreibung des Szenarios

Fabrikam, Inc., ein fiktives Unternehmen, erstellt eine Lösung, mit der Remote Vertriebsteams Kontaktinformationen von einer Weboberfläche speichern und abrufen können.

Die Application Lifecycle Management (ALM)-Prozesse bei Fabrikam, Inc. erfordern, dass die Lösung in verschiedenen Phasen des Softwareentwicklungsprozesses in drei Serverumgebungen bereitgestellt wird:

- Ein Entwickler Test oder eine Sandkasten Umgebung.
- Eine intranetbasierte Stagingumgebung.
- Eine Produktionsumgebung mit Internet Zugriff.

Jede dieser Umgebungen hat unterschiedliche Konfigurations-und Sicherheitsanforderungen, und jede dieser Umgebungen stellt besondere Probleme bei der Bereitstellung dar.

### <a name="the-fabrikam-inc-server-infrastructure"></a>Die Server Infrastruktur von Fabrikam, Inc.

Dies ist die allgemeine Entwicklungs-und Bereitstellungs Infrastruktur bei Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Die Arbeitsstationen für Entwickler, die Quell Code Verwaltungsinfrastruktur, die Entwickler Testumgebung und die Stagingumgebung befinden sich alle im Intranetnetzwerk innerhalb der Fabrikam.net-Domäne. Die Produktionsumgebung befindet sich in einem Umkreis Netzwerk (auch bekannt als DMZ, abmilitarisierte Zone und überwachtes Subnetz), das durch eine Firewall vom Intranetnetzwerk isoliert ist. Dies ist ein gängiges Bereitstellungs Szenario: Sie isolieren in der Regel Ihre Internet seitigen Webserver von ihrer internen Serverinfrastruktur durch die Verwendung von Firewalls oder Gatewayservern.

In diesem Beispiel:

- Ein Team Foundation Server Server (TFS) 2010 mit einem separaten Buildserver stellt Funktionen für die Quell Code Verwaltung und Continuous Integration (CI) bereit.
- Die Testumgebung für Entwickler umfasst einen Internetinformationsdienste (IIS) 7,5-Webserver und einen SQL Server 2008 R2-Datenbankserver.
- Die Produktionsumgebung umfasst mehrere IIS 7,5-Webserver, die von einem Wff-Controller Server (Web Farm Framework) synchronisiert werden, sowie einen SQL Server 2008 R2-Datenbankserver. In der Praxis kann der Datenbankserver Clustering oder Spiegelung verwenden, um die Skalierbarkeit und Verfügbarkeit zu verbessern.
- Die Stagingumgebung ist so konzipiert, dass die Konfiguration der Produktionsumgebung so genau wie möglich repliziert wird.
- Die Firewall-und Netzwerk Isolations Richtlinien erlauben keine direkte, automatisierte Bereitstellung aus dem Intranet in das Umkreis Netzwerk.

Die Konfiguration der einzelnen Umgebungen wird im zweiten Tutorial, [Konfigurieren von Server Umgebungen für die Webbereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md), ausführlicher beschrieben.

### <a name="team-roles-for-alm"></a>Team Rollen für Alm

Diese Benutzer sind daran beteiligt, die Contact Manager-Lösung zu erstellen, zu verwalten, zu erstellen und zu veröffentlichen:

- Matt hink ist ein Webanwendungs Entwickler bei Fabrikam, Inc. Er ist Teil des Teams, das die Contact Manager-Lösung mithilfe von Visual Studio 2010 entwickelt hat. Matt hat vollständige Administratorrechte für die Server in der Testumgebung des Entwicklers, sodass er die Umgebung so konfigurieren kann, dass er seine Anforderungen erfüllt. Er verfügt auch über Benutzer Zugriff auf die Visual Studio 2010 TFS-Instanz, in der er den Quellcode für die Contact Manager-Lösung speichert.
- Rob Walters ist Server Administrator für das Entwicklungsteam von Fabrikam, Inc. Rob hat Administrator Zugriff auf den TFS-Server, sodass er alle Aspekte von TFS und Team Build konfigurieren kann. Rob verfügt auch über Administrator Zugriff auf die Test-und Stagingwebserver und fungiert als Datenbankadministrator (DBA) für die Datenbankserver in den Test-und Stagingumgebungen. Rob hat Team Build auf dem TFS-Server konfiguriert, um die folgenden Aufgaben auszuführen:

    - Erstellen Sie Komponententests für die Anwendung, und führen Sie Sie aus, wenn ein Benutzer eine Datei in TFS eincheckt. Dies wird als CI bezeichnet.
    - Stellen Sie die Kontakt-Manager-Anwendung automatisch in der Testumgebung bereit, sobald die Anwendung Komponententests bestanden hat. Dies umfasst das Veröffentlichen der Datenbank auf den Testservern bei der erstmaligen Bereitstellung und die Aktualisierung der Datenbank nach der erstmaligen Bereitstellung.
    - Stellen Sie die Kontakt-Manager-Anwendung in einem einzigen Schritt in der Stagingumgebung bereit.
    - Erstellen Sie ein Webpaket, das von einem Webserver Administrator und einem DBA verwendet werden kann, um die Anwendung in der Produktionsumgebung zu veröffentlichen.
- Lisa Andrews ist ein Server Administrator, der für die Bereitstellung von Anwendungen auf den Produktionsservern von Fabrikam, Inc. zuständig ist. Sie verfügt über Lesezugriff auf die Freigabe, in der der TFS-TeamBuild das Webbereitstellungs Paket speichert, nachdem es die Contact Manager-Anwendung erstellt hat. Außerdem verfügt sie über Administrator Zugriff auf die produktionsweb Server, sodass Sie die Anwendung für die Produktion bereitstellen kann. Außerdem fungiert sie als DBA, der Datenbanken und Datenbankupdates für den Datenbankserver in der Produktionsumgebung bereitstellt.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>Contact Manager-Lösung

Mithilfe der Contact Manager-Lösung können registrierte, angemeldete Benutzer Kontaktinformationen über eine Weboberfläche hinzufügen und bearbeiten. Die Contact Manager-Lösung besteht aus vier einzelnen Projekten:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager. MVC**. Dies ist ein ASP.net MVC3-Webanwendungs Projekt, das den Einstiegspunkt für die Lösung darstellt. Sie bietet einige grundlegende Webanwendungs Funktionen, wie z. b. die Bereitstellung von Kontaktinformationen durch Benutzer. Die Anwendung basiert auf einem Windows Communication Foundation (WCF)-Dienst zum Verwalten von Kontakten und einer ASP.NET-Anwendungs Dienst Datenbank zum Verwalten der Authentifizierung und Autorisierung.
- **ContactManager. Database**. Dabei handelt es sich um ein Visual Studio 2010-Datenbankprojekt. Das-Projekt definiert das Schema für eine Datenbank, in der die Kontaktinformationen gespeichert werden.
- **ContactManager. Service**. Dies ist ein WCF-Webdienst Projekt. Der WCF macht einen Endpunkt verfügbar, der es Aufrufern ermöglicht, in der Contact Manager-Datenbank Vorgänge zum Erstellen, abrufen, aktualisieren und löschen (CRUD) auszuführen. Der Dienst basiert auf der Contact Manager-Datenbank und der Assembly ContactManager. Common. dll.
- **ContactManager. Common**. Dies ist ein Klassen Bibliotheksprojekt. Der WCF-Dienst basiert auf in dieser Assembly definierten Typen.

Eine umfassende Übersicht über die Lösung und die zugehörigen Bereitstellungs Anforderungen finden Sie im ersten Tutorial dieser Reihe, der [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Bereitstellungsaufgaben

Bei der Bereitstellung von Anwendungen in unterschiedlichen Umgebungen in einem großen Unternehmen sind verschiedene Aufgaben zu erledigen. Dies sind die wichtigsten Aufgaben, die in den Tutorials behandelt werden:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Im folgenden finden Sie eine Liste der einzelnen Schritte im Bereitstellungs Prozess aus der Sicht der Benutzer, die weiter oben in diesem Dokument beschrieben wurden:

1. Alle Mitglieder des Teams überprüfen die Contact Manager-Lösung in Visual Studio 2010, um wichtige Bereitstellungs Anforderungen und-Probleme zu ermitteln.
2. Matt hink kann die Contact Manager-Lösung direkt von der Entwickler Arbeitsstation aus in der Entwicklungs Testumgebung bereitstellen, um einen anfänglichen Test der Bereitstellungs Logik auszuführen.
3. Matt hink fügt die Anwendung der Quell Code Verwaltung in TFS hinzu.
4. Rob Walters erstellt verschiedene Builddefinitionen für die Contact Manager-Lösung in Team Build. Eine Builddefinition verwendet CI, um die Lösung in der Testumgebung des Entwicklers bereitzustellen, wenn ein Benutzer neuen Code eincheckt. Eine andere Builddefinition ermöglicht Benutzern, bereit Stellungen in der Stagingumgebung nach Bedarf zu initiieren.
5. Jedes Mal, wenn ein Benutzer neuen Code eincheckt, erstellt Team Build automatisch die Lösungskomponenten, führt Komponententests aus und stellt die Projekt Mappe in der Testumgebung des Entwicklers bereit, wenn der Build erfolgreich war und die Komponententests bestanden wurden.
6. Wenn ein Benutzer eine Bereitstellung in der Stagingumgebung auslöst, wird die Projekt Mappe in einem einzigen Schritt verpackt und bereitgestellt. Dieser Prozess generiert auch ein Paket für die manuelle Bereitstellung in der Produktionsumgebung.
7. Lisa Andrews stellt die Anwendung in der Produktionsumgebung bereit, indem das in Schritt 6 erstellte Webpaket manuell importiert wird.

### <a name="key-deployment-issues"></a>Wichtige Bereitstellungs Probleme

Die Contact Manager-Lösung und das Szenario Fabrikam, Inc. zeigen verschiedene häufige Probleme und Herausforderungen, die bei der Bereitstellung komplexer, Unternehmens basierter Lösungen auftreten können. Beispiel:

- Sie müssen Projekte in mehreren Umgebungen bereitstellen können, z. b. in Entwickler-oder Testumgebungen, Staging-und Produktionsservern. Die Lösung muss mit unterschiedlichen Konfigurationseinstellungen für jede Umgebung bereitgestellt werden.
- Sie müssen mehrere abhängige Projekte gleichzeitig im Rahmen eines einstufigen oder automatisierten Build-und Bereitstellungs Prozesses bereitstellen.
- Sie müssen in der Lage sein, die Bereitstellung aus einem automatisierten Prozess zu steuern. Sie möchten z. b. einen CI-Prozess zum Bereitstellen von Webanwendungen in einer Stagingumgebung verwenden, wenn neuer Code eingeglichen wird.
- Sie müssen in der Lage sein, den Bereitstellungs Prozess zu steuern und Bereitstellungs Variablen von außerhalb von Visual Studio festzulegen, da es unwahrscheinlich ist, dass Entwickler die richtigen Konfigurationseinstellungen oder die erforderlichen Anmelde Informationen für jede Zielumgebung haben.
- Sie müssen Schema basierte Datenbankprojekte bereitstellen und vorhandene Daten in nachfolgenden bereit Stellungen beibehalten.
- Sie müssen Mitgliedschafts Datenbanken auf Ad-hoc-Basis bereitstellen, ohne Benutzerkontodaten bereitzustellen. Sie müssen möglicherweise auch das Schema der bereitgestellten Mitgliedschafts Datenbanken aktualisieren, ohne vorhandene Benutzerkontodaten zu verlieren.
- Sie müssen bestimmte Dateien oder Ordner ausschließen, wenn Sie Inhalte in verschiedenen Ziel Umgebungen bereitstellen.

Außerdem werden beim Verwalten der Bereitstellung bei häufigen und inkrementellen Updates einige zusätzliche Herausforderungen entstehen. Beispiel:

- Komponententests werden jedes Mal ausgeführt, wenn ein Entwickler neuen Code eincheckt. Sie möchten die Projekt Mappe nur dann bereitstellen, wenn der Code die Komponententests übergibt.
- Wenn Sie eine Webanwendung in einer Staging-oder Produktionsumgebung bereitstellen, möchten Sie die Benutzer für die Dauer des Bereitstellungs Prozesses zu einer *App\_offline. htm* -Datei umleiten.
- Sie möchten Bereitstellungs Aktivitäten protokollieren. Der Bereitstellungs Prozess sollte e-Mail-Benachrichtigungen erfolgreicher oder fehlgeschlagener bereit Stellungen an bestimmte Empfänger senden.
- Wenn bei einer automatisierten Bereitstellung ein Fehler auftritt, sollte der Bereitstellungs Prozess die aktuelle Bereitstellung wiederholen oder stattdessen das vorherige Webpaket bereitstellen.

> [!div class="step-by-step"]
> [Zurück](deploying-web-applications-in-enterprise-scenarios.md)
> [Weiter](application-lifecycle-management-from-development-to-production.md)
