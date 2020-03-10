---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Konfigurieren eines TFS-Buildservers für die Webbereitstellung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie ein Team Foundation Server-Buildserver (TFS) zum Erstellen und Bereitstellen Ihrer Lösungen mithilfe von TeamBuild und Internet Information (Internet Information) vorbereitet wird...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: b3aaf7234706d149a3c784347528923f662c3511
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78512223"
---
# <a name="configuring-a-tfs-build-server-for-web-deployment"></a>Konfigurieren eines TFS-Buildservers für die Webbereitstellung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie ein Team Foundation Server-Buildserver (TFS) zum Erstellen und Bereitstellen Ihrer Lösungen mithilfe von TeamBuild und Internetinformationsdienste (IIS)-Webbereitstellungs Tools (Web deploy) vorbereitet wird.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Kern dieser Tutorials basiert auf dem Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz, in dem der Buildprozess von zwei Projektdateien&#x2014;gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="task-overview"></a>Aufgaben Übersicht

Um einen Buildserver für die Erstellung und Bereitstellung Ihrer Lösungen vorzubereiten, müssen Sie folgende Schritte ausführen:

- Installieren und konfigurieren Sie den TFS-Builddienst.
- Installieren Sie Visual Studio 2010.
- Installieren Sie alle Produkte oder Komponenten, die für die Erstellung der Lösung erforderlich sind, wie z. b. Versionen der .NET Framework oder ASP.NET MVC.
- Installieren Sie Web deploy 2,0 oder höher.

In diesem Thema wird gezeigt, wie Sie diese Prozeduren ausführen oder auf andere Ressourcen verweisen, in denen Sie vorhanden sind. In den Aufgaben und exemplarischen Vorgehensweisen in diesem Thema wird Folgendes vorausgesetzt:

- Sie beginnen mit einem sauberen Server Build, auf dem Windows Server 2008 R2 Service Pack 1 ausgeführt wird.
- Der Server ist mit einer statischen IP-Adresse verbunden.
- Sie haben die TFS-Anwendungsebene auf einem separaten Server installiert, wie unter [Unternehmensweb Bereitstellung: Szenarioübersicht](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)beschrieben.

### <a name="who-performs-these-procedures"></a>Wer führt diese Prozeduren aus?

In den meisten Fällen ist ein TFS-Administrator für die Konfiguration von Buildservern verantwortlich. In einigen Fällen übernimmt das Entwicklerteam möglicherweise den Besitz bestimmter Buildserver.

## <a name="install-and-configure-the-tfs-build-service"></a>Installieren und Konfigurieren des TFS-builddienstanbieter

Wenn Sie einen Buildserver konfigurieren, besteht die erste Aufgabe darin, den TFS-Builddienst zu installieren und zu konfigurieren. Im Rahmen dieses Prozesses müssen Sie folgende Schritte ausführen:

- Installieren Sie den TFS-Builddienst, und konfigurieren Sie ein Dienst Konto. Alle BuildTasks, einschließlich der Bereitstellung, werden mit der Identität des Builddienstkontos ausgeführt.
- Erstellen Sie einen *BuildController* und einen oder mehrere *Build-Agents*. Jeder BuildController verwaltet einen Satz von Build-Agents. Wenn Sie einen Build in die Warteschlange stellen, weist der BuildController den Build-Task einem verfügbaren Build-Agent zu. Jede Teamprojekt Sammlung in TFS ist einem einzelnen BuildController zugeordnet.
- Konfigurieren Sie einen Ablage Ordner für die Buildausgaben. Dies ist eine Netzwerkfreigabe. Alle Buildausgaben, wie z. b. Webbereitstellungs Pakete, werden an den Ablage Ordner gesendet.

Das Kapitel [Verwalten von Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) auf MSDN enthält alle Ressourcen, die Sie benötigen, um die folgenden Aufgaben auszuführen:

