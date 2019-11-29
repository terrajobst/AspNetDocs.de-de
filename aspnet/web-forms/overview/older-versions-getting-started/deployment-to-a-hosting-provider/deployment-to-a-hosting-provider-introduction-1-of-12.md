---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio: Einführung-1 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser Reihe von Tutorials wird gezeigt, wie Sie ein ASP.NET-Webanwendungs Projekt bereitstellen (veröffentlichen), das eine SQL Server Compact-Datenbank mit Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: ea88da1e6d510f706fc7ca370cfa32974c1243f8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74587744"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio: Einführung-1 von 12

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser Reihe von Tutorials erfahren Sie, wie Sie ein ASP.NET-Webanwendungs Projekt, das eine SQL Server Compact Datenbank enthält, mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web bereitstellen (veröffentlichen). Sie können auch Visual Studio 2010 verwenden, wenn Sie das Webveröffentlichungs Update installieren.
> 
> Ein Tutorial, das nach der RC-Version von Visual Studio 2012 eingeführte Bereitstellungs Funktionen zeigt, zeigt, wie SQL Server Editionen außer SQL Server Compact bereitgestellt werden, und zeigt, wie Sie die Bereitstellung für Azure App Service Web-Apps ausführen. Weitere Informationen finden Sie unter [ASP.net Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Diese Tutorials führen Sie durch die Bereitstellung von zuerst in IIS auf dem lokalen Entwicklungs Computer zu Testzwecken und dann durch einen Drittanbieter-Hostinganbieter. Die Anwendung, die Sie bereitstellen, verwendet eine Anwendungsdatenbank und eine ASP.net-Mitgliedschafts Datenbank. Sie beginnen mit der Verwendung von SQL Server Compact und der Bereitstellung in SQL Server Compact, und in späteren Tutorials erfahren Sie, wie Sie Daten Bank Änderungen bereitstellen und die Migration zu SQL Server durchzuführen.
> 
> In den Tutorials wird davon ausgegangen, dass Sie mit der Arbeit mit ASP.net in Visual Studio vertraut sind. Wenn Sie dies nicht tun, empfiehlt es sich, ein [einfaches ASP.net-Web Forms Tutorial](../tailspin-spyworks/tailspin-spyworks-part-1.md) oder ein [einfaches ASP.NET MVC-Tutorial](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)zu starten.
> 
> Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net-Bereitstellungs Forum](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)veröffentlichen.

## <a name="overview"></a>Übersicht über

Diese Tutorials führen Sie durch die Bereitstellung von zuerst in IIS auf dem lokalen Entwicklungs Computer zu Testzwecken und dann durch einen Drittanbieter-Hostinganbieter. Die Anwendung, die Sie bereitstellen, verwendet eine Anwendungsdatenbank und eine ASP.net-Mitgliedschafts Datenbank. Sie beginnen mit der Verwendung von SQL Server Compact und der Bereitstellung in SQL Server Compact, und in späteren Tutorials erfahren Sie, wie Sie Daten Bank Änderungen bereitstellen und die Migration zu SQL Server durchzuführen.

Die Anzahl der Lernprogramme – 11 und eine Problem Behandlungs Seite – könnte dazu führen, dass der Bereitstellungs Prozess erschreckend erscheint. Tatsächlich bilden die grundlegenden Verfahren für die Bereitstellung eines Standorts einen relativ kleinen Teil des Lernprogramm Satzes. In realen Situationen benötigen Sie jedoch oft Informationen zu einigen kleinen, aber wichtigen zusätzlichen Aspekten der Bereitstellung, z. –. zum Festlegen der Ordner Berechtigungen auf dem Zielserver. Wir haben viele dieser zusätzlichen Techniken in die Lernprogramme eingefügt, mit der Hoffnung, dass die Lernprogramme keine Informationen hinterlassen, die möglicherweise verhindern, dass eine echte Anwendung erfolgreich bereitgestellt wird.

