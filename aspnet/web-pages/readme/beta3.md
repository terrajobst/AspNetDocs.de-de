---
uid: web-pages/readme/beta3
title: Info zum Release von Web Matrix und ASP.net Web Pages (Razor) Beta 3 | Microsoft-Dokumentation
author: rick-anderson
description: Infodatei für das Release WebMatrix und ASP.NET Web Pages (Razor) Beta 3
ms.author: riande
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: dc1d9237c04a7fcdbf4db6ccc8c36d255f6de003
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510339"
---
# <a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Infodatei für das Release WebMatrix und ASP.NET Web Pages (Razor) Beta 3

> Infodatei für das Release WebMatrix und ASP.NET Web Pages (Razor) Beta 3

9\. November 2010

## <a name="contents"></a>Inhalt

- [Übersicht](#Overview)
- [Installation](#Installation_Notes)
- [Neue Features, Änderungen und bekannte Probleme in der Beta 3-Version](#Known_Issues)

    - [Webmatrix-Installationsprobleme](#Known_Issues_Installation)
    - [ASP.NET-Webseiten 2](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Installieren von Anwendungen](#Known_Issues_Installing_Applications)
    - [Veröffentlichen von Anwendungen](#Known_Issues_Publishing_Applications)
    - [Andere Probleme](#Known_Issues_Other_Issues)
- [Weitere Informationen](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Übersicht

> Microsoft webmatrix Beta ist ein kostenloser Webentwicklungs Stapel, der innerhalb von Minuten installiert wird. Es integriert einen Webserver in Datenbank-und Programmier Frameworks, um eine einzelne, integrierte Umgebung zu erstellen. Sie können webmatrix Beta verwenden, um die Art und Weise zu optimieren, wie Sie Ihre eigene ASP.net-oder PHP-Website programmieren, testen und veröffentlichen, oder Sie können webmatrix Beta verwenden, um eine neue Website mithilfe beliebter Open Source-apps wie DotNetNuke, Umbraco, WordPress oder Joomla zu starten. Webmatrix Beta verwendet den gleichen leistungsfähigen Webserver, die gleiche Datenbank-Engine und die gleiche Frameworks-Umgebung, die Ihre Website im Internet ausführen, sodass der Übergang von der Entwicklung zur Produktion reibungslos und nahtlos verläuft.

<a id="Installation_Notes"></a>

## <a name="installation"></a>Installation

> Zum Installieren von webmatrix Beta 3 verwenden Sie [Microsoft-Webplattform-Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638). Nachdem Sie den Webplattform-Installer installiert haben, können Sie ihn verwenden, um webmatrix Beta 3 zu installieren.
> 
> Wenn während der Installation Probleme auftreten, finden Sie weitere Informationen unter Beheben von [Problemen mit Microsoft-Webplattform-Installer](https://go.microsoft.com/fwlink/?LinkId=196212).

<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Anweisungen zum Veröffentlichen von Anwendungen

> Lesen Sie die [Schritt-für-Schritt-Anleitungen zum Veröffentlichen von Anwendungen](https://go.microsoft.com/fwlink/?LinkID=196149) .

<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Neue Features, Änderungen und bekannte Probleme

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Webmatrix Beta 3-Installation

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problem: webmatrix Beta 3 ist nur auf Plattformen verfügbar, die Microsoft .NET Framework 4 unterstützen.

> Der .NET Framework Version 4 ist für die webmatrix-Beta Version erforderlich. In bestimmten Fällen können Sie mit dem webmatrix-Beta-Installer versuchen, auf einer Plattform zu installieren, die nicht Teil des unterstützten Konfigurations Satzes ist. Insbesondere Windows Vista ohne das SP1-Update ermöglicht Ihnen die Installation von webmatrix Beta, aber die Komponente .NET Framework 4 schlägt fehl, und die Installation wird blockiert.
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

#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problem: webmatrix Beta 3 kann nicht installiert werden, wenn Microsoft Visual Studio 2008 ohne Microsoft Visual Studio 2008 SP1 installiert ist.

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

In diesem Abschnitt des Dokuments werden neue Features, Änderungen und bekannte Probleme mit der Beta 3-Version von ASP.net Web Pages mit Razor-Syntax beschrieben.

- [Neue Features](#NewFeatures)
- [Änderungen](#Changes)
- [Probleme](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Neue Features in Beta 3 für ASP.net Web Pages mit Razor-Syntax

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Neu: "HTML. RAW"-Methode rendert nicht codiertes Markup

> Die neue `Html.Raw`-Methode ermöglicht das Rendern von HTML-Markup als Markup anstelle der Ausgabe der codierten Ausgabe. (Standardmäßig codiert ASP.net Razor Zeichen folgen vor dem Rendern.) Die Syntax lautet wie folgt:
> 
> `Html.Raw(value)`
> 
> Das folgende Beispiel veranschaulicht die Verwendung von `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]

<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Änderungen in Beta 3 für ASP.net Web Pages mit Razor-Syntax

#### <a name="change-hrefattribute-method-removed"></a>Änderung: die Methode "hrefattribute" wurde entfernt.

> Die `HrefAttribute`-Methode der `WebPage`-Klasse wurde entfernt. Dieses Hilfsprogramm wurde verwendet, um unsichere Zeichen in URLs zu codieren. Dies ist nicht mehr erforderlich, da ASP.net Razor automatisch Zeichen folgen codiert. (Verwenden Sie die neue `Html.Raw`-Methode, um nicht codierte Zeichen folgen zu erzeugen.)

#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Änderung: Syntax für deklarative "@helper"-Hilfsprogramme geändert

> In der Beta 3-Version ändert ASP.net, wie die mit der `@helper`-Syntax erstellten Hilfsprogramme analysiert werden. Im Wesentlichen wird die `@helper`-Syntax nun als Codeblock anstelle von als Markup Block analysiert, der Code enthalten kann. Daher muss Code im Hilfsprogramm nicht in `@{ }` Blöcke eingeschlossen werden. Umgekehrt muss Markup innerhalb des Hilfsprogramms explizit in HTML-Elemente oder ASP.net Razor-`<text></text>` Tags eingeschlossen werden.
> 
> Beispielsweise funktioniert die folgende `@helper` Syntax in der Beta 3-Version:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> In der Beta 3-Version muss dieses Hilfsprogramm so geändert werden, dass es wie im folgenden Beispiel aussieht:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Beachten Sie, dass die `@{ }` Zeichen um den ursprünglichen Code im Hilfsprogramm nicht mehr verwendet werden. Dies liegt daran, dass der Inhalt der Hilfsprogramme standardmäßig als Codeblock behandelt wird. Das Hilfsprogramm rendert Markup, das mit dem öffnenden `<a>`-Tag beginnt. Wenn das Hilfsprogramm nur-Text oder Tags rendern muss, die kein Schließ Endes Tag enthalten (z. b. `<meta>` Tags), muss sich der zu rendernde Inhalt in `<text></text>` Tags befinden.

#### <a name="change-webpagecontexthttpcontext-removed"></a>Änderung: "webpagecontext. HttpContext" entfernt

> Die `WebPageContext.HttpContext`-Eigenschaft wurde entfernt. Verwenden Sie stattdessen `HttpContext.Current`. (Die `WebPageContext.HttpContext`-Eigenschaft ist einfach umschließen.)

#### <a name="change-facebook-helper-moved-to-new-package"></a>Ändern: "Facebook"-Hilfsprogramm wurde in neues Paket verschoben

> Das `Facebook`-Hilfsprogramm wurde in die *Facebook. Helper* -Bibliothek verschoben, die die `Facebook`-Hilfsprogramm und zusätzliche Funktionen enthält. Sie müssen diese Bibliothek als separates Paket installieren, wie unter "Installieren von Hilfsprogrammen mit dem Paket-Manager" im Tutorial " [Getting Started with ASP.net Pages](https://go.microsoft.com/fwlink/?LinkId=202889)" beschrieben.

#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Änderung: Mitgliedschafts-, Rollen-und Sicherheits Typen werden in neue Assembly verschoben

> Die folgenden Typen wurden in die `WebMatrix.WebData` Assembly verschoben:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`

#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Ändern: die "tagbuilder"-Klasse wurde in die Assembly "System. Web. Webseiten. dll" verschoben.

> Die `TagBuilde` r-Klasse wurde in die Assembly "System. Web. Webseiten. dll" verschoben. Zuvor war dies eine Assembly, die Teil von ASP.NET MVC war. Diese Änderung bedeutet, dass Sie ASP.NET MVC nicht installieren müssen, um die `TagBuilder`-Klasse zu verwenden.
> 
> Die-Klasse befindet sich jedoch noch im `System.Web.Mvc`-Namespace. Um die `TagBuilder`-Klasse zu verwenden (z. b. in einem benutzerdefinierten Razor-Hilfsprogramm ASP.net), müssen Sie auf den-Namespace verweisen (z. b. durch Hinzufügen von `@using System.Web.Mvc` zum Code).

#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Änderung: die Syntax der Anforderungs Validierung wurde geändert. "Validation"-Klasse entfernt

> Wenn Sie in der Beta 3-Version die Validierung für ein einzelnes Feld oder einen Satz von Feldern deaktivieren möchten, können Sie die `Validation.Exclude`-Methode und die Namen der Felder, die von der Überprüfung ausgeschlossen werden sollen, übergeben. In der Beta 3-Version ist eine neue Syntax zum Umgehen der Validierung verfügbar. Die in Beta 3 verwendete `Validation`-Methode wurde entfernt.
> 
> > [!NOTE]
> > Wenn Sie die Anforderungs Überprüfung nicht deaktivieren, wenn Benutzer versuchen, HTML-Markup hochzuladen (z. b. mithilfe eines Rich-Text-Editors auf einer Seite), meldet die Website einen Fehler wie *eine potenziell gefährliche Anforderung. der Formular Wert wurde vom Client erkannt* , und die Benutzereingabe wird nicht akzeptiert. Wenn Sie die Anforderungs Überprüfung deaktivieren, müssen Sie die Benutzereingabe manuell überprüfen, um sicherzustellen, dass Sie keine potenziell gefährlichen Markup-oder Skript Skripts wie die [Microsoft Anti-Cross-Site Scripting Library v 4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651)enthält.
> 
> 
> Um die automatische Anforderungs Validierung zu deaktivieren, müssen Sie die `Request.Unvalidated`-Methode abrufen und dabei den Namen des Felds oder eines anderen Post-Objekts übergeben, für das Sie die Anforderungs Überprüfung umgehen möchten. Mit dieser Methode können Sie die Überprüfung für alle Elemente in den `Form`-, `QueryString`-, `Cookies`-und `ServerVariables`-Auflistungen umgehen. In den folgenden Beispielen wird gezeigt, wie die `Unvalidated`-Methode verwendet wird:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]

<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Bekannte Probleme bei der ASP.net Web Pages mit Razor-Syntax

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problem: unerwartetes Verhalten bei Verwendung einer benutzerdefinierten Benutzertabelle für die Mitgliedschaft

> Um den Mitgliedschafts Anbieter für eine ASP.net Razor-Website zu initialisieren, müssen Sie die `WebSecurity.InitializeDatabaseConnection`-Methode aufrufen. (In webmatrix enthält die Vorlage Starter Site einen Aufrufen dieser Methode in der *\_AppStart. cshtml* -Datei.) Wenn der `autoCreateTables`-Parameter dieser Methode auf true festgelegt ist (Standardmäßig ist er in der Starter Site-Vorlage auf true festgelegt), und wenn ein unbekannter Tabellenname an die-Methode (der zweite Parameter) übergeben wird, löst die Methode keinen Fehler aus. Stattdessen wird die Tabelle automatisch erstellt.
> 
> Dies kann ein Problem darstellen, wenn Sie beabsichtigen, eine benutzerdefinierte Benutzertabelle für die Mitgliedschaft zu verwenden, aber den falschen Tabellennamen an die `WebSecurity.InitializeDatabaseConnection`-Methode zu übergeben. Da die-Methode nicht standardmäßig einen Fehler verursacht, wenn die angegebene Tabelle nicht vorhanden ist, und da Sie stattdessen eine neue Tabelle erstellt, kann die Anwendung scheinbar funktionieren. Allerdings kann der Anwendungscode, der auf der benutzerdefinierten Benutzertabelle basiert (und in den Feldern), möglicherweise mit unerwarteten Fehlern fehlschlagen.
> 
> **Problemumgehung**  
> Stellen Sie sicher, dass der in der `InitializeDatabaseConnection`-Methode übergebenen Name mit der Benutzerprofil Tabelle in der Mitgliedschafts Datenbank übereinstimmt, oder stellen Sie sicher, dass der `autoCreateTables`-Parameter auf false festgelegt ist.

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problem: "Fehler beim Generieren einer Benutzer Instanz von SQL Server".

> Wenn eine webmatrix-Webanwendung SQL Server Express verwendet und IIS 7,5 unter Windows 7 oder Windows Server 2008 R2 ausgeführt wird, wird möglicherweise ein Fehler angezeigt, der darauf hinweist, dass SQL Server den lokalen Anwendungspfad des Benutzers zur Laufzeit nicht abrufen kann.
> 
> Problem **Umgehung** Stellen Sie sicher, dass das Windows-Konto, unter dem die Anwendung ausgeführt wird (in der Regel Netzwerkdienst), über Lese-/Schreibberechtigungen für Stamm Ordner der Anwendung und für Unterordner wie *App\_-Daten*verfügt. Ausführlichere Informationen finden Sie im Knowledge Base [-Artikel Probleme mit SQL Server Express benutzerinstancing-und ASP.NET-Webanwendungs Projekten](https://support.microsoft.com/kb/2002980).

#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problem: in Visual Studio werden Namespaces für benutzerdefinierte Assemblys (DLLs) nicht automatisch importiert.

> Wenn Sie benutzerdefinierte Assemblys in einem Projekt in Visual Studio verwenden, werden die in diesen Assemblys deklarierten Namespaces nicht automatisch zur Entwurfszeit importiert. Folglich werden Verweise auf benutzerdefinierte Typen möglicherweise zur Entwurfszeit nicht erkannt und in Visual Studio als nicht erkannt gekennzeichnet (mit einer Wellenlinie). Dieses Problem tritt nur zur Entwurfszeit in Visual Studio auf. die Anwendung selbst wird ordnungsgemäß ausgeführt.
> 
> **Problemumgehung**  
> Fügen Sie eine `using`-Anweisung (`imports` in Visual Basic) ein, die auf die Entitäten verweist, die zur Entwurfszeit nicht erkannt werden.

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problem: Visual Studio IntelliSense und Projektvorlagen sind nur in ASP.NET MVC Version 3 verfügbar.

> Bei der Installation von ASP.net Web Pages werden nicht auch Tools für Visual Studio installiert, z. b. IntelliSense und Projektvorlagen für ASP.net Web Pages Anwendungen.
> 
> Problem **Umgehung** Wenn Sie IntelliSense und Projektvorlagen für ASP.net Web Pages Anwendungen in Visual Studio verwenden möchten, installieren Sie ASP.NET MVC 3 RC entweder über den Webplattform-Installer oder über das [eigenständige Installations](https://go.microsoft.com/fwlink/?LinkID=191797)Programm.

#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problem: "&lt;Helper&gt; Klasse wurde nicht gefunden".

> Nachdem Sie auf Beta 3 aktualisiert haben, wird möglicherweise ein Fehler angezeigt, dass eine Hilfsklasse (z. b. die `Facebook` Klasse) nicht gefunden werden kann. Beginnend mit der Beta 2-Version und Fortsetzung in Beta 3 wurden Hilfsprogramme zu Paketen verschoben, die Sie explizit installieren müssen. Vorhandene Standorte werden nicht aktualisiert, um diese Pakete einzubeziehen. Dies umfasst Websites in den Ordnern " *\My documents\iisexpress* " oder " *\My Documents\My Websites* ". Insbesondere wird dieser Fehler angezeigt, wenn Sie die Standard Website in " *Meine Websites* " (website1) verwenden, die einen Verweis auf das `Twitter`-Hilfsprogramm enthält.
> 
> **Problemumgehung**  
> Kommentieren Sie Aufrufe an alle Hilfsprogramme der Site aus, führen Sie die *\_Administrator* Seite aus, und installieren Sie das Paket bzw. die Pakete, die die zu verwendenden Hilfsprogramme enthalten. Nachdem Sie das Paket installiert haben, können Sie die Auskommentierung der Zeilen aufheben, die auf Hilfsprogramme verweisen.

#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problem: das Bereitstellen von Beta 3 ASP.net Razor-Assemblys im bin-Ordner funktioniert möglicherweise nicht auf Host Standorten

> Wenn Sie eine ASP.net Web Pages-Website auf einer Hostingsite bereitstellen und die ASP.net Razor Beta 3-Assemblys im Ordner " *bin* " der Website bereitstellen, treten möglicherweise Fehler auf, einschließlich der folgenden:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Dies kann der Fall sein, wenn der Hostinganbieter die ASP.net Web Pages Beta 1-Assemblys im globalen Anwendungscache (GAC) des Servers installiert hat. Assemblys im GAC erhalten Vorrang vor Assemblys, die lokal im Ordner " *bin* " installiert sind.
> 
> Problem **Umgehung** Wenden Sie sich an Ihren Hostinganbieter, um zu bestätigen, dass die angezeigten Fehler auf einen Konflikt zwischen den Versionen der Assemblys des Anbieters und Ihrem Host zurückzuführen sind. Wenn ja, fordern Sie an, dass der Hostinganbieter die Assemblys im GAC des Servers aktualisiert.

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problem: Lesen von Feeds oder anderen externen Daten über einen Proxy Server

> Wenn sich der Server, auf dem der Standort ausgeführt wird, hinter einem Proxy Server befindet, müssen Sie möglicherweise Proxy Informationen in der Datei " *Web. config* " konfigurieren, damit Sie Informationen lesen können, die von außerhalb Ihres Standorts stammen. Wenn Sie z. b. das `ReCaptcha`-Hilfsprogramm verwenden, kommuniziert das Hilfsprogramm mit dem Dienst "reCAPTCHA", kann jedoch vom Proxy Server blockiert werden. Analog dazu kann für Feeds, die in ASP.net Web Pages verwendet werden, wie z. b. der vom Paket-Manager verwendete Feed, eine Proxykonfiguration erforderlich sein.
> 
> Wenn beim Arbeiten mit einem externen Dienst oder beim Arbeiten mit dem paketfeed Probleme auftreten, platzieren Sie die folgenden Elemente in der *Web. config* -Stammdatei Ihrer Anwendung:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Weitere Informationen zum Konfigurieren eines Proxy Servers finden Sie unter [&lt;Proxy&gt;-Element (Netzwerkeinstellungen)](https://msdn.microsoft.com/library/sa91de1e.aspx) auf der MSDN-Website.

#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problem: "Microsoft. Web. Infrastructure. dll kann nicht geladen werden"

> Wenn Sie zuvor die Beta 1-Version von ASP.net Web Pages mit Razor-Syntax installiert und dann die Beta 3-Version installiert haben, werden alle entsprechenden Assemblys im globalen Assemblycache installiert, mit Ausnahme von " *Microsoft. Web. Infrastructure. dll*". Wenn Sie ASP.net Razor Pages ausführen, wird daher ein Fehler angezeigt, der darauf hinweist, dass " *Microsoft. Web. Infrastructure. dll* " nicht geladen werden konnte.
> 
> Dieses Problem tritt nicht auf, wenn Sie die Beta 3-Version auf einem sauberen Computer geladen haben.
> 
> **Problemumgehung**  
> Deinstallieren Sie ASP.net Web Pages in der Systemsteuerung. Installieren Sie dann die Beta 3-Version neu.

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problem: beim Deinstallieren des .NET Framework Version 4 wird ASP.net Web Pages mit Razor-Syntax deaktiviert.

> Wenn Sie die .NET Framework Version 4 deinstallieren und dann neu installieren, ist ASP.net Web Pages mit Razor-Syntax deaktiviert. Seiten mit der Erweiterung *. cshtml* werden nicht ordnungsgemäß ausgeführt. ASP.net Web Pages eine Assembly in der *Web. config* -Stammdatei des Computers registrieren, und durch das Entfernen des .NET Framework wird diese Datei entfernt. Beim erneuten Installieren des .NET Framework wird eine neue Version der Konfigurationsdatei installiert, aber der Verweis für die ASP.net Web Pages Assembly wird nicht hinzugefügt.
> 
> Problem **Umgehung** Installieren Sie nach der erneuten Installation des .NET Framework ASP.net Web Pages mit Razor-Syntax neu. Dadurch wird der Datei " *Web. config* " im Stammverzeichnis des Computers Folgendes Element hinzugefügt, das sich in der Regel an folgendem Speicherort befindet:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]

#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problem: Anwendungen, die zuvor mit ASP.NET-Assemblys im Ordner "bin Folder

> Während der Bereitstellung kopiert die ASP.net Web Pages-Assemblys (z *. b. "Microsoft. Webseiten. dll*") in den Ordner " *bin* " der Website auf dem Server. (Dies ist möglicherweise während der Bereitstellung automatisch aufgetreten oder weil der Entwickler die Assemblys explizit kopiert hat.) Wenn jedoch die Beta 3-Version installiert ist, treten Fehler auf, wie z. b. Fehler, dass bestimmte Typen nicht gefunden werden können. Dies tritt auf, weil eine Reihe von ASP.net Web Pages Typen in verschiedene Namespaces für die Beta 3-Version verschoben wurden.
> 
> Problem **Umgehung**   
> Löschen Sie den Ordner " *bin* " der bereitgestellten Anwendung, kopieren Sie die neuen Assemblys in den Ordner (oder stellen Sie die Anwendung erneut bereit), und starten Sie die Anwendung dann neu.

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
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]

#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problem: Verwenden von Webanwendungs Projekten oder ASP.NET MVC und ASP.net Web Pages in derselben Anwendung

> Wenn Sie ASP.net Web Pages in einem Webanwendungs Projekt oder einer ASP.NET MVC-Anwendung verwendet haben, wird möglicherweise ein Fehler angezeigt, dass *webpagehttpapplication* nicht gefunden werden kann.
> 
> **Problemumgehung**  
> Wenn Sie diesen Fehler erhalten, ändern Sie die Basisklasse, von der die Anwendung abgeleitet ist. Ändern Sie in der Datei *Global. asax* die folgende Zeile:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> Folgendermaßen:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Dadurch wird eine Änderung rückgängig gemacht, die für die Beta 1-Version von ASP.net Web Pages mit Razor-Syntax eingeführt wurde.

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problem: Bereitstellen einer Anwendung auf einem Computer, auf dem SQL Server Compact nicht installiert ist

> Anwendungen, die SQL Server Compact-Datenbanken enthalten, können auf einem Computer ausgeführt werden, auf dem SQL Server Compact nicht installiert ist. Microsoft webmatrix Beta 3 kopiert diese Binärdateien automatisch für Sie und führt die entsprechenden *Web. config* -Datei Transformationen aus.
> 
> Problem **Umgehung** Wenn Sie diese Dateien kopieren und die Datei " *Web. config* " manuell ändern müssen, gehen Sie wie folgt vor:
> 
> 1. Kopieren Sie die Datenbank-Engine-Assemblys in den Ordner *bin* (und die Unterordner) der Anwendung auf dem Zielcomputer: 
> 
>     - Kopieren Sie " *c:\Programme\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* " **in** " *\bin* ".
>     - Kopieren Sie *c:\Programme\Microsoft SQL Server Compact edition\v4.0\private\x86\\* * **in** *\bin\x86*
>     - Kopieren Sie *c:\Programme\Microsoft SQL Server Compact edition\v4.0\private\amd64\\* * **in** *\bin\amd64*
> 2. Erstellen oder öffnen Sie eine *Web. config* -Datei im Stamm Ordner der Website. (In webmatrix Beta 3 ist dieser Dateityp verfügbar, wenn Sie im Dialogfeld **Dateityp auswählen** auf **alle** klicken.)
> 3. Fügen Sie das folgende-Element als untergeordnetes Element des **&lt;Configuration&gt;** -Elements hinzu (nicht innerhalb des **&lt;System. Web&gt;** Element):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problem: die Datenbank-und WebGrid-Hilfsprogramme funktionieren in Visual Basic mit mittlerer Vertrauensstellung nicht.

> Wenn Sie Visual Basic verwenden ( *vbhtml* -Dateien werden erstellt), funktionieren die `Database`-und `WebGrid`-Hilfsprogramme nicht, wenn die Anwendung für die Verwendung von mittlerer Vertrauensstellung festgelegt ist.
> 
> **Problemumgehung**  
> Legen Sie die Anwendung vorübergehend auf volle Vertrauenswürdigkeit fest.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problem: die "verschlüsseln"-Eigenschaft wurde nicht erkannt.

> SQL Server Compact 4,0 erkennt die `Encrypt`-Eigenschaft der `SqlCeConnection`-Klasse nicht. Sie sollten diese Eigenschaft nicht zum Verschlüsseln von Datenbankdateien verwenden. Die `Encrypt`-Eigenschaft wurde in SQL Server Compact 3,5-Version als veraltet markiert und wurde nur aus Gründen der Abwärtskompatibilität beibehalten. 
> 
> **Problemumgehung**  
> Verwenden Sie die `Encryption Mode`-Eigenschaft der `SqlCeConnection`-Klasse, um SQL Server Compact 4,0-Datenbankdateien zu verschlüsseln. Im folgenden Beispiel wird gezeigt, wie eine verschlüsselte SQL Server Compact 4,0-Datenbank mit der `Encryption Mode`-Eigenschaft erstellt wird:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Gehen Sie folgendermaßen vor, um den Verschlüsselungs Modus einer vorhandenen SQL Server Compact 4,0-Datenbank zu ändern:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Gehen Sie folgendermaßen vor, um eine unverschlüsselte SQL Server Compact 4,0-Datenbank zu verschlüsseln:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]

#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problem: Microsoft Visual C++ 2008-Laufzeitbibliotheken sind erforderlich.

> Die systemeigenen DLLs von SQL Server Compact 4,0 benötigen die Microsoft C++ Visual 2008-Laufzeitbibliotheken (x86, ia64 und x64), Service Pack 1.
> 
> **Problemumgehung**  
> Installieren Sie den .NET Framework 3,5 SP1. Dadurch wird auch die Visual C++ 2008-Laufzeitbibliotheken SP1 installiert. Sie können die Bibliotheken von folgendem Speicherort herunterladen:   
>   
> [Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL-Sicherheits Update](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Beachten Sie, dass die Visual C++ 2008-Laufzeitbibliotheken SP1 *nicht* durch die Installation des .NET Framework 2,0, 3,0 oder 4 installiert werden.

#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problem: Wenn SQL Server Compact vor der Installation von .NET Framework auf dem Computer installiert ist, ist der invariante Name des Anbieters nicht in der Datei "Machine. config" des .NET Framework registriert.

> SQL Server Compact kann auf einem Computer installiert werden, auf dem nicht .NET Framework installiert ist, da für SQL Server Compact .NET Framework erforderlich ist. Wenn weder .NET Framework Version 3,5 noch 4 installiert ist, bevor Sie SQL Server Compact installieren, wird der invariante Name SQL Server Compact des Anbieters nicht in der Datei " *Machine. config* " registriert. Jede Anwendung, die auf dem SQL Server Compact Eintrag in der Datei *Machine. config* basiert, schlägt fehl. Der Eintrag für die invariante namens Registrierung in *Machine. config* sieht wie im folgenden Beispiel aus:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Problemumgehung**  
> Deinstallieren Sie SQL Server Compact 4,0 CTP1. Laden Sie die Vollversionen der .NET Framework von folgendem Speicherort herunter, und installieren Sie Sie:
> 
> [Microsoft .NET Framework 3,5 Service Pack 1 (vollständiges Paket)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Microsoft .NET Framework 4,0-Release (vollständiges Paket)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Installieren Sie [SQL Server Compact 4,0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en)erneut.

<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Installieren von Anwendungen

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problem: das Installieren einer Anwendung kann einige Zeit in Anspruch nehmen, wenn der Ordner "eigene Dateien" des Benutzers an eine Netzwerkfreigabe umgeleitet wird.

> **Problemumgehung**  
> Keine. Die Installation der Anwendung kann einige Zeit in Anspruch nehmen, wird aber ordnungsgemäß installiert.

<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Veröffentlichen von Anwendungen

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

<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Andere Probleme

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problem: das suchen/filtern funktioniert nicht in Berichten für Group by: Issue Type.

> Wenn Sie einen Bericht für einen Standort ausführen, wenn Sie Text in das Feld *nach URL Filtern* eingeben und auf *Suchen*klicken, geschieht nichts. Dies liegt daran, dass dieses Steuerelement nicht funktionsfähig ist, während der " *Gruppieren nach* "-Status des Berichts auf " *Issue Type*" festgelegt ist.
> 
> Problem **Umgehung** Klicken Sie auf der Registerkarte *Gruppieren nach* im Menüband auf *URL* , um die Einträge nach ihrer Quell-URL zu gruppieren. Das Textfeld und die Schaltfläche zum Filtern der Einträge sind in diesem Zustand funktionsfähig.

#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problem: WCF-Anwendungen können mit IIS Express nicht ausgeführt werden.

> Das Navigieren zu einer WCF-Anwendung führt zu einem Fehler wie dem folgenden:
> 
> *Die Datei oder Assembly ' Microsoft. Web. Administration, Version = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bs3856ad364e35 ' oder eine ihrer Abhängigkeiten konnte nicht geladen werden. Das System kann die angegebene Datei nicht finden.*
> 
> Dies tritt auf, weil IIS Express Beta Version WCF nicht standardmäßig unterstützt.
> 
> Problem **Umgehung** Verwenden Sie eine der folgenden Problem Umgehungen (Problem Umgehung #2 erfordert Microsoft Windows Vista oder höher):
> 
> 
> 1. Kopieren Sie die Assemblys " *Microsoft. Web. dll* " und " *Microsoft. Web. Administration. dll* " vom webmatrix-Installationsort in das Verzeichnis " *bin* " der WCF-Anwendung. Standardmäßig wird webmatrix im *Microsoft webmatrix* -Unterordner unter dem Ordner " *Programme* " des Systems installiert.
> 2. Erstellen Sie unter Microsoft Windows Vista oder höher einen Symlink zu den Assemblys im Verzeichnis " *bin* " mithilfe der folgenden Befehle. (Dieser Ansatz hat den Vorteil, dass er keine Kopie der Assemblys erstellt.)
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Installieren Sie die beiden Assemblys im GAC. Führen Sie an einer Eingabeaufforderung mit erhöhten Rechten die folgenden Befehle aus:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]

#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problem: webmatrix Beta 3 kann bestimmte Tasks, für die eine Erhöhung erforderlich ist, nicht ausführen.

> Webmatrix Beta 3 kann bestimmte Aufgaben, die eine Erhöhung erfordern, nicht ausführen, wie z. b. die Installation zusätzlicher Komponenten in den folgenden Situationen:
> 
> - Unter Windows Vista oder Windows 7 sind Sie mit einem Konto angemeldet, das nicht über Administratorrechte verfügt, und die Benutzerkontensteuerung (User Account Control, UAC) ist deaktiviert.
> - Sie verwenden Microsoft Windows XP oder Microsoft Windows Server 2003.
> 
> **Problemumgehung**  
> Für die meisten Aufgaben in webmatrix Beta 3 sind keine Administrator Berechtigungen erforderlich. Für solche, die dies tun, können Sie den Vorgang als Administrator ausführen oder die folgenden Schritte ausführen:
> 
> - Aktivieren Sie UAC unter Windows Vista oder Windows 7.
> - Fügen Sie unter Windows XP den Benutzer der Sicherheitsgruppe Administratoren hinzu.

#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problem: "Site from Web Gallery" ist deaktiviert.

> Die Option **Site from Web Gallery** ist deaktiviert, wenn der Webplattform-Installer 3,0 nicht installiert ist.
> 
> **Problemumgehung**  
> Installieren Sie den [Microsoft-Webplattform-Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).

#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problem: auf Windows Server 2003 startet IIS Express nicht für einen Benutzer, der kein Administrator ist.

> Wenn Sie unter Windows Server 2003 eine Seite starten oder IIS Express starten, wird IIS Express nicht gestartet. Für Webseiten wird ein Fehler angezeigt, der anzeigt, dass die Anwendung von einem Benutzer ohne Administrator Schritte gestartet wurde.
> 
> **Problemumgehung**  
> Starten Sie webmatrix Beta 3 als Administrator. Weitere Informationen finden Sie im folgenden Knowledge Base-Artikel:  
>   
> [Eine Anwendung, die von einem nicht administrativen Benutzer gestartet wird, kann nicht auf den HTTP-Datenverkehr des Computers lauschen, auf dem die Anwendung unter Windows Vista, Windows Server 2003 oder Windows XP ausgeführt wird.](https://support.microsoft.com/kb/939786)

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

#### <a name="issue-the-relationships-button-is-disabled"></a>Problem: die Schaltfläche "Beziehungen" ist deaktiviert.

> Die Schaltfläche **Beziehungen** auf der Registerkarte **Tabelle** im Arbeitsbereich **Datenbanken** ist für SQL Server Compact-Datenbanken deaktiviert.
> 
> **Problemumgehung**  
> Keine. Der SQL Server Compact unterstützt keine Beziehungen zwischen Tabellen.

#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problem: parametrisierte SQL-Abfragen lösen Ausnahmen aus.

> Wenn Sie in SQL Server Compact 4,0 keinen Datentyp, z. b. `SqlDbType` oder `DbType`, für Parameter in parametrisierten Abfragen angeben, wird eine Ausnahme ausgelöst, wenn die Abfrage ausgeführt wird.
> 
> **Problemumgehung**  
> Legen Sie den Datentyp für Parameter wie `SqlDbType` oder `DbType`explizit fest. Dies ist im Fall von BLOB-Datentypen (`image` und `ntext`) von entscheidender Bedeutung. Verwenden Sie Code wie den folgenden:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]

<a id="More_Info"></a>

## <a name="for-more-information"></a>Weitere Informationen

Weitere Informationen zu webmatrix Beta 3 finden Sie auf den folgenden Websites:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

---

© 2010 Microsoft Corporation. Alle Rechte vorbehalten. [Nutzungsbedingungen](https://msdn.microsoft.cos/cc300389.aspx).
