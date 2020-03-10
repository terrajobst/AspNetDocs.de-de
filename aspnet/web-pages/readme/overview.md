---
uid: web-pages/readme/overview
title: WebMatrix Readme | Microsoft Docs
author: rick-anderson
description: Webmatrix und ASP.net Web Pages (Razor) 1,0 Release-Info
ms.author: riande
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: fac53e935860a90d8f2aa96699d56d66ade3a40f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454269"
---
# <a name="webmatrix-readme"></a>WebMatrix-Infodatei

13. Januar 2011

## <a name="contents"></a>Inhalt

> [!NOTE]
> Diese Info gilt für die Version 1,0 von webmatrix.

- [Übersicht](#Overview)
- [Installation](#Installation_Notes)
- [Veröffentlichen von Anwendungen](#InstructionsForPublishingApplications)
- [Änderungen und Probleme](#ChangesAndIssues)

    - [Webmatrix 1,0-Installation](#Known_Issues_Installation)
    - [ASP.NET-Webseiten 2](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Installieren von Anwendungen](#Known_Issues_Installing_Applications)
    - [Veröffentlichen von Anwendungen](#Known_Issues_Publishing_Applications)
- [Weitere Informationen](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Übersicht

> Microsoft webmatrix 1,0 ist ein kostenloser Webentwicklungs Stapel, der innerhalb von Minuten installiert wird. Es integriert einen Webserver in Datenbank-und Programmier Frameworks, um eine einzelne, integrierte Umgebung zu erstellen. Sie können webmatrix verwenden, um die Art und Weise zu optimieren, wie Sie Ihre eigene ASP.net-oder PHP-Website programmieren, testen und veröffentlichen. Sie können webmatrix auch verwenden, um eine neue Website mithilfe beliebter Open Source-apps wie DotNetNuke, Umbraco, WordPress oder Joomla zu starten. Webmatrix verwendet den gleichen leistungsfähigen Webserver, die gleiche Datenbank-Engine und die gleiche Frame Umgebung, in der Ihre Website im Internet ausgeführt wird, sodass der Übergang von der Entwicklung zur Produktion reibungslos und nahtlos verläuft.

<a id="Installation_Notes"></a>

## <a name="installation"></a>Installation

> Zum Installieren von webmatrix 1,0 müssen Sie zuerst den [Microsoft-Webplattform-Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638)installieren. Nachdem Sie den Webplattform-Installer installiert haben, können Sie ihn zum Installieren von webmatrix verwenden.
> 
> Wenn während der Installation Probleme auftreten, finden Sie weitere Informationen unter Beheben von [Problemen mit Microsoft-Webplattform-Installer](https://go.microsoft.com/fwlink/?LinkId=196212).

<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Veröffentlichen von Anwendungen

> Lesen Sie die [Schritt-für-Schritt-Anleitungen zum Veröffentlichen von Anwendungen](https://go.microsoft.com/fwlink/?LinkID=196149) .

<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Änderungen und Probleme

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>WebMatrix 1.0 Installation Issues

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problem: webmatrix 1,0 ist nur auf Plattformen verfügbar, die Microsoft .NET Framework 4 unterstützen.

> Der .NET Framework Version 4 ist für webmatrix erforderlich. In bestimmten Fällen können Sie mit dem Installationsprogramm von webmatrix 1,0 versuchen, auf einer Plattform zu installieren, die nicht Teil des unterstützten Konfigurations Satzes ist. Insbesondere Windows Vista ohne das SP1-Update ermöglicht Ihnen die Installation von webmatrix, aber die Komponente .NET Framework 4 schlägt fehl, und die Installation wird blockiert.
> 
> **Problemumgehung**  
> Installieren Sie auf einer unterstützten Plattform, die Folgendes umfasst:
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 oder höher
> - Windows XP SP3
> - Windows Server 2003 SP2

#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problem: webmatrix 1,0 kann nicht installiert werden, wenn Microsoft Visual Studio 2008 ohne Microsoft Visual Studio 2008 SP1 installiert ist.

> **Problemumgehung**  
> Installieren Sie [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) aus dem Microsoft Download Center.

#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problem: einige Assemblys für SQL Server Compact 4,0 werden nicht im GAC installiert.

> Die verwalteten Assemblys für SQL Server Compact 4,0 werden nicht im globalen Assemblycache (GAC) abgelegt, wenn Sie SQL Server Compact 4,0 auf einem 64-Bit-Computer installieren und auf dem Computer nur das .NET Framework 3,5 SP1-Client Profil installiert ist. Die verwalteten Assemblys, die nicht im GAC installiert sind, lauten wie folgt:
> 
> - *System. Data. SqlServerCe. dll* (ADO.NET-Anbieter)
> - *System. Data. SqlServerCe. Entity. dll* (ADO.NET Entity Framework)
> 
> **Problemumgehung**  
> Deinstallieren Sie SQL Server Compact 4,0. Laden Sie die vollständige Version von .NET Framework 3,5 SP1 von folgendem Speicherort herunter, und installieren Sie Sie:  
>   
> [Microsoft .NET Framework 3,5 Service Pack 1 (vollständiges Paket)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Installieren Sie dann SQL Server Compact 4,0 neu.

#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problem: SQL Server Compact kann nicht über die Befehlszeile deinstalliert werden.

> Die deinstalstallation von SQL Server Compact mithilfe von Befehlszeilenoptionen funktioniert in dieser Version nicht.
> 
> **Problemumgehung**  
> Deinstallieren Sie Microsoft SQL Server Compact 4,0 mithilfe der Option *Programme und Funktionen* in der Windows-Systemsteuerung.

<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

In diesem Abschnitt des Dokuments werden neue Features, Änderungen und bekannte Probleme mit der 1,0-Version von ASP.net Web Pages mit Razor-Syntax beschrieben.

- [Neue Features](#NewFeatures)
- [Änderungen](#Changes)
- [Probleme](#Issues)

#### <a id="NewFeatures"></a>Neue Features

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Neu: Konfigurationseinstellung zum Deaktivieren des Paket-Managers hinzugefügt

> Für das `<appSettings>`-Element in der Datei " *Web. config* " steht ein neuer `asp:AdminManagerEnabled` Schlüssel zur Verfügung, mit dem Sie den Paket-Manager vollständig deaktivieren können. Der Standardwert für dieses Element ist true. Dies bedeutet, dass der Paket-Manager aktiviert ist, wenn er nicht in der Datei *Web. config* enthalten ist. Fügen Sie der Datei " *Web. config* " im Stammverzeichnis der Website das folgende-Element hinzu, um den Paket-Manager zu deaktivieren:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]

#### <a id="Changes"></a>Erd

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Ändern: der Schlüssel "Webseiten: adminfoldervirtualpath" wurde in "ASP: adminfoldervirtualpath" umbenannt.

> Der `webPages:AdminFolderVirtualPath` Schlüssel, der der Datei " *Web. config* " hinzugefügt werden kann, um den Speicherort des Paket-Managers anzugeben, wurde so umbenannt, dass der `asp:`-Namespace anstelle des `webPages`-Namespace verwendet wird. Wenn Sie dieses Element verwendet haben, müssen Sie es in der Konfigurationsdatei umbenennen.

#### <a id="Issues"></a> Bekannte Probleme

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problem: Kenn Wörter für Mitgliedschafts Benutzer werden nicht mehr erkannt.

> Der Algorithmus zum Erstellen und Speichern von Mitgliedschafts Kennwörtern (Login) wurde geändert, um sicherer zu werden. Folglich werden die Kenn Wörter, die für Mitglieder (Benutzer) gespeichert werden, die in Beta Versionen von ASP.net Razor erstellt wurden, nicht erkannt. 
> 
> Problem **Umgehung** Wenn der Standort noch nicht in der Produktionsumgebung abgelegt wurde, entfernen Sie die Benutzerdaten Sätze aus der Mitgliedschafts Datenbank. Wenn die Datenbank Live ist, generieren Sie in der Mitgliedschafts Datenbank Programm gesteuert vorhandene Kenn Wörter erneut.

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problem: unerwartetes Verhalten bei Verwendung einer benutzerdefinierten Benutzertabelle für die Mitgliedschaft

> Um den Mitgliedschafts Anbieter für eine ASP.net Razor-Website zu initialisieren, müssen Sie die `WebSecurity.InitializeDatabaseConnection`-Methode aufrufen. (In webmatrix enthält die Vorlage Starter Site einen Aufrufen dieser Methode in der *\_AppStart. cshtml* -Datei.) Wenn der `autoCreateTables`-Parameter dieser Methode auf true festgelegt ist (Standardmäßig ist er in der Starter Site-Vorlage auf true festgelegt), und wenn ein unbekannter Tabellenname an die-Methode (der zweite Parameter) übergeben wird, löst die Methode keinen Fehler aus. Stattdessen wird die Tabelle automatisch erstellt.
> 
> Dies kann ein Problem darstellen, wenn Sie beabsichtigen, eine benutzerdefinierte Benutzertabelle für die Mitgliedschaft zu verwenden, aber den falschen Tabellennamen an die `WebSecurity.InitializeDatabaseConnection`-Methode zu übergeben. Da die-Methode nicht standardmäßig einen Fehler verursacht, wenn die angegebene Tabelle nicht vorhanden ist, und da Sie stattdessen eine neue Tabelle erstellt, kann die Anwendung scheinbar funktionieren. Allerdings kann der Anwendungscode, der auf der benutzerdefinierten Benutzertabelle basiert (und in den Feldern), möglicherweise mit unerwarteten Fehlern fehlschlagen.
> 
> **Problemumgehung**  
> Stellen Sie sicher, dass der in der `InitializeDatabaseConnection`-Methode übergebenen Name mit der Benutzerprofil Tabelle in der Mitgliedschafts Datenbank übereinstimmt, oder stellen Sie sicher, dass der `autoCreateTables`-Parameter auf false festgelegt ist.

#### <a name="issue-error-message-the-admin-module-requires-access-to-app_data"></a>Problem: Fehlermeldung "das Administrator Modul benötigt Zugriff auf ~/App\_Daten"

> Unter bestimmten Umständen kann der Versuch, Benutzer zu erstellen oder anderweitig mit dem ASP.NET-Mitgliedschaftssystem zu arbeiten, dazu führen, dass auf der Seite der Fehler angezeigt wird, *den das Admin-Modul benötigt, um ~/App\_Daten* Dies tritt auf, wenn das Konto, unter dem IIS oder IIS Express ausgeführt wird, nicht über Berechtigungen zum Erstellen und Schreiben in den *App-\_Daten* Ordner unter dem Stammverzeichnis der Website verfügt. 
> 
> Problem **Umgehung** Erstellen Sie manuell einen *App-\_Daten* Ordner für die Website. Stellen Sie dann sicher, dass das Windows-Konto, unter dem die Anwendung ausgeführt wird (in der Regel Netzwerkdienst), über Lese-/Schreibberechtigungen für Stamm Ordner der Anwendung und für Unterordner wie App\_-Daten verfügt. Ausführlichere Informationen finden Sie im Knowledge Base [-Artikel Probleme mit SQL Server Express benutzerinstancing-und ASP.NET-Webanwendungs Projekten](https://support.microsoft.com/kb/2002980).

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problem: "Fehler beim Generieren einer Benutzer Instanz von SQL Server".

> Wenn eine webmatrix-Webanwendung SQL Server Express verwendet und IIS 7,5 unter Windows 7 oder Windows Server 2008 R2 ausgeführt wird, wird möglicherweise ein Fehler angezeigt, der darauf hinweist, dass SQL Server den lokalen Anwendungspfad des Benutzers zur Laufzeit nicht abrufen kann.
> 
> Problem **Umgehung** Stellen Sie sicher, dass das Windows-Konto, unter dem die Anwendung ausgeführt wird (in der Regel Netzwerkdienst), über Lese-/Schreibberechtigungen für Stamm Ordner der Anwendung und für Unterordner wie *App\_-Daten*verfügt. Ausführlichere Informationen finden Sie im Knowledge Base [-Artikel Probleme mit SQL Server Express benutzerinstancing-und ASP.NET-Webanwendungs Projekten](https://support.microsoft.com/kb/2002980).

#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problem: Dateien, die Paket-Manager-Ressourcen oder Paket-Manager-Kenn Wörter enthalten, sind unter IIS 6,0 und früher verwendbar.

> Wenn Sie eine ASP.net Web Pages (Razor)-Anwendung bereitstellen, die mit der RC2-Version erstellt wurde, und die Anwendung eine Datei " *Password. txt* " oder " *PackageSources. txt* " unter */App\_data/admin*enthält, wird die Datei von IIS 6,0 bereitgestellt, und die Kenn Wörter für die Paket-Manager-Instanz werden möglicherweise verfügbar gemacht. 
> 
> Problem **Umgehung** Benennen Sie die Datei " *Password. txt* " oder " *PackageSources. txt* " in *Password. config* oder *PackageSources. config*um. Standardmäßig verarbeitet IIS 6,0 keine Dateien, die über die Erweiterung " *. config* " verfügen. (In IIS 7 werden keine Dateien in der *App\_Daten* Ordner verarbeitet, sodass Sie die Dateien nicht umbenennen müssen.)

#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problem: das Deinstallieren von Paketen, die mit der Beta 3-Version installiert wurden, entfernt Paket Komponenten nicht vollständig

> Wenn Sie ein Paket mit dem Paket-Manager in der Beta 3-Version installiert haben und dann versuchen, es mit der aktuellen Version zu deinstallieren, wird das Paket nicht vollständig deinstalliert. Mithilfe der Schaltfläche **deinstallieren** des Paket-Managers werden einige Komponenten entfernt, aber der Bibliotheks Code des Pakets bleibt erhalten, und die Datei " *Package. config* " wird nicht aktualisiert.
> 
> Problem **Umgehung**   
> Führen Sie die folgenden Schritte aus:  
> 1. Löschen Sie die *App\_Ordner "data\packages* ". Dadurch werden alle Pakete entfernt.   
> 2. Löschen Sie die Datei " *Packages. config* " im Stammverzeichnis der Website.

#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problem: in Visual Studio wird die Anwendung durch Aufrufen des webbasierten Paket-Managers offline geschaltet.

> Wenn Sie in Visual Studio (nicht in webmatrix) arbeiten und die *\_admin* -Funktion verwenden, um den Paket-Manager zu starten, wird die Anwendung von Visual Studio offline geschaltet und die *App\_offline. htm* in das Stammverzeichnis der Website übermittelt, wodurch Sie die Verwendung des Paket-Managers stören können.
> 
> [!NOTE]
> Obwohl dieses Verhalten in der Regel bei Verwendung der webbasierten Paket-Manager-Schnittstelle angezeigt wird, tritt das gleiche Verhalten auf, wenn Sie Dateien im *App-\_Daten* Ordner hinzufügen, entfernen oder ändern.
> 
> Problem **Umgehung**   
> Verwenden Sie die nuget-Erweiterung anstelle des webbasierten Paket-Managers, um mit Paketen in Visual Studio zu arbeiten. Weitere Informationen finden Sie in der [nuget-Dokumentation](https://docs.microsoft.com/nuget/). Wenn Sie mit anderen Dateien im Ordner *App\_Data* arbeiten, sollten Sie die Dateien in Erwägung behalten, um dieses Problem zu vermeiden. Wenn dies nicht praktikabel ist, löschen Sie die *App\_offline. htm* -Datei manuell, oder warten Sie, bis die Website automatisch wieder online geschaltet wird (standardmäßig nach 30 Sekunden).

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problem: Visual Studio IntelliSense und Projektvorlagen sind nur in ASP.NET MVC Version 3 verfügbar.

> Bei der Installation von ASP.net Web Pages werden nicht auch Tools für Visual Studio installiert, z. b. IntelliSense und Projektvorlagen für ASP.net Web Pages Anwendungen.
> 
> Problem **Umgehung** Wenn Sie IntelliSense und Projektvorlagen für ASP.net Web Pages Anwendungen in Visual Studio verwenden möchten, installieren Sie ASP.NET MVC 3 RC entweder über den Webplattform-Installer oder über das [eigenständige Installations](https://go.microsoft.com/fwlink/?LinkID=191797)Programm.

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problem: Lesen von Feeds oder anderen externen Daten über einen Proxy Server

> Wenn sich der Server, auf dem der Standort ausgeführt wird, hinter einem Proxy Server befindet, müssen Sie möglicherweise Proxy Informationen in der Datei " *Web. config* " konfigurieren, damit Sie Informationen lesen können, die von außerhalb Ihres Standorts stammen. Wenn Sie z. b. das `ReCaptcha`-Hilfsprogramm verwenden, kommuniziert das Hilfsprogramm mit dem Dienst "reCAPTCHA", kann jedoch vom Proxy Server blockiert werden. Analog dazu kann für Feeds, die in ASP.net Web Pages verwendet werden, wie z. b. der vom Paket-Manager verwendete Feed, eine Proxykonfiguration erforderlich sein.
> 
> Wenn beim Arbeiten mit einem externen Dienst oder beim Arbeiten mit dem paketfeed Probleme auftreten, platzieren Sie die folgenden Elemente in der *Web. config* -Stammdatei Ihrer Anwendung:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Weitere Informationen zum Konfigurieren eines Proxy Servers finden Sie unter [&lt;Proxy&gt;-Element (Netzwerkeinstellungen)](https://msdn.microsoft.com/library/sa91de1e.aspx) auf der MSDN-Website.

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problem: beim Deinstallieren des .NET Framework Version 4 wird ASP.net Web Pages mit Razor-Syntax deaktiviert.

> Wenn Sie die .NET Framework Version 4 deinstallieren und dann neu installieren, ist ASP.net Web Pages mit Razor-Syntax deaktiviert. Seiten mit der Erweiterung *. cshtml* werden nicht ordnungsgemäß ausgeführt. ASP.net Web Pages eine Assembly in der *Web. config* -Stammdatei des Computers registrieren, und durch das Entfernen des .NET Framework wird diese Datei entfernt. Beim erneuten Installieren des .NET Framework wird eine neue Version der Konfigurationsdatei installiert, aber der Verweis für die ASP.net Web Pages Assembly wird nicht hinzugefügt.
> 
> Problem **Umgehung** Installieren Sie nach der erneuten Installation des .NET Framework ASP.net Web Pages mit Razor-Syntax neu. Dadurch wird der Datei " *Web. config* " im Stammverzeichnis des Computers Folgendes Element hinzugefügt, das sich in der Regel an folgendem Speicherort befindet:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]

#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problem: Erweiterungen ohne Erweiterung finden keine cshtml/vbhtml-Dateien in IIS 7 oder IIS 7,5.

> Bei IIS 7 oder IIS 7,5 können Anforderungen mit einer URL wie den folgenden keine Seiten finden, die die Erweiterung " *. cshtml* " oder " *. vbhtml* " aufweisen:  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> Das Problem tritt auf, weil das Umschreiben von URLs für IIS 7 oder IIS 7,5 nicht standardmäßig aktiviert ist. Das wahrscheinlichste Szenario besteht darin, dass das Problem beim lokalen testen mithilfe IIS Express nicht angezeigt wird, Sie es jedoch beim Bereitstellen Ihrer Website auf einer Hostingwebsite erleben.
> 
> **Problemumgehung**
> 
> - Wenn Sie die Kontrolle über den Server Computer haben, installieren Sie auf dem Server Computer das in einem Update beschriebene Update, [das bestimmten IIS 7,0-oder IIS 7,5-Handlern ermöglicht, Anforderungen zu verarbeiten, deren URLs nicht mit einem Punkt enden](https://support.microsoft.com/kb/980368).
> - Wenn Sie keine Kontrolle über den Server Computer haben (z. b. die Bereitstellung auf einer Hostingwebsite), fügen Sie der Datei *Web. config* der Website Folgendes hinzu: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problem: Bereitstellen einer Anwendung auf einem Computer, auf dem SQL Server Compact nicht installiert ist

> Anwendungen, die SQL Server Compact-Datenbanken enthalten, können auf einem Computer ausgeführt werden, auf dem SQL Server Compact nicht installiert ist. Microsoft webmatrix 1,0 kopiert diese Binärdateien automatisch für Sie und führt die entsprechenden *Web. config* -Datei Transformationen aus.
> 
> Problem **Umgehung** Wenn Sie diese Dateien kopieren und die Datei " *Web. config* " manuell ändern müssen, gehen Sie wie folgt vor:
> 
> 1. Kopieren Sie die Datenbank-Engine-Assemblys in den Ordner *bin* (und die Unterordner) der Anwendung auf dem Zielcomputer:  
> 
>    - Kopieren Sie *c:\Programme\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>      **in** *\bin*
>    - Kopieren Sie *c:\Programme\Microsoft SQL Server Compact edition\v4.0\private\x86\\* **in** *\bin\x86*
>    - Kopieren Sie *c:\Programme\Microsoft SQL Server Compact edition\v4.0\private\amd64\\* * **in** *\bin\amd64*
> 
> 2. Erstellen oder öffnen Sie eine *Web. config* -Datei im Stamm Ordner der Website. (In webmatrix 1,0 ist dieser Dateityp verfügbar, wenn Sie im Dialogfeld **Dateityp auswählen** auf **alle** klicken.)
> 3. Fügen Sie das folgende-Element als untergeordnetes Element des `<configuration>`-Elements hinzu (nicht innerhalb des `<system.web>`-Elements):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problem: die Hilfsprogramme "Database" und "WebGrid" funktionieren in Visual Basic mit mittlerer Vertrauensstellung nicht.

> Wenn Sie Visual Basic verwenden ( *vbhtml* -Dateien werden erstellt), funktionieren die `Database`-und `WebGrid`-Hilfsprogramme nicht, wenn die Anwendung für die Verwendung von mittlerer Vertrauensstellung festgelegt ist.
> 
> **Problemumgehung**  
> Wenn Sie Visual Studio 2010 verwenden, können Sie dieses Problem beheben, indem Sie die Service Pack 1-Version installieren. Bis die endgültige Version der SP1-Version verfügbar ist, können Sie die Beta Version von SP1 von der Beta-Version von [Microsoft Visual Studio 2010 Service Pack 1](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) im Microsoft Download Center herunterladen.   
>   
> Wenn dies nicht praktikabel ist, oder wenn Sie Visual Studio 2010 nicht verwenden, können Sie die Anwendung vorübergehend für die Verwendung voller Vertrauenswürdigkeit festlegen.

#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problem: "applicationpart"-Ressourcen sind extern zugänglich.

> Wenn eine Assembly-Objekte enthält, die von der `ApplicationPart`-Klasse abgeleitet sind, werden die Ressourcen dieser Assembly von der `ResourceRouteHandler`-Klasse verfügbar gemacht. Nehmen wir beispielsweise die folgende URL:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Mit dieser Anforderung werden alle Ressourcen Zeichenfolgen in der Assembly " *System. Web. Webseiten. Administration. dll* " heruntergeladen. Alle eingebetteten Ressourcen (auch solche, die nicht als statischer Inhalt dienen sollen) werden heruntergeladen. Wenn die eingebetteten Ressourcen vertrauliche Informationen enthalten, kann dies ein Sicherheitsrisiko darstellen. 
> 
> Problem **Umgehung**   
> Wenn Sie ein **applicationpart** -Objekt erstellen, stellen Sie sicher, dass die eingebetteten Ressourcen, die der Assembly des **applicationpart** -Objekts zugeordnet sind, keine vertraulichen Informationen enthalten.

<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Informationen zu Installationsproblemen für webmatrix finden Sie weiter oben in diesem Dokument unter [webmatrix-Installationsprobleme](#Known_Issues_Installation) .

In diesem Abschnitt des Dokuments werden bekannte Probleme für die webmatrix-Entwicklungsumgebung beschrieben.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problem: Änderungen am Benutzernamen oder Kennwort einer Daten bankverbindungs Zeichenfolge in einer Web. config-Datei werden im Arbeitsbereich "Datenbanken" nicht widergespiegelt.

> **Problemumgehung**  
> 
> 1. Ändern Sie in der Datei " *Web. config* " den Datenbanknamen in der Verbindungs Zeichenfolge (fügen Sie z. b. "1" hinzu).
> 2. Speichern Sie die Datei " *Web. config* ".
> 3. Klicken Sie auf **Datenbanken** und aktualisieren.
> 4. Ändern Sie den Datenbanknamen in der Verbindungs Zeichenfolge in der Datei " *Web. config* " zurück in den ursprünglichen Datenbanknamen.
> 5. Speichern Sie die Datei " *Web. config* ".
> 6. Klicken Sie auf **Datenbanken** und aktualisieren.

#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problem: die von webmatrix erstellten Ordner können nicht gelöscht werden.

> Wenn webmatrix mit erweiterten Berechtigungen ausgeführt wird (d. h., Sie haben webmatrix mithilfe der Option **als Administrator ausführen** in Windows gestartet), können Ordner, die von webmatrix erstellt wurden, nicht mithilfe von Windows-Explorer gelöscht werden.
> 
> **Problemumgehung**  
> Führen Sie Windows-Explorer mit erhöhten Berechtigungen aus. Folgen Sie diesen Schritten:  
> 
> 1. Klicken Sie unter Windows auf **Start**.
> 2. Geben Sie "Windows Explorer" ein, und klicken Sie mit der rechten Maustaste auf den Eintrag für **Windows Explorer**.
> 3. Klicken Sie auf **als Administrator ausführen**. Anschließend können Sie die Ordner löschen.

#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problem: webmatrix 1,0 kann bestimmte Tasks, für die eine Erhöhung erforderlich ist, nicht ausführen.

> Webmatrix 1,0 kann bestimmte Tasks, für die eine Erhöhung erforderlich ist, nicht ausführen, wie z. b. die Installation zusätzlicher Komponenten in den folgenden Situationen:
> 
> - Unter Windows Vista oder Windows 7 sind Sie mit einem Konto angemeldet, das nicht über Administratorrechte verfügt, und die Benutzerkontensteuerung (User Account Control, UAC) ist deaktiviert.
> - Sie verwenden Microsoft Windows XP oder Microsoft Windows Server 2003.
> 
> **Problemumgehung**  
> Für die meisten Aufgaben in webmatrix 1,0 ist keine Administrator Berechtigung erforderlich. Für solche, die dies tun, können Sie den Vorgang als Administrator ausführen oder die folgenden Schritte ausführen:
> 
> - Aktivieren Sie UAC unter Windows Vista oder Windows 7.
> - Fügen Sie unter Windows XP den Benutzer der Sicherheitsgruppe Administratoren hinzu.

#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problem: "Site from Web Gallery" ist deaktiviert.

> Die Option **Site from Web Gallery** ist deaktiviert, wenn der Webplattform-Installer 3,0 nicht installiert ist.
> 
> **Problemumgehung**  
> Installieren Sie den [Microsoft-Webplattform-Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).

#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problem: Google Chrome ist nicht als Run-Option verfügbar.

> Google Chrome wird in der Liste der Browser, die auf der Registerkarte **Home** unter **Run** angezeigt werden, nicht angezeigt.
> 
> **Problemumgehung**  
> Einige Versionen von Google Chrome registrieren sich nicht korrekt mit dem Feature "Standardprogramme" in Windows. Um dieses Problem zu umgehen, starten Sie Google Chrome, klicken Sie auf das Menü *anpassen und Steuern von Google Chrome* , klicken Sie auf *Optionen*, und klicken Sie dann auf *Google Chrome My Default Browser*.

#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problem: das Eingeben eines Primärschlüssels durch das Dialogfeld "Fremdschlüssel" ist nicht zulässig.

> Im Dialogfeld **Fremdschlüssel** können Sie den Primärschlüssel Namen aus der Primärschlüssel Tabelle nicht eingeben.
> 
> **Problemumgehung**  
> Dies ist beabsichtigt. Der Name des Primärschlüssels muss nicht aus der Primärschlüssel Tabelle eingegeben werden.

#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problem: IntelliSense ist in webmatrix für Razor-Syntax-, C#-oder-Visual Basic nicht verfügbar.

> IntelliSense wird in webmatrix für HTML und CSS unterstützt. Es ist jedoch nicht für andere Sprachen verfügbar. 
> 
> Problem **Umgehung**   
> Keine.

#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problem: IntelliSense für HTML und CSS schlägt Elemente vor, die sich nicht am besten eignen.

> IntelliSense für Markup in webmatrix unterstützt HTML mit dem [XHTML 1,0-Übergangs Schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) und CSS unter Verwendung des [CSS 2,1-Schemas](http://www.w3.org/TR/CSS2/). Da IntelliSense auf diesen spezifischen Schemas basiert, können bestimmte Tags, Attribute oder Eigenschaften vorgeschlagen werden, die für die aktuelle Seiten-oder Format Definition nicht geeignet sind. Für HTML kann es auch zu unerwarteten Vorschlägen in Inhalten führen, die möglicherweise als falsch formatierte XHTML interpretiert werden (z. b. Wenn Tags nicht geschlossen werden). Dieses Problem ist möglicherweise auffälliger, wenn sich die Einfügemarke innerhalb eines unvollständigen Tags befindet. in diesem Fall kann IntelliSense neue öffnende Tags vorschlagen oder andere falsche Vorschläge anbieten. 
> 
> Problem **Umgehung**   
> Stellen Sie für HTML sicher, dass Sie in einer wohlgeformten, abgeschlossenen XHTML-Seite arbeiten. Für CSS gibt es keine Problem Umgehung.

#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problem: IntelliSense wird nicht aufgerufen, wenn Sie Folgendes eingeben:

> Manchmal wird IntelliSense möglicherweise nicht aufgerufen, weil HTML oder CSS im Editor eingegeben wird. Dies kann insbesondere der Fall sein, wenn die Einfügemarke direkt neben einem anderen Element oder am Ende einer Datei liegt. 
> 
> Problem **Umgehung**   
> Stellen Sie sicher, dass die Einfügemarke leer ist und die Einfügemarke nicht am Ende einer Datei liegt. Sie können IntelliSense auch manuell aufrufen, indem Sie STRG + LEERTASTE drücken.

#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problem: Es ist keine Benutzeroberfläche zum Deaktivieren von IntelliSense verfügbar.

> Webmatrix 1,0 bietet keine Benutzeroberfläche oder Geste zum Deaktivieren von IntelliSense. 
> 
> Problem **Umgehung**   
> Starten Sie webmatrix mit dem folgenden Befehl, der einen Schalter enthält, der IntelliSense deaktiviert:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`

<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express verfügt über eine eigene Infodatei, die unter der folgenden URL verfügbar ist:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact verfügt über eine eigene Infodatei, die unter der folgenden URL verfügbar ist:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Informationen zu Problemen, die das Installieren von SQL Server Compact als Teil von webmatrix einschließen, finden Sie weiter oben in diesem Dokument unter [webmatrix-Installationsprobleme](#Known_Issues_Installation) .

### <a id="Known_Issues_Installing_Applications"></a>Installieren von Anwendungen

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problem: das Installieren einer Anwendung kann einige Zeit in Anspruch nehmen, wenn der Ordner "eigene Dateien" des Benutzers an eine Netzwerkfreigabe umgeleitet wird.

> **Problemumgehung**  
> Keine. Die Installation der Anwendung kann einige Zeit in Anspruch nehmen, wird aber ordnungsgemäß installiert.

### <a id="Known_Issues_Publishing_Applications"></a>Veröffentlichen von Anwendungen

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problem: Fehler "erforderliche Berechtigungen können nicht abgerufen werden" beim Veröffentlichen einer SQL Compact-Datenbank

> Webmatrix unterstützt die Bereitstellung unterstützender Binärdateien für SQL Server Compact auf einem Server mit .NET Framework Version 3,5 mit einer mittleren Vertrauens Konfiguration nicht vollständig.
> 
> **Problemumgehung**  
> Die bevorzugte Problem Umgehung besteht darin, die .NET Framework 4 auf dem Server zu installieren. Führen Sie alternativ die folgenden Schritte aus:
> 
> 1. Fügen Sie dem Abschnitt "`SecurityClasses`" in der Datei " *Web\_Medium Trust. config* " die folgenden Elemente hinzu:
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Erstellen Sie einen neuen Berechtigungs Satz in der Datei " *Web\_Medium Trust. config* " mit den folgenden erforderlichen Berechtigungen:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Wenden Sie den Berechtigungs Satz auf SQL Server Compact an, indem Sie die folgenden Elemente in die Datei " *Web\_mediumschlag Trust. config* " einfügen:
> 
>     [!code-html[Main](overview/samples/sample8.html)]

#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problem: im Katalog und in den phpBB-Webanwendungen wird der Fehler "der Dienst ist nicht verfügbar" angezeigt.

> Unter bestimmten Umständen verursacht das Veröffentlichen einer Anwendung den Fehler "der Dienst ist nicht verfügbar".
> 
> **Problemumgehung**  
> Fügen Sie in webmatrix einen umgekehrten Schrägstrich (\) am Ende des Server namens im Fenster **Veröffentlichungs Einstellungen** hinzu, und veröffentlichen Sie die Anwendung dann erneut.

#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problem: das Moodle-Website Layout und die Verknüpfungen werden nach der Veröffentlichung getrennt

> Nachdem Sie eine Moodle-Anwendung veröffentlicht haben, funktioniert die Anwendung nicht ordnungsgemäß.
> 
> **Problemumgehung**  
> Fügen Sie in webmatrix am Ende des Felds " **Website Name** " im Fenster " **Veröffentlichungs Einstellungen** " einen Schrägstrich (/) hinzu, und veröffentlichen Sie die Anwendung dann erneut.

#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problem: bei der Veröffentlichung von nopcommerce tritt ein Datenbankfehler auf

> Das Veröffentlichen von nopcommerce schlägt fehl und meldet einen Datenbankfehler wie "Insert Into the NOP\_log table failed".
> 
> **Problemumgehung**  
> 
> 1. Klicken Sie in webmatrix auf **Ausführen** , um nopcommerce lokal zu starten.
> 2. Melden Sie sich auf der Verwaltungsseite an.
> 3. Klicken Sie auf das Menü **System** .
> 4. Klicken Sie auf die Option **Protokoll** .
> 5. Klicken Sie auf die Schaltfläche **Protokoll löschen** .
> 6. Veröffentlichen Sie nopcommerce erneut.

#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problem: SilverStripe CMS zeigt beim Herunterladen einer veröffentlichten Website einen "http 500 PHP-Fehler in der Fehlermeldung" an.

> **Problemumgehung**  
> Nachdem Sie auf **veröffentlichte Website herunterladen**klicken, überspringen Sie `silverstripe-cache/manifest_main` in der **Veröffentlichungs Vorschau**. Diese Datei dient zum Zwischenspeichern und ist für jeden Computer spezifisch.

#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problem: der unter Text zeigt "Server Fehler in '/' Anwendung '" an, wenn Sie eine veröffentlichte Website herunterladen.

> **Problemumgehung**  
> Öffnen Sie die Datei *Web. config* der Website, und ersetzen Sie die Benutzer-ID und das Kennwort in der Daten bankverbindungs Zeichenfolge durch die SQL Server Administrator Anmelde Informationen (die Anmelde Informationen "SA").
> 
> Alternativ können Sie die folgenden Schritte ausführen, um dem Benutzerkonto, mit dem Sie angemeldet sind, `db_owner` Berechtigungen zu übergeben:
> 
> 1. Installieren Sie SQL Server Management Studio mithilfe des Webplattform-Installers.
> 2. Stellen Sie eine Verbindung mit der lokalen SQL Server Express Instanz her (standardmäßig `.\SQLEXPRESS`).
> 3. Klicken Sie auf **Datenbanken** &gt; *[localsubtextdatabase]* &gt; **Sicherheit** &gt; **Benutzer** &gt; *[localsubtextuser*] (standardmäßig `subtextuser`], klicken Sie mit der rechten Maustaste, und klicken Sie auf **Eigenschaften**.
> 4. Wählen Sie im Abschnitt Rollen Mitgliedschaft die Option **DB-\_Besitzer** aus.

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problem: die Site funktioniert nach der Veröffentlichung möglicherweise nicht, wenn das Feld "Ziel-URL" nicht mit http://oder https://vorangestellt ist.

> Wenn die Ziel-URL im Dialogfeld **Veröffentlichungs Einstellungen** nicht mit `http://` oder `https://`beginnt, funktioniert die Website möglicherweise nach der Bereitstellung nicht.
> 
> **Problemumgehung**  
> Vergewissern Sie sich, dass die Ziel-URL im Dialogfeld **Veröffentlichungs Einstellungen** mit `http://` oder `https://`gestartet wird, bevor Sie eine Website veröffentlichen.

#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problem: das Veröffentlichen einer MySQL-Datenbank schlägt mit dem Fehler "Fehler beim Veröffentlichen der Datenbank fehl. Dies kann vorkommen, wenn das Skript von der Remote Datenbank nicht ausgeführt werden kann. "

> Der Fehler kann aus verschiedenen Gründen auftreten. Ein Grund, warum Sie diesen Fehler sehen können, ist, wenn das Datenbankskript ein einzelnes Anführungszeichen (') enthält und der Standard Zeichensatz der MySQL-Zieldatenbank nicht UTF-8 ist.
> 
> **Problemumgehung**  
> Legen Sie den Standardzeichensatz für die MySQL-Remote Datenbank auf UTF-8 fest.

#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problem: einige Links sind nach dem veröffentlichen oder Herunterladen der Website in DotNetNuke nicht sichtbar.

> Wenn Sie eine DotNetNuke-Website veröffentlichen oder herunterladen, müssen Sie möglicherweise den Cache löschen, damit die neuen Links auf der Website angezeigt werden.
> 
> **Problemumgehung**
> 
> 1. Melden Sie sich als "Host" an.
> 2. Wechseln Sie zum Hostmenü, und wählen Sie **Host Einstellungen**aus.
> 3. Scrollen Sie nach unten, und erweitern Sie unter **Erweiterte Einstellungen**die Option **Leistungseinstellungen**.
> 4. Klicken Sie auf den Link **Cache löschen** für Seiten.
> 5. Wechseln Sie zum unteren Rand der Seite, und starten Sie die Anwendung neu.

#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problem: einige Links in atomsite sind nach dem Herunterladen einer veröffentlichten Website beschädigt.

> **Problemumgehung**  
> Ersetzen Sie in der Datei " *Service. config* ", der Datei " *users. config* " und allen *XML* -Dateien die URL-Zeichenfolge (z. b. `http://myhost.com/atomsite`) durch die lokale Datei (z. b. `http://localhost:1239`).

#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problem: MySQL-basierte Anwendungen wie WordPress können nicht veröffentlicht werden und melden einen Datenbankfehler.

> Standardmäßig installiert webmatrix MySQL mit dem UTF-8-Zeichensatz. Wenn Sie MySQL eigenständig installieren und der Zeichensatz nicht UTF-8 ist (z. b. latin1), kann der Veröffentlichungsprozess für Datenbanken fehlschlagen.
> 
> **Problemumgehung**
> 
> 1. Ändern Sie den Zeichensatz für MySQL in UTF-8. (Ausführliche Informationen finden Sie unter [Server Character Set and collations](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) auf der MySQL-Website.)
> 2. Installieren Sie die Anwendung neu.
> 3. Veröffentlichen Sie die Anwendung erneut.

#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problem: "Herunterladen der veröffentlichten Website" schlägt fehl, wenn Anwendungen mit Browser basierter Installation ausgeführt werden.

> Einige Anwendungen (z. b. Kentico CMS) erfordern, dass Sie Sie im Browser starten, um das Setup nach der Installation auszuführen, z. b. das Erstellen einer Datenbank. Wenn Sie eine Anwendung wie diese veröffentlichen, ohne das browserbasierte Setup abzuschließen, tritt beim Versuch, dieselbe Website von einem Remote Server herunterzuladen, ein Fehler auf.
> 
> **Problemumgehung**  
> Beenden Sie das browserbasierte Setup, bevor Sie den Standort veröffentlichen.

#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problem: "Herunterladen der veröffentlichten Site" schlägt mit einem Datenbankfehler für DotNetNuke und kooboo CMS fehl

> Wenn Sie versuchen, eine Anwendung von einem Server herunterzuladen, und Sie im Dialogfeld " **Veröffentlichungs Einstellungen** " in der Daten bankverbindungs Zeichenfolge über Administrator Anmelde Informationen verfügen, wird möglicherweise der folgende Fehler im Veröffentlichungs Protokoll angezeigt:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Problemumgehung**  
> Wenn dies praktikabel ist, veröffentlichen Sie die Website erneut (oder veröffentlichen Sie Sie), indem Sie für die Datenbank nicht Administrator-Anmelde Informationen verwenden.

<a id="More_Info"></a>

## <a name="for-more-information"></a>Weitere Informationen

Weitere Informationen zu webmatrix 1,0 finden Sie auf den folgenden Websites:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Alle Rechte vorbehalten. [Nutzungsbedingungen](https://msdn.microsoft.cos/cc300389.aspx).
