---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: Die Contact Manager-Lösung | Microsoft-Dokumentation
author: jrjlee
description: In dieser Reihe von Tutorials wird eine Beispiel&#x2014;Lösung für die Kontakt&#x2014;-Manager-Lösung verwendet, um eine Unternehmens Anwendung mit einem realistischen...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 12ed7827f7392e559e04121386f7cd045de8462b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78473811"
---
# <a name="the-contact-manager-solution"></a>Contact Manager-Lösung

von [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In dieser [Reihe von Tutorials](web-deployment-in-the-enterprise.md) wird eine Beispiel&#x2014;Lösung für die Kontakt&#x2014;-Manager-Lösung verwendet, um eine Unternehmens Anwendung mit einem realistischen Komplexitäts Grad darzustellen. In diesem Thema wird die Contact Manager-Lösung vorgestellt, die Hauptkomponenten der Lösung beschrieben und die Herausforderungen bei der Bereitstellung dieser Art von Anwendung auf verschiedenen Zielplattformen in einer Unternehmensumgebung aufgezeigt.
> 
> Wenn Sie die Themen in diesen Tutorials durcharbeiten, können Sie die Contact Manager-Lösung als Referenz Implementierung verwenden, die veranschaulicht, wie Sie bestimmte Herausforderungen in Szenarios für die Unternehmens Bereitstellung erfüllen können. Im nächsten Thema, [Einrichten der Contact Manager-Lösung](setting-up-the-contact-manager-solution.md), wird beschrieben, wie Sie die Lösung herunterladen und auf Ihrer Entwickler Arbeitsstation ausführen.

## <a name="solution-overview"></a>Übersicht über die Lösungen

Die Contact Manager-Lösung besteht aus vier einzelnen Projekten:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager. MVC**. Dabei handelt es sich um ein ASP.NET MVC 3-Webanwendungs Projekt, das den Einstiegspunkt für die Lösung darstellt. Sie bietet einige grundlegende Webanwendungs Funktionen, wie z. b. die Bereitstellung von Kontaktinformationen durch Benutzer. Die Anwendung basiert auf einem Windows Communication Foundation (WCF)-Dienst zum Verwalten von Kontakten und einer ASP.NET-Anwendungs Dienst Datenbank zum Verwalten der Authentifizierung und Autorisierung.
- **ContactManager. Database**. Dies ist ein Visual Studio-Datenbankprojekt. Das-Projekt definiert das Schema für eine Datenbank, in der die Kontaktinformationen gespeichert werden.
- **ContactManager. Service**. Dies ist ein WCF-Webdienst Projekt. Der WCF-Dienst macht einen Endpunkt verfügbar, mit dem Aufrufer Vorgänge zum Erstellen, abrufen, aktualisieren und löschen (CRUD) für die **ContactManager** -Datenbank ausführen können. Der Dienst basiert auf der **ContactManager** -Datenbank und der **ContactManager. Common. dll** -Assembly.
- **ContactManager. Common**. Dies ist ein Klassen Bibliotheksprojekt. Der WCF-Dienst basiert auf in dieser Assembly definierten Typen.

Die Lösung umfasst auch einen Projektmappenordner mit dem Namen veröffentlichen. Diese enthält verschiedene benutzerdefinierte Projektdateien und Befehls Dateien, die veranschaulichen, wie Sie den Build-und Bereitstellungs Prozess steuern und bearbeiten können. Diese werden im weiteren Verlauf dieses Tutorials ausführlicher behandelt.

Die Komponenten der Lösung werden auf konzeptioneller Ebene wie folgt angepasst:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Während die ASP.NET MVC 3-Webanwendung den ASP.net-Mitgliedschafts Anbieter verwendet, lassen alle Seiten innerhalb der Webanwendung den anonymen Zugriff zu. Dabei handelt es sich eindeutig nicht um eine realistische Konfiguration. Die Lösung wird jedoch auf diese Weise eingerichtet, um Ihnen das Bereitstellen und Testen der Lösung zu erleichtern, ohne Benutzerkonten und Rollen zu konfigurieren.

## <a name="deployment-challenges"></a>Bereitstellungs Herausforderungen

Die Contact Manager-Lösung veranschaulicht verschiedene Probleme bei der Bereitstellung, die in vielen Szenarien für die Unternehmens Bereitstellung auftreten:

- Die Lösung besteht aus mehreren abhängigen Projekten. Sie müssen diese Projekte gleichzeitig bereitstellen.
- Verbindungs Zeichenfolgen und Dienst Endpunkte müssen für jede Umgebung aktualisiert werden, und in vielen Fällen sind diese Informationen für den Entwickler nicht verfügbar.
- Wenn Sie die **ContactManager** -Datenbank in Staging-und Produktionsumgebungen bereitstellen, müssen Sie vorhandene Daten in nachfolgenden bereit Stellungen beibehalten.
- Beim Bereitstellen der ASP.NET-Anwendungs Dienst Datenbank müssen Sie einige Konfigurationsdaten bereitstellen, aber keine Benutzerkontodaten weglassen.
- Die Projekte enthalten einige Dateien und Ordner, die nicht bereitgestellt werden sollen. Diese Dateien und Ordner müssen vom Bereitstellungs Prozess ausgeschlossen werden.
- Die Lösung muss die automatisierte Bereitstellung von einem Team Foundation Server-Buildserver (TFS) unterstützen.

## <a name="conclusion"></a>Zusammenfassung

In diesem Thema wurde eine allgemeine Übersicht über die Contact Manager-Lösung bereitgestellt, und es wurden einige der inhärenten Probleme bei der Bereitstellung identifiziert, die in vielen Szenarien für die Unternehmens Bereitstellung auftreten. In den restlichen Themen in diesem Tutorial werden einige der Techniken beschrieben, die Sie verwenden können, um diese Herausforderungen zu erfüllen.

Im nächsten Thema, [Einrichten der Contact Manager-Lösung](setting-up-the-contact-manager-solution.md), wird beschrieben, wie Sie die Lösung herunterladen und auf Ihrer Entwickler Arbeitsstation ausführen.

> [!div class="step-by-step"]
> [Zurück](web-deployment-in-the-enterprise.md)
> [Weiter](setting-up-the-contact-manager-solution.md)
