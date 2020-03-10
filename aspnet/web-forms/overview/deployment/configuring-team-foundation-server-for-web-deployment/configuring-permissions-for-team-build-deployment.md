---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Konfigurieren von Berechtigungen für die Team Build-Bereitstellung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie Berechtigungen konfigurieren, damit der Buildserver Inhalte für Webserver und Datenbankserver als Teil eines automatisierten b bereitstellen kann...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 5699f72af6b8d7f18d1a2c631dfdedd63c66e1e6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518505"
---
# <a name="configuring-permissions-for-team-build-deployment"></a>Konfigurieren von Berechtigungen für Team-Buildbereitstellung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie Berechtigungen konfigurieren, damit der Buildserver Inhalte für Webserver und Datenbankserver im Rahmen eines automatisierten Buildprozesses bereitstellen kann.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

Die Bereitstellungs Methode im Kern dieser Tutorials basiert auf dem Untergrund Legendes [zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)beschriebenen Ansatz, in dem der Buildprozess von zwei Projektdateien&#x2014;gesteuert wird, die Buildanweisungen enthalten, die für jede Zielumgebung gelten, und eine mit Umgebungs spezifischen Build-und Bereitstellungs Einstellungen. Zum Zeitpunkt der Erstellung wird die Umgebungs spezifische Projektdatei in der Umgebungs unabhängigen Projektdatei zusammengeführt, um einen kompletten Satz von Buildanweisungen zu bilden.

## <a name="task-overview"></a>Aufgaben Übersicht

Wenn Sie den Team Foundation Server-Builddienst (TFS) 2010 installieren, geben Sie die Identität an, mit der der Dienst ausgeführt werden soll. Standardmäßig ist dies das Netzwerkdienst Konto. Alternativ können Sie den Builddienst so konfigurieren, dass er mit einem Domänen Konto ausgeführt wird.

Alle Bereitstellungs Aufgaben, bei denen die Windows-Authentifizierung erforderlich ist, und die Sie mithilfe von Team Build automatisieren möchten, werden mithilfe der builddienstidentität ausgeführt. Daher müssen Sie der builddienstidentität alle erforderlichen Berechtigungen auf ihren Webservern und ihren Datenbankservern erteilen.

> [!NOTE]
> Das Netzwerkdienst Konto verwendet das Computer Konto, um sich bei anderen Computern zu authentifizieren. Computer Konten haben das Format * [Domänen Name]\[Computername] * **$** &#x2014;z. **b. fabrikam\tfsbuild $** . Wenn Ihr Builddienst mithilfe der Netzwerkdienst Identität ausgeführt wird, sollten Sie der Computer Konto Identität für den Buildserver alle erforderlichen Berechtigungen erteilen.

## <a name="configuring-web-server-permissions"></a>Konfigurieren von Webserver Berechtigungen

Wie unter [auswählen des richtigen Ansatzes für die Webbereitstellung](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)beschrieben, gibt es zwei Hauptansätze, die Sie verwenden können, wenn Sie Webpakete auf einem Remoteweb Server bereitstellen möchten:

- Stellen Sie die Anwendung von einem Remote Speicherort aus bereit, indem Sie auf dem Zielserver den *Web Deployment Agent-Dienst* (auch als Remote-Agent bezeichnet) Anzielen.
- Stellen Sie die Anwendung von einem Remote Speicherort aus bereit, indem Sie auf den *Internetinformationsdienste* (*IIS) Web deploy Handler* auf dem Zielserver abzielen.

Der Remote-Agent hat in diesem Fall zwei wichtige Einschränkungen:

- Der Remote-Agent unterstützt nur die NTLM-Authentifizierung. Anders ausgedrückt: die Bereitstellung muss die builddienstidentität&#x2014;verwenden, Sie können die Identität eines anderen Kontos nicht annehmen.
- Um den Remote-Agent verwenden zu können, muss das Konto, mit dem die Bereitstellung erfolgt, ein Administrator auf dem Zielserver sein.

Durch diese beiden Einschränkungen wird der Remote-Agent-Ansatz für eine automatisierte TeamBuild-Bereitstellung unerwünscht. Um diese Vorgehensweise verwenden zu können, müssen Sie das Builddienstkonto als Administrator auf jedem Zielweb Server einrichten.

Im Gegensatz dazu bietet der Web deploy handleransatz verschiedene Vorteile:

