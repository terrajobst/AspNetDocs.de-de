---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.net and Web Tools 2013,2 für Visual Studio 2013 Anmerkungen zu dieser Version | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: riande
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 22d4d4afd6963f23d6cfef1745a859c20b69d599
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505449"
---
# <a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET and Web Tools 2013.2 für Visual Studio 2013 – Anmerkungen zu dieser Version

von [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Installationshinweise

ASP.net and Web Tools für Visual Studio 2013,2 sind im Hauptinstallations Programm gebündelt und können im Rahmen [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)heruntergeladen werden.

## <a name="documentation"></a>Dokumentation

Lernprogramme und weitere Informationen zu ASP.net and Web Tools für Visual Studio 2013,2 stehen auf der [ASP.NET-Website](https://www.asp.net/)zur Verfügung.

## <a name="software-requirements"></a>Softwareanforderungen

ASP.net and Web Tools für Visual Studio 2013,2 erfordert Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Neue Features in ASP.net and Web Tools für Visual Studio 2013,2

In den folgenden Abschnitten werden die Features beschrieben, die in der-Version eingeführt wurden.

- [One ASP.net-Projektvorlagen](#oneaspnet)
- [SSL-Unterstützung beim Starten von Webanwendungen auf IIS Express](#ssl)
- [Erweiterungen von Visual Studio-Web-Editor](#vswebeditor)
- [Browserverknüpfung](#browserlink)
- [Unterstützung für Azure App Service-Web-Apps in Visual Studio](#waws)
- [Erstellen von Azure-Remote Ressourcen beim Erstellen eines neuen Webprojekts](#AzureResources)
- [Erweiterungen für die Webveröffentlichung](#webpublish)
- [ASP.net Gerüstbau](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [ASP.net Web Forms](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.net-Web-API 2.1.2](#webapi)
- [ASP.net Web Pages 3.1.2](#webpages)
- [Entity Framework 6,1](#ef)
- [ASP.net Identity 2.0.0](#identity)
- [Microsoft-owin-Komponenten](#owin)
- [ASP.net signalr 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>One ASP.net-Projektvorlagen

- Aktualisierungen an ASP.net-Projektvorlagen zur Unterstützung der Konto Bestätigung und der Kenn Wort Zurücksetzung.
- Aktualisieren Sie ASP.net-Web-API Vorlage, um die Authentifizierung über lokale Organisations Konten zu unterstützen.
- Die ASP.net Spa-Vorlage enthält jetzt eine Authentifizierung, die auf MVC-und serverseitigen Ansichten basiert. Die Vorlage verfügt über einen WebAPI-Controller, auf den nur authentifizierte Benutzer zugreifen können.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>SSL-Unterstützung beim Starten von Webanwendungen auf IIS Express

Um die Sicherheitswarnung beim Durchsuchen und Debuggen von HTTPS auf "localhost" auszuschließen, haben wir ein Dialogfeld hinzugefügt, mit dem Internet Explorer und Chrome dem selbst signierten IIS Express-SSL-Zertifikat vertrauen können

Beispielsweise kann eine Webprojekt Eigenschaft für die Verwendung von SSL festgelegt werden. Klicken Sie auf F4, um das Eigenschaften Dialogfeld zu aktivieren. Ändern Sie **SSL** in "true". Kopieren Sie die SSL-URL.

![SSL aktiviert (Eigenschaft)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Legen Sie die webregister Karte Webprojekt-Eigenschaften Seite auf die HTTPS-basierte URL fest (die SSL-URL wird `https://localhost:44300/`, es sei denn, Sie haben zuvor SSL-Websites erstellt).

![Festlegen der Projekt-URL (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Drücken Sie STRG+F5, um die Anwendung auszuführen. Befolgen Sie die Anweisungen, um dem von IIS Express generierten selbst signierten Zertifikat zu vertrauen.

![SSL-Warnung](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Lesen Sie das Dialogfeld **Sicherheitswarnung** , und klicken Sie dann auf **Ja** , wenn Sie das Zertifikat installieren möchten, das localhost darstellt.

![Sicherheitshinweis](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Die Website wird in IE oder Chrome ohne die Zertifikat Warnung im Browser angezeigt.

![HTTPS-Seite ohne Warnungen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox verwendet einen eigenen Zertifikat Speicher, sodass eine Warnung angezeigt wird.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Erweiterungen von Visual Studio-Web-Editor

- **Neues JSON-Projekt Element und-Editor**: Wir haben Visual Studio ein JSON-Projekt Element und einen Editor hinzugefügt. Zu den aktuellen JSON-Editor-Features gehören Farbgebung, Syntax Validierung, Klammer Vervollständigung, Gliederung, Options Einstellung für Extras und mehr.

    ![JSON-Editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense unterstützt jetzt [JSON-Schema](http://json-schema.org/) V3 und v4. Es gibt ein Schema-Kombinations Feld, in dem vorhandene Schemas ausgewählt, der Pfad des lokalen Schemas bearbeitet oder einfach eine Projekt-JSON-Datei auf die Datei gezogen werden kann, um den relativen Pfad zu erhalten.

    ![JSON-IntelliSense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![JSON-Schema-Editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **New Sass (scss)-Editor**: Wir haben weniger in VS2013 RTM hinzugefügt, und wir verfügen nun über ein Sass-Projekt Element und einen Editor. Die Sass-Editor-Features sind mit dem less-Editor vergleichbar und enthalten farbliche Farbgebung, Variable und Mixins IntelliSense, Kommentar/uncomment, Quick Info, Formatierung, Syntax Validierung, Gliederung, gehe zu Definition, Farbauswahl, Extras Options Einstellung usw.

    ![Neues Element hinzufügen: scss-Stylesheet](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Stylesheet-Editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Neue URL-Auswahl in HTML-, Razor-, CSS-, Less-und sass-Dokumenten:** VS 2013 ohne URL-Auswahl außerhalb Web Forms Seiten ausgeliefert. Die neue URL-Auswahl für HTML-, Razor-, CSS-, Less-und sass-Editoren ist eine Dialog freie, überflüssige Typisierungs Auswahl, die ".." versteht. und filtert Dateilisten entsprechend für IMG-Tags und Verknüpfungen.

    ![URL-Auswahl für imagetag](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![URL-Auswahl für Sichten](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![URL-Auswahl für CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Aktualisierungen von less Editor durch Hinzufügen von weiteren Features**
- **Knockout IntelliSense-Upgrade**: Wir haben eine nicht standardmäßige Knockout-Syntax für vs IntelliSense, "Ko-vs-Editor ViewModel:"-Syntax, hinzugefügt. Sie kann verwendet werden, um mithilfe von Kommentaren in der Form an mehrere Ansichts Modelle auf einer Seite zu binden:

    ![Knockout IntelliSense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    Wir haben auch Unterstützung für das geschachtelte ViewModel-IntelliSense hinzugefügt, sodass Sie in tief geschachtelte Objekte im ViewModel weiter suchen können.

    `<div data-bind="text: foo.bar.baz.etc" />`

    Die angezeigte IntelliSense-Version ist die vollständige IntelliSense-Version des JavaScript-Objekts.

    ![IntelliSense mit vollständigem JavaScript-Objekt](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Neue URL-Auswahl in HTML-, Razor-, CSS-, Less-und sass-Dokumenten**: vs 2013 ohne URL-Auswahl außerhalb der Web Forms Seiten. Die neue URL-Auswahl für HTML-, Razor-, CSS-, Less-und sass-Editoren ist eine Dialog freie, überflüssige Typisierungs Auswahl, die ".." versteht. und filtert Dateilisten entsprechend für IMG-Tags und Verknüpfungen.

    ![URL-Auswahl für imagetag](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![URL-Auswahl für Sichten](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![URL-Auswahl für CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Browserverknüpfung

- Browser Link unterstützt jetzt HTTPS-Verbindungen und listet diese im Dashboard mit anderen Verbindungen auf, solange das Zertifikat vom Browser als vertrauenswürdig eingestuft wird.
- Statische HTML-Quell Zuordnung
- Spa-Unterstützung zum Mapping von Daten
- Zuordnung von Daten automatisch aktualisieren

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Unterstützung für Azure App Service-Web-Apps in Visual Studio

- **Unterstützung der Azure-Anmeldung.**
- **Remote Debuggen und Remote Ansicht für Web-Apps**: Wir unterstützen jetzt das [Remote Debuggen für Web-Apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) und Remote Ansicht von Web-App-Inhalts Dateien im Server-Explorer.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Erstellen von Azure-Remote Ressourcen beim Erstellen eines neuen Webprojekts

Das Kontrollkästchen ["Remote Ressourcen erstellen"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) im Dialogfeld "neue Webanwendung" wurde hinzugefügt. Durch Auswahl dieser Option können Sie die Benutzerumgebung für das Erstellen einer neuen Webanwendung, das Einrichten der Azure-Veröffentlichungs Website zum Testen und das Erstellen eines Veröffentlichungs Profils in wenigen einfachen Schritten integrieren.

![Neues Projekt mit Azure-Ressourcen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Veröffentlichen in Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Erweiterungen für die Webveröffentlichung

- Verbessern Sie die Benutzer Leistung für die Veröffentlichung.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.net Gerüstbau

- Aufzählungs **Unterstützung:** Wenn Ihr Modell enums verwendet, generiert das MVC-Gerüst die Dropdown Liste für die Enum. Dabei werden die Aufzählungs Hilfen in MVC verwendet.
- **Bootstrap-Unterstützung**: aktualisierte EditorFor-Vorlagen in MVC-Gerüstbau, sodass Sie die Bootstrap-Klassen verwenden.
- **Paket Unterstützung**: MVC-und Web-API-Gerüsts fügen 5,1-Pakete für MVC und die Web-API hinzu.

Die folgenden Screenshots veranschaulichen Gerüstbau Modelle.

- Modellcode:

     ![Modellcode](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Kompilieren Sie den Modellcode, klicken Sie mit der rechten Maustaste, und wählen Sie **Hinzufügen**, **Neues Gerüst Element**aus.

     ![Neues Gerüst Element hinzufügen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Wählen Sie **MVC5 Controller mit Ansichten unter Verwendung Entity Framework**aus:

     ![Neuen MVC5-Controller mit Ansichten hinzufügen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- Controller mit dem Modell **Hinzufügen** :

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Überprüfen Sie den generierten Code, z. b. views/weekdaymodels/Edit. cshtml enthält `@Html.EnumDropDownListFor`: ![Sicht mit enumdropdownlistfor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Führen Sie die Seite aus, um das generierte enumbobox Enumeration anzuzeigen. Beachten Sie, dass eine leere Zeichenfolge für das Kombinations Feld ausgewählt werden kann, wenn ein Wert NULL sein kann. Die Seite **Erstellen** zeigt z. b. Folgendes:

    ![Kombinations Feld mit leerer Zeichenfolge](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

Nuget 2.8.1 RTM wird im April 2014 veröffentlicht. Im folgenden finden Sie die wichtigsten Punkte der Anmerkungen zu dieser Version. Weitere Informationen zu diesen Änderungen finden Sie in den Anmerkungen zu dieser [Version](http://docs.nuget.org/docs/release-notes/nuget-2.8) .

- **Ziel Windows Phone 8,1-Anwendungen**: nuget 2.8.1 unterstützt jetzt das Ausrichten von Windows Phone 8,1-Anwendungen mithilfe der zielframeworkmoniker "windowsphoneapp", "WPA", "WindowsPhoneApp81" und "WPA81".

- **Patchauflösung für Abhängigkeiten**: beim Auflösen von Paketabhängigkeiten hat nuget in der Vergangenheit eine Strategie zum Auswählen der niedrigsten Haupt-und neben Paketversion implementiert, die die Abhängigkeiten des Pakets erfüllt. Anders als bei der Haupt-und neben Version wurde die Patchversion jedoch immer auf die höchste Version aufgelöst. Obwohl das Verhalten wohl gemeint war, wurde für die Installation von Paketen mit Abhängigkeiten ein fehlender Determinismus geschaffen.
- **Dependencyversion-Switch**: Obwohl nuget 2,8 das *Standard* Verhalten für das Auflösen von Abhängigkeiten ändert, wird die Abhängigkeitsauflösung durch den Schalter-dependencyversion in der Paket-Manager-Konsole genauer gesteuert. Der-Switch ermöglicht das Auflösen von Abhängigkeiten zur niedrigsten möglichen Version (Standardverhalten), der höchstmöglichen Version oder der höchsten neben Version bzw. der höchsten Version des Patches. Dieser Switch funktioniert nur für install-Package im PowerShell-Befehl.
- **Dependencyversion-Attribut**: Zusätzlich zum oben beschriebenen-dependencyversion-Schalter hat nuget auch die Möglichkeit, ein neues Attribut in der Datei "nuget. config" festzulegen, wobei der Standardwert festgelegt ist, wenn der Schalter "-dependencyversion" nicht in einem Aufruf von "Install-Package" angegeben ist. Dieser Wert wird auch vom Dialog Feld nuget-Paket-Manager für alle Installationspaket Vorgänge beachtet. Fügen Sie der Datei "nuget. config" das unten angegebene-Attribut hinzu, um diesen Wert festzulegen:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Vorschau von nuget-Vorgängen mit-WhatIf**: einige nuget-Pakete können über Deep-Abhängigkeits Diagramme verfügen. Daher kann es bei einem Installations-, Deinstallations-oder Aktualisierungs Vorgang hilfreich sein, um zunächst festzustellen, was passiert. Nuget 2,8 fügt den standardmäßigen PowerShell-Befehl zum Installieren von Paketen, Deinstallieren von Paketen und Update Paketen hinzu, um die Visualisierung des gesamten Abschlusses von Paketen zu ermöglichen, auf die der Befehl angewendet wird.
- **Downgrade-Paket**: Es ist nicht ungewöhnlich, eine Vorabversion eines Pakets zu installieren, um neue Funktionen zu untersuchen und dann ein Rollback auf die letzte stabile Version auszuführen. Vor nuget 2,8 war dies ein mehrstufiger Prozess der Deinstallieren des vorab Pakets und seiner Abhängigkeiten und der anschließenden Installation der früheren Version. Mit nuget 2,8 führt das Update-Package nun jedoch ein Rollback für den gesamten Paket Abschluss (z. b. die Abhängigkeitsstruktur des Pakets) zur vorherigen Version aus.
- **Entwicklungs Abhängigkeiten**: viele verschiedene Arten von Funktionen können als nuget-Pakete übermittelt werden, einschließlich Tools, die zur Optimierung des Entwicklungsprozesses verwendet werden. Diese Komponenten, während Sie für die Entwicklung eines neuen Pakets wichtig sein können, sollten beim späteren veröffentlichen nicht als Abhängigkeit des neuen Pakets angesehen werden. Nuget 2,8 ermöglicht einem Paket, sich selbst in der nuspec-Datei als developmentdependenz zu identifizieren. Bei der Installation werden diese Metadaten auch der Datei "Packages. config" des Projekts hinzugefügt, in dem das Paket installiert wurde. Wenn diese Datei "Packages. config" später für nuget-Abhängigkeiten während des Pakets "nuget. exe" analysiert wird, werden diese Abhängigkeiten ausgeschlossen, die als Entwicklungs Abhängigkeiten gekennzeichnet sind.
- **Einzelne Packages. config-Dateien für verschiedene Plattformen**: Wenn Sie Anwendungen für mehrere Zielplattformen entwickeln, werden für jede der jeweiligen Buildumgebungen häufig verschiedene Projektdateien erstellt. Es ist auch üblich, dass unterschiedliche nuget-Pakete in verschiedenen Projektdateien verwendet werden, da Pakete unterschiedliche Ebenen der Unterstützung für verschiedene Plattformen aufweisen. Nuget 2,8 bietet eine verbesserte Unterstützung für dieses Szenario, indem unterschiedliche Pakete. config-Dateien für verschiedene plattformspezifische Projektdateien erstellt werden.
- **Fallback zum lokalen Cache**: Obwohl nuget-Pakete in der Regel aus einem Remote Katalog wie dem [nuget](http://www.nuget.org) -Katalog über eine Netzwerkverbindung genutzt werden, gibt es viele Szenarien, in denen der Client nicht verbunden ist. Ohne eine Netzwerkverbindung konnte der nuget-Client Pakete nicht erfolgreich installieren, auch wenn sich diese Pakete bereits auf dem Client Computer im lokalen nuget-Cache befanden. Nuget 2,8 fügt der Paket-Manager-Konsole einen automatischen Fall Back für den Cache hinzu.

    Das Cache Fall Back Feature erfordert keine bestimmten Befehlsargumente. Außerdem funktioniert der Cache Fall Back zurzeit nur in der Paket-Manager-Konsole. das Verhalten funktioniert derzeit nicht im Dialogfeld Paket-Manager.
- **Fehlerbehebungen**: eine der wichtigsten Fehlerbehebungen war die Leistungsverbesserung im Befehl Update-Package-REINSTALL.

    Zusätzlich zu diesen Features und der erwähnten Leistungs Korrektur enthält diese Version von nuget auch viele andere Fehlerbehebungen. In der Version wurden 181 Gesamtprobleme behoben. Eine vollständige Liste der Arbeitselemente, die in nuget 2,8 korrigiert wurden, finden Sie in der [nuget-Problem](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) Verfolgung für diese Version.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>ASP.NET Web Forms

- Die Web Forms Vorlagen veranschaulichen nun, wie Sie die Konto Bestätigung und das Zurücksetzen des Kennworts für ASP.net Identity ausführen.
- Das Entitäts Datenquellen-Steuerelement und der dynamische Daten Anbieter für Entity Framework 6. Weitere Informationen finden Sie im folgenden MSDN-Blog: [dynamische Daten Provider und EntityDataSource-Steuerelement für Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Verbesserungen am Attribut](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Bootstrap-Unterstützung für Editor-Vorlagen](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Aufzählungs Unterstützung in Sichten](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Unaufdringliche Unterstützung für MinLength/MaxLength-Attribute](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Unterstützen des "This"-Kontexts in unaufdringlichem AJAX](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Verschiedene [Fehlerbehebungen](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.net-Web-API 2.1.2

- [Globale Fehlerbehandlung](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Erweiterungen für Attribut Routing](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Verbesserung der Hilfeseite](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Ignoreroute-Unterstützung](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Bson Media-Type-Formatierer](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Bessere Unterstützung für Async-Filter](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Abfrage Verarbeitung für die Client Formatierungs Bibliothek](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Verschiedene [Fehlerbehebungen](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.net Web Pages 3.1.2

- Verschiedene [Fehlerbehebungen](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6,1

Entity Framework wurde sowohl für die Laufzeit als auch für das Tool auf Version 6,1 aktualisiert. Entity Framework (EF) 6,1 ist ein kleineres Update für Entity Framework 6 und umfasst eine Reihe von Fehlerbehebungen und neuen Features. Ausführliche Informationen zu EF 6.1, einschließlich Links zur Dokumentation für die neuen Features, finden Sie unter [Entity Framework Versionsgeschichte](https://msdn.microsoft.com/data/jj574253). Die neuen Funktionen in dieser Version umfassen Folgendes:

- Die Tool **Konsolidierung** bietet eine konsistente Möglichkeit zum Erstellen eines neuen EF-Modells. Dieses Feature erweitert den ADO.NET Entity Data Model-Assistenten, um das Erstellen von Code First Modellen, einschließlich Reverse Engineering aus einer vorhandenen Datenbank, zu unterstützen. Diese Features waren zuvor in den EF Power Tools in der Beta Qualität verfügbar.
- Die **Behandlung von transaktionscommitfehlern** bietet den neuen [System. Data. Entity. Infrastructure. commitfailurehandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) , der die neu eingeführte Möglichkeit zum Abfangen von Transaktions Vorgängen nutzt. **Commitfailurehandler** ermöglicht die automatische Wiederherstellung nach Verbindungsfehlern beim Ausführen eines Commits für eine Transaktion.
- **Indexattribute** ermöglicht das Angeben von Indizes durch Platzieren eines Attributs für eine Eigenschaft (oder Eigenschaften) im Code First Modell. Code First erstellt dann einen entsprechenden Index in der Datenbank.
- **Die öffentliche Mapping-API** bietet Zugriff auf die Informationen, die EF für die Zuordnung von Eigenschaften und Typen zu Spalten und Tabellen in der Datenbank bietet. In früheren Versionen war diese API intern.
- **Die Möglichkeit, Interceptors über die APP/Web. config-Datei zu konfigurieren**(sodass Interceptors hinzugefügt werden können, ohne die Anwendung erneut zu kompilieren).
- **Databaselogger** ist ein neuer Interceptor, der es einfach macht, alle Daten Bank Vorgänge in einer Datei zu protokollieren. In Kombination mit der vorherigen Funktion können Sie auf diese Weise problemlos auf die Protokollierung von Daten Bank Vorgängen für eine bereitgestellte Anwendung umstellen, ohne dass eine erneute Kompilierung erforderlich ist.
- Die **migrationsmodell-Änderungs Erkennung** wurde verbessert, sodass das Gerüst für Migrationen genauer ist. die Leistung des Änderungs Erkennungsprozesses wurde ebenfalls erheblich verbessert.
- **Leistungsverbesserungen** einschließlich reduzierter Daten Bank Vorgänge während der Initialisierung, Optimierungen für den NULL-Gleichheits Vergleich in LINQ-Abfragen, schnellere Ansichts Generierung (Modell Erstellung) in weiteren Szenarien und effizientere Materialisierung von nach verfolgten Entitäten mit mehreren Zuordnungen.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.net Identity 2.0.0

- **Zweistufige Authentifizierung**: ASP.net Identity unterstützt jetzt die zweistufige Authentifizierung. Die zweistufige Authentifizierung bietet eine zusätzliche Sicherheitsebene für ihre Benutzerkonten, wenn Ihr Kennwort kompromittiert wird. Es gibt auch Schutz für Brute-Force-Angriffe auf die beiden Faktor Codes.
- **Kontosperrung:** Bietet eine Möglichkeit, den Benutzer zu sperren, wenn der Benutzer sein Kennwort oder zweistufige Codes nicht korrekt eingegeben hat. Die Anzahl der ungültigen Versuche und die Zeitspanne für die Benutzer können nicht konfiguriert werden. Ein Entwickler kann optional die Kontosperrung für bestimmte Benutzerkonten deaktivieren, wenn dies erforderlich ist.
- **Konto Bestätigung:** Das ASP.net Identity System unterstützt jetzt die Konto Bestätigung. Dies ist ein gängiges Szenario auf den meisten Websites, bei dem Sie, wenn Sie sich für ein neues Konto auf der Website registrieren, Ihre e-Mail-Adresse bestätigen müssen, bevor Sie etwas auf der Website ausführen können. Eine e-Mail-Bestätigung ist nützlich, da Sie verhindert, dass falsche Konten erstellt werden. Dies ist äußerst nützlich, wenn Sie e-Mail als Methode für die Kommunikation mit den Benutzern Ihrer Website verwenden, z. b. Forums Websites, Banking, eCommerce oder Websites für soziale Netzwerke.
- Kenn Wort Zurücksetzung **:** Die Kenn Wort Zurücksetzung ist eine Funktion, bei der der Benutzer sein Kennwort zurücksetzen kann, wenn er sein Kennwort vergessen hat
- **Sicherheits Stempel (bei überall Abmelden):** Unterstützt eine Möglichkeit zum erneuten Generieren des Sicherheits Tokens für den Benutzer in Fällen, in denen der Benutzer sein Kennwort ändert, oder andere sicherheitsrelevante Informationen, z. b. das Entfernen eines zugeordneten Anmelde namens (z. b. Facebook, Google, Microsoft-Konto usw.). Dies ist erforderlich, um sicherzustellen, dass alle mit dem alten Kennwort generierten Token für ungültig erklärt werden. Wenn Sie im Beispiel Projekt das Kennwort des Benutzers ändern, wird ein neues Token für den Benutzer generiert, und alle vorherigen Token werden für ungültig erklärt. Diese Funktion bietet eine zusätzliche Sicherheitsebene für Ihre Anwendung, denn wenn Sie Ihr Kennwort ändern, werden Sie von überall (allen anderen Browsern) abgemeldet, auf dem Sie sich bei dieser Anwendung angemeldet haben.
- **Legen Sie den Typ des Primärschlüssels für Benutzer und Rollen erweiterbar**: in ASP.net Identity 1,0 war der Typ des Primärschlüssels für Tabellen Benutzer und-Rollen Zeichen folgen. Dies bedeutet Folgendes: Wenn das ASP.net Identity System mithilfe Entity Framework in SQL Server beibehalten wurde, verwendeten wir nvarchar. Es gab viele Diskussionen um diese Standard Implementierung auf Stack Overflow und auf der Grundlage des eingehenden Feedbacks. Wir haben einen Erweiterbarkeits Hook bereitgestellt, in dem Sie angeben können, was der Primärschlüssel für die Tabelle "Benutzer und Rollen" sein soll. Dieser Erweiterbarkeits Hook ist besonders nützlich, wenn Sie Ihre Anwendung migrieren und die Anwendung userids als GUIDs oder int speichert.
- **Unterstützung von iquerable für Benutzer und Rollen**: zusätzliche Unterstützung für iquervable für usersstore und rolesstore. Sie können problemlos die Liste der Benutzer und Rollen abrufen.
- **Unterstützen des Löschvorgangs über den usermanager**
- **Indizierung für username**: in ASP.net Identity Entity Framework-Implementierung haben wir einen eindeutigen Index für den Benutzernamen hinzugefügt, indem wir das neue Indexattribute in EF 6.1.0 verwenden. Dadurch wird sichergestellt, dass Benutzernamen immer eindeutig sind und keine Racebedingung aufgetreten ist, in der Sie möglicherweise doppelte Benutzernamen haben.
- **Verbessertes Kennwort-Validierungs** Steuerelement: Das Kennwort-Validierungs Steuerelement, das in ASP.net Identity 1,0 ausgeliefert wurde, war ein recht einfaches Kennwort-Validierungs Steuerelement, das nur die Mindestlänge überprüft hat. Es gibt ein neues Kennwort-Validierungs Steuerelement, mit dem Sie die Komplexität des Kennworts besser steuern können. Beachten Sie, dass Sie, selbst wenn Sie alle Einstellungen in diesem Kennwort aktivieren, die zweistufige Authentifizierung für die Benutzerkonten aktivieren sollten.
- **Identityfactory Middleware/kreateperowincontext**:

    - **Benutzer-Manager**: Sie können die Factory-Implementierung verwenden, um eine Instanz von usermanager aus dem owin-Kontext abzurufen. Dieses Muster ähnelt dem, was wir zum Authentifizieren von AuthenticationManager aus dem owin-Kontext für die Anmeldung und SignOut verwenden. Dies ist eine empfohlene Methode, um eine Instanz von usermanager pro Anforderung für die Anwendung zu erhalten.
    - **Dbcontextfactory**: ASP.net Identity verwendet Entity Framework zur Beibehaltung des Identitäts Systems in SQL Server. Zu diesem Zweck verfügt das Identitäts System über einen Verweis auf den applicationdbcontext. Die dbcontextfactory-Middleware gibt eine Instanz von applicationdbcontext pro Anforderung zurück, die Sie in Ihrer Anwendung verwenden können.
- **ASP.net Identity Beispiele nuget-Paket**: mit dem nuget-Paket "Beispiele" können Sie die Beispiele für die ASP.net Identity einfacher installieren und ausführen und die bewährten Methoden befolgen. Dies ist ein Beispiel für eine ASP.NET-MVC-Anwendung. Ändern Sie den Code so, dass er ihrer Anwendung entspricht, bevor Sie diesen in der Produktionsumgebung bereitstellen. Das Beispiel sollte in einer leeren ASP.NET-Anwendung installiert werden. Weitere Informationen zum Paket finden Sie im folgenden Blogbeitrag: [Ankündigung RTM von ASP.net Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Microsoft-owin-Komponenten

Es gab viele Fehler, die in dieser Version behoben wurden. Ausführlichere Informationen finden Sie in den [Anmerkungen zu dieser Version von 2.1.0](https://katanaproject.codeplex.com/releases/view/113281) .

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Es gab viele Fehler, die in dieser Version behoben wurden. Ausführlichere Informationen finden Sie in den Versions [Hinweisen für die Version 2.0.2](https://github.com/SignalR/SignalR/releases/tag/2.0.2) .