- Eine konzeptionelle Übersicht über Team Foundation Build, einschließlich Builddienst, BuildController und Build-Agents, finden Sie Untergrund Legendes zu [einem Team Foundation-Buildsystem](https://msdn.microsoft.com/library/dd793166.aspx).
- Weitere Informationen zum Installieren und Konfigurieren des builddienstanbieter finden Sie unter [Konfigurieren eines](https://msdn.microsoft.com/library/ms181712.aspx)Buildcomputers.
- Weitere Informationen zum Erstellen von buildcontrollern finden Sie unter [Erstellen und arbeiten mit einem BuildController](https://msdn.microsoft.com/library/ee330987.aspx).
- Weitere Informationen zum Erstellen von Build-Agents finden Sie unter [Erstellen und arbeiten mit Build-Agents](https://msdn.microsoft.com/library/bb399135.aspx).
- Weitere Informationen zum Erstellen und Konfigurieren von Ablage Ordnern finden [Sie unter Einrichten](https://msdn.microsoft.com/library/bb778394.aspx)von Ablage Ordnern.

## <a name="install-required-products-and-components"></a>Erforderliche Produkte und Komponenten installieren

Damit der Buildserver ihre Lösungen erstellen kann, müssen Sie alle Produkte, Komponenten oder Assemblys installieren, die für die Lösung erforderlich sind. Vor der Installation von webplatt Form Komponenten sollten Sie Visual Studio 2010 (beliebige Version) auf dem Buildserver installieren. Dadurch wird sichergestellt, dass die Zieldateien der Core-Microsoft-Build-Engine (MSBuild) und die WPP-Zieldateien (Web Publishing Pipeline) für den Builddienst verfügbar sind. Der Visual Studio-Installer sollte auch Web deploy installieren, den Sie benötigen, wenn Sie die Bereitstellung von Webpaketen als Teil des Buildprozesses planen.

Die beste Möglichkeit, gängige Webplattform-Komponenten zu installieren, ist die Verwendung des [Webplattform-Installers](https://go.microsoft.com/?linkid=9805118). Dadurch wird sichergestellt, dass Sie die neueste Version der einzelnen Produkte installieren, und außerdem werden alle Voraussetzungen für die einzelnen Produkte automatisch erkannt und installiert. Im Fall der [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) -Lösung sollten Sie den Webplattform-Installer verwenden, um diese Produkte und Komponenten zu installieren:

- **.NET Framework 4,0**. Dies ist erforderlich, um Anwendungen auszuführen, die auf dieser Version des .NET Framework erstellt wurden.
- **Webbereitstellungs Tool 2,1 oder**höher. Dadurch wird Web deploy (und die zugrunde liegende ausführbare Datei msbereitstellung. exe) auf dem Server installiert. Im Rahmen dieses Prozesses wird der Web Deployment Agent-Dienst installiert und gestartet. Mit diesem Dienst können Sie Webpakete von einem Remote Computer aus bereitstellen.
- **ASP.NET MVC 3**. Dadurch werden die Assemblys installiert, die Sie für die ASP.NET MVC 3-Anwendungen benötigen.

**So installieren Sie die erforderlichen Produkte und Komponenten**

1. Installieren Sie Visual Studio 2010. Wenn Sie zur Auswahl der zu installier Zeitpunkt aufgefordert werden, sollten Sie Folgendes einschließen:

    1. Alle Programmiersprachen, die Sie kompilieren müssen.
    2. Visual Web Developer. Dadurch wird sichergestellt, dass die WPP-Ziele dem Buildserver hinzugefügt werden.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Wenn die Installation von Visual Studio 2010 fertig ist, laden Sie [Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) herunter, und installieren Sie es (sofern es noch nicht in den Installationsmedien enthalten ist).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 löst einen Fehler auf, der verhindert, dass MSBuild die ausführbare Datei "msbereitstellung" findet.
3. Herunterladen und Starten des [Webplattform-Installers](https://go.microsoft.com/?linkid=9805118).
4. Klicken Sie oben im Fenster **Webplattform-Installer 3,0** auf **Produkte**.
5. Klicken Sie im Navigationsbereich auf der linken Seite des Fensters auf **Frameworks**.
6. Wenn die .NET Framework in der Zeile **Microsoft .NET Framework 4** nicht bereits installiert ist, klicken Sie auf **Hinzufügen**.

    > [!NOTE]
    > Möglicherweise haben Sie den .NET Framework 4,0 bereits über Windows Update installiert. Wenn ein Produkt oder eine Komponente bereits installiert ist, zeigt der Webplattform-Installer dies an, indem die Schaltfläche " **Hinzufügen** " durch den **installierten**Text ersetzt wird.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. Klicken Sie in der Zeile **ASP.NET MVC 3 (Visual Studio 2010)** auf **Hinzufügen**.
8. Klicken Sie im Navigationsbereich auf **Server**.
9. Klicken Sie in der Zeile **Webbereitstellungs Tool 2,1** auf **Hinzufügen**.
10. Klicken Sie auf **Installieren**. Der Webplattform-Installer zeigt eine Liste der Produkte&#x2014;zusammen mit allen zugehörigen Abhängigkeiten&#x2014;an, die installiert werden müssen, und fordert Sie auf, die Lizenzbedingungen zu akzeptieren.
11. Lesen Sie die Lizenzbedingungen, und klicken Sie auf **akzeptieren**, wenn Sie den Bedingungen zustimmen.
12. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen, und schließen Sie dann das Fenster **Webplattform-Installer 3,0** .

> [!NOTE]
> Wenn der Bereitstellungs Prozess die Verwendung von Tools wie "VSDBCMD. exe" oder "sqlcmd. exe" umfasst, müssen Sie sicherstellen, dass diese auf dem Buildserver installiert sind. "VSDBCMD. exe" ist ein Visual Studio-Tool, das in der Regel dem Server beim Installieren von Team Foundation Build hinzugefügt wird. Sqlcmd. exe ist ein SQL Server Tool. Sie können eine eigenständige Version von sqlcmd. exe von der Seite [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) herunterladen.

## <a name="conclusion"></a>Zusammenfassung

An diesem Punkt ist der Buildserver bereit, mit dem Erstellen und Bereitstellen von Webanwendungs Projekten zu beginnen. Im nächsten Thema [Erstellen einer Builddefinition, die die Bereitstellung unterstützt](creating-a-build-definition-that-supports-deployment.md), wird beschrieben, wie Sie eine Builddefinition erstellen und konfigurieren, um zu steuern, wann und wie Ihre Projekte erstellt und bereitgestellt werden.

## <a name="further-reading"></a>Weitere nützliche Informationen

Allgemeine Anleitungen zum Arbeiten mit Team Build finden Sie unter [Verwalten von Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Zurück](adding-content-to-source-control.md)
> [Weiter](creating-a-build-definition-that-supports-deployment.md)
