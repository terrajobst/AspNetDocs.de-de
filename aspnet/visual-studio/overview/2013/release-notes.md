---
uid: visual-studio/overview/2013/release-notes
title: ASP.net and Web Tools für Visual Studio 2013 Anmerkungen zu dieser Version | Microsoft-Dokumentation
author: microsoft
description: In diesem Dokument wird die Veröffentlichung von ASP.net and Web Tools für Visual Studio 2013 beschrieben.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: d8af9c8e7ee1316a5eac90c5959d07c628154e09
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600437"
---
# <a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET and Web Tools für Visual Studio 2013 – Anmerkungen zu dieser Version

von [Microsoft](https://github.com/microsoft)

> In diesem Dokument wird die Veröffentlichung von ASP.net and Web Tools für Visual Studio 2013 beschrieben.

## <a name="contents"></a>Inhalt

- [Installationshinweise](#TOC1)
- [Dokumentation](#TOC2)
- [Software Anforderungen](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Neue Features in ASP.net and Web Tools für Visual Studio 2013

- [Eine ASP.net](#TOC6)
- [Neues Webprojekt](#newproj)
- [ASP.net Gerüstbau](#scaffold)
- [Browserverknüpfung](#browser-link)
- [Erweiterungen von Visual Studio-Web-Editor](#web-editor)
- [Unterstützung von Web-Apps in Visual Studio Azure App Service](#waws)
- [Erweiterungen für die Webveröffentlichung](#publish)
- [NuGet 2.7](#nuget)
- [ASP.net Web Forms](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.net-Web-API 2](#TOC11)
- [ASP.net signalr](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Microsoft-owin-Komponenten](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.net Razor 3](#TOC14)
- [ASP.net App aussetzen](#TOC15)
- [Bekannte Probleme und wichtige Änderungen](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Installationshinweise

ASP.net and Web Tools für Visual Studio 2013 werden im Haupt Installationsprogramm gebündelt und können [hier](https://www.asp.net/downloads)heruntergeladen werden.

<a id="TOC2"></a>
## <a name="documentation"></a>Dokumentation

Lernprogramme und weitere Informationen zu ASP.net and Web Tools für Visual Studio 2013 stehen auf der [ASP.NET-Website](https://www.asp.net/)zur Verfügung.

<a id="TOC4"></a>
## <a name="software-requirements"></a>Softwareanforderungen

ASP.net and Web Tools erfordert Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Neue Features in ASP.net and Web Tools für Visual Studio 2013

In den folgenden Abschnitten werden die Features beschrieben, die in der-Version eingeführt wurden.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Eine ASP.net

Mit der Veröffentlichung von Visual Studio 2013 haben wir einen Schritt unternommen, um die Verwendung von ASP.NET-Technologien zu vereinheitlichen, damit Sie die gewünschten Möglichkeiten problemlos mischen können. Beispielsweise können Sie ein Projekt mithilfe von MVC starten und später ganz einfach Web Forms Seiten zum Projekt hinzufügen oder ein Gerüst für Web-APIs in einem Web Forms-Projekt verwenden. Eine ASP.net ist alles, was Sie als Entwickler für die Dinge, die Sie in ASP.net lieben, erleichtern soll. Unabhängig davon, welche Technologie Sie auswählen, können Sie sicher sein, dass Sie auf dem vertrauenswürdigen zugrunde liegenden Framework eines ASP.net aufbauen.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Neues Webprojekt

Wir haben das Erstellen neuer Webprojekte in Visual Studio 2013 verbessert. Im Dialogfeld **Neues ASP.NET-Webprojekt** können Sie den gewünschten Projekttyp auswählen, eine beliebige Kombination von Technologien (Web Forms, MVC, Web-API) konfigurieren, Authentifizierungs Optionen konfigurieren und ein Komponenten Testprojekt hinzufügen.

![Neues ASP.net-Projekt](release-notes/_static/image1.png)

Das neue Dialogfeld ermöglicht es Ihnen, die Standard Authentifizierungs Optionen für viele Vorlagen zu ändern. Wenn Sie z. b. ein ASP.net Web Forms-Projekt erstellen, können Sie eine der folgenden Optionen auswählen:

- Keine Authentifizierung
- Einzelne Benutzerkonten (ASP.NET Mitgliedschaft oder Social Provider Log in)
- Organisations Konten (Active Directory in einer Internetanwendung)
- Windows-Authentifizierung (Active Directory in einer Intranetanwendung)

![Authentifizierungs Optionen](release-notes/_static/image2.png)

Weitere Informationen zum neuen Prozess zum Erstellen von Webprojekten finden Sie unter [Erstellen von ASP.NET-Webprojekten in Visual Studio 2013](creating-web-projects-in-visual-studio.md). Weitere Informationen zu den neuen Authentifizierungs Optionen finden Sie unter [ASP.net Identity](#TOC8) weiter unten in diesem Dokument.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.net Gerüstbau

ASP.net Gerüstbau ist ein Code Generierungs Framework für ASP.NET-Webanwendungen. Es vereinfacht das Hinzufügen von Code Bausteinen zu Ihrem Projekt, das mit einem Datenmodell interagiert.

In früheren Versionen von Visual Studio waren Gerüstbau auf ASP.NET MVC-Projekte beschränkt. Mit Visual Studio 2013 können Sie nun Gerüstbau für alle ASP.net-Projekte verwenden, einschließlich Web Forms. Visual Studio 2013 unterstützt derzeit keine Erstellung von Seiten für ein Web Forms Projekt, aber Sie können weiterhin Gerüstbau mit Web Forms verwenden, indem Sie dem Projekt MVC-Abhängigkeiten hinzufügen. Die Unterstützung für das Erstellen von Seiten für Web Forms wird in einem späteren Update hinzugefügt.

Wenn Sie Gerüstbau verwenden, stellen wir sicher, dass alle erforderlichen Abhängigkeiten im Projekt installiert sind. Wenn Sie z. b. mit einem ASP.net-Web Forms Projekt beginnen und dann mithilfe von Gerüstbau einen Web-API-Controller hinzufügen, werden die erforderlichen nuget-Pakete und-Verweise automatisch dem Projekt hinzugefügt.

Fügen Sie ein **Neues Gerüstbau Element** hinzu, und wählen Sie **MVC 5-Abhängigkeiten** im Dialogfenster aus, um einem Web Forms Projekt MVC-Gerüstbau hinzuzufügen. Es gibt zwei Optionen für das Gerüstbau von MVC: Minimal und vollständig. Wenn Sie minimal auswählen, werden nur die nuget-Pakete und-Verweise für ASP.NET MVC dem Projekt hinzugefügt. Wenn Sie die Option vollständig auswählen, werden die minimalen Abhängigkeiten sowie die erforderlichen Inhalts Dateien für ein MVC-Projekt hinzugefügt.

Die Unterstützung für Gerüstbau-asynchrone Controller verwendet die neuen Async-Features aus Entity Framework 6.

Weitere Informationen und Tutorials finden Sie unter [Übersicht über ASP.net Gerüstbau](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Browser Link – signalr-Kanal zwischen Browser und Visual Studio

Mit der neuen [Browser Link](using-browser-link.md) Funktion können Sie mehrere Browser mit Visual Studio verbinden und alle aktualisieren, indem Sie auf der Symbolleiste auf eine Schaltfläche klicken. Sie können mehrere Browser mit ihrer Entwicklungswebsite verbinden, einschließlich mobiler Emulatoren, und auf Aktualisieren klicken, um alle Browser gleichzeitig zu aktualisieren. Der Browser Link macht auch eine API verfügbar, die Entwicklern das Schreiben von Browser Link Erweiterungen ermöglicht.

![](release-notes/_static/image3.png)

Wenn Sie Entwicklern ermöglichen, die Browser Link-API zu nutzen, können Sie sehr erweiterte Szenarios erstellen, die Grenzen zwischen Visual Studio und jedem verbundenen Browser überschreiten. Web Essentials nutzt die API, um eine integrierte Darstellung zwischen Visual Studio und den Entwicklertools des Browsers zu erstellen, die Remote Steuerung mobiler Emulatoren und vieles mehr.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Erweiterungen von Visual Studio-Web-Editor

Visual Studio 2013 enthält einen neuen HTML-Editor für Razor-Dateien und HTML-Dateien in Webanwendungen. Der neue HTML-Editor bietet ein einzelnes einheitliches Schema, das auf HTML5 basiert. Sie verfügt über automatische geschweifter Klammer, jQuery UI und das angularjs-Attribut IntelliSense, Attribute IntelliSense-Gruppierung, ID-und Klassenname IntelliSense und andere Verbesserungen, einschließlich besserer Leistung, Formatierung und Smarttags.

Der folgende Screenshot veranschaulicht die Verwendung des Bootstrap-Attributs IntelliSense im HTML-Editor.

![IntelliSense im HTML-Editor](release-notes/_static/image4.png)

Visual Studio 2013 enthält auch die integrierten coffeescript-und less-Editoren. Der less-Editor enthält alle coolen Features aus dem CSS-Editor und verfügt über bestimmte IntelliSense-Funktionen für Variablen und Mixins in allen weniger Dokumenten in der @import Kette.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Unterstützung von Web-Apps in Visual Studio Azure App Service

In Visual Studio 2013 mit dem Azure SDK für .NET 2,2 können Sie **Server-Explorer** verwenden, um direkt mit ihren Remote-Web-Apps zu interagieren. Sie können sich bei Ihrem Azure-Konto anmelden, neue Web-Apps erstellen, Apps konfigurieren, echt Zeitprotokolle anzeigen und vieles mehr. Bald nach der Veröffentlichung von SDK 2,2 können Sie in Azure Remote im Debugmodus ausführen. Die meisten der neuen Features für Azure App Service Web-Apps funktionieren auch in Visual Studio 2012, wenn Sie die aktuelle Version des Azure SDK für .NET installieren.

Weitere Informationen finden Sie in den folgenden Ressourcen:

- [Erstellen einer ASP.net-Web-App in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Problembehandlung in einer Web-App in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Erweiterungen für die Webveröffentlichung

Visual Studio 2013 enthält neue und verbesserte Webveröffentlichungs Features. Dies sind einige der folgenden:

- Automatisieren Sie mühelos die [Dateiverschlüsselung von Web. config](https://go.microsoft.com/fwlink/?LinkId=325529). (Dieser Link und die folgenden beiden weisen auf die Dokumentation auf MSDN hin, die möglicherweise erst am Ende des Tages am 10/17 verfügbar ist.)
- Einfaches [Automatisieren der Offline Schaltung einer Anwendung während der Bereitstellung](https://go.microsoft.com/fwlink/?LinkId=325530).
- Konfigurieren Sie Web deploy, um die [Datei Prüf Summe anstelle des letzten Änderungs Datums zu verwenden](https://go.microsoft.com/fwlink/?LinkId=325531) , um zu bestimmen, welche Dateien auf den Server kopiert werden sollen.
- Veröffentlichen Sie die einzelnen ausgewählten Dateien (einschließlich der Datei "Web. config") schnell, wenn Sie die FTP-oder Dateisystem-Veröffentlichungs Methoden sowie die Web deploy verwenden.

Weitere Informationen zur ASP.net-Webbereitstellung finden Sie auf [der ASP.NET-Website](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

Nuget 2,7 umfasst einen umfangreichen Satz neuer Features, die in den Anmerkungen zur [nuget-Version 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7)ausführlich beschrieben werden.

Diese nuget-Version entfernt auch das Bereitstellen der expliziten Zustimmung für das Paket Wiederherstellungs Feature von nuget, um Pakete herunterzuladen. Die Zustimmung (und das zugehörige Kontrollkästchen im Dialogfeld "Einstellungen" von nuget) wird jetzt durch die Installation von nuget erteilt. Nun funktioniert die Paket Wiederherstellung standardmäßig einfach.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>ASP.NET-Web Forms

### <a name="one-aspnet"></a>Eine ASP.net

Die Web Forms-Projektvorlagen integrieren sich nahtlos in die neue ASP.net-Funktion. Sie können Ihrem Web Forms Projekt MVC-und Web-API-Unterstützung hinzufügen, und Sie können die Authentifizierung mithilfe des Assistenten zum Erstellen einer ASP.net-Projekt Erstellung konfigurieren. Weitere Informationen finden Sie unter [Erstellen von ASP.NET-Webprojekten in Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Die Web Forms-Projektvorlagen unterstützen das neue ASP.net Identity Framework. Außerdem unterstützen die Vorlagen nun die Erstellung eines Web Forms intranetprojekts. Weitere Informationen finden Sie unter [Authentifizierungsmethoden](creating-web-projects-in-visual-studio.md#auth) zum **Erstellen von ASP.NET-Webprojekten in Visual Studio 2013**.

### <a name="bootstrap"></a>Bootstrap

Die Web Forms Vorlagen verwenden [Bootstrap](http://twitter.github.io/bootstrap/) , um ein schlankes und reaktionsfähiges Aussehen und Gefühl bereitzustellen, das Sie problemlos anpassen können. Weitere Informationen finden Sie unter [Bootstrap in den Visual Studio 2013-Webprojekt Vorlagen](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Eine ASP.net

Die Web-MVC-Projektvorlagen integrieren sich nahtlos in die neue ASP.NET-Benutzeroberflächen. Sie können Ihr MVC-Projekt anpassen und die Authentifizierung mithilfe des Assistenten für die Erstellung eines ASP.NET-Projekts konfigurieren. Ein Einführungs Tutorial für ASP.NET MVC 5 finden Sie unter [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Weitere Informationen zum Aktualisieren von MVC 4-Projekten auf MVC 5 finden Sie unter [Upgrade eines ASP.NET MVC 4-und Web-API-Projekts auf ASP.NET MVC 5 und Web-API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Die MVC-Projektvorlagen wurden aktualisiert, um ASP.net Identity für die Authentifizierung und Identitätsverwaltung zu verwenden. Ein Tutorial mit Facebook-und Google-Authentifizierung und der neuen Mitgliedschafts-API finden Sie unter [Erstellen einer ASP.NET MVC 5-App mit Facebook und Google OAuth2 und OpenID-Anmeldung](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) und [Erstellen einer ASP.NET MVC-App mit Authentifizierung und SQL-Datenbank und Bereitstellen in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap

Die MVC-Projektvorlage wurde aktualisiert, um [Bootstrap](http://getbootstrap.com/) zu verwenden, um ein schlankes und reaktionsfähiges Erscheinungsbild bereitzustellen, das Sie problemlos anpassen können. Weitere Informationen finden Sie unter [Bootstrap in den Visual Studio 2013-Webprojekt Vorlagen](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Authentifizierungs Filter

Authentifizierungs Filter sind eine neue Art von Filter in ASP.NET MVC, die vor Autorisierungs Filtern in der ASP.NET MVC-Pipeline ausgeführt werden und es Ihnen ermöglichen, die Authentifizierungs Logik pro Aktion, pro Controller oder global für alle Controller anzugeben. Authentifizierungs Filter verarbeiten Anmelde Informationen in der Anforderung und geben einen entsprechenden Prinzipal an. Authentifizierungs Filter können auch Authentifizierungs Herausforderungen als Reaktion auf nicht autorisierte Anforderungen hinzufügen.

### <a name="filter-overrides"></a>Filter Überschreibungen

Sie können jetzt überschreiben, welche Filter auf eine bestimmte Aktionsmethode oder einen bestimmten Controller angewendet werden, indem Sie einen Überschreibungs Filter angeben. Überschreibungs Filter geben einen Satz von Filtertypen an, die für einen bestimmten Bereich (Aktion oder Controller) nicht ausgeführt werden sollen. Dies ermöglicht es Ihnen, Filter zu konfigurieren, die Global angewendet werden, dann jedoch bestimmte globale Filter von der Anwendung auf bestimmte Aktionen oder Controller auszuschließen.

### <a name="attribute-routing"></a>Attributrouting

ASP.NET MVC unterstützt jetzt das Attribut Routing, dank eines Beitrags von Tim McCall, dem Autor von [http://attributerouting.net](http://attributerouting.net). Mit dem Attribut Routing können Sie Ihre Routen durch kommentieren ihrer Aktionen und Controller angeben.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET-Web-API 2

### <a name="attribute-routing"></a>Attributrouting

ASP.net-Web-API unterstützt jetzt das Attribut Routing, dank eines Beitrags von Tim McCall, dem Autor von [http://attributerouting.net](http://attributerouting.net). Mit dem Attribut Routing können Sie Ihre Web-API-Routen angeben, indem Sie Ihre Aktionen und Controller wie folgt kommentieren:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Das Attribut Routing ermöglicht Ihnen mehr Kontrolle über die URIs in Ihrer Web-API. Beispielsweise können Sie einfach eine Ressourcen Hierarchie mit einem einzigen API-Controller definieren:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Attribut Routing bietet auch eine bequeme Syntax zum Angeben optionaler Parameter, Standardwerte und Routen Einschränkungen:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Weitere Informationen zum Attribut Routing finden Sie unter [Attribut Routing in der Web-API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Die Projektvorlagen Web-API und Single-Page-Anwendung unterstützen jetzt die Autorisierung mithilfe von OAuth 2,0. OAuth 2,0 ist ein Framework für die Autorisierung des Client Zugriffs auf geschützte Ressourcen. Es funktioniert für eine Vielzahl von Clients, einschließlich Browsern und mobilen Geräten.

Die Unterstützung für OAuth 2,0 basiert auf der neuen Sicherheits Middleware, die von den Microsoft owin-Komponenten für die Träger Authentifizierung und die Implementierung der autorisierungsserver Rolle bereitgestellt wird. Alternativ können Clients mithilfe eines Organisations Autorisierungs Servers, z. b. Azure Active Directory oder ADFS in Windows Server 2012 R2, autorisiert werden.

### <a name="odata-improvements"></a>Odata

**Unterstützung für $SELECT, $Expand, $Batch und $Value**

ASP.net-Web-API odata bietet jetzt vollständige Unterstützung für $SELECT, $Expand und $Value. Sie können auch $Batch für die Anforderungs Batch Verarbeitung und Verarbeitung von Changesets verwenden.

Mit den Optionen $SELECT und $Expand können Sie die Form der Daten ändern, die von einem odata-Endpunkt zurückgegeben werden. Weitere Informationen finden Sie [unter Einführung in die $SELECT und $Expand Unterstützung in Web-API-odata](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Verbesserte Erweiterbarkeit**

Die odata-Formatierer sind jetzt erweiterbar. Sie können Atom-Eingabe Metadaten hinzufügen, benannte Stream-und Medien Link Einträge unterstützen, instanzanmerkungen hinzufügen und die Art und Weise anpassen, wie Links generiert werden.

**Typlose Unterstützung**

Sie können jetzt odata-Dienste erstellen, ohne CLR-Typen für die Entitäts Typen definieren zu müssen. Stattdessen können Ihre odata-Controller Instanzen von **iedmubject**verwenden oder zurückgeben, die die odata-Formatierer serialisieren/deserialisieren.

**Wieder verwenden eines vorhandenen Modells**

Wenn Sie bereits über ein vorhandenes Entity Data Model (EDM) verfügen, können Sie es jetzt direkt wieder verwenden, anstatt ein neues zu erstellen. Wenn Sie z. b. Entity Framework verwenden, können Sie das EDM verwenden, das EF für Sie erstellt.

### <a name="request-batching"></a>Anforderungs Batch Verarbeitung

Bei der Anforderungs Batch Verarbeitung werden mehrere Vorgänge in einer einzelnen HTTP POST-Anforderung kombiniert, um den Netzwerk Datenverkehr zu reduzieren und eine reibungslosere, weniger Benutzeroberfläche zu bieten. ASP.net-Web-API unterstützt jetzt mehrere Strategien für die Batch Verarbeitung von Anforderungen:

- Verwenden Sie den $Batch-Endpunkt eines odata-Dienstanbieter.
- Verpacken mehrerer Anforderungen in eine einzelne MIME-mehrteiligen-Anforderung.
- Verwenden Sie ein benutzerdefiniertes Batch Verarbeitungs Format.

Fügen Sie der Web-API-Konfiguration einfach eine Route mit einem Batch Verarbeitungs Handler hinzu, um die Anforderungs Batch Verarbeitung zu aktivieren:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

Sie können auch steuern, ob Anforderungen oder sequenziell oder in beliebiger Reihenfolge ausgeführt werden.

### <a name="portable-aspnet-web-api-client"></a>Portabler ASP.net-Web-API Client

Sie können jetzt den ASP.net-Web-API-Client verwenden, um Portable Klassenbibliotheken zu erstellen, die in Ihren Windows Store-und Windows Phone 8-Anwendungen funktionieren. Sie können auch portablen Formatierer erstellen, die auf Client und Server freigegeben werden können.

### <a name="improved-testability"></a>Verbesserte Testability

Web-API 2 macht es viel einfacher, Komponententests für Ihre API-Controller durchzusetzen. Instanziieren Sie einfach Ihren API-Controller mit ihrer Anforderungs Nachricht und-Konfiguration, und wählen Sie dann die Aktionsmethode aus, die Sie testen möchten. Es ist auch leicht, die **Urlhelper** -Klasse für Aktionsmethoden zu simulieren, die eine Link Generierung ausführen.

### <a name="ihttpactionresult"></a>IHttpActionResult

Sie können nun ihttpactionresult implementieren, um das Ergebnis Ihrer Web-API-Aktionsmethoden zu kapseln. Ein von einer Web-API-Aktionsmethode zurück gegebenes ihttpactionresult wird von der ASP.net-Web-API Runtime ausgeführt, um die resultierende Antwortnachricht zu erzeugen. Ein ihttpactionresult kann von jeder Web-API-Aktion zurückgegeben werden, um Komponententests Ihrer Web-API-Implementierung zu vereinfachen. Zur einfacheren Bereitstellung werden eine Reihe von ihttpactionresult-Implementierungen bereitgestellt, einschließlich der Ergebnisse zum Zurückgeben bestimmter Statuscodes, formatierten Inhalts oder von Inhalten ausgehandelten Antworten.

### <a name="httprequestcontext"></a>HttpRequestContext

Mit dem neuen **httprequestcontext** wird ein beliebiger Status nachverfolgt, der an die Anforderung gebunden ist, aber nicht sofort über die Anforderung verfügbar ist. Beispielsweise können Sie mit **httprequestcontext** Routendaten, den Prinzipal, der der Anforderung zugeordnet ist, das Client Zertifikat, das **Urlhelper** -Element und den virtuellen Pfad Stamm erhalten. Sie können einfach einen **httprequestcontext** für Komponenten Testzwecke erstellen.

Da der Prinzipal für die Anforderung mit der Anforderung weitergeleitet wird, anstatt **Thread. CurrentPrincipal**zu verwenden, ist der Prinzipal nun während der gesamten Lebensdauer der Anforderung verfügbar, während er sich in der Web-API-Pipeline befindet.

### <a name="cors"></a>CORS

Dank eines weiteren großen Beitrags von Brock allen unterstützt ASP.net jetzt vollständig die Cross Origin Request Sharing (cors).

Die Browser Sicherheit verhindert, dass eine Webseite AJAX-Anforderungen an eine andere Domäne sendet. [Cors](http://www.w3.org/TR/cors/) ist ein W3C-Standard, der es einem Server ermöglicht, die Richtlinie für denselben Ursprung zu lockern. Mithilfe von cors kann ein Server einige Ursprungs übergreifende Anforderungen explizit zulassen und andere ablehnen.

Web-API 2 unterstützt jetzt cors, einschließlich automatischer Behandlung von Preflight-Anforderungen. Weitere Informationen finden Sie unter [Aktivieren von Ursprungs übergreifenden Anforderungen in ASP.net-Web-API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Authentifizierungs Filter

Authentifizierungs Filter sind eine neue Art von Filter in ASP.net-Web-API, die vor Autorisierungs Filtern in der ASP.net-Web-API Pipeline ausgeführt werden und es Ihnen ermöglichen, die Authentifizierungs Logik pro Aktion, pro Controller oder global für alle Controller anzugeben. Authentifizierungs Filter verarbeiten Anmelde Informationen in der Anforderung und geben einen entsprechenden Prinzipal an. Authentifizierungs Filter können auch Authentifizierungs Herausforderungen als Reaktion auf nicht autorisierte Anforderungen hinzufügen.

### <a name="filter-overrides"></a>Filter Überschreibungen

Sie können jetzt überschreiben, welche Filter auf eine bestimmte Aktionsmethode oder einen bestimmten Controller angewendet werden, indem Sie einen Überschreibungs Filter angeben. Überschreibungs Filter geben einen Satz von Filtertypen an, die für einen bestimmten Bereich (Aktion oder Controller) nicht ausgeführt werden sollen. Dies ermöglicht es Ihnen, globale Filter hinzuzufügen, aber dann einige von bestimmten Aktionen oder Controllern auszuschließen.

### <a name="owin-integration"></a>Owin-Integration

ASP.net-Web-API unterstützt nun owin vollständig und kann auf einem beliebigen owin-fähigen Host ausgeführt werden. Außerdem ist ein **hostauthenticationfilter** enthalten, der die Integration mit dem owin-Authentifizierungssystem ermöglicht.

Mit der owin-Integration können Sie die Web-API in Ihrem eigenen Prozess zusammen mit anderen owin-Middleware (z. b. signalr) selbst hosten. Weitere Informationen finden Sie unter [Verwenden von owin für Self-Host-ASP.net-Web-API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.net signalr 2,0

In den folgenden Abschnitten werden die Funktionen von signalr 2,0 beschrieben.

- [Basiert auf owin](#builtonowin)
- [Maphubs und mapconnection sind jetzt mapsignalr](#MapSignalR)
- [Domänen übergreifende Unterstützung](#crossdomain)
- [IOS-und Android-Unterstützung über MonoTouch und monodroid](#mobile)
- [Portabler .NET-Client](#portable)
- [Neues Self-Host-Paket](#selfhost)
- [Abwärts kompatible Serverunterstützung](#backwardcompat)
- [Entfernte Serverunterstützung für .NET 4,0](#remove40)
- [Senden einer Nachricht an eine Liste von Clients und Gruppen](#messagelist)
- [Senden einer Nachricht an einen bestimmten Benutzer](#sendtouser)
- [Bessere Unterstützung bei der Fehlerbehandlung](#errorhandling)
- [Einfachere Komponententests von Hubs](#unittesting)
- [JavaScript-Fehlerbehandlung](#javascripterror)

Ein Beispiel für das Upgrade eines vorhandenen 1. x-Projekts auf signalr 2,0 finden Sie unter [Aktualisieren eines signalr 1. x-Projekts](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Basiert auf owin

Signalr 2,0 wird vollständig auf [owin (Open Web Interface für .net)](http://owin.org/)erstellt. Diese Änderung führt dazu, dass der Setup Vorgang für signalr zwischen im Internet gehosteten und selbst gehosteten signalr-Anwendungen wesentlich konsistent ist, aber auch eine Reihe von API-Änderungen erforderte.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>Maphubs und mapconnection sind jetzt mapsignalr

Aus Gründen der Kompatibilität mit owin-Standards wurden diese Methoden in `MapSignalR`umbenannt. `MapSignalR` ohne Parameter aufgerufen wird, werden alle Hubs zugeordnet (wie `MapHubs` in Version 1. x); Wenn Sie einzelne **persistentconnection** -Objekte zuordnen möchten, geben Sie den Verbindungstyp als Typparameter und die URL-Erweiterung für die Verbindung als erstes Argument an.

Die `MapSignalR`-Methode wird in einer owin-Startklasse aufgerufen. Visual Studio 2013 enthält eine neue Vorlage für eine owin-Startklasse. gehen Sie folgendermaßen vor, um diese Vorlage zu verwenden:

1. Klicken Sie mit der rechten Maustaste auf das Projekt.
2. Wählen Sie **Hinzufügen**, **Neues Element... aus.**
3. Wählen Sie **owin-Startklasse aus**. Nennen Sie die neue Klasse **Startup.cs**.

In einer **Webanwendung** wird die owin-Startklasse, die die `MapSignalR`-Methode enthält, dann dem Startprozess von owin mithilfe eines Eintrags im Knoten Anwendungseinstellungen der Datei "Web. config" hinzugefügt, wie unten gezeigt.

In einer **selbst gehosteten Anwendung**wird die Startup-Klasse als Typparameter der `WebApp.Start`-Methode übergeben.

**Mapping von Hubs und Verbindungen in signalr 1. x (aus der globalen Anwendungsdatei in einer Webanwendung):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Zuordnung von Hubs und Verbindungen in signalr 2,0 (aus einer owin-Startklassen Datei):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

In einer **selbst gehosteten Anwendung**wird die Startup-Klasse als Typparameter für die `WebApp.Start`-Methode übergeben, wie unten gezeigt.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Domänen übergreifende Unterstützung

In signalr 1. x wurden Domänen übergreifende Anforderungen von einem einzelnen enablecrossdomain-Flag gesteuert. Dieses Flag hat sowohl JSONP-als auch cors-Anforderungen gesteuert. Um die Flexibilität zu erhöhen, wurde die gesamte cors-Unterstützung aus der Serverkomponente von signalr entfernt (JavaScript-Clients verwenden weiterhin üblicherweise cors, wenn erkannt wird, dass der Browser Sie unterstützt), und neue owin-Middleware wurde zur Unterstützung dieser Szenarios zur Verfügung gestellt.

Wenn in signalr 2,0 JSONP auf dem Client erforderlich ist (um Domänen übergreifende Anforderungen in älteren Browsern zu unterstützen), muss es explizit aktiviert werden, indem `EnableJSONP` für das `HubConfiguration` Objekt auf `true`festgelegt wird (siehe unten). JSONP ist standardmäßig deaktiviert, da es weniger sicher als cors ist.

Um die neue cors-Middleware in signalr 2,0 hinzuzufügen, fügen Sie Ihrem Projekt die `Microsoft.Owin.Cors` Bibliothek hinzu, und nennen Sie `UseCors` vor der signalr-Middleware, wie im folgenden Abschnitt gezeigt.

**Hinzufügen von "Microsoft. owin. cors" zu Ihrem Projekt**: Um diese Bibliothek zu installieren, führen Sie den folgenden Befehl in der Paket-Manager-Konsole aus:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Mit diesem Befehl wird dem Projekt die Version 2.0.0 des Pakets hinzugefügt.

**Aufrufen von usecors**

Die folgenden Code Ausschnitte veranschaulichen, wie Domänen übergreifende Verbindungen in signalr 1. x und 2,0 implementiert werden.

**Implementieren von Domänen übergreifenden Anforderungen in signalr 1. x (aus der globalen Anwendungsdatei)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementieren von Domänen übergreifenden Anforderungen in signalr 2,0 (aus C# einer Codedatei)**

Der folgende Code veranschaulicht, wie cors oder JSONP in einem signalr 2,0-Projekt aktiviert wird. Dieses Codebeispiel verwendet `Map` und `RunSignalR` anstelle von `MapSignalR`, sodass die cors-Middleware nur für die signalr-Anforderungen ausgeführt wird, die cors-Unterstützung benötigen (anstatt für den gesamten Datenverkehr an dem Pfad, der in `MapSignalR`angegeben ist). `Map` kann auch für alle anderen Middleware verwendet werden, die für ein bestimmtes URL-Präfix ausgeführt werden muss, und nicht für die

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>IOS-und Android-Unterstützung über MonoTouch und monodroid

Die Unterstützung für IOS-und Android-Clients wurde mithilfe von MonoTouch-und monodroid-Komponenten aus der [xamarin-Bibliothek](https://xamarin.com/)hinzugefügt. Weitere Informationen zur Verwendung finden Sie unter [Verwenden von xamarin-Komponenten](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Diese Komponenten werden im [xamarin Store](https://store.xamarin.com/) verfügbar sein, wenn die signalr RTW-Version verfügbar ist.

<a id="portable"></a># # # Portabler .NET-Client

Um die plattformübergreifende Entwicklung besser zu vereinfachen, wurden die Silverlight-, WinRT-und Windows Phone Clients durch einen einzelnen portablen .NET-Client ersetzt, der die folgenden Plattformen unterstützt:

- NET 4,5
- Silverlight 5
- WinRT (.net für Windows Store-Apps)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Neues Self-Host-Paket

Es gibt jetzt ein nuget-Paket, mit dem Sie die ersten Schritte mit signalr-Self-Host (signalr-Anwendungen, die in einem Prozess oder einer anderen Anwendung gehostet werden und nicht auf einem Webserver gehostet werden) vereinfachen. Um ein mit signalr 1. x erstelltes Self-Host-Projekt zu aktualisieren, entfernen Sie das Paket Microsoft. Aspnet. signalr. owin, und fügen Sie das Paket Microsoft. Aspnet. signalr. SelfHost hinzu. Weitere Informationen zu den ersten Schritten mit dem Self-Host-Paket finden Sie unter [Tutorial: Signalr-Self-Host-](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Abwärts kompatible Serverunterstützung

In früheren Versionen von signalr mussten die Versionen des signalr-Pakets, die auf dem Client und dem Server verwendet wurden, identisch sein. Um Thick-Client Anwendungen zu unterstützen, die nur schwer zu aktualisieren sind, unterstützt signalr 2,0 jetzt die Verwendung einer neueren Server Version mit einem älteren Client. **Hinweis: Signalr 2,0 unterstützt keine Server, die mit älteren Versionen mit neueren Clients erstellt wurden.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Entfernte Serverunterstützung für .NET 4,0

Signalr 2,0 hat die Unterstützung für die Server Interoperabilität mit .NET 4,0 verworfen. .NET 4,5 muss mit signalr 2,0-Servern verwendet werden. Es gibt noch einen .NET 4,0-Client für signalr 2,0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Senden einer Nachricht an eine Liste von Clients und Gruppen

In signalr 2,0 ist es möglich, eine Nachricht mithilfe einer Liste von Client-und Gruppen-IDs zu senden. Die folgenden Code Ausschnitte veranschaulichen, wie dies zu tun ist.

**Senden einer Nachricht an eine Liste von Clients und Gruppen mithilfe von persistentconnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Senden einer Nachricht an eine Liste von Clients und Gruppen mithilfe von Hubs**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Senden einer Nachricht an einen bestimmten Benutzer

Mit dieser Funktion können Benutzer angeben, welche Benutzer-ID auf einem irequest basiert, und zwar über eine neue Schnittstelle iuseridprovider:

**Die iuseridprovider-Schnittstelle**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Standardmäßig wird eine Implementierung verwendet, die den IPrincipal.Identity.Name des Benutzers als Benutzernamen verwendet.

In Hubs können Sie Nachrichten über eine neue API an diese Benutzer senden:

**Verwenden der Clients. User-API**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Bessere Unterstützung bei der Fehlerbehandlung

Benutzer können nun **hubexception aus jedem hubaufruf** auslösen. Der Konstruktor von **hubexception** kann eine Zeichen folgen Nachricht und ein Objekt zusätzliche Fehler Daten annehmen. Signalr serialisiert die Ausnahme automatisch und sendet Sie an den Client, wo Sie verwendet wird, um den Aufruf der hubmethode abzulehnen/fehlschlagen zu lassen.

Die Einstellung **detaillierte Hub-Ausnahmen anzeigen** hat keinen Einfluss darauf, dass **hubexception** zurück an den Client gesendet wird. Sie wird immer gesendet.

**Server seitiger Code, der zeigt, wie eine hubexception an den Client gesendet wird**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**JavaScript-Client Code, der auf eine vom Server gesendete hubexception zeigt**

[!code-html[Main](release-notes/samples/sample16.html)]

**.NET-Client Code, der auf eine vom Server gesendete hubexception zeigt**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Einfachere Komponententests von Hubs

Signalr 2,0 enthält eine Schnittstelle mit dem Namen "`IHubCallerConnectionContext`" auf Hubs, die das Erstellen von Pseudo Client seitigen aufrufen erleichtert. Die folgenden Code Ausschnitte veranschaulichen die Verwendung dieser Schnittstelle mit gängigen Test Umgebungen [xUnit.net](https://github.com/xunit/xunit) und in [der Praxis](https://code.google.com/p/moq/).

**Komponententests von signalr mit xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Komponententests von signalr mit der Verwendung von muq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>JavaScript-Fehlerbehandlung

In signalr 2,0 geben alle JavaScript-Fehler Behandlungs Rückrufe anstelle von unformatierten Zeichen folgen JavaScript-Fehler Objekte zurück. Dies ermöglicht signalr, umfangreichere Informationen zu ihren Fehler Handlern zu übertragen. Sie können die innere Ausnahme von der `source`-Eigenschaft des Fehlers erhalten.

**JavaScript-Client Code, der die Start. Fail-Ausnahme behandelt**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Neues ASP.net-Mitgliedschafts System

ASP.net Identity ist das neue Mitgliedschaftssystem für ASP.NET-Anwendungen. Mit ASP.net Identity können benutzerspezifische Profildaten problemlos in Anwendungsdaten integriert werden. ASP.net Identity können Sie auch das Persistenzmodell für Benutzerprofile in Ihrer Anwendung auswählen. Sie können die Daten in einer SQL Server Datenbank oder einem anderen Datenspeicher speichern, einschließlich der nosql-Datenspeicher, z. b. Azure Storage Tabellen. Weitere Informationen finden Sie unter [individuelle Benutzerkonten](creating-web-projects-in-visual-studio.md#indauth) in **Erstellen von ASP.NET-Webprojekten in Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Anspruchbasierte Authentifizierung

ASP.NET unterstützt jetzt die Anspruchs basierte Authentifizierung, bei der die Identität des Benutzers als Satz von Ansprüchen von einem vertrauenswürdigen Aussteller dargestellt wird. Benutzer können mit einem Benutzernamen und einem Kennwort authentifiziert werden, die in einer Anwendungsdatenbank verwaltet werden, oder mithilfe von Identitäts Anbietern sozialer Netzwerke (z. b. Microsoft-Konten, Facebook, Google, Twitter) oder die Verwendung von Organisations Konten über Azure Active Directory oder Active Directory-Verbunddienste (AD FS) (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integration in Azure Active Directory und Windows Server Active Directory

Sie können jetzt ASP.net-Projekte erstellen, die Azure Active Directory oder Windows Server Active Directory (AD) für die Authentifizierung verwenden. Weitere Informationen finden Sie unter [Organisations Konten](creating-web-projects-in-visual-studio.md#orgauth) in **Erstellen von ASP.NET-Webprojekten in Visual Studio 2013**.

### <a name="owin-integration"></a>Owin-Integration

Die ASP.NET-Authentifizierung basiert jetzt auf der owin-Middleware, die auf einem beliebigen owin-basierten Host verwendet werden kann. Weitere Informationen zu owin finden Sie im Abschnitt [Microsoft-owin-Komponenten](#TOC7) .

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Microsoft OWIN-Komponenten

[Open Web Interface for .net](http://owin.org/) (owin) definiert eine Abstraktion zwischen .net-Webservern und Webanwendungen. Owin entkoppelt die Webanwendung vom Server, sodass Webanwendungen Host agnostisch werden. Beispielsweise können Sie eine owin-basierte Webanwendung in IIS hosten oder Sie in einem benutzerdefinierten Prozess selbst hosten.

Zu den Änderungen, die in den Microsoft owin-Komponenten (auch als Katana-Projekt bezeichnet) eingeführt werden, gehören neue Server-und Host Komponenten, neue Hilfsbibliotheken und Middleware sowie neue Authentifizierungs Middleware.

Weitere Informationen zu owin und Katana finden Sie unter [Neues in owin und Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Hinweis: [Owin](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) -Anwendungen können im klassischen IIS-Modus nicht ausgeführt werden. Sie müssen im integrierten Modus ausgeführt werden.**

**Hinweis: [Owin](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) -Anwendungen müssen mit voller Vertrauenswürdigkeit ausgeführt werden.**

### <a name="new-servers-and-hosts"></a>Neue Server und Hosts

Mit dieser Version wurden neue Komponenten hinzugefügt, um selbst gehosteter Szenarios zu ermöglichen. Zu diesen Komponenten gehören die folgenden nuget-Pakete:

- **Microsoft.Owin.Host.HttpListener**. Stellt einen owin-Server bereit, der **HttpListener** verwendet, um auf HTTP-Anforderungen zu lauschen und Sie an die owin-Pipeline weiterzuleiten.
- **Microsoft. owin. Hosting** bietet eine Bibliothek für Entwickler, die eine owin-Pipeline in einem benutzerdefinierten Prozess selbst hosten möchten, z. b. eine Konsolenanwendung oder ein Windows-Dienst.
- **Owinhost**. Bietet eine eigenständige ausführbare Datei, die `Microsoft.Owin.Hosting` umschließt und es Ihnen ermöglicht, eine owin-Pipeline selbst zu hosten, ohne eine benutzerdefinierte Host Anwendung schreiben zu müssen.

Außerdem ermöglicht das `Microsoft.Owin.Host.SystemWeb`-Paket jetzt der Middleware, Hinweise für den **systemWeb** Server bereitzustellen, was darauf hinweist, dass die Middleware während einer bestimmten ASP.NET-Pipeline Phase aufgerufen werden soll. Diese Funktion ist besonders nützlich für die Authentifizierungs Middleware, die früh in der ASP.NET-Pipeline ausgeführt werden sollte.

### <a name="helper-libraries-and-middleware"></a>Hilfsbibliotheken und Middleware

Obwohl Sie owin-Komponenten nur mit den Funktions-und Typdefinitionen aus der owin-Spezifikation schreiben können, bietet das neue `Microsoft.Owin`-Paket einen benutzerfreundlicheren Satz von Abstraktionen. Dieses Paket kombiniert mehrere frühere Pakete (z. b. `Owin.Extensions``Owin.Types`) zu einem einzelnen, gut strukturierten Objektmodell, das dann problemlos von anderen owin-Komponenten verwendet werden kann. Tatsächlich verwendet die Mehrzahl der Microsoft owin-Komponenten jetzt dieses Paket.

> [!NOTE]
> [Owin](http://www.owin.org) -Anwendungen können im klassischen IIS-Modus nicht ausgeführt werden. Sie müssen im integrierten Modus ausgeführt werden.

> [!NOTE]
> [Owin](http://www.owin.org) -Anwendungen müssen mit voller Vertrauenswürdigkeit ausgeführt werden.

Diese Version enthält auch das Microsoft. owin. Diagnostics-Paket, das Middleware zum Überprüfen einer laufenden owin-Anwendung sowie fehlerseitenmiddleware zum Untersuchen von Fehlern enthält.

### <a name="authentication-components"></a>Authentifizierungs Komponenten

Die folgenden Authentifizierungs Komponenten sind verfügbar.

- **Microsoft.Owin.Security.ActiveDirectory**. Aktiviert die Authentifizierung mithilfe lokaler oder cloudbasierter Verzeichnisdienste.
- **Microsoft. owin. Security. Cookies** ermöglicht die Authentifizierung mithilfe von Cookies. Dieses Paket wurde zuvor `Microsoft.Owin.Security.Forms`benannt.
- **Microsoft. owin. Security. Facebook** ermöglicht die Authentifizierung über den OAuth-basierten Dienst von Facebook.
- **Microsoft. owin. Security. Google** ermöglicht die Authentifizierung mithilfe des OpenID-basierten Diensts von Google.
- **Microsoft. owin. Security. JWT** ermöglicht die Authentifizierung mithilfe von JWT-Token.
- **Microsoft.Owin.Security.MicrosoftAccount** Enables authentication using Microsoft accounts.
- **Microsoft.Owin.Security.OAuth**. Stellt einen OAuth-autorisierungsserver und Middleware zum Authentifizieren von bearertoken bereit.
- **Microsoft. owin. Security. Twitter** ermöglicht die Authentifizierung über den OAuth-basierten Dienst von Twitter.

Diese Version enthält auch das `Microsoft.Owin.Cors`-Paket, das Middleware für die Verarbeitung von Ursprungs übergreifenden HTTP-Anforderungen enthält.

> [!NOTE]
> Die Unterstützung für JWT-Signierung wurde in der endgültigen Version von Visual Studio 2013 entfernt.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Eine Liste der neuen Features und anderen Änderungen in Entity Framework 6 finden Sie unter [Entity Framework Versionsverlauf](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.net Razor 3

ASP.net Razor 3 umfasst die folgenden neuen Features:

- Unterstützung für die Registerkarten Bearbeitung. Zuvor funktionierte der Befehl zum **Formatieren von Dokumenten** , der automatische Einzug und die automatische Formatierung in Visual Studio bei Verwendung der Option Tabstopps **beibehalten** nicht ordnungsgemäß. Diese Änderung korrigiert die Visual Studio-Formatierung für Razor-Code für die Tabulator Formatierung.
- Unterstützung für URL-Rewrite-Regeln beim Generieren von Verknüpfungen.
- Entfernen des Sicherheits transparenten Attributs.
  > [!NOTE]
  > Dabei handelt es sich um eine Breaking Change, die Razor 3 inkompatibel mit MVC4 und früher ist, während Razor 2 nicht mit MVC5 oder Assemblys kompatibel ist, die für MVC5 kompiliert wurden.

Razor 3-Probleme, die in Visual Studio 2013 von vorab Versionen behoben wurden, finden Sie [hier](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>ASP.net App aussetzen

ASP.net App Suspend ist ein Feature für die spieländerung in der .NET Framework 4.5.1, das die Benutzer Funktionalität und das wirtschaftliche Modell für das Hosting einer großen Anzahl von ASP.NET-Sites auf einem einzelnen Computer drastisch ändert. Weitere Informationen finden Sie unter [ASP.net App Suspend – reaktivem](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx), frei gegebenes .net-Webhosting.

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und wichtige Änderungen

In diesem Abschnitt werden bekannte Probleme und wichtige Änderungen im ASP.net and Web Tools für Visual Studio 2013 beschrieben.

### <a name="nuget"></a>NuGet

- Die [neue Paket Wiederherstellung funktioniert nicht bei Mono, wenn die SLN-Datei verwendet](https://nuget.codeplex.com/workitem/3596) wird – wird in einem bevorstehenden nuget. exe-Download und [nuget. commandline-Paket](http://www.nuget.org/packages/NuGet.CommandLine/) Update korrigiert.
- Die [neue Paket Wiederherstellung funktioniert nicht mit WiX-Projekten](https://nuget.codeplex.com/workitem/3598) – wird in einem bevorstehenden nuget. exe-Download und [nuget. commandline-Paket](http://www.nuget.org/packages/NuGet.CommandLine/) Update korrigiert.
- Die [Automatische Paket Wiederherstellung funktioniert nicht für Projekte in einem Projektmappenordner](https://nuget.codeplex.com/workitem/3625) – wird in nuget 2,8 korrigiert.

### <a name="aspnet-web-api"></a>ASP.NET-Web-API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` gibt `IQueryable<T>` nicht immer zurück, da die Unterstützung für `$select` und `$expand`hinzugefügt wurde.

    In den vorherigen Beispielen für `ODataQueryOptions<T>` der Rückgabewert von `ApplyTo` stets in `IQueryable<T>`umgewandelt. Dies hat früher funktioniert, weil die Abfrage Optionen, die wir zuvor unterstützten (`$filter`, `$orderby`, `$skip`, `$top`), nicht die Form der Abfrage ändern. Da wir jetzt `$select` unterstützen und `$expand` der Rückgabewert von `ApplyTo` nicht immer `IQueryable<T>` werden.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Wenn Sie den obigen Beispielcode verwenden, funktioniert er weiterhin, wenn der Client keine `$select` und `$expand`sendet. Wenn Sie jedoch `$select` und `$expand` unterstützen möchten, müssen Sie diesen Code in diese ändern.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **"Request. URL" oder "RequestContext. URL" ist während einer Batch Anforderung NULL.**

    In einem Batch Verarbeitungs Szenario ist **Urlhelper** beim Zugriff über " **Request. URL** " oder " **RequestContext. URL**" NULL.

    Dieses Problem wird zurzeit nachverfolgt: " [Batchrequestcontext. URL" ist für die Batch Verarbeitungs Anforderung NULL](http://aspnetwebstack.codeplex.com/workitem/1301).

    Um dieses Problem zu umgehen, können Sie eine neue Instanz von **Urlhelper**erstellen, wie im folgenden Beispiel gezeigt:

    **Erstellen einer neuen Instanz von Urlhelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Wenn Sie Übersichten verfügen, für die die Überprüfung des antiforgertokens durchzuführen ist, kann bei Verwendung von MVC5 und orgauth der folgende Fehler auftreten, wenn Sie Daten in der Ansicht bereitstellen:

    **Fehler**:

    *Server Fehler in "/"-Anwendung.*

    <em>ein Anspruch vom Typ "<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>" oder "<https://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>" in der angegebenen "ClaimsIdentity" nicht vorhanden ist. Überprüfen Sie, ob der konfigurierte Anspruchs Anbieter beide Ansprüche für die von ihm generierten ClaimsIdentity-Instanzen bereitstellt, um die Unterstützung für antifälschungstokenunterstützung mit Anspruchs basierter Authentifizierung zu aktivieren. Wenn der konfigurierte Anspruchs Anbieter stattdessen einen anderen Anspruchstyp als eindeutigen Bezeichner verwendet, kann er durch Festlegen der statischen Eigenschaft antiforgeryconfig. uniqueclaimtypeidentifier konfiguriert werden.</em>

    **Problemumgehung:**

    Fügen Sie die folgende Zeile in "Global. asax" hinzu, um Sie zu beheben:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Dies wird für die nächste Version korrigiert.
2. Nachdem Sie eine MVC4-App auf MVC5 aktualisiert haben, erstellen Sie die Projekt Mappe, und starten Sie Sie. Der folgende Fehler wird angezeigt:

    Ein System. Web. Webseiten. Razor. Configuration. hostsection kann nicht in [B] System. Web. Webseiten. Razor. Configuration. hostsection umgewandelt werden. Type A stammt von "System. Web. Webseiten". Razor, Version = 2.0.0.0, Culture = neutral, PublicKeyToken = 31bfi3856ad364e35 "im Kontext" Default "am Speicherort" c:\WINDOWS\Microsoft.net\assembly\gac\_MSIL\System.Web.Webpages.Razor\v4.0\_2.0.0.0\_\_31bfi3856ad364e35 \ System. Web. Webseiten. Razor. dll ". Typ B stammt aus System. Web. Webseiten. Razor, Version = 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' im Kontext ' default ' am Speicherort ' C:\WINDOWS\Microsoft.Net\Framework\v4.0.30319\Temporary ASP.net files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01 \ System. Web. Webseiten. Razor. dll '.

    Um den obigen Fehler zu beheben, öffnen Sie *alle* Web. config-Dateien (einschließlich der Dateien im Ordner "Views") in Ihrem Projekt, und führen Sie die folgenden Schritte aus:

   1. Aktualisieren Sie alle Vorkommen der Version "4.0.0.0" von "System. Web. MVC" auf "5.0.0.0".
   2. Aktualisieren Sie alle Vorkommen der Version "2.0.0.0" von "System. Web. Hilfsprogramme", &quot;System. Web. Webseiten&quot; und &quot;System. Web. Webseiten. Razor&quot; in "3.0.0.0".

      Nachdem Sie z. b. die oben genannten Änderungen vorgenommen haben, sollten die Assemblybindungen wie folgt aussehen:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Weitere Informationen zum Aktualisieren von MVC 4-Projekten auf MVC 5 finden Sie unter [Upgrade eines ASP.NET MVC 4-und Web-API-Projekts auf ASP.NET MVC 5 und Web-API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Wenn die Client seitige Validierung mit der unaufdringlichen jquery-Validierung verwendet wird, ist die Validierungs Meldung für ein HTML-Eingabe Element mit dem Typ "Number" manchmal falsch. Der Überprüfungs Fehler für einen erforderlichen Wert ("das Alters Feld ist erforderlich") wird angezeigt, wenn eine ungültige Zahl eingegeben wird, anstelle der korrekten Meldung, dass eine gültige Zahl erforderlich ist.

    Dieses Problem wird häufig mit einem Gerüst Code für ein Modell mit einer ganzzahligen Eigenschaft in den Ansichten erstellen und bearbeiten gefunden.

    Um dieses Problem zu umgehen, ändern Sie das Editor-Hilfsprogramm von:

    `@Html.EditorFor(person => person.Age)`

    Nach:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 unterstützt keine teilweise Vertrauenswürdigkeit mehr. Projekte, die mit den MVC-oder WebAPI-Binärdateien verknüpft sind, sollten das [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) -Attribut und das [allowpartiallytrust dcaller](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) -Attribut entfernen. Wenn Sie diese Attribute entfernen, werden Compilerfehler wie die folgenden vermieden.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Beachten Sie, dass Sie als Nebeneffekt nicht 4,0-und 5,0-Assemblys in derselben Anwendung verwenden können. Sie müssen alle auf 5,0 aktualisieren.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Die Spa-Vorlage mit Facebook-Autorisierung kann zu Instabilität in IE führen, während die Website in der Intranetzone gehostet wird

Die Spa-Vorlage bietet eine externe Anmeldung mit Facebook. Wenn das mit der Vorlage erstellte Projekt lokal ausgeführt wird, kann das Anmelden dazu führen, dass der Internet Explorer abstürzt.

Projektmappe:

1. Hosten der Website in der Internet Zone noch

2. Testen Sie das Szenario in einem anderen Browser als IE.

### <a name="web-forms-scaffolding"></a>Web Forms Gerüstbau

Web Forms Gerüstbau wurde aus VS2013 entfernt und ist in einem zukünftigen Update von Visual Studio verfügbar. Sie können Gerüstbau jedoch weiterhin in einem Web Forms Projekt verwenden, indem Sie MVC-Abhängigkeiten hinzufügen und Gerüstbau für MVC erstellen. Das Projekt enthält eine Kombination aus Web Forms und MVC.

Wenn Sie Ihrem Web Forms Projekt MVC hinzufügen möchten, fügen Sie ein neues Gerüst Element hinzu, und wählen Sie **MVC 5-Abhängigkeiten**aus. Wählen Sie entweder minimal oder vollständig aus, je nachdem, ob Sie alle Inhalts Dateien, z. b. Skripts, benötigen. Fügen Sie dann ein Gerüst Element für MVC hinzu, mit dem Sichten und ein Controller in Ihrem Projekt erstellt werden.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC-und Web-API-Gerüstbau: Fehler HTTP 404, nicht gefunden

Wenn ein Fehler beim Hinzufügen eines gerüstten Elements zu einem Projekt auftritt, wird das Projekt möglicherweise in einem inkonsistenten Zustand belassen. Für einige der Änderungen, die an Gerüstbau vorgenommen werden, wird ein Rollback ausgeführt, aber für andere Änderungen, wie z. b Wenn für die Routing Konfigurationsänderungen ein Rollback ausgeführt wird, erhalten Benutzer einen HTTP 404-Fehler, wenn Sie zu Gerüst Elementen navigieren.

Problemumgehung:

- Um diesen Fehler für MVC zu beheben, fügen Sie ein neues Gerüst Element hinzu, und wählen Sie MVC 5-Abhängigkeiten (Minimal oder vollständig) aus. Durch diesen Vorgang werden alle erforderlichen Änderungen in Ihrem Projekt hinzugefügt.
- So beheben Sie diesen Fehler für die Web-API:

  1. Fügen Sie dem Projekt die webapiconfig-Klasse hinzu.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Konfigurieren Sie webapiconfig. Register in der Anwendung\_Start-Methode in Global. asax wie folgt:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