Die Lernprogramme sind so konzipiert, dass Sie nacheinander ausgeführt werden, und jeder Teil basiert auf dem vorherigen Teil. Sie können jedoch Teile überspringen, die für Ihre Situation nicht relevant sind. (Das Überspringen von Teilen erfordert möglicherweise die Anpassung der Prozeduren in späteren Tutorials.)

## <a name="intended-audience"></a>Beabsichtigte Zielgruppe

Die Tutorials richten sich an ASP.NET-Entwickler, die in kleinen Organisationen oder in anderen Umgebungen arbeiten, in denen Folgendes gilt:

- Ein Continuous Integration Prozess (automatisierte Builds und Bereitstellung) wird nicht verwendet.
- Die Produktionsumgebung ist ein Drittanbieter-Hostinganbieter.
- Eine Person füllt üblicherweise mehrere Rollen auf (dieselbe Person entwickelt, testet und bereitgestellt).

In Unternehmensumgebungen ist es typischer, Continuous Integration Prozesse zu implementieren, und die Produktionsumgebung wird normalerweise von den eigenen Servern des Unternehmens gehostet. Verschiedene Personen führen in der Regel auch verschiedene Rollen aus. Weitere Informationen zur Unternehmens Bereitstellung finden Sie unter Bereitstellen von [Webanwendungen in Unternehmens Szenarien](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Organisationen aller Größen können auch Webanwendungen in Azure bereitstellen, und die meisten der in diesen Tutorials gezeigten Verfahren gelten auch für Azure-App Services-Web-Apps. Eine Einführung in Azure finden Sie unter [https://azure.microsoft.com](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>Der in den Tutorials gezeigte Hostinganbieter

Die Lernprogramme führen Sie durch den Prozess der Einrichtung eines Kontos bei einem Hostingunternehmen und Bereitstellen der Anwendung für diesen Hostinganbieter. Es wurde ein bestimmtes Hostingunternehmen ausgewählt, damit die Tutorials die gesamte Umgebung für die Bereitstellung auf einer Live Website veranschaulichen. Jedes Hostingunternehmen bietet verschiedene Features, und die Umgebung für die Bereitstellung auf den Servern variiert etwas. der in diesem Tutorial beschriebene Prozess ist jedoch typisch für den gesamten Prozess.

Der für dieses Tutorial verwendete Hostinganbieter (Cytanium.com) ist einer von vielen, die verfügbar sind, und die Verwendung in diesem Tutorial stellt keine Bestätigung oder Empfehlung dar.

## <a name="deploying-web-site-projects"></a>Bereitstellen von Website Projekten

Die ""-Website ist ein Visual Studio-Webanwendungs Projekt. Die meisten Bereitstellungs Methoden und-Tools, die in diesem Tutorial veranschaulicht werden, gelten nicht für [Website Projekte](https://msdn.microsoft.com/library/dd547590.aspx). Weitere Informationen zum Bereitstellen von Website Projekten finden Sie unter [ASP.net Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Bereitstellen von ASP.NET MVC-Projekten

In diesem Tutorial stellen Sie ein ASP.net Web Forms-Projekt bereit, aber alles, was Sie lernen, gilt auch für ASP.NET MVC. Ein Visual Studio-MVC-Projekt ist nur eine andere Form des Webanwendungs Projekts. Der einzige Unterschied besteht darin, dass Sie bei der Bereitstellung für einen Hostinganbieter, der ASP.NET MVC oder Ihre Zielversion nicht unterstützt, sicherstellen müssen, dass das entsprechende nuget-Paket ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) oder [MVC 4](http://nuget.org/packages/aspnetmvc)) in Ihrem Projekt installiert ist.

## <a name="programming-language"></a>Programmiersprache

Die Beispielanwendung verwendet C# , aber die Lernprogramme erfordern keine Kenntnisse C#von, und die von den Tutorials gezeigten Bereitstellungs Techniken sind nicht sprachspezifisch.

## <a name="troubleshooting-during-this-tutorial"></a>Problembehandlung in diesem Tutorial

Wenn während der Bereitstellung ein Fehler auftritt oder wenn der bereitgestellte Standort nicht ordnungsgemäß ausgeführt wird, stellen die Fehlermeldungen nicht immer eine Lösung dar. Zur Unterstützung bei einigen häufigen Problem Szenarios ist eine [Referenzseite](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) zur Problembehandlung verfügbar. Wenn Sie eine Fehlermeldung erhalten oder etwas nicht funktioniert, wenn Sie die Tutorials durchlaufen, achten Sie darauf, dass Sie die Seite Problembehandlung überprüfen.

## <a name="comments-welcome"></a>Begrüßungs Kommentare

Kommentare zu den Tutorials sind willkommen, und wenn das Tutorial aktualisiert wird, werden alle Maßnahmen zur Unterweisung von Korrekturen oder Vorschlägen für Verbesserungen durchgeführt, die in den Lernprogramm Kommentaren bereitgestellt werden.

## <a name="prerequisites"></a>Erforderliche Voraussetzungen

Bevor Sie beginnen, stellen Sie sicher, dass Sie Windows 7 oder höher und eines der folgenden Produkte auf Ihrem Computer installiert haben:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC oder Visual Studio Express 2012 RC für Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Wenn Sie über Visual Studio 2010 SP1 oder Visual Web Developer Express 2010 SP1 verfügen, installieren Sie auch die folgenden Produkte:

- [Azure SDK für .net (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (enthält das Webveröffentlichungs Update)
- [Microsoft Visual Studio 2010 SP1-Tools für SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Zum Abschluss des Tutorials ist eine andere Software erforderlich, aber Sie müssen diese noch nicht geladen haben. Das Tutorial führt Sie durch die Schritte zur Installation, wenn Sie es benötigen.

## <a name="downloading-the-sample-application"></a>Herunterladen der Beispielanwendung

Die Anwendung, die Sie bereitstellen, hat die Bezeichnung "Configuration University" und wurde bereits für Sie erstellt. Dabei handelt es sich um eine vereinfachte Version einer University-Website, die in den [Entity Framework Tutorials auf der ASP.NET-Website](https://asp.net/entity-framework/tutorials)beschrieben wird.

Wenn Sie die erforderlichen Komponenten installiert haben, laden Sie die [Webanwendung](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)der "Web. Die *ZIP* -Datei enthält mehrere Versionen des Projekts und eine PDF-Datei, die alle 12 Tutorials enthält. Wenn Sie die Schritte im Tutorial durcharbeiten möchten, beginnen Sie mit condesouniversity-BEGIN. Um zu sehen, wie das Projekt am Ende der Tutorials aussieht, öffnen Sie contosouniversity-End. Öffnen Sie contosouniversity-AfterTutorial09, um zu sehen, wie das Projekt vor der Migration zum vollständigen SQL Server in Tutorial 10 aussieht.

Um die Arbeit mit den Lernprogramm Schritten vorzubereiten, speichern Sie condesouniversity-BEGIN in jedem Ordner, den Sie für die Arbeit mit Visual Studio-Projekten verwenden. Standardmäßig handelt es sich hierbei um den folgenden Ordner:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Für die Bildschirmfotos in diesem Tutorial befindet sich der Projektordner im Stammverzeichnis auf dem `C`:-Laufwerk.)

Starten Sie Visual Studio, öffnen Sie das Projekt, und drücken Sie STRG + F5, um es auszuführen.

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Die Website Seiten können über die Menüleiste aufgerufen werden, und Sie können die folgenden Funktionen ausführen:

- Zeigen Sie die Statistik Statistik an (die Seite "Info").
- Anzeigen, bearbeiten, löschen und Hinzufügen von Schülern.
- Anzeigen und Bearbeiten von Kursen.
- Dozenten anzeigen und bearbeiten.
- Anzeigen und Bearbeiten von Abteilungen.

Im folgenden finden Sie Screenshots von einigen repräsentativen Seiten.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Überprüfen von Anwendungs Features, die die Bereitstellung

Die folgenden Funktionen der Anwendung wirken sich darauf aus, wie Sie Sie bereitstellen oder was Sie tun müssen, um Sie bereitzustellen. Diese werden in den folgenden Tutorials der Reihe ausführlicher erläutert.

- Die Configuration Manager-Datenbank verwendet eine SQL Server Compact Datenbank, um Anwendungsdaten wie z. b. Studenten-und Dozenten Namen zu speichern. Die Datenbank enthält eine Mischung aus Testdaten und Produktionsdaten. Wenn Sie eine Bereitstellung in der Produktionsumgebung durchzuführen, müssen Sie die Testdaten ausschließen. Später in der tutorialreihe migrieren Sie von SQL Server Compact zu SQL Server.
- Die Anwendung verwendet das ASP.NET-Mitgliedschaftssystem, in dem die Benutzerkontoinformationen in einer SQL Server Compact-Datenbank gespeichert werden. Die Anwendung definiert einen Administrator Benutzer, der Zugriff auf einige eingeschränkte Informationen hat. Sie müssen die Mitgliedschafts Datenbank ohne Testkonten, jedoch mit einem Administrator Konto bereitstellen.
- Da die Anwendungsdatenbank und die Mitgliedschafts Datenbank SQL Server Compact als Datenbankmodul verwenden, müssen Sie die Datenbank-Engine sowohl für den Hostinganbieter als auch die Datenbanken selbst bereitstellen.
- Die Anwendung verwendet universelle ASP.net-Mitgliedschafts Anbieter, damit das Mitgliedschaftssystem seine Daten in einer SQL Server Compact Datenbank speichern kann. Die Assembly, die die universellen Mitgliedschafts Anbieter enthält, muss mit der Anwendung bereitgestellt werden.
- Die Anwendung verwendet den Entity Framework 5,0, um auf Daten in der Anwendungsdatenbank zuzugreifen. Die Assembly, die Entity Framework 5,0 enthält, muss mit der Anwendung bereitgestellt werden.
- Die Anwendung verwendet das Dienstprogramm zur Fehler Protokollierung und Berichterstellung von Drittanbietern. Dieses Hilfsprogramm wird in einer Assembly bereitgestellt, die mit der Anwendung bereitgestellt werden muss.
- Das Hilfsprogramm für die Fehler Protokollierung Schreibt Fehlerinformationen in XML-Dateien in einen Datei Ordner. Sie müssen sicherstellen, dass das Konto, unter dem ASP.NET ausgeführt wird, über Schreibberechtigungen für diesen Ordner verfügt und dass Sie diesen Ordner von der Bereitstellung ausschließen müssen. (Andernfalls können Fehlerprotokoll Daten aus der Testumgebung in Produktions-und/oder Produktionsfehler Protokolldateien gelöscht werden.)
- Die Anwendung enthält einige Einstellungen, die in der bereitgestellten *Web. config* -Datei in Abhängigkeit von der Zielumgebung (Test oder Produktion) und anderen Einstellungen, die je nach Buildkonfiguration geändert werden müssen, geändert werden müssen (Debug oder Release).
- Die Visual Studio-Projekt Mappe enthält ein Klassen Bibliotheksprojekt. Nur die Assembly, die von diesem Projekt generiert wird, sollte bereitgestellt werden, nicht das Projekt selbst.

In diesem ersten Tutorial der Reihe haben Sie das Visual Studio-Beispiel Projekt heruntergeladen und die Website Features überprüft, die sich auf die Bereitstellung der Anwendung auswirken. In den folgenden Tutorials bereiten Sie sich auf die Bereitstellung vor, indem Sie einige dieser Aktionen einrichten, die automatisch verarbeitet werden sollen. Andere, die Sie manuell erledigen.

> [!div class="step-by-step"]
> [Nächste](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
