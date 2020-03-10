---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Application Lifecycle Management: von der Entwicklung bis zur Produktion | Microsoft-Dokumentation'
author: jrjlee
description: In diesem Thema wird veranschaulicht, wie ein fiktives Unternehmen die Bereitstellung einer ASP.NET-Webanwendung über Test-, Staging-und Produktionsumgebungen als par verwaltet...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 230cf4393db0ee19cfc42ed54359d61e7926a49d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520425"
---
# <a name="application-lifecycle-management-from-development-to-production"></a>Application Lifecycle Management: von der Entwicklung bis zur Produktion

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird veranschaulicht, wie ein fiktives Unternehmen die Bereitstellung einer ASP.NET-Webanwendung mithilfe von Test-, Staging-und Produktionsumgebungen als Teil eines kontinuierlichen Entwicklungsprozesses verwaltet. Im gesamten Thema werden Links zu weiteren Informationen und exemplarischen Vorgehensweisen zur Durchführung bestimmter Aufgaben bereitgestellt.
> 
> Das Thema bietet eine allgemeine Übersicht über eine [Reihe von Tutorials](deploying-web-applications-in-enterprise-scenarios.md) zur Webbereitstellung im Unternehmen. Machen Sie sich keine Sorgen, wenn Sie mit einigen der hier&#x2014;beschriebenen Konzepte nicht vertraut sind. in den folgenden Tutorials finden Sie ausführliche Informationen zu all diesen Aufgaben und Techniken.
> 
> > [!NOTE]
> > Der Einfachheit halber wird in diesem Thema nicht erläutert, wie Datenbanken im Rahmen des Bereitstellungs Prozesses aktualisiert werden. Das Erstellen von inkrementellen Updates an Datenbankfunktionen ist jedoch eine Anforderung vieler Szenarien für die Unternehmens Bereitstellung, und Sie finden eine Anleitung dazu, wie Sie dies später in dieser tutorialreihe erledigen können. Weitere Informationen finden Sie unter Bereitstellen von [Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md).

## <a name="overview"></a>Übersicht

Der hier dargestellte Bereitstellungs Prozess basiert auf dem Bereitstellungs Szenario Fabrikam, Inc., das unter [Unternehmensweb Bereitstellung: Szenarioübersicht](enterprise-web-deployment-scenario-overview.md)beschrieben wird. Lesen Sie die Szenarioübersicht, bevor Sie dieses Thema lesen. Im Wesentlichen wird das Szenario untersucht, wie eine Organisation die Bereitstellung einer relativ komplexen Webanwendung, der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), über verschiedene Phasen in einer typischen Unternehmensumgebung verwaltet.

Im Rahmen des Entwicklungs-und Bereitstellungs Prozesses durchläuft die Contact Manager-Lösung auf hoher Ebene die folgenden Phasen:

1. Ein Entwickler überprüft Code in Team Foundation Server (TFS) 2010.
2. TFS erstellt den Code und führt alle Komponententests aus, die dem Teamprojekt zugeordnet sind.
3. TFS stellt die Lösung für die Testumgebung bereit.
4. Das Entwicklerteam überprüft und überprüft die Projekt Mappe in der Testumgebung.
5. Der Administrator der Stagingumgebung führt in der Stagingumgebung eine "Was-wäre-wenn"-Bereitstellung aus, um festzustellen, ob die Bereitstellung Probleme verursacht
6. Der Administrator der Stagingumgebung führt eine Live Bereitstellung in der Stagingumgebung durch.
7. Die Lösung durchläuft Benutzer Akzeptanz Tests in der Stagingumgebung.
8. Die Webbereitstellungs Pakete werden manuell in die Produktionsumgebung importiert.

Diese Stufen bilden einen Teil eines kontinuierlichen Entwicklungsprozesses.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

In der Praxis ist der Prozess etwas komplizierter als dies, wie Sie sehen werden, wenn wir uns jede Phase ausführlicher ansehen. Fabrikam, Inc. verwendet einen anderen Ansatz für die Bereitstellung für jede Zielumgebung.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Im weiteren Verlauf dieses Themas werden diese wichtigen Phasen dieses Bereitstellungs Lebenszyklus untersucht:

