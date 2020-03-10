---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Anpassen von Daten Bank Bereitstellungen für mehrere Umgebungen | Microsoft-Dokumentation
author: jrjlee
description: 'In diesem Thema wird beschrieben, wie die Eigenschaften einer Datenbank im Rahmen des Bereitstellungs Prozesses an bestimmte Ziel Umgebungen angepasst werden. Hinweis: in diesem Thema wird davon ausgegangen, dass...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 8ae8cb1a322afb95c5d2e8d5e73c7825c7b2fe5a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78489033"
---
# <a name="customizing-database-deployments-for-multiple-environments"></a>Anpassen von Datenbankbereitstellungen für mehrere Umgebungen

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie die Eigenschaften einer Datenbank im Rahmen des Bereitstellungs Prozesses an bestimmte Ziel Umgebungen angepasst werden.
> 
> > [!NOTE]
> > In diesem Thema wird davon ausgegangen, dass Sie ein Visual Studio 2010-Datenbankprojekt mithilfe von MSBuild. exe und VSDBCMD. exe bereitstellen. Weitere Informationen dazu, warum Sie diesen Ansatz auswählen können, finden Sie unter [Webbereitstellung im Unternehmen und bereit](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) stellen von [Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Wenn Sie ein Datenbankprojekt für mehrere Ziele bereitstellen, möchten Sie häufig die Eigenschaften der Daten Bank Bereitstellung für jede Zielumgebung anpassen. Beispielsweise würden Sie in Testumgebungen in der Regel die Datenbank bei jeder Bereitstellung neu erstellen, während Sie in der Stagingumgebung oder in der Produktionsumgebung viel wahrscheinlicher sind, inkrementelle Updates zu erstellen, um Ihre Daten beizubehalten.
> 
> In einem Visual Studio 2010-Datenbankprojekt sind Bereitstellungs Einstellungen in einer Bereitstellungs Konfigurationsdatei (. sqldeployment) enthalten. In diesem Thema erfahren Sie, wie Sie Umgebungs spezifische Konfigurationsdateien für die Bereitstellung erstellen und angeben, welche Dateien Sie als VSDBCmd-Parameter verwenden möchten.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Kern dieser Tutorials basiert auf dem Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz, in dem der Buildprozess von zwei Projektdateien&#x2014;gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="task-overview"></a>Aufgaben Übersicht

In diesem Thema wird Folgendes vorausgesetzt:

- Sie verwenden den Ansatz zum Aufteilen von Projektdateien für die Lösungs Bereitstellung, wie in Grundlegendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschrieben.
- Sie können VSDBCmd aus der Projektdatei abrufen, um das Datenbankprojekt bereitzustellen, wie Untergrund Legendes [zum Buildprozess](../web-deployment-in-the-enterprise/understanding-the-build-process.md)beschrieben.

Zum Erstellen eines Bereitstellungs Systems, das unterschiedliche Daten Bank Bereitstellungs Eigenschaften Zwischenziel Umgebungen unterstützt, müssen Sie folgende Schritte ausführen:

- Erstellen Sie eine Bereitstellungs Konfigurationsdatei (. sqldeployment) für jede Zielumgebung.
- Erstellen Sie einen VSDBCmd-Befehl, der die Bereitstellungs Konfigurationsdatei als Befehls Zeilenschalter angibt.
- Parametrisieren Sie den VSDBCmd-Befehl in einer Microsoft-Build-Engine-Projektdatei (MSBuild), damit die VSDBCmd-Optionen für die Zielumgebung geeignet sind.

In diesem Thema wird gezeigt, wie Sie die einzelnen Prozeduren ausführen.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Erstellen von Umgebungs spezifischen Bereitstellungs Konfigurationsdateien

Standardmäßig enthält ein Datenbankprojekt eine einzige Bereitstellungs Konfigurationsdatei mit dem Namen *Database. sqldeployment*. Wenn Sie diese Datei in Visual Studio 2010 öffnen, werden Ihnen die verschiedenen Bereitstellungs Optionen angezeigt, die Ihnen zur Verfügung stehen:

- **Sortierung des Bereitstellungs Vergleichs**. Auf diese Weise können Sie auswählen, ob Sie die Daten Bank Sortierung Ihres Projekts (die *Quell* Sortierung) oder die Daten Bank Sortierung des Zielservers (die *Ziel* Sortierung) verwenden möchten. In den meisten Fällen sollten Sie die Quell Sortierung verwenden, wenn Sie die Bereitstellung in einer Entwicklungs-oder Testumgebung durchführen. Wenn Sie in einer Staging-oder Produktionsumgebung bereitstellen, sollten Sie die Ziel Sortierung in der Regel unverändert lassen, um Interoperabilitätsprobleme zu vermeiden.
- **Daten Bank Eigenschaften**bereitstellen. Auf diese Weise können Sie auswählen, ob die Daten Bank Eigenschaften wie in der Datei " *Database. sqlsettings* " definiert angewendet werden sollen. Wenn Sie eine Datenbank zum ersten Mal bereitstellen, sollten Sie die Daten Bank Eigenschaften bereitstellen. Wenn Sie eine vorhandene Datenbank aktualisieren, sollten die Eigenschaften bereits vorhanden sein, und Sie müssen Sie nicht erneut bereitstellen.
- **Erstellen Sie die Datenbank immer neu**. Auf diese Weise können Sie auswählen, ob die Zieldatenbank jedes Mal neu erstellt werden soll, wenn Sie eine Bereitstellung vornehmen oder inkrementelle Änderungen vornehmen, um die Zieldatenbank mit dem Schema auf dem neuesten Stand Wenn Sie die Datenbank neu erstellen, gehen alle Daten in der vorhandenen Datenbank verloren. Daher sollten Sie dies in der Regel auf **false** festlegen, wenn Sie bereit Stellungen für Staging-oder Produktionsumgebungen ausführen.
- **Inkrementelle Bereitstellung blockieren, wenn Datenverlust auftreten kann**. Auf diese Weise können Sie auswählen, ob die Bereitstellung beendet werden soll, wenn eine Änderung am Datenbankschema den Datenverlust verursacht. In der Regel legen Sie diese Einstellung für eine Bereitstellung in einer Produktionsumgebung auf " **true** " fest, um Ihnen die Möglichkeit zu geben, wichtige Daten zu schützen und zu schützen. Wenn Sie **Datenbank immer neu erstellen** auf **false**festgelegt haben, hat diese Einstellung keine Auswirkungen.
- **Führen Sie die Bereitstellung im Einzelbenutzermodus aus**. Dies ist in der Regel kein Problem in Entwicklungs-oder Testumgebungen. Sie sollten dies jedoch in der Regel für bereit Stellungen in Staging-oder Produktionsumgebungen auf " **true** " festlegen. Dadurch wird verhindert, dass Benutzer Änderungen an der Datenbank vornehmen, während die Bereitstellung ausgeführt wird.
- **Sichern der Datenbank vor der Bereitstellung**. In der Regel legen Sie diese Einstellung auf " **true** " fest, wenn Sie in einer Produktionsumgebung bereitstellen, als Vorsichtsmaßnahme gegen Datenverluste. Wenn Sie in einer Stagingumgebung bereitstellen, können Sie Sie auch auf " **true** " festlegen, wenn die Stagingdatenbank viele Daten enthält.
- **Generieren Sie DROP-Anweisungen für Objekte, die sich in der Zieldatenbank befinden, aber nicht im Datenbankprojekt sind**. In den meisten Fällen ist dies ein integraler Bestandteil von inkrementellen Änderungen an einer Datenbank. Wenn Sie **Datenbank immer neu erstellen** auf **false**festgelegt haben, hat diese Einstellung keine Auswirkungen.
- **Verwenden Sie Alter Assembly-Anweisungen nicht zum Aktualisieren von CLR-Typen**. Diese Einstellung bestimmt, wie SQL Server Common Language Runtime (CLR)-Typen auf neuere Assemblyversionen aktualisieren soll. Dies sollte in den meisten Szenarien auf **false** festgelegt werden.

Diese Tabelle zeigt typische Bereitstellungs Einstellungen für unterschiedliche Ziel Umgebungen. Ihre Einstellungen können jedoch je nach den genauen Anforderungen abweichen.

|  | Entwickler/Test | Staging/Integration | Produktion |
| --- | --- | --- | --- |
| **Sortierung des Bereitstellungs Vergleichs** | `Source` | Ziel | Ziel |
| **Daten Bank Eigenschaften bereitstellen** | True | Nur das erste Mal | Nur das erste Mal |
| **Datenbank immer neu erstellen** | True | False | False |
| **Inkrementelle Bereitstellung blockieren, wenn Datenverluste auftreten** | False | Vielleicht | True |
| **Ausführen des Bereitstellungs Skripts im Einzelbenutzermodus** | False | True | True |
| **Sichern der Datenbank vor der Bereitstellung** | False | Vielleicht | True |
| **DROP-Anweisungen für Objekte generieren, die sich in der Zieldatenbank, aber nicht im Datenbankprojekt befinden** | False | True | True |
| **Verwenden Sie Alter Assembly-Anweisungen nicht zum Aktualisieren von CLR-Typen.** | False | False | False |

> [!NOTE]
> Weitere Informationen zu Daten Bank Bereitstellungs Eigenschaften und Überlegungen zur Umgebung finden Sie unter [Übersicht über Datenbankprojekt Einstellungen](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), Gewusst [wie: Konfigurieren von Eigenschaften für Bereitstellungs Details](https://msdn.microsoft.com/library/dd172125.aspx), [Erstellen und Bereitstellen einer Datenbank in einer isolierten Entwicklungsumgebung](https://msdn.microsoft.com/library/dd193409.aspx)und erstellen und Bereitstellen von [Datenbanken in einer Staging-oder Produktionsumgebung](https://msdn.microsoft.com/library/dd193413.aspx).

Um die Bereitstellung eines Datenbankprojekts in mehreren Zielen zu unterstützen, sollten Sie eine Bereitstellungs Konfigurationsdatei für jede Zielumgebung erstellen.

**So erstellen Sie eine Umgebungs spezifische Konfigurationsdatei**

1. Klicken Sie in Visual Studio 2010 im Fenster **Projektmappen-Explorer** mit der rechten Maustaste auf das Datenbankprojekt, und klicken Sie dann auf **Eigenschaften**.
2. Klicken Sie auf der Seite Datenbankprojekteigenschaften auf **der Registerkarte** bereitstellen in der Zeile **Bereitstellungs Konfigurationsdatei** auf **neu**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. Geben Sie im Dialogfeld **Neue Bereitstellungs Konfigurationsdatei** einen aussagekräftigen Namen (z. b **. Testenvironment. sqldeployment**) ein, und klicken Sie dann auf **Speichern**.
4. Legen Sie auf der Seite *[fileName] * * *. sqldeployment** die Bereitstellungs Eigenschaften entsprechend den Anforderungen Ihrer Zielumgebung fest, und speichern Sie die Datei.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Beachten Sie, dass die neue Datei dem Ordner "Properties" in Ihrem Datenbankprojekt hinzugefügt wird.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Angeben der Bereitstellungs Konfigurationsdatei in VSDBCmd

Wenn Sie Projektmappenkonfigurationen (z. b. Debug und Release) in Visual Studio 2010 verwenden, können Sie eine Bereitstellungs Konfigurationsdatei jeder Konfiguration zuordnen. Beim Erstellen einer bestimmten Konfiguration wird vom Buildprozess eine Konfigurations spezifische Bereitstellungs Manifest-Datei generiert, die auf die Konfigurations spezifische Bereitstellungs Konfigurationsdatei verweist. Eines der Hauptziele des Ansatzes bei der Bereitstellung, die in diesen Tutorials beschrieben wird, besteht darin, Benutzern die Möglichkeit zu bieten, den Bereitstellungs Prozess ohne Verwendung von Visual Studio 2010 und Projektmappenkonfigurationen zu steuern. Bei diesem Ansatz ist die Projektmappenkonfiguration unabhängig von der Ziel Bereitstellungs Umgebung identisch. Zum Anpassen der Daten Bank Bereitstellung an eine bestimmte Zielumgebung können Sie die VSDBCmd-Befehlszeilenoptionen verwenden, um die Bereitstellungs Konfigurationsdatei anzugeben.

Zum Angeben einer Bereitstellungs Konfigurationsdatei in der VSDBCmd verwenden Sie den Schalter **p:/deploymentconfigurationfile** , und geben Sie den vollständigen Pfad zu Ihrer Datei an. Dadurch wird die Bereitstellungs Konfigurationsdatei außer Kraft gesetzt, die das Bereitstellungs Manifest identifiziert. Beispielsweise können Sie diesen VSDBCmd-Befehl verwenden, um die **ContactManager** -Datenbank in einer Testumgebung bereitzustellen:

[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]

> [!NOTE]
> Beachten Sie, dass der Buildprozess die sqldeployment-Datei umbenennen kann, wenn die Datei in das Ausgabeverzeichnis kopiert wird.

Wenn Sie SQL-Befehls Variablen in den SQL-Skripts vor oder nach der Bereitstellung verwenden, können Sie einen ähnlichen Ansatz verwenden, um eine Umgebungs spezifische sqlcmdvars-Datei zu Ihrer Bereitstellung zuzuordnen. In diesem Fall verwenden Sie den Schalter **p:/sqlcommandvariablesfile** , um Ihre. sqlcmdvars-Datei zu identifizieren.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Ausführen des VSDBCmd-Befehls aus einer MSBuild-Projektdatei

Sie können einen VSDBCmd-Befehl aus einer MSBuild-Projektdatei aufrufen, indem Sie eine **exec** -Aufgabe innerhalb eines MSBuild-Ziels verwenden. In seiner einfachsten Form sieht dies wie folgt aus:

[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]

- Wenn Sie die Projektdateien in der Praxis leicht lesen und wieder verwenden möchten, sollten Sie Eigenschaften erstellen, um die verschiedenen Befehlszeilenparameter zu speichern. Dies erleichtert Benutzern das Bereitstellen von Eigenschafts Werten in einer Umgebungs spezifischen Projektdatei oder das Überschreiben von Standardwerten über die MSBuild-Befehlszeile. Wenn Sie den in Grundlegendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz zum Aufteilen von Projektdateien verwenden, sollten Sie die Buildanweisungen und-Eigenschaften zwischen den beiden Dateien entsprechend aufteilen:
- Umgebungs spezifische Einstellungen, wie z. b. der Dateiname der Bereitstellungs Konfiguration, die Daten bankverbindungs Zeichenfolge und der Name der Zieldatenbank, sollten in der Umgebungs spezifischen Projektdatei gespeichert werden.
- Das MSBuild-Ziel, das den VSDBCmd-Befehl ausführt, und alle universellen Eigenschaften wie der Speicherort der ausführbaren VSDBCmd-Datei sollten in die universelle Projektdatei gelangen.

Sie sollten auch sicherstellen, dass Sie das Datenbankprojekt erstellen, bevor Sie VSDBCmd aufrufen, damit die DeployManifest-Datei erstellt wurde und einsatzbereit ist. Ein vollständiges Beispiel für diesen Ansatz finden Sie im Thema "Grundlegendes [zum Buildprozess](../web-deployment-in-the-enterprise/understanding-the-build-process.md)", der Sie durch die Projektdateien in der [Beispiellösung "Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)" führt.

## <a name="conclusion"></a>Zusammenfassung

In diesem Thema wird beschrieben, wie Sie Daten Bank Eigenschaften auf verschiedene Ziel Umgebungen anpassen können, wenn Sie Datenbankprojekte mithilfe von MSBuild und VSDBCmd bereitstellen. Diese Vorgehensweise ist nützlich, wenn Sie Datenbankprojekte als Teil größerer Lösungen für Unternehmen bereitstellen müssen. Diese Lösungen werden häufig für mehrere Ziele bereitgestellt, wie z. b. Sandbox-oder Testumgebungen, Staging-oder Integrationsplattformen und Produktions-oder Live Umgebungen. Jede dieser Ziel Umgebungen erfordert in der Regel einen eindeutigen Satz von Eigenschaften für die Daten Bank Bereitstellung.

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zum Bereitstellen von Datenbankprojekten mit VSDBCMD. exe finden Sie unter Bereitstellen von [Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md). Weitere Informationen zur Verwendung von benutzerdefinierten MSBuild-Projektdateien zum Steuern des Bereitstellungs Prozesses finden Sie Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und Grundlegendes [zum Buildprozess](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

In den folgenden Artikeln auf MSDN finden Sie allgemeinere Hinweise zur Daten Bank Bereitstellung:

- [Übersicht über Datenbankprojekt Einstellungen](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Vorgehensweise: Konfigurieren von Eigenschaften für Bereitstellungs Details](https://msdn.microsoft.com/library/dd172125.aspx)
- [Erstellen und Bereitstellen von Datenbanken in einer isolierten Entwicklungsumgebung](https://msdn.microsoft.com/library/dd193409.aspx)
- [Erstellen und Bereitstellen von Datenbanken in einer Staging-oder Produktionsumgebung](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Zurück](performing-a-what-if-deployment.md)
> [Weiter](deploying-database-role-memberships-to-test-environments.md)
