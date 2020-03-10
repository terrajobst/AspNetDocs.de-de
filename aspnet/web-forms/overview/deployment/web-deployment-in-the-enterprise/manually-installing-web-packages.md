---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Manuelles Installieren von Webpaketen | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie ein Webbereitstellungs Paket manuell in Internetinformationsdienste (IIS) importiert wird. Thema zum entwickeln und Verpacken von Webanwendungen...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: f778549d3e26989a2e71ef21171adec521842729
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78514887"
---
# <a name="manually-installing-web-packages"></a>Manuelles Installieren von Webpaketen

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie ein Webbereitstellungs Paket manuell in Internetinformationsdienste (IIS) importiert wird.
> 
> Im Thema [Erstellen und Verpacken von Webanwendungs Projekten](building-and-packaging-web-application-projects.md) wird beschrieben, wie das IIS-Webbereitstellungs Tool (Web deploy) in Verbindung mit dem Microsoft-Build-Engine (MSBuild) und der Web Publishing Pipeline (WPP) ihre Webanwendungs Projekte in eine einzelne ZIP-Datei packen kann. Diese Datei, die im Allgemeinen als Webbereitstellungs Paket (oder einfach als Bereitstellungs Paket) bezeichnet wird, enthält alle Inhalte und Konfigurationsinformationen, die IIS benötigt, um die Webanwendung auf einem Webserver neu zu erstellen.
> 
> Nachdem Sie ein Webbereitstellungs Paket erstellt haben, können Sie es auf verschiedene Weise auf einem IIS-Server veröffentlichen. In vielen Szenarien sollten Sie die Integrationspunkte zwischen MSBuild, WPP und Web deploy nutzen, um Webpakete im Rahmen eines automatisierten oder einstufigen Build-und Bereitstellungs Prozesses Remote zu erstellen und zu installieren. Dieser Vorgang wird unter Bereitstellen von [Webpaketen](deploying-web-packages.md)beschrieben. Dies ist jedoch nicht immer möglich. Angenommen, Sie möchten eine Webanwendung in einer Produktionsumgebung mit Internet Zugriff bereitstellen. Aus Sicherheitsgründen ist eine solche Produktionsumgebung mit der geringsten Wahrscheinlichkeit hinter einer Firewall in einem Subnetz, das vom Buildserver getrennt ist, in einem Umkreis Netzwerk (auch bekannt als DMZ, abmilitarisierte Zone und überwachtes Subnetz). In vielen Fällen wird die Produktionsumgebung in einer separaten Domäne oder in einem physisch isolierten Netzwerk verwendet.
> 
> In diesen Szenarien kann die einzige Möglichkeit sein, das Webpaket auf den Zielserver zu portieren und es manuell in IIS zu importieren. Obwohl dieser Ansatz eine automatisierte Bereitstellung verhindert, ist es immer noch ein sehr effektives Verfahren zum Veröffentlichen einer&#x2014;Webanwendung. Sie kopieren lediglich eine einzelne ZIP-Datei auf Ihren Webserver und verwenden einen Assistenten, der Sie durch den Import Vorgang führt.

Dieses Thema ist Teil einer Reihe von Tutorials, basierend auf den Anforderungen an die Unternehmens Bereitstellung eines fiktiven Unternehmens namens Fabrikam, Inc. In dieser tutorialreihe wird&#x2014;eine Beispiellösung der [Contact Manager-Lösung](the-contact-manager-solution.md)&#x2014;verwendet, um eine Webanwendung mit einem realistischen Komplexitäts Grad darzustellen, einschließlich einer ASP.NET MVC 3-Anwendung, eines Windows Communication Foundation (WCF)-Diensts und eines Datenbankprojekts.

## <a name="task-overview"></a>Aufgaben Übersicht

Sie müssen diese allgemeine Aufgaben durchführen, um ein Webbereitstellungs Paket in IIS zu importieren:

- Erstellen Sie ein Webbereitstellungs Paket mit der MSBuild-Befehlszeile, mit Team Build oder mit Visual Studio 2010.
- Kopieren Sie das Webpaket auf den Zielweb Server.
- Verwenden Sie den Assistenten zum Importieren von Anwendungspaketen im IIS-Manager, um das Webpaket zu installieren und Werte für Variablen wie Verbindungs Zeichenfolgen und Dienst Endpunkte bereitzustellen.

In diesem Thema wird gezeigt, wie Sie diese Prozeduren ausführen. In den Aufgaben und exemplarischen Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie bereits mit den Konzepten der Webpakete, Web deploy und WPP vertraut sind. Weitere Informationen finden Sie unter [Erstellung und Verpacken von Webanwendungs Projekten](building-and-packaging-web-application-projects.md).