- **Voraussetzungen**: Sie müssen Ihre Serverinfrastruktur konfigurieren, bevor Sie die Bereitstellungs Logik platzieren.
- **Anfängliche Entwicklung und Bereitstellung**: was Sie tun müssen, bevor Sie die Lösung zum ersten Mal bereitstellen.
- **Bereitstellung zum Testen**: das Verpacken und Bereitstellen von Inhalt in einer Testumgebung automatisch, wenn ein Entwickler neuen Code eincheckt.
- **Bereitstellung in**der Stagingumgebung: Bereitstellen spezifischer Builds in einer Stagingumgebung und Ausführen von "Was-wäre-wenn"-bereit Stellungen, um sicherzustellen, dass eine Bereitstellung keine Probleme verursacht.
- **Bereitstellung in der Produktion**: Importieren von Webpaketen in eine Produktionsumgebung, wenn die Netzwerkinfrastruktur eine Remote Bereitstellung verhindert.

## <a name="prerequisites"></a>Voraussetzungen

Die erste Aufgabe in jedem Bereitstellungs Szenario besteht darin, sicherzustellen, dass Ihre Serverinfrastruktur den Anforderungen Ihrer Bereitstellungs Tools und-Verfahren entspricht. In diesem Fall hat Fabrikam, Inc. seine Serverinfrastruktur wie folgt konfiguriert:

