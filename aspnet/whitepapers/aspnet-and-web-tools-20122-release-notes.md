---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: Anmerkungen zu ASP.net and Web Tools 2012,2 | Microsoft-Dokumentation
author: rick-anderson
description: Anmerkungen zu dieser Version von ASP.net and Web Tools 2012,2.
ms.author: riande
ms.date: 02/14/2013
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: a4ea1d7c146309e1d5e8be944d496e9fd87bca3e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78420249"
---
# <a name="aspnet-and-web-tools-20122-release-notes"></a>ASP.NET and Web Tools 2012.2 – Anmerkungen zu dieser Version

> In diesem Dokument wird die Veröffentlichung von ASP.net and Web Tools 2012,2 beschrieben. Es handelt sich um ein Update für Visual Studio Web Tooling und ASP.net.

- [Installationshinweise](#_Installation)
- [Dokumentation](#_Documentation)
- [Unterstützung](#_Support)
- [Software Anforderungen](#_Software_Requirements)
- [Neue Features in ASP.net and Web Tools 2012,2](#_New_Features_in)

    - [Tools](#_Tooling)
    - [Webpublishing](#_Web_Publishing)
    - [ASP.NET-MVC-Vorlagen](#_Templates)
    - [ASP.NET-Web-API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [ASP.net friendly URLs](#_ASP.NET_Friendly_URLs)
- [Bekannte Probleme und wichtige Änderungen](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Installationshinweise

ASP.net and Web Tools 2012,2 für Visual Studio 2012 kann mithilfe des [Webplattform-Installers](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2)installiert werden. Dies ist ein Update für Visual Studio 2012 oder Visual Studio Express 2012 für das Web, was erforderlich ist. Wenn Visual Studio nicht installiert ist, wird Visual Studio Express 2012 für das Web installiert.

Sie können ASP.net and Web Tools 2012,2 auch manuell installieren. Für das Web muss Visual Studio 2012 oder Visual Studio Express 2012 installiert sein. Verwenden Sie dann die folgenden Anweisungen: 

1. Laden Sie den Installer [ASP.net und Web Frameworks 2012,2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) aus dem Download Center herunter.
2. Klicken Sie bei Aufforderung auf ausführen. Sie können die Datei auch speichern, um Sie später auszuführen.
3. Überprüfen Sie die Version von Visual Studio, die Sie aktualisieren werden. Hierzu können Sie das Visual Studio starten, das Sie aktualisieren möchten. Klicken Sie dann auf das Menü Element Hilfe.   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. Wenn das Menü Element &quot;etwa Microsoft Visual Studio 2012 für Web&quot; angezeigt wird, laden Sie [Web Entwicklertools 2012,2-Visual Studio Express 2012 für das Web](https://go.microsoft.com/fwlink/?LinkID=282228)herunter. Andernfalls laden Sie [Web Entwicklertools 2012,2-Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228)herunter.
5. Klicken Sie bei Aufforderung auf ausführen. Sie können die Datei auch speichern, um Sie später auszuführen.

> [!NOTE]
> ASP.net and Web Tools Version 2012,2 enthält keine SQL Server Data Tools. SQL Server und Windows Azure SQL-Datenbanken bieten einen umfassenderen Satz von Datenbanktools, einschließlich der Offline projektbasierten Entwicklung, des Schema Vergleichs und erweiterter Funktionen für die Daten Bank Bereitstellung. Weitere Informationen oder eine Installation SQL Server Data Tools besuchen Sie [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Dokumentation

Lernprogramme und weitere Informationen zu ASP.net and Web Tools 2012,2 sind auf der ASP.NET-Website verfügbar (https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Support

ASP.net and Web Tools 2012,2 wird offiziell veröffentlicht und unterstützt. Sie können Ihren normalen Supportkanal verwenden. Sie können auch Fragen in den ASP.net-Foren ([https://forums.asp.net/](https://forums.asp.net/)) stellen, in denen Mitglieder der ASP.net-Community häufig informelle Unterstützung bieten können.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Softwareanforderungen

Der ASP.net and Web Tools 2012,2 erfordert Visual Studio 2012 oder Visual Studio Express 2012 für das Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Neue Features in ASP.net and Web Tools 2012,2

In diesem Abschnitt werden die Funktionen beschrieben, die in der Version ASP.net and Web Tools 2012,2 eingeführt wurden.

<a id="_Tooling"></a>
### <a name="tooling"></a>Tools

- Seitenprüfung 

    - Unterstützung der JavaScript-Auswahl Zuordnung, die es Seitenprüfung ermöglicht, Elemente, die der Seite dynamisch hinzugefügt wurden, dem entsprechenden JavaScript-Code zuzuordnen.
    - Die Fähigkeit, CSS-Updates in Echtzeit anzuzeigen.
    - Weitere Informationen finden Sie [unter CSS-automatische Synchronisierung und JavaScript-Auswahl Zuordnung in Seitenprüfung](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Unterstützung der Syntax Hervorhebung für coffeescript, Mustache, Lenker und jsrendering.
    - Der HTML-Editor stellt IntelliSense für Knockout-Bindungen bereit.
    - Weniger Bearbeitungs-und Compilerunterstützung, um das entwickeln dynamischer CSS mithilfe von less zu ermöglichen.
    - Fügen Sie JSON als .NET-Klasse ein. Mit diesem speziellen Einfüge Befehl fügen sie JSON in C# eine-oder VB.net-Codedatei ein, und Visual Studio generiert automatisch .NET-Klassen, die aus dem JSON-Code abgeleitet werden.
- Die Unterstützung mobiler Emulatoren fügt Erweiterbarkeits Hooks hinzu, damit Emulatoren von Drittanbietern als VSIX installiert werden können. Die installierten Emulatoren werden in der F5-Dropdown Liste angezeigt, damit Entwickler ihre Websites auf einer Vielzahl von mobilen Geräten anzeigen können. Weitere Informationen zu diesem Feature finden Sie im Blogbeitrag von Scott Hanselman in [der neuen browserstack-Integration in Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Webpublishing

- Website Projekte verfügen jetzt über die gleiche Veröffentlichungs Methode wie Webanwendungs Projekte, einschließlich der Veröffentlichung auf Windows Azure-Websites.
- Selektive Veröffentlichung &#8211; für eine oder mehrere Dateien können Sie die folgenden Aktionen ausführen (nach der Veröffentlichung auf einem Web deploy Endpunkt): 

    - Ausgewählte Dateien veröffentlichen.
    - Sehen Sie sich den Unterschied zwischen einer lokalen Datei und einer Remote Datei an.
    - Aktualisieren Sie die lokale Datei mit der Remote Datei, oder aktualisieren Sie die Remote Datei mit der lokalen Datei.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>ASP.NET-MVC-Vorlagen

- Mit der neuen Facebook-Anwendungs Vorlage wird das Schreiben von Facebook-Canvas-Anwendungen vereinfacht. In wenigen einfachen Schritten können Sie eine Facebook-Anwendung erstellen, die Daten von einem angemeldeten Benutzer abruft und in Ihre Freunde integriert ist. Die Vorlage enthält eine neue Bibliothek, mit der Sie alle erforderlichen Schritte zum Erstellen einer Facebook-App ausführen können, einschließlich Authentifizierung, Berechtigungen, Zugriff auf Facebook-Daten und mehr. Weitere Informationen zur Verwendung der Facebook-Anwendungs Vorlage finden Sie unter [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921).
- Eine neue MVC-Vorlage für die Einzelseiten Anwendung ermöglicht Entwicklern das Erstellen interaktiver Client seitiger Web-Apps mithilfe von HTML 5, CSS 3 und den beliebten Knockout-und jQuery-JavaScript-Bibliotheken, zusätzlich zu ASP.net-Web-API. Die Vorlage enthält eine "ToDo"-List-Anwendung, die gängige Verfahren zum Erstellen einer JavaScript-HTML5-Anwendung veranschaulicht, die eine Rest-Server-API verwendet. Weitere Informationen finden Sie unter [https://www.asp.net/single-page-application](../single-page-application/index.md).
- Sie können jetzt eine VSIX erstellen, mit der neue Vorlagen zum Dialogfeld "ASP.NET MVC New Project" hinzugefügt werden. Weitere Informationen finden Sie hier: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Die MVC-Projektvorlagen &#8211; des fixeddisplaymodes-Pakets wurden aktualisiert, um das neue nuget-Paket "fixeddisplaymodes" aufzunehmen, das eine Problem Umgehung für einen Fehler in MVC 4 enthält. Weitere Informationen über die im Paket enthaltene Korrektur finden Sie in diesem Blogbeitrag ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) des MVC-Teams.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET-Web-API

ASP.net-Web-API wurde mit mehreren neuen Features erweitert:

- ASP.net-Web-API odata
- ASP.net-Web-API Ablauf Verfolgung
- ASP.net-Web-API Hilfeseite

#### <a name="aspnet-web-api-odata"></a>ASP.net-Web-API odata

ASP.net-Web-API odata bietet Ihnen die Flexibilität, die Sie benötigen, um odata-Endpunkte mit umfassender Geschäftslogik für jede beliebige Datenquelle zu erstellen. Mit ASP.net-Web-API odata steuern Sie die Menge der odata-Semantik, die Sie verfügbar machen möchten. ASP.net-Web-API odata ist in den ASP.NET MVC 4-Projektvorlagen enthalten und auch über nuget ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)) verfügbar.

ASP.net-Web-API odata unterstützt derzeit die folgenden Funktionen:

- Aktivieren Sie die odata-Abfrage Semantik, indem Sie das [quervable]-Attribut anwenden.
- Überprüfen Sie problemlos odata-Abfragen, und schränken Sie den Satz unterstützter Abfrage Optionen, Operatoren und Funktionen ein.
- Der Parameter wird direkt an odataqueryoptions gebunden, um eine abstrakte Syntax Struktur Darstellung der Abfrage zu erhalten, die dann überprüft und auf ein iquerable-oder IEnumerable-Element angewendet werden kann.
- Aktivieren Sie das Dienst gesteuerte Paging und die Generierung der nächsten Seiten Verknüpfung, indem Sie die Ergebnis Limits für das [querable]-Attribut angeben.
- Fordern Sie die Gesamtanzahl der übereinstimmenden Ressourcen mithilfe $inlinecount an.
- Steuern der NULL-Propagierung.
- Any/all-Operatoren in $Filter.
- Ableiten eines Entity Data Model per Konvention oder explizites Anpassen eines Modells ähnlich wie Entity Framework Code First.
- Machen Sie Entitätenmengen durch Ableiten von entitysetcontroller verfügbar.
- Einfache, anpassbare Konventionen für das verfügbar machen von Navigations Eigenschaften, das Bearbeiten von Links und Implementieren von odata-Aktionen
- Vereinfachtes Routing mithilfe der mapodataroute-Erweiterungsmethode.
- Unterstützung für die Versionsverwaltung durch die Bereitstellung mehrerer EDM-Modelle.
- Machen Sie Dienst Dokumente und $Metadata verfügbar, damit Sie Clients (.net, Windows Phone, Windows Store usw.) für Ihre Web-API generieren können.
- Unterstützung für die ausführlichen odata-Atom-, JSON-und JSON-Formatierungen.
- Erstellen, aktualisieren, teilweise aktualisieren (Patch) und Löschen von Entitäten.
- Abfragen und Bearbeiten von Beziehungen zwischen Entitäten.
- Erstellen Sie Beziehungslinks, die mit ihren Routen verknüpft sind.
- Komplexe Typen
- Vererbung von Entitäts Typen.
- Sammlungs Eigenschaften.
- Enumerationen.
- Odata-Aktionen.
- Basiert auf der gleichen Grundlage wie WCF Data Services, nämlich odatalib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Weitere Informationen zu ASP.net-Web-API odata finden Sie unter [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>ASP.net-Web-API Ablauf Verfolgung

ASP.net-Web-API Ablauf Verfolgung integriert Ablauf Verfolgungs Daten aus Ihren Web-APIs in die .net-Ablauf Verfolgung. Sie ist nun in der Web-API-Projektvorlage standardmäßig aktiviert. Ablauf Verfolgungs Daten für Ihre Web-APIs werden an das Ausgabefenster gesendet und über IntelliTrace zur Verfügung gestellt. ASP.net-Web-API Ablauf Verfolgung ermöglicht es Ihnen, Informationen zu Ihrer Web-API zu verfolgen, wenn Sie in Windows Azure über die Integration mit [Windows Azure-Diagnose](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx)gehostet werden. Mithilfe des ASP.net-Web-API-Ablaufverfolgungs-nuget-Pakets ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)) können Sie auch ASP.net-Web-API Ablauf Verfolgung in einer beliebigen Anwendung installieren und aktivieren.

Weitere Informationen zum Konfigurieren und Verwenden der ASP.net-Web-API Ablauf Verfolgung finden Sie unter [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>ASP.net-Web-API Hilfeseite

Die ASP.net-Web-API Hilfeseite ist jetzt standardmäßig in der Web-API-Projektvorlage enthalten. Die ASP.net-Web-API Hilfeseite generiert automatisch Dokumentation für Web-APIs, einschließlich der HTTP-Endpunkte, der unterstützten HTTP-Methoden, Parameter und der Anforderungs-und Antwort Nachrichten-Nutzlasten. Die Dokumentation wird automatisch aus Kommentaren im Code abgerufen. Sie können die ASP.net-Web-API Hilfeseite auch einer beliebigen Anwendung hinzufügen, indem Sie das ASP.net-Web-API Hilfeseite nuget-Paket ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)) verwenden.

Weitere Informationen zum Einrichten und Anpassen der ASP.net-Web-API Hilfeseite finden Sie unter [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.net signalr vereinfacht das Hinzufügen von Echtzeit-Webfunktionen zu Ihrer ASP.NET-Anwendung, wobei websockets verwendet werden, wenn Sie verfügbar sind, und automatisch auf andere Techniken zurückgreifen, wenn dies nicht der Fall ist.

Weitere Informationen zur Verwendung von ASP.net signalr finden Sie unter [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.net friendly URLs

ASP.net friendlyurls erleichtern Webformular Entwicklern das Generieren von übersichtlicheren URLs (ohne die Erweiterung ". aspx"). Es ist nur wenig oder keine Konfiguration erforderlich, und es kann mit vorhandenen ASP.NET v 4.0-Anwendungen verwendet werden. Mithilfe der friendlyurls-Funktion können Entwickler ihre Anwendungen auch einfacher unterstützen, indem Sie den Wechsel zwischen Desktop-und mobilen Ansichten unterstützen.

Weitere Informationen zum Installieren und Verwenden von ASP.net-freundlichen URLs finden Sie unter [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und wichtige Änderungen

In diesem Abschnitt werden bekannte Probleme und wichtige Änderungen in der Version ASP.net and Web Tools 2012,2 beschrieben.

### <a name="installation-issues"></a>Probleme bei der Installation

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Nicht ordnungs mäßig installierte Installationen von Visual Studio 2012

Das Installieren einer zusätzlichen SKU von Visual Studio 2012 nach dem Installieren des ASP.net and Web Tools 2012,2 erfordert einen Reparaturvorgang. Gehen Sie dabei von der folgenden Abfolge aus:

1. Installieren von Visual Studio 2012 Express für das Web
2. Installieren von ASP.net and Web Tools 2012,2
3. Installieren von Visual Studio 2012 Professional, Premium oder Ultimate

Schritt 2 führt nur zur Installation von Updates für Express für das Web. Um sicherzustellen, dass die während Schritt 3 installierte zusätzliche SKU das Update enthält, müssen Sie den ASP.net and Web Tools 2012,2 reparieren, um die Updates für die zuletzt installierte SKU zu installieren. Dies gilt auch, wenn die SKUs in Schritt 1 und 3 umgekehrt werden.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Installieren von Microsoft ASP.net and Web Tools 2012,2, wenn Visual Studio geöffnet ist

Wenn vs während der Installation von Microsoft ASP.net and Web Tools 2012,2 geöffnet ist, kann es vorkommen, dass Visual Studio einen fehlerhaften Zustand aufweist. Es wird empfohlen, dass Benutzer alle Instanzen von Visual Studio schließen, bevor Sie die Installation fortsetzen.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Abbrechen von ASP.net and Web Tools 2012,2-Setup während der Installation

Wenn Sie ASP.net and Web Tools 2012,2-Setup während der Installation abbrechen, wird Visual Studio in einem fehlerhaften Zustand belassen. Um dieses Problem zu beheben, führen Sie folgende Schritte aus: 

- Zum Hinzufügen von "Software" wechseln
- Deinstallieren Sie Microsoft ASP.net and Web Tools 2012,2, falls vorhanden.
- Neuinstallation Microsoft ASP.net and Web Tools 2012,2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Nach dem Deinstallieren von ASP.net and Web Tools 2012,2 fehlen die ASP.NET MVC 4-Vorlagen und Razor v2-Website Vorlagen.

Beim Deinstallieren von ASP.net and Web Tools 2012,2 werden auch alle ASP.NET MVC 4-und Razor v2-Website Vorlagen von Visual Studio 2012 deinstalliert.

Die Problem Umgehung besteht darin, die Visual Studio 2012-Installation zu reparieren, um ASP.NET MVC 4-und Razor v2-Website Vorlagen neu zu installieren.

### <a name="tooling-issues"></a>Tool Probleme

#### <a name="nuget-error-reported-during-project-creation"></a>Während der Projekt Erstellung gemeldeter nuget-Fehler

Nach der Installation von ASP.net and Web Tools 2012,2 wird möglicherweise der folgende Fehler angezeigt, wenn Sie ein MVC 4-Projekt erstellen.

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

Der ASP.net and Web Tools 2012,2 ausgeliefert nuget 2,1 und führt ein Upgrade der Erweiterung in Visual Studio 2012 aus. In einigen Fällen kann das VSIX-Installationsprogramm die VSIX nicht ordnungsgemäß aktualisieren. Mit den folgenden Schritten können Sie dieses Problem beheben:

1. Starten Sie Visual Studio 2012 als Administrator.
2. Wechseln Sie zu Extras-&gt;Erweiterungen und Updates, und deinstallieren Sie nuget.
3. Schließen Sie Visual Studio.
4. Navigieren Sie zum Installationsordner ASP.net and Web Tools 2012,2:

    1. Für Visual Studio 2012: **Programme\Microsoft ASP. net\asp.net Web stack\visual Studio 2012**
    2. Für Visual Studio 2012 Express für Web: **Programme\Microsoft ASP. net\asp.net Web stack\visual Studio Express 2012 for Web**
5. Doppelklicken Sie auf nuget. Tools. VSIX, um nuget erneut zu installieren.

### <a name="web-api-issues"></a>Web-API-Probleme

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Behandeln von Problemen in $Filter-und DateTime-literalen

Der odata-URI-Parser kann partielle datetime-Literale nicht ordnungsgemäß analysieren. Beispielsweise kann $Filter = Start EQ DateTime "2012-12-31t12:00" nicht ordnungsgemäß analysiert werden. Eine Problem Umgehung besteht in der Verwendung des vollständigen Literals, $Filter = Start EQ DateTime "2012-12-31t12:00:00".

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>Odata unterstützt keine Unterscheidung nach Groß-/Kleinschreibung.

OData unterstützt keine Unterscheidung nach Groß-/Kleinschreibung in OData-Abfragen und OData-Pfad. Siehe Workitems:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Wenn Benutzer eine andere Schreibweise auf JavaScript-Clientseite und Serverseite haben, wird dieses Problem wahrscheinlich auftreten. Dieses Problem ist Entwurfs bedingt im odata-Protokoll. Viele Benutzer melden dieses Problem jedoch. Um dieses Problem zu umgehen, müssen die Benutzer ihre Fälle in der URL korrigieren.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Standardmäßige odata-Routing Konventionen unterstützen keine Post/Put-on-Navigations Eigenschaft.

Standardmäßige odata-Routing Konventionen unterstützen keine Post/Put-on-Navigations Eigenschaft. Weitere Informationen finden Sie unter Arbeits Element [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366). Diese häufig verwendete Konvention ist in Standard Konventionen nicht vorhanden.

Um dieses Problem zu umgehen, müssen die Benutzer die neue Routing Konvention erweitern, um Sie zu unterstützen.

### <a name="facebook-template-issues"></a>Facebook-Vorlagen Probleme

#### <a name="facebook-application-template-only-works-using-net-45"></a>Die Facebook-Anwendungs Vorlage funktioniert nur mit .NET 4,5

Sie müssen .NET 4,5 in der Dropdown Liste Framework im Dialogfeld Neues Projekt auswählen, um die Facebook-Anwendungs Vorlage in ASP.NET MVC 4 anzuzeigen.

#### <a name="real-time-update-controller"></a>Echt Zeit Update Controller

Die Facebook-Anwendungs Vorlage ermöglicht Benutzern das einfache Erstellen eines Web-API-Controllers, um Echt Zeit Updates von Facebook zu verarbeiten. Wenn sich Ihr Entwicklungs Computer hinter NAT befindet, funktioniert Ihr Controller möglicherweise nicht ohne weitere Netzwerkkonfiguration. Weitere Informationen finden Sie hier: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Abfrage Zeichen folgen Werte verursachen einen Konflikt mit Facebook OAuth-Parametern

Die folgenden Felder stehen in Konflikt mit der Rückruf-URL des Facebook OAuth-Dialog Felds. Fügen Sie keine eigenen Werte für Abfrage Zeichenfolgen mit den folgenden Namen hinzu: Code, Error, Error\_Description, Error\_Reason.

#### <a name="using-page-inspector-with-facebook-template"></a>Verwenden von Seitenprüfung mit der Facebook-Vorlage

Beim Debuggen der Facebook-Anwendung können Sie das Seitenprüfung-Feature in Visual Studio 2012 nicht verwenden. Der Seitenprüfung unterstützt derzeit keine IFRAMES.

### <a name="single-page-application-template-issues"></a>Probleme mit der Einzelseiten Anwendungs Vorlage

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Bei der Aktualisierung von jQuery 1.9/Knockout 2.2.1 wird beim Ausführen des standardmäßigen MVC-Spa-Projekts das neue TODO-Element Edit Enter Enter-Ereignis nicht ordnungsgemäß behandelt.

Wenn das jQuery 1.9/Knockout 2.2.1-Update ausgeführt wird, wird beim Ausführen des standardmäßigen MVC-Spa-Projekts die neue TODO-Element Bearbeitung nicht mehr auf das neue TODO-Element Bearbeitungsfeld zurücksetzen, nachdem das neue TODO-Element in die TODO-Liste eingegeben wurde

Um den Verweis [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html)zu umgehen und den folgenden Beispielcode ähnlich zu beheben:

Datei "ToDo. Model. js"  
 Funktions-zu-Liste (Daten), hinzufügen:  
 **Self. issgewählter = ko. Observable (false);**

Funktion ToDoList. Prototype. addtodo: Fügen Sie den folgenden verdunkelte-Text hinzu:  
 **Self. issgewählt (true);**  
 Self. newydotitle (&quot;&quot;);

File Index. cshtml: Fügen Sie den folgenden verdunkelte-Text hinzu:  
 &lt;Form Data-BIND =&quot;Submit: addtodo&quot;&gt;  
 &lt;Input Class =&quot;addtodo&quot; Type =&quot;Text&quot; Data-BIND =&quot;Value: newtodotitle, Platzhalter: ' Type here to Add ', bluronenter: true, **HasFocus: issgewählter**, Event: {Blur: addtodo}&quot; /&gt;  
 &lt;"/Form"&gt;