> [!NOTE]
> Dieses Thema wird am besten in Verbindung mit der [Konfiguration eines Webservers für die Web deploy Veröffentlichung (Offline Bereitstellung)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)verwendet. darin wird erläutert, wie Sie die erforderlichen Komponenten installieren und eine IIS-Website für den Paket Import vorbereiten.

## <a name="create-a-web-deployment-package"></a>Erstellen eines Webbereitstellungs Pakets

Die erste Aufgabe besteht darin, ein Webbereitstellungs Paket für das Webanwendungs Projekt zu erstellen, das Sie bereitstellen möchten. Webpakete können auf verschiedene Arten erstellt werden.

**Ansatz 1: Erstellen eines Pakets im Rahmen des Buildprozesses mit Visual Studio**

Sie können das Webanwendungs Projekt so konfigurieren, dass nach jedem Build ein Webbereitstellungs Paket auf den Eigenschaften Seiten des Projekts auf der Registerkarte " **packen/veröffentlichen** " erstellt wird. Dieser Prozess wird unter entwickeln [und Verpacken von Webanwendungs Projekten](building-and-packaging-web-application-projects.md)beschrieben.

**Ansatz 2: Erstellen eines Pakets im Rahmen des Buildprozesses mit MSBuild**

Wenn Sie das Webanwendungs Projekt mithilfe von MSBuild direkt über eine benutzerdefinierte MSBuild-Projektdatei oder über die Befehlszeile erstellen, können Sie im Rahmen des Buildprozesses ein Webbereitstellungs Paket erstellen, indem Sie die Eigenschaften **deployonbuild = true** und **deploytarget = Package** in den Befehl einschließen. Dieser Prozess wird Untergrund Legendes [zum Buildprozess](understanding-the-build-process.md)beschrieben.

**Ansatz 3: Bedarfs gesteuertes Erstellen eines Pakets in Visual Studio**

Sie können in Visual Studio 2010 jederzeit ein Webbereitstellungs Paket für ein Webanwendungs Projekt erstellen. Klicken Sie hierzu im **Projektmappen-Explorer** Fenster mit der rechten Maustaste auf das Webanwendungs Projekt, und klicken Sie dann auf **Bereitstellungs Paket erstellen**.

![](manually-installing-web-packages/_static/image1.png)

**Ansatz 4: Bedarfs gesteuertes Erstellen eines Pakets über die Befehlszeile**

Sie können ein Webbereitstellungs Paket über die Befehlszeile erstellen, indem Sie das **Paket** Ziel in Ihrem Webanwendungs Projekt mithilfe von MSBuild aufrufen. Der Befehl sollte in etwa wie folgt aussehen:

[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]

Je nachdem, welchen Ansatz Sie verwenden, ist das Endergebnis identisch. Das WPP erstellt ein Webbereitstellungs Paket als ZIP-Datei und verschiedene unterstützende Ressourcen im Ausgabeordner für Ihr Webanwendungs Projekt.

![](manually-installing-web-packages/_static/image2.png)

Wenn Sie beabsichtigen, das Webpaket manuell zu importieren, benötigen Sie nur die ZIP-Datei. Kopieren Sie diese Datei auf den Zielweb Server, und Sie können den Import Vorgang starten.

## <a name="import-a-web-package-into-iis"></a>Importieren eines Webpakets in IIS

Sie können das nächste Verfahren verwenden, um ein Webbereitstellungs Paket aus dem lokalen Dateisystem in eine IIS-Website zu importieren. Bevor Sie dieses Verfahren ausführen, stellen Sie sicher, dass Sie über Folgendes verfügen:

- Das Webbereitstellungs Paket wurde auf den Webserver kopiert.
- Konfigurieren eines IIS-Webservers zum Hosten der Anwendung.