- TFS ist so konfiguriert, dass eine Teamprojekt Auflistung, BuildController und Build-Agents einbezogen werden. Weitere Informationen finden Sie unter [Konfigurieren von Team Foundation Server für die automatisierte Webbereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) .
- Die Testumgebung ist so konfiguriert, dass Remote Bereitstellungen mithilfe des Web Deployment Agent-Diensts (dem "Remote-Agent") akzeptiert werden, wie in [Szenario: Konfigurieren einer Testumgebung für die Webbereitstellung](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) und [Konfigurieren eines Webservers für die Web deploy Veröffentlichung (Remote-Agent)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)beschrieben.
- Die Stagingumgebung ist so konfiguriert, dass Remote Bereitstellungen mithilfe des Web deploy handlerendpunkts akzeptiert werden, wie in [Szenario: Konfigurieren einer Stagingumgebung für die Webbereitstellung](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) und [Konfigurieren eines Webservers für die Web deploy Veröffentlichung (Web deploy Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)beschrieben.
- Die Produktionsumgebung ist so konfiguriert, dass ein Administrator Webbereitstellungs Pakete manuell in Internetinformationsdienste (IIS) importieren kann, wie in [Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) und [Konfigurieren eines Webservers für die Web deploy Veröffentlichung (Offline Bereitstellung)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)beschrieben.

## <a name="initial-development-and-deployment"></a>Anfängliche Entwicklung und Bereitstellung

Bevor das Entwicklungsteam von Fabrikam, Inc. das Entwicklungsteam zum ersten Mal die Contact Manager-Lösung bereitstellen kann, muss es diese Aufgaben ausführen:

- Erstellen Sie ein neues Teamprojekt in TFS.
- Erstellen Sie die Microsoft-Build-Engine (MSBuild)-Projektdateien, die die Bereitstellungs Logik enthalten.
- Erstellen Sie die TFS-Builddefinitionen, die die Bereitstellungs Prozesse auslöst.

### <a name="create-a-new-team-project"></a>Neues Team Projekt erstellen

- Der TFS-Administrator Rob Walters erstellt ein neues Teamprojekt für die Anwendung, wie unter [Erstellen eines Teamprojekts in TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md)beschrieben. Als Nächstes erstellt der Lead Entwickler Matt hink eine Skeleton-Lösung. Er prüft seine Dateien in das neue Teamprojekt in TFS, wie unter [Hinzufügen von Inhalt zur Quell](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md)Code Verwaltung beschrieben.

### <a name="create-the-deployment-logic"></a>Erstellen der Bereitstellungs Logik

Matt hink erstellt verschiedene benutzerdefinierte MSBuild-Projektdateien und verwendet dabei den in Grundlegendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz zum Aufteilen von Projektdateien. Matt erstellt Folgendes:

- Eine Projektdatei mit dem Namen " *Publish. proj* ", die den Bereitstellungs Prozess ausführt. Diese Datei enthält MSBuild-Ziele, die die Projekte in der Projekt Mappe erstellen, Webpakete erstellen und die Pakete in einer Zielserver Umgebung bereitstellen.
- Umgebungs spezifische Projektdateien mit dem Namen " *env-dev. proj* " und " *env-Stage. proj*". Diese enthalten Einstellungen, die spezifisch für die Testumgebung und die Stagingumgebung sind, wie z. b. Verbindungs Zeichenfolgen, Dienst Endpunkte und Details des Remote Diensts, der das Webpaket empfängt. Anleitungen zum Auswählen der richtigen Einstellungen für bestimmte Ziel Umgebungen finden Sie unter [Konfigurieren von Bereitstellungs Eigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Zum Ausführen der Bereitstellung führt ein Benutzer die Datei *Publish. proj* mithilfe von MSBuild oder Team Build aus und gibt den Speicherort der relevanten Umgebungs spezifischen Projektdatei (*env-dev. proj* oder *env-Stage. proj*) als Befehlszeilenargument an. Die Datei *Publish. proj* importiert dann die Umgebungs spezifische Projektdatei, um einen kompletten Satz von Veröffentlichungs Anweisungen für jede Zielumgebung zu erstellen.

> [!NOTE]
> Die Funktionsweise dieser benutzerdefinierten Projektdateien ist unabhängig von dem Mechanismus, den Sie zum Aufrufen von MSBuild verwenden. Beispielsweise können Sie die MSBuild-Befehlszeile direkt verwenden, wie Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschrieben. Sie können die Projektdateien aus einer Befehlsdatei ausführen, wie unter [Erstellen und Ausführen einer Befehlsdatei für die Bereitstellung](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md)beschrieben. Alternativ können Sie die Projektdateien aus einer Builddefinition in TFS ausführen, wie unter [Erstellen einer Builddefinition beschrieben, die die Bereitstellung unterstützt](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> In jedem Fall ist das Endergebnis dasselbe&#x2014;: MSBuild führt die zusammengeführte Projektdatei aus und stellt die Lösung in der Zielumgebung bereit. Dadurch erhalten Sie viel Flexibilität bei der Auslösung Ihres Veröffentlichungsprozesses.

Nachdem er die benutzerdefinierten Projektdateien erstellt hat, fügt Matt diese einem Projektmappenordner hinzu und überprüft Sie in die Quell Code Verwaltung.

### <a name="create-build-definitions"></a>Builddefinitionen erstellen

Als abschließende Vorbereitungs Aufgabe arbeiten Matt und Rob zusammen, um drei Builddefinitionen für das neue Teamprojekt zu erstellen:

- **Deploydetest**. Dadurch wird die Contact Manager-Lösung erstellt und bei jedem Einchecken in der Testumgebung bereitgestellt.
- **Deploydestaging**. Dadurch werden Ressourcen aus einem angegebenen vorherigen Build in der Stagingumgebung bereitgestellt, wenn ein Entwickler den Build in die Warteschlange eingereiht.
- **Deploydestaging-WhatIf**. Dadurch wird eine "Was-wäre-wenn"-Bereitstellung in der Stagingumgebung durchführt, wenn ein Entwickler den Build in

In den folgenden Abschnitten werden die einzelnen Builddefinitionen ausführlicher erläutert.

## <a name="deployment-to-test"></a>Zu testende Bereitstellung

Das Entwicklungsteam von Fabrikam, Inc. verwaltet Testumgebungen, um eine Vielzahl von Softwaretest Aktivitäten auszuführen, wie z. b. Überprüfung und Validierung, benutzernutzungstests, Kompatibilitätstests und Ad-hoc-oder explorative Tests.

Das Entwicklungsteam hat eine Builddefinition in TFS namens **deploydetest**erstellt. Diese Builddefinition verwendet einen Continuous Integration-Triggerwert. Dies bedeutet, dass der Buildprozess jedes Mal ausgeführt wird, wenn ein Mitglied des Fabrikam, Inc.-Entwicklungsteams einen Eincheck Vorgang ausführt. Wenn ein Build ausgelöst wird, führt die Builddefinition Folgendes aus:

- Erstellen Sie die Projekt Mappe "ContactManager. sln". Dies wiederum erstellt jedes Projekt in der Projekt Mappe.
- Führen Sie alle Komponententests in der Projektmappenordnerstruktur aus (wenn die Projekt Mappe erfolgreich erstellt wurde).
- Führen Sie die benutzerdefinierten Projektdateien aus, die den Bereitstellungs Prozess steuern (wenn die Projekt Mappe erfolgreich erstellt wird und alle Komponententests übergibt).

Das Endergebnis ist, dass die Webpakete und alle anderen Bereitstellungs Ressourcen in der Testumgebung bereitgestellt werden, wenn die Projekt Mappe erfolgreich erstellt wird und Komponententests bestanden werden.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Wie funktioniert der Bereitstellungs Prozess?

Die Builddefinition **deploydetest** liefert diese Argumente für MSBuild:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]

Die Eigenschaften **deployonbuild = true** und **deploytarget = Package** werden verwendet, wenn Team Build die Projekte in der Projekt Mappe erstellt. Wenn das Projekt ein Webanwendungs Projekt ist, weisen diese Eigenschaften MSBuild an, ein Webbereitstellungs Paket für das Projekt zu erstellen. Mit der Eigenschaft " **targetenvpropsfile** " wird der Datei " *Publish. proj* " mitgeteilt, wo die zu importierende Umgebungs spezifische Projektdatei zu finden ist.

> [!NOTE]
> Eine ausführliche Exemplarische Vorgehensweise zum Erstellen einer Builddefinition wie dieser finden Sie [unter Erstellen einer Builddefinition, die die Bereitstellung unterstützt](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

Die Datei *Publish. proj* enthält Ziele, mit denen jedes Projekt in der Projekt Mappe erstellt wird. Sie enthält jedoch auch bedingte Logik, die diese Buildziele überspringt, wenn Sie die Datei in Team Build ausführen. Auf diese Weise können Sie die zusätzlichen Buildfunktionen von Team Build nutzen, wie z. b. die Möglichkeit zum Ausführen von Komponententests. Wenn der Projektmappenbuild oder die Komponententests fehlschlagen, wird die Datei *Publish. proj* nicht ausgeführt, und die Anwendung wird nicht bereitgestellt.

Die bedingte Logik wird durch Auswerten der **buildinginteambuild** -Eigenschaft erreicht. Dies ist eine MSBuild-Eigenschaft, die automatisch auf **true** festgelegt wird, wenn Sie Team Build zum Erstellen Ihrer Projekte verwenden.

## <a name="deployment-to-staging"></a>Bereitstellung für Staging

Wenn ein Build alle Anforderungen des Entwicklerteams in der Testumgebung erfüllt, möchte das Team möglicherweise denselben Build in einer Stagingumgebung bereitstellen. Stagingumgebungen werden in der Regel so konfiguriert, dass Sie den Merkmalen der Produktions-oder "Live"-Umgebung so genau wie möglich entsprechen, z. b. in Bezug auf Server Spezifikationen, Betriebssysteme, Software und Netzwerkkonfiguration. Stagingumgebungen werden häufig für Auslastungs Tests, Benutzer Akzeptanz Tests und umfassendere interne Überprüfungen verwendet. Builds werden direkt vom Buildserver in der Stagingumgebung bereitgestellt.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Die Builddefinitionen für die Bereitstellung der Lösung in der Stagingumgebung **deploydestaging-WhatIf** und **deploydestaging**haben diese Merkmale gemeinsam:

- Sie erstellen nichts. Wenn Rob die Lösung in der Stagingumgebung bereitstellt, möchte er einen bestimmten, vorhandenen Build bereitstellen, der bereits in der Testumgebung überprüft und validiert wurde. Die Builddefinitionen müssen lediglich die benutzerdefinierten Projektdateien ausführen, die den Bereitstellungs Prozess steuern.
- Wenn Rob einen Build auslöst, verwendet er die Buildparameter, um anzugeben, welcher Build die Ressourcen enthält, die er vom Buildserver bereitstellen möchte.
- Die Builddefinitionen werden nicht automatisch ausgelöst. Rob fügt einen Build manuell in die Warteschlange ein, wenn er die Lösung in der Stagingumgebung bereitstellen möchte.

Dies ist der allgemeine Prozess für eine Bereitstellung in der Stagingumgebung:

1. Der stagingumgebungs Administrator Rob Walters fügt einen Build mithilfe der Builddefinition **deploydestaging-WhatIf** in die Warteschlange ein. Rob verwendet die builddefinitionsparameter, um anzugeben, welcher Build bereitgestellt werden soll.
2. Die Builddefinition **deploydestaging-WhatIf** führt die benutzerdefinierten Projektdateien im "Was-wäre-wenn"-Modus aus. Dadurch werden Protokolldateien generiert, als hätte Rob eine Live Bereitstellung durchführen, aber keine Änderungen an der Zielumgebung.
3. Rob prüft die Protokolldateien, um die Auswirkungen der Bereitstellung in der Stagingumgebung zu ermitteln. Vor allem möchte Rob überprüfen, was hinzugefügt wird, was aktualisiert wird und was gelöscht wird.
4. Wenn Rob zufrieden ist, dass die Bereitstellung keine unerwünschten Änderungen an vorhandenen Ressourcen oder Daten vornimmt, fügt er einen Build mithilfe der Builddefinition **deploydestaging** in die Warteschlange ein.
5. Die Builddefinition **deploydestaging** führt die benutzerdefinierten Projektdateien aus. Diese veröffentlichen die Bereitstellungs Ressourcen auf dem primären Webserver in der Stagingumgebung.
6. Der WFF-Controller (Web Farm Framework) synchronisiert die Webserver in der Stagingumgebung. Dadurch ist die Anwendung auf allen Webservern in der Serverfarm verfügbar.

### <a name="how-does-the-deployment-process-work"></a>Wie funktioniert der Bereitstellungs Prozess?

Die Builddefinition **deploydestaging** stellt diese Argumente für MSBuild bereit:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]

Mit der Eigenschaft " **targetenvpropsfile** " wird der Datei " *Publish. proj* " mitgeteilt, wo die zu importierende Umgebungs spezifische Projektdatei zu finden ist. Die **outputroot** -Eigenschaft überschreibt den integrierten Wert und gibt den Speicherort des buildordners an, der die Ressourcen enthält, die Sie bereitstellen möchten. Wenn Rob den Build in die Warteschlange eingereiht, verwendet er die Registerkarte **Parameter** , um einen aktualisierten Wert für die **outputroot** -Eigenschaft anzugeben.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Weitere Informationen zum Erstellen einer Builddefinition wie dieser finden Sie unter Bereitstellen [eines bestimmten Builds](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).

Die Builddefinition **deploydestaging-WhatIf** enthält dieselbe Bereitstellungs Logik wie die **deploydestaging** -Builddefinition. Sie enthält jedoch das zusätzliche Argument **WhatIf = true**:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]

Die **WhatIf** -Eigenschaft in der Datei *Publish. proj* gibt an, dass alle Bereitstellungs Ressourcen im "Was-wäre-wenn"-Modus veröffentlicht werden sollen. Das heißt, dass Protokolldateien generiert werden, als ob die Bereitstellung fortgeführt worden wäre, aber in der Zielumgebung hat nichts geändert. Auf diese Weise können Sie die Auswirkung einer vorgeschlagenen bereit&#x2014;Stellung bewerten, was hinzugefügt wird, welche Updates aktualisiert werden und was gelöscht&#x2014;wird, bevor Sie tatsächlich Änderungen vornehmen.

> [!NOTE]
> Weitere Informationen zum Konfigurieren von "Was-wäre-wenn"-bereit Stellungen finden Sie unter [Ausführen einer "What if"-Bereitstellung](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).

Nachdem Sie die Anwendung auf dem primären Webserver in der Stagingumgebung bereitgestellt haben, wird die Anwendung automatisch von der WFF auf allen Servern in der Serverfarm synchronisiert.

> [!NOTE]
> Weitere Informationen zum Konfigurieren des WFF zum Synchronisieren von Webservern finden Sie unter [Erstellen einer Server Farm mit dem Webfarm-Framework](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).

## <a name="deployment-to-production"></a>Bereitstellung in der Produktion

Wenn ein Build in der Stagingumgebung genehmigt wurde, kann das Team von Fabrikam, Inc. die Anwendung in der Produktionsumgebung veröffentlichen. In der Produktionsumgebung geht die Anwendung "Live" und erreicht Ihre Zielgruppe von Endbenutzern.

Die Produktionsumgebung befindet sich in einem Umkreis Netzwerk mit Internet Zugriff. Dies ist vom internen Netzwerk isoliert, das den Buildserver enthält. Der Administrator der Produktionsumgebung, Lisa Andrews, muss die Webbereitstellungs Pakete manuell vom Buildserver kopieren und auf dem primären produktionsweb Server in IIS importieren.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Dies ist der allgemeine Prozess für eine Bereitstellung in der Produktionsumgebung:

1. Das Entwicklerteam rät Lisa, dass ein Build für die Bereitstellung in der Produktion bereit ist. Das Team rät Lisa vom Speicherort der Webbereitstellungs Pakete innerhalb des Ablage Ordners auf dem Buildserver ab.
2. Lisa sammelt die Webpakete vom Buildserver und kopiert Sie auf den primären Webserver in der Produktionsumgebung.
3. Lisa verwendet IIS-Manager, um die Webpakete auf dem primären Webserver zu importieren und zu veröffentlichen.
4. Der WFF-Controller synchronisiert die Webserver in der Produktionsumgebung. Dadurch ist die Anwendung auf allen Webservern in der Serverfarm verfügbar.

### <a name="how-does-the-deployment-process-work"></a>Wie funktioniert der Bereitstellungs Prozess?

IIS-Manager enthält einen Assistenten zum Importieren von Anwendungspaketen, mit dem Sie Webpakete auf einer IIS-Website leicht veröffentlichen können. Eine exemplarische Vorgehensweise zum Ausführen dieses Verfahrens finden Sie unter [Manuelles Installieren von Webpaketen](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Zusammenfassung

Dieses Thema bietet eine Abbildung des Bereitstellungs Lebenszyklus für eine typische Webanwendung im Unternehmensbereich.

Dieses Thema ist Teil einer Reihe von Tutorials, die Anleitungen zu verschiedenen Aspekten der Bereitstellung von Webanwendungen bereitstellen. In der Praxis gibt es in jeder Phase des Bereitstellungs Prozesses viele zusätzliche Aufgaben und Überlegungen, und es ist nicht möglich, Sie in einer einzigen exemplarischen Vorgehensweise zu behandeln. Weitere Informationen finden Sie in den folgenden Tutorials:

- [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Tutorial bietet eine umfassende Einführung in Webbereitstellungs Verfahren mithilfe von MSBuild und dem IIS-Webbereitstellungs Tool (Web deploy).
- [Konfigurieren von Server Umgebungen für die Webbereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Dieses Tutorial enthält Anleitungen zum Konfigurieren von Windows Server-Umgebungen zur Unterstützung verschiedener Bereitstellungs Szenarien.
- [Konfigurieren von Team Foundation Server für die automatisierte Webbereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Dieses Tutorial enthält Anleitungen zum Integrieren von Bereitstellungs Logik in TFS-Buildprozesse.
- [Erweiterte Unternehmensweb Bereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Dieses Tutorial enthält Anleitungen zum erfüllen einiger komplexer Bereitstellungs Herausforderungen, denen Organisationen ausgesetzt sind.

> [!div class="step-by-step"]
> [Previous](enterprise-web-deployment-scenario-overview.md)