- Der Web deploy Handler unterstützt die Standard Authentifizierung über HTTPS, sodass Sie die Anmelde Informationen eines alternativen Kontos an das IIS-Webbereitstellungs Tool (Web deploy) übergeben können.
- Sie können Zielweb Server so konfigurieren, dass Benutzer, die nicht Administratoren sind, Inhalte für bestimmte IIS-Websites mithilfe des Web deploy-Handlers bereitstellen können.

Folglich empfiehlt es sich, den Web deploy-Handler als Ziel zu bevorzugen, wenn Sie die Webpaket Bereitstellung über Team Build automatisieren. Dies ist der empfohlene Prozess:

1. Erstellen Sie ein Domänen Konto mit geringen Berechtigungen, das für die Bereitstellung verwendet werden soll.
2. Konfigurieren Sie den Web deploy Handler, und erteilen Sie dem Konto die erforderlichen Berechtigungen zum Bereitstellen von Inhalten für eine bestimmte IIS-Website, wie unter [Konfigurieren eines Webservers für die Web deploy Veröffentlichung (Web deploy Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)beschrieben.
3. Rufen Sie Web deploy auf, und verwenden Sie für die Bereitstellung die Anmelde Informationen des Domänen Kontos, das Sie erstellt haben, um die Web deploy Handler als Ziel zu verwenden.

In der Beispiellösung [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) geben Sie den Authentifizierungstyp (Basic oder NTLM), die Web deploy Anmelde Informationen und die Endpunkt Adresse (Remote-Agent oder Web deploy Handler) in der Umgebungs spezifischen Projektdatei an. Diese Werte werden verwendet, um einen Web deploy Befehl zu formulieren und auszuführen, wenn die Projektdatei ausgeführt wird. Weitere Informationen finden Sie unter Bereitstellen von [Webpaketen](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Weitere Informationen zum Konfigurieren des Web deploy Handlers, einschließlich der Vorgehensweise zum Konfigurieren von Berechtigungen, finden Sie unter [Konfigurieren eines Webservers für die Web deploy Veröffentlichung (Web deploy Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Weitere Informationen zum Konfigurieren des Remote-Agents finden Sie unter [Konfigurieren eines Webservers für die Web deploy Veröffentlichung (Remote-Agent)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Konfigurieren von Berechtigungen für Daten Bank Server

Zum Bereitstellen einer Datenbank für SQL Server müssen Sie folgende Schritte ausführen:

- Erstellen Sie einen Anmelde Namen für das Bereitstellungs Konto auf der SQL Server Instanz.
- Erteilen Sie dem Anmelde Namen **dbcreator** -Berechtigungen für die SQL Server Instanz.
- Fügen Sie den Anmelde Namen nach der erstmaligen Bereitstellung der Datenbank- **\_Besitzer** -Rolle in der Zieldatenbank hinzu. Dies ist erforderlich, da Sie bei nachfolgenden bereit Stellungen eine vorhandene Datenbank ändern, anstatt eine neue Datenbank zu erstellen.

Sie können sich bei einer SQL Server Instanz mithilfe der NTLM-Authentifizierung oder SQL Server Authentifizierung authentifizieren:

- Wenn Sie die NTLM-Authentifizierung verwenden, müssen Sie die oben beschriebenen Berechtigungen dem Builddienstkonto erteilen.
- Wenn Sie SQL Server-Authentifizierung verwenden, müssen Sie die oben beschriebenen Berechtigungen für das SQL Server Konto erteilen. Außerdem müssen Sie den SQL Server Benutzernamen und das Kennwort in die Verbindungs Zeichenfolge einschließen, die Sie zum Bereitstellen der Datenbank verwenden.

Schritt-für-Schritt-Anleitungen zum Konfigurieren von Berechtigungen für die Daten Bank Bereitstellung finden Sie unter [Konfigurieren eines Datenbankservers für die Web deploy Veröffentlichung](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Zusammenfassung

An diesem Punkt sollten Sie die erforderlichen Berechtigungen sowie die für Sie geöffneten Authentifizierungs Optionen kennen, wenn Sie Webanwendungen und Daten Bank Bereitstellungen über Team Build automatisieren. Außerdem sollten Sie in der Lage sein, die erforderlichen Berechtigungen für IIS-Webserver und SQL Server-Datenbankserver zu implementieren.

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zum Konfigurieren von Windows Server-Umgebungen zur Unterstützung der Remote Bereitstellung finden Sie unter [Konfigurieren von Serverumgebungen für die Webbereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Previous](deploying-a-specific-build.md)