Weitere Informationen zum Konfigurieren eines IIS-Webservers für die Unterstützung von Webbereitstellungs Paketen finden Sie unter [Konfigurieren eines Webservers für die Web deploy-Veröffentlichung (Offline Bereitstellung)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**So importieren Sie ein Webbereitstellungs Paket mit IIS-Manager**

1. Klicken Sie im IIS-Manager im Bereich **Verbindungen** mit der rechten Maustaste auf Ihre IIS-Website, zeigen **Sie auf bereit**stellen, und klicken Sie dann auf **Anwendung importieren**.

    ![](manually-installing-web-packages/_static/image3.png)
2. Navigieren Sie im Assistenten zum Importieren von Anwendungspaketen auf der Seite **Paket auswählen** zum Speicherort Ihres Webbereitstellungs Pakets, und klicken Sie dann auf **weiter**.
3. Deaktivieren Sie auf der Seite **Wählen Sie die Inhalte des Pakets aus** den Inhalt, den Sie nicht benötigen, und klicken Sie dann auf **weiter**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > In vielen Fällen möchten Sie möglicherweise nicht alles importieren, was ein Webbereitstellungs Paket enthält. Beispielsweise möchten Sie möglicherweise Web deploy nicht zulassen, dass die zugehörige Datenbank ersetzt wird.  
    > Mit den Einträgen zum **Erteilen von Berechtigungen** werden Berechtigungen für das Ziel Dateisystem festgelegt, um sicherzustellen, dass die Identität des Anwendungs Pools auf den physischen Ordner zugreifen kann, in dem der Website Inhalt Außerdem erhält der Benutzer der anonymen Authentifizierung Leseberechtigung für den Ordner, damit die Anwendung Multipurpose Internet Mail Extensions (MIME)-Typdateien bedient. Wenn Sie möchten, können Sie diese Einträge entfernen und Berechtigungen manuell konfigurieren.
4. Geben Sie auf der Seite **Anwendungspaket Informationen eingeben** die angeforderten Informationen an.

    ![](manually-installing-web-packages/_static/image5.png)
5. Wenn Sie ein Webpaket erstellen, analysiert WPP die Konfigurationsdatei für die Anwendung und erkennt alle Variablen, wie z. b. Verbindungs Zeichenfolgen und Dienst Endpunkte. In diesem Fall:

    1. Der **Anwendungspfad** ist der IIS-Pfad, in dem Sie Ihre Anwendung installieren möchten. Diese Einstellung gilt für alle Bereitstellungs Pakete, die von WPP erstellt werden.
    2. Die Adresse des **contactservice-Dienst Endpunkts** ist die Adresse, die die Anwendung für die Kommunikation mit dem bereitgestellten WCF-Dienst verwenden soll. Diese Einstellung entspricht einem Eintrag in der Datei " *Web. config* ".
    3. Die erste **Verbindungs Zeichenfolge** ist die Verbindungs Zeichenfolge, die Web deploy zum Bereitstellen der Datenbank verwenden soll, die der Anwendung zugeordnet ist (in diesem Fall eine ASP.net-Mitgliedschafts Datenbank). Diese Einstellung entspricht der Einstellung auf der Registerkarte **SQL packen/veröffentlichen** in Visual Studio.
    4. Die zweite Einstellung der **Verbindungs Zeichenfolge** ist die Verbindungs Zeichenfolge, die von der Anwendung verwendet wird, um mit der Datenbank zu kommunizieren, wenn Sie ausgeführt wird. Dies entspricht einem Eintrag der Verbindungs Zeichenfolge in der Datei " *Web. config* ".

        > [!NOTE]
        > Weitere Informationen zum Speicherort dieser Parameter finden Sie unter [Konfigurieren von Parametern für die Webpaket Bereitstellung](configuring-parameters-for-web-package-deployment.md).
6. Klicken Sie auf **Weiter**.
7. Wenn Sie die Anwendung nicht zum ersten Mal auf dieser Website bereitgestellt haben, werden Sie aufgefordert, anzugeben, ob Sie vor der Installation den gesamten vorhandenen Inhalt löschen möchten. Wählen Sie die Option aus, die Ihren Anforderungen entspricht, und klicken Sie dann auf **weiter**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Wenn die Installation des Pakets von IIS abgeschlossen ist, klicken Sie auf **Fertig**stellen.

    ![](manually-installing-web-packages/_static/image7.png)

An diesem Punkt haben Sie Ihre Webanwendung erfolgreich in IIS veröffentlicht.

## <a name="conclusion"></a>Zusammenfassung

In diesem Thema wird beschrieben, wie Sie ein Webbereitstellungs Paket mithilfe des IIS-Managers in eine IIS-Website importieren. Diese Vorgehensweise für die Veröffentlichung von Webanwendungen ist sinnvoll, wenn die Remote Bereitstellung durch Sicherheits-oder Infrastruktur Einschränkungen unmöglich oder unerwünscht ist

## <a name="further-reading"></a>Weitere nützliche Informationen

Anleitungen zum Konfigurieren eines IIS-Webservers zur Unterstützung des manuellen Importierens eines Webpakets finden Sie unter [Konfigurieren eines Webservers für die Web deploy Veröffentlichung (Offline Bereitstellung)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Allgemeine Anleitungen zum Bereitstellen von Webpaketen finden Sie unter Exemplarische Vorgehensweise: bereitstellen [eines Webanwendungs Projekts mithilfe eines Webbereitstellungs Pakets (Teil 1 von 4)](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Previous](creating-and-running-a-deployment-command-file.md)
