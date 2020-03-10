---
uid: whitepapers/aspnet-web-deployment-content-map
title: ASP.net Web Deployment-Empfohlene Ressourcen | Microsoft-Dokumentation
author: rick-anderson
description: Dieses Thema enthält Links zu Dokumentations Ressourcen zum Bereitstellen (veröffentlichen) ASP.NET Webanwendungen in IIS mithilfe von Visual Studio 2010, Visual Web de...
ms.author: riande
ms.date: 03/14/2014
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 96873c8f2b0ad2415f371aceb651400c801a3338
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78517275"
---
# <a name="aspnet-web-deployment---recommended-resources"></a>ASP.NET: Webbereitstellung – Empfohlene Ressourcen

> Dieses Thema enthält Links zu Dokumentations Ressourcen zum Bereitstellen (veröffentlichen) von ASP.NET-Webanwendungen in IIS mithilfe von Visual Studio 2010, Visual Web Developer 2010 und höheren Versionen.
> 
> Wenn Sie einen tollen Blogbeitrag, [StackOverflow](http://stackoverflow.com) -Thread oder einen anderen Link, der nützlich wäre, kennen, [Senden Sie uns eine e-Mail](mailto:aspnetue@microsoft.com?subject=Deployment%20Content%20Map) mit dem Link.
> 
> > [!NOTE] 
> > 
> > Viele dieser Ressourcen beschreiben Bereitstellungs Funktionen, die nur verfügbar sind, wenn Sie eine neue Version des [Visual Studio-Webveröffentlichungs Updates](https://go.microsoft.com/fwlink/?LinkID=208120)installieren. Einige der Features sind nur in Visual Studio 2012 oder Visual Studio 2013 verfügbar.

Dieses Thema enthält folgende Abschnitte:

- [Grundlegendes zu Bereitstellungs Optionen für Webprojekte](#understanding)
- [Suchen von Hostinganbietern für eine ASP.NET-Anwendung](#findinghosting)
- [Bereitstellen einer Webanwendung aus Visual Studio](#fromvs)
- [Bereitstellen einer Webanwendung durch Erstellen und Installieren eines Webbereitstellungs Pakets](#package)
- [Bereitstellen einer Webanwendung mithilfe eines Continuous Integration (CI)-Prozesses](#ci)
- [Verwenden von Web. config-Transformationen zum Ändern der Einstellungen in der Web. config-Zieldatei oder der app. config-Datei während der Bereitstellung](#transforms)
- [Verwenden von Web deploy-Parametern zum Ändern der Einstellungen in der Zielweb Anwendung während der Bereitstellung](#webdeployparms)
- [Sicherstellen, dass eine Anwendung während der Bereitstellung offline ist](#appoffline)
- [Bereitstellen einer Datenbank oder von Änderungen an einer Datenbank im Rahmen der Bereitstellung von Webanwendungen](#databasewithweb)
- [Separates Bereitstellen einer Datenbank von der webanwendungsbereit](#databaseseparate)
- [Bereitstellen einer Webanwendung, die ASP.NET-Anwendungsdienste wie Mitgliedschaft und Profilerstellung verwendet](#aspnetmembership)
- [Vorkompilieren für die Bereitstellung](#precompiling)
- [Bereitstellen einer Intranet-Webanwendung](#intranet)
- [Automatisieren allgemeiner Bereitstellungs Aufgaben, die standardmäßig nicht automatisiert sind](#automating)
- [Konfigurieren von Webservern, damit Entwickler mithilfe von Web deploy Webanwendungen bereitstellen können](#configuringservers)
- [Konfigurieren von Servern für einen Hostinganbieter](#hostingprovider)
- [Behandeln von Bereitstellungs Problemen](#troubleshooting)
- [Hilfe zu einer bestimmten Bereitstellungs Frage](#gettinghelp)
- [Weitere Ressourcen](#additional)

<a id="understanding"></a>

## <a name="understanding-deployment-options-for-web-projects"></a>Grundlegendes zu Bereitstellungs Optionen für Webprojekte

- [Übersicht über die Webbereitstellung für Visual Studio und ASP.net](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- Bereitstellen [einer Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)-Website Erläutert Optionen und Links zu Ressourcen für die Bereitstellung von Webprojekten für Windows Azure-Websites, einschließlich [Continuous Delivery](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (automatisiert aus der [Quell](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)Code Verwaltung) und Verwendung von Visual Studio.
- [Verbesserungen an der Webveröffentlichung in Visual Studio 2012](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (Video von Scott Hanselman).
- [Übersichts Beitrag für die Webbereitstellung in VS 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (Vishal Joshi es Blog). Ein älterer Blogbeitrag, aber einige der Visual Studio 2010-Ressourcen, auf die er verweist, enthalten Informationen, die für Visual Studio 2012 noch relevant sind.

<a id="findinghosting"></a>

## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Suchen von Hostinganbietern für eine ASP.NET-Anwendung

- [Hosten von ASP.NET](https://asp.net/hosting)

<a id="fromvs"></a>

## <a name="deploying-a-web-application-from-visual-studio"></a>Bereitstellen einer Webanwendung aus Visual Studio

- Bereitstellen [einer Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)-Website Erläutert Optionen und stellt Links zu Ressourcen für die Bereitstellung von Webprojekten für Windows Azure-Websites bereit. Enthält einen Abschnitt zum Bereitstellen von Visual Studio.
- [ASP.NET-Webbereitstellung mithilfe von Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)Dabei handelt es sich um eine Lernprogrammserie mit zwölf Teilen, in der die Bereitstellungsaufgaben vollständig vorgestellt werden. die 12-teilige tutorialreihe zeigt, wie Webanwendungen mit SQL Server-Datenbanken bereitgestellt werden. Bei der Daten Bank Bereitstellung werden sowohl der dbdacfx-Anbieter als auch Entity Framework Code First-Migrationen verwendet. Umfasst auch Informationen zu den [Transformationen der Datei "Web. config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)", zum Bereitstellen [einzelner Dateien](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), zur [Befehlszeilen Bereitstellung](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md)und [zum Anpassen der Visual Studio-Webveröffentlichungs Pipeline durch Bearbeiten von pubxml-Dateien](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Gilt für alle ASP.NET-Webprojekte, einschließlich Web Forms, MVC und Web-API.)
- Vorgehens [Weise: Bereitstellen eines Webprojekts mithilfe der One-Click-Veröffentlichung in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (Referenzinformationen für den Visual Studio-Webveröffentlichungs-Assistenten)
- Bereitstellen [einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Dies ist eine frühere Version der **ASP.net-Webbereitstellung mithilfe von Visual Studio** , die oben in diesem Abschnitt aufgeführt ist. Dieses Thema ist in erster Linie für Informationen zum Bereitstellen von SQL Server Compact-Datenbanken und zum Migrieren von SQL Server Compact zu einer Vollversion von SQL Server nützlich.
- [.NET-Anwendungen mit mehreren Ebenen mithilfe von Speicher Tabellen, Warteschlangen und BLOB](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure Site). die 5-teilige tutorialreihe zeigt, wie Sie ein MVC-Projekt erstellen und es in einem Windows Azure-clouddienst bereitstellen.

<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Bereitstellen einer Webanwendung durch Erstellen und Installieren eines Webbereitstellungs Pakets

- Vorgehens [Weise: Erstellen eines Webbereitstellungs Pakets in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- Gewusst [wie: Installieren eines Bereitstellungs Pakets mithilfe der von Visual Studio (MSDN) erstellten Datei "bereitstellen. cmd](https://msdn.microsoft.com/library/ff356104.aspx) ".
- [Verwenden eines Web deploy Pakets für die Bereitstellung auf IIS in der Entwicklungs-und einem Drittanbieter](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (Blog von SA. Hashimi). Verwenden des IIS-Managers zum Installieren eines Bereitstellungs Pakets in IIS auf dem lokalen Computer und in einem Hostingunternehmen, das IIS-Manager für die Remote Verwaltung unterstützt.
- Das entwickeln [eines Web deploy Pakets aus Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (IIS.NET-Website). Enthält Anweisungen zum Erstellen und Installieren von Befehlszeilen Paketen.
- [Paket einmal veröffentlichen](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (Sayed Hashimi Blog). Führt ein nuget-Paket ein, das den Prozess der Transformation der Datei "Web. config" für mehrere Ziel Umgebungen automatisiert, damit Sie ein Paket auf mehreren Servern bereitstellen können. Siehe auch das [packageweb-Video](https://www.youtube.com/watch?v=-LvUJFI8CzM) von Sayed Hashimi.

Weitere Informationen finden Sie auch im folgenden Abschnitt.

<a id="ci"></a>

## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Bereitstellen einer Webanwendung mithilfe eines Continuous Integration (CI)-Prozesses

- [Continuous Integration und Continuous Delivery (entwickeln realer Cloud-apps mit Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Ein E-Book-Kapitel, in dem Continuous Integration und Continuous Delivery vorgestellt werden.
- Bereitstellen [einer Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)-Website Erläutert Optionen und Links zu Ressourcen für die Bereitstellung von Webprojekten für Windows Azure-Websites. Enthält einen Abschnitt zur Automatisierung der Bereitstellung aus der Quell Code Verwaltung.
- Bereitstellen von [Webanwendungen in Unternehmens Szenarios](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) 40-teilige tutorialreihe zeigt, wie Sie die Bereitstellung in einem CI-Prozess mithilfe von Visual Studio 2010 und Team Foundation Server 2010 automatisieren.
- [In der Microsoft-Build-Engine: Verwenden von MSBuild und Team Foundation Build, von Sayed Hashimi und William bartholomomew](http://msbuildbook.com). Dabei handelt es sich um ein Buch, nicht um eine Webressource, aber es ist ein wesentlicher Leitfaden für das Erlernen der Konfiguration von MSBuild für Continuous Integration Szenarien.
- [MSBuild-Erweiterungspaket](https://github.com/mikefourie/MSBuildExtensionPack). Umfasst Bereitstellungs Aufgaben.
- [Team Foundation Build-Anpassungs Handbuch](https://aka.ms/vsarsolutions). Dokumentation von Alm Rangers bei der Einrichtung Team Foundation Server deckt die Webbereitstellung ab und enthält Tutorials und Videos.
- [Slowcheetah XML-Transformationen von einem CI-Server](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (Sayed Hashimi-Blog). Erläutert die Verwendung von slowcheetah, einem Visual Studio-Add-in zum Transformieren von App. config und anderen XML-Dateien.

Weitere Informationen finden Sie auch unter [sicherstellen, dass eine Anwendung während der Bereitstellung offline ist](aspnet-web-deployment-content-map.md#appoffline) .

<a id="transforms"></a>

## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Verwenden von Web. config-Transformationen zum Ändern der Einstellungen in der Web. config-Zieldatei oder der app. config-Datei während der Bereitstellung

- [Transformationen der Datei "Web. config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)".
- [Syntax der Web. config-Transformation für die Webprojekt Bereitstellung mithilfe von Visual Studio](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Webtools 2012,2-Web. config-Transformationen](https://www.youtube.com/watch?v=HdPK8mxpKEI) (YouTube-Video von Sayed Hashimi). Veranschaulicht die Einrichtung und Vorschau von Web. config-Transformationen.
- [Gewusst wie die Web. config-Transformation deaktivieren?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Wann sollte ich anstelle von Web. config-Transformationen Web deploy Parameter verwenden?](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [Xdt (XML Document Transform) veröffentlicht auf CodePlex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (.net-Webentwicklung und Tools-Blog). Kündigt die Verfügbarkeit des Quellcodes für das Web. config-dateitransformations-Engine an und listet einige Tools auf, die diese verwenden.
- [Windows Azure-Websites: Funktionsweise von Anwendungs-und Verbindungs](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) Zeichenfolgen (Microsoft Azure-Blog). Eine Alternative zu "Web. config" ist, wenn es sich bei Ihrer Zielumgebung um Windows Azure-Websites handelt und Sie `appSettings` oder `connectionStrings`transformieren möchten.

<a id="webdeployparms"></a>

## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Verwenden von Web deploy-Parametern zum Ändern der Einstellungen in der Zielweb Anwendung während der Bereitstellung

- Gewusst [wie: Verwenden von Web deploy-Parametern in einem Webbereitstellungs Paket](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [Msbereitstellung: Aktualisieren von App-Einstellungen bei der Veröffentlichung basierend auf dem Veröffentlichungs Profil](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (Blog von Sayed Hashimi). Zeigt, wie Sie Webbereitstellungs-Parameter in Visual Studio-Veröffentlichungs profile integrieren.
- [Web deploy Parametrisierung](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (IIS.NET-Website).
- [Web deploy Parametrisierung in Aktion](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (Vishal Joshi es Blog).
- [Web deploy-Parametrisierung im Vergleich zur Web. config-Transformation](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (Vishal Joshi es Blog).
- [Windows Azure-Websites: Funktionsweise von Anwendungs-und Verbindungs](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) Zeichenfolgen (Microsoft Azure-Blog). Eine Alternative zu Webbereitstellungs-Parametern, wenn die Zielumgebung Windows Azure-Websites ist und Sie `appSettings` oder `connectionStrings`parametrisieren möchten.

<a id="appoffline"></a>

## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Sicherstellen, dass eine Anwendung während der Bereitstellung offline ist

- [ASP.net-Webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Code Updates](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Weitere Informationen finden **Sie im Abschnitt offline schalten der Anwendung während der Bereitstellung.**
- [Offline schalten einer Anwendung vor der Veröffentlichung](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (IIS.NET-Website). Erläutert ein in Web Deploy 3,0 integriertes Feature, mit dem die Behandlung einer APP\_der Datei "offline. htm" automatisiert wird. Diese Funktion funktioniert nicht mit benutzerdefinierten App-\_Offline Dateien.
- Erfahren Sie [, wie Sie Ihre Web-App während der Veröffentlichung Offline](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) schalten (Sayed Hashimi Blog). Automatisieren des Prozesses der Verwendung einer benutzerdefinierten App\_offline. htm-Datei.
- [Webpublishing Updates für App offline und useCheckSum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (Blog zur Microsoft-Webentwicklung). Eine weitere Option für die Automatisierung der Verwendung von App-\_die Datei "offline. htm".
- [Web Deploy 3,5 RTW](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.NET-Website). Neues Feature in Web Deploy 3,5 für benutzerdefinierte App\_Offline Dateien. htm.

<a id="databasewithweb"></a>

## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Bereitstellen einer Datenbank oder von Änderungen an einer Datenbank im Rahmen der Bereitstellung von Webanwendungen

- [Konfigurieren der Daten Bank Bereitstellung in Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Übersicht über Optionen zum Bereitstellen einer Datenbank mit einem Webprojekt.
- [ASP.NET-Webbereitstellung mithilfe von Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)Dabei handelt es sich um eine Lernprogrammserie mit zwölf Teilen, in der die Bereitstellungsaufgaben vollständig vorgestellt werden. 12-teilige tutorialreihe, zeigt die Daten Bank Bereitstellung mithilfe des dbdacfx-Anbieters und der Entity Framework Code First-Migrationen.
- Gewusst [wie: Bereitstellen eines Webprojekts mithilfe der One-Click-Veröffentlichung in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- Stellen [Sie eine sichere ASP.NET MVC 5-App mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Windows Azure-Website](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)bereit. Ein langes Tutorial, in dem eine Anwendung erstellt und bereitgestellt wird, die eine einzelne SQL Server Datenbank sowohl für Mitgliedschafts-als auch für Anwendungsdaten verwendet.
- Bereitstellen [einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). die 12-teilige tutorialreihe zeigt, wie Sie SQL Server Compact-Datenbanken bereitstellen und wie Sie die Migration von SQL Server Compact zu einer Vollversion von SQL Server durchzuführen.

Weitere Informationen finden Sie unter Bereitstellen einer Webanwendung durch Erstellen und Installieren eines Webbereitstellungs Pakets und Bereitstellen einer Webanwendung mit einem Continuous Integration (CI)-Prozess weiter oben auf dieser Seite.

<a id="databaseseparate"></a>

## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Separates Bereitstellen einer Datenbank von der webanwendungsbereit

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [Einschließen von Daten in ein SQL Server Datenbankprojekt](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (SQL Server Data Tools Teamblog). Bereitstellen von Schemas und Daten beim Bereitstellen einer Datenbank.
- Vorgehensweise beim Bereitstellen [einer Datenbank in Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (Microsoft Azure Site)
- [Migrieren von Datenbanken zu Windows Azure SQL-Datenbank (früher SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Migrieren einer Datenbank zu SQL Azure mithilfe von SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (SQL Server Data Tools Teamblog).
- [Migrieren Daten orientierter Anwendungen zu Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [Migrieren von SQL Server-Datenbanken zu Windows Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).

<a id="aspnetmembership"></a>

## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Bereitstellen einer Webanwendung, die ASP.NET-Anwendungsdienste wie Mitgliedschaft und Profilerstellung verwendet

- Stellen [Sie eine sichere ASP.NET MVC 5-App mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Windows Azure-Website](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)bereit. Ein langes Tutorial, in dem eine Anwendung erstellt und bereitgestellt wird, die eine einzelne SQL Server Datenbank sowohl für Mitgliedschafts-als auch für Anwendungsdaten verwendet.
- [ASP.net Identity](https://asp.net/identity/). Ressourcen für ASP.net Identity.
- [ASP.NET-Webbereitstellung mithilfe von Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)Dabei handelt es sich um eine Lernprogrammserie mit zwölf Teilen, in der die Bereitstellungsaufgaben vollständig vorgestellt werden. die 12-teilige tutorialreihe zeigt, wie eine ASP.net-Mitgliedschafts Datenbank bereitgestellt wird.
- [Konfigurieren einer Website, die Anwendungsdienste verwendet](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Für Website Projekte, aber auch für Webanwendungs Projekte relevant.
- [Benutzer und Rollen auf der Produktions Website](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Für Website Projekte, aber auch für Webanwendungs Projekte relevant.

<a id="precompiling"></a>

## <a name="precompiling-for-deployment"></a>Vorkompilieren für die Bereitstellung

- [ASP.net Übersicht über die Vorkompilierung des Webanwendungs Projekts](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Registerkarte "Web packen/veröffentlichen", Projekteigenschaften](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Dialog Feld "Erweiterte vorkompilierungs Einstellungen](https://msdn.microsoft.com/library/hh475319.aspx) " (MSDN).

<a id="intranet"></a>

## <a name="deploying-an-intranet-web-application"></a>Bereitstellen einer Intranet-Webanwendung

- [Verwenden Sie die lokale Organisations Authentifizierungs Option (ADFS) mit ASP.net in Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (Blog von Vittorio Bertocci).
- [Erstellen einer Intranetsite mithilfe von ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). In älteren exemplarischen Vorgehensweisen für Visual Studio 2010 werden wichtige Änderungen an intranetprojektvorlagen, die in Visual Studio 2013 eingeführt wurden, nicht widerspiegelt.

<a id="automating"></a>

## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Automatisieren allgemeiner Bereitstellungs Aufgaben, die standardmäßig nicht automatisiert sind

- [ASP.net-Webbereitstellung mithilfe von Visual Studio: Bereitstellen zusätzlicher Dateien](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Festlegen von Ordner Berechtigungen für die Webveröffentlichung](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (Blog von sagraue Hashimi).
- [Erweitern der Zieldatei zum Einschließen von Registrierungs Einstellungen für ein Webprojekt Paket](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (Webentwicklungs Tools-Blog).
- [Erweiterung der XML-Transformation (Web. config)](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (Sayed Hashimi Blog). Zeigt, wie benutzerdefinierte xdt-Transformationen erstellt werden.
- Der [benutzerdefinierte Anbieter des Webbereitstellungs Tools (msdeployment) nimmt 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (sagraue Hashimi-Blog) an. Zeigt, wie ein Web deploy benutzerdefinierter Anbieter erstellt wird.
- [Verpacken und Bereitstellen von COM-Komponenten](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (Webentwicklungs Tools-Blog).
- [Verpacken von .net](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) -Assemblys (Webentwicklungs Tools-Blog). Bereitstellen von Assemblys im GAC.
- [Skripterstellung für alles: Initialisieren Sie Ihre Windows Azure-VM für Ihren Webserver mit IIS, Web deploy und anderen Dingen](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (tugberk Ugurlu es Blog).

<a id="configuringservers"></a>

## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Konfigurieren von Webservern, damit Entwickler mithilfe von Web deploy Webanwendungen bereitstellen können

- [Installieren und Konfigurieren von Web deploy für Administratoren und nicht Administrator Bereitstellungen](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.net Site).

<a id="hostingprovider"></a>

## <a name="configuring-servers-for-a-hosting-provider"></a>Konfigurieren von Servern für einen Hostinganbieter

- [Microsoft ASP.NET 4 Hosting-Bereitstellungs Handbuch](https://go.microsoft.com/fwlink/?LinkId=191365) (Microsoft Download Center).
- [Generieren Sie eine XML-Profil Datei](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (IIS.net Site).

<a id="troubleshooting"></a>

## <a name="troubleshooting-deployment-problems"></a>Behandeln von Bereitstellungs Problemen

- [Problembehandlung bei Windows Azure-Websites in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (Microsoft Azure-Website).
- [ASP.net-Webbereitstellung mithilfe von Visual Studio: Problem](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md)Behandlung.
- [Behandeln allgemeiner Probleme mit Web deploy](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Web deploy Fehler Codes](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.net Site).
- Häufig gestellte Fragen [zur Webbereitstellung für Visual Studio und ASP.net](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Grundlegende Unterschiede zwischen IIS und dem ASP.NET Development Server](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Allgemeine Konfigurations Unterschiede zwischen Entwicklungs-und Produktions](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md)Umgebungen.
- [Hosting von ASP.NET-Anwendungen mit mittlerer Vertrauens](http://www.4guysfromrolla.com/articles/100307-1.aspx) Stellung (4 Guys von der Rolla-Website).

<a id="gettinghelp"></a>

## <a name="getting-help-with-a-specific-deployment-question"></a>Hilfe zu einer bestimmten Bereitstellungs Frage

- [ASP.NET Konfiguration und Bereitstellungs Forum](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).

<a id="additional"></a>

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Dieser Abschnitt enthält Links zu zusätzlichen Ressourcen, die für weitere Informationen zur Verwendung von Visual Studio und IIS-Bereitstellungs Tools nützlich sind.

Die folgenden Blogs enthalten häufig Informationen zur Visual Studio-Webbereitstellung:

- [Webentwicklungs Tools im Microsoft-Blog](https://blogs.msdn.com/b/webdevtools/).
- [Der Blog von sahimi Hashimi](http://www.sedodream.com/).

Die folgenden Ressourcen enthalten Dokumentation zu Web deploy, dem IIS-Framework, das von Visual Studio zum Ausführen von Webanwendungs-Projekt Bereitstellungs Aufgaben verwendet wird. Im [Forum für Webbereitstellungs Tools](https://go.microsoft.com/fwlink/?LinkId=149411) auf der IIS.NET-Website können Sie Fragen zu Web deploy stellen.

- [Einführung in Web deploy](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Installieren und Konfigurieren von Web deploy](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [PowerShell-Skripts für die Automatisierung Web deploy-Setups](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Webbereitstellungstool](https://go.microsoft.com/fwlink/?LinkId=151481). Knoten Inhaltsverzeichnis der obersten Ebene für Web deploy Dokumentation auf der TechNet-Website. Enthält hilfreiche Referenzinformationen, aber die meisten TechNet-Seiten wurden seit Jahren nicht aktualisiert.
- [Microsoft. Web. Deployment-Namespace](https://go.microsoft.com/fwlink/?LinkId=148630). Die API-Dokumentation wurde seit Version 1,0 nicht aktualisiert.
- [Der Blog des Microsoft Web Deployment-Teams](https://blogs.iis.net/msdeploy/default.aspx).
- [Registerkarte "veröffentlichen" auf der](https://www.iis.net/learn/publish)Website "IIS.net".
