---
uid: single-page-application/overview/templates/hottowel-template
title: Vorlage für Hot-Handtuch | Microsoft-Dokumentation
author: madskristensen
description: Hothandtuch-Vorlage
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: eeab69e75546791978bb09d7823d95caf9dca1a0
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075059"
---
# <a name="hot-towel-template"></a>Hot Towel-Vorlage

von [Mads Kristensen](https://github.com/madskristensen)

> Die MVC-Vorlage "Hot Handtuch" wird von John Papa verfasst.
> 
> Wählen Sie die herunter zuladbare Version aus:
> 
> [Hot-Handtuch-MVC-Vorlage für Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [MVC-Vorlage für das Hot-Handtuch für Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Heißes Handtuch: da Sie nicht in die Spa wechseln möchten.

Möchten Sie eine Spa erstellen, aber nicht entscheiden, wo Sie beginnen sollen? Verwenden Sie das heiße Handtuch, und in Sekunden haben Sie eine Spa und alle Tools, die Sie für die Erstellung benötigen!

Hot-Handtuch ist ein idealer Ausgangspunkt für das Erstellen einer Single-Page-Anwendung (Single Page Application, Spa) mit ASP.net. Standardmäßig verfügen Sie über eine modulare Struktur für Ihren Code, die Navigationsansicht, die Datenbindung, die umfassende Datenverwaltung und einfache, aber elegante Formate. Hot-Handtuch bietet alles, was Sie zum Erstellen einer Spa benötigen, damit Sie sich auf Ihre APP konzentrieren können, nicht auf die Grundlagen.

> Erfahren Sie mehr über das Entwickeln einer Spa aus [den Videos, Tutorials und Pluralsight-Kursen von John Papa](http://johnpapa.net/spa?vsix).

## <a name="application-structure"></a>Anwendungsstruktur

Hot Handtuch Spa bietet einen app-Ordner, der die JavaScript-und HTML-Dateien enthält, die Ihre Anwendung definieren.

Im App-Ordner:

- Durandal
- services
- ViewModels
- views

Der app-Ordner enthält eine Sammlung von Modulen. Diese Module Kapseln Funktionen und deklarieren Abhängigkeiten von anderen Modulen. Der Ordner "Views" enthält den HTML-Code für Ihre Anwendung, und der Ordner "ViewModels" enthält die Präsentationslogik für die Ansichten (ein gängiges MVVM-Muster). Der Ordner Dienste eignet sich ideal für alle gängigen Dienste, die Ihre Anwendung möglicherweise benötigt, z. b. http-Datenabruf oder lokale Speicher Interaktion. Es ist üblich, dass mehrere ViewModels Code aus den Dienst Modulen wieder verwenden.

## <a name="aspnet-mvc-server-side-application-structure"></a>ASP.NET MVC-Server seitige Anwendungs Struktur

Hot-Handtuch baut auf der vertrauten und leistungsstarken ASP.NET MVC-Struktur auf.

- App-\_starten
- Inhalt
- Controllers
- Modelle
- Skripts
- Sichten

## <a name="featured-libraries"></a>Ausgestattete Bibliotheken

- ASP.NET MVC
- ASP.NET-Web-API
- ASP.net-Weboptimierung: Bündelung und Minimierung
- [Breeze. js](http://Breezejs.com) : umfassende Datenverwaltung
- [Durandal. js](http://Durandaljs.com) -Navigation und Ansichts Komposition
- [Knockout. js](http://Knockoutjs.com) -Daten Bindungen
- [Erfordern von. js](http://requirejs.org) -Modularität mit AMD und Optimierung
- " [Deastr. js](http://jpapa.me/c7toastr) "-Popup Meldungen
- [Twitter-Bootstrap](https://twitter.github.com/bootstrap/) -robustes CSS-Format

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Installieren über die Visual Studio 2012-Hot-Handtuch-Spa-Vorlage

Hot-Handtuch kann als Visual Studio 2012-Vorlage installiert werden. Klicken Sie einfach auf `File` | `New Project`, und wählen Sie `ASP.NET MVC 4 Web Application`aus. Wählen Sie dann die Vorlage "Hot Handtuch Single Page Application" aus, und führen Sie aus.

## <a name="installing-via-the-nuget-package"></a>Installieren über das nuget-Paket

Hot-Handtuch ist auch ein nuget-Paket, das ein vorhandenes leeres ASP.NET MVC-Projekt erweitert. Installieren Sie einfach mithilfe von nuget, und führen Sie dann aus.

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Wie erstelle ich ein heißes Handtuch?

Fügen Sie einfach Code hinzu.

1. Fügen Sie Ihren eigenen serverseitigen Code hinzu, vorzugsweise Entity Framework und WebAPI (was mit Breeze. js wirklich glänzt).
2. Hinzufügen von Ansichten zum Ordner "`App/views`"
3. Hinzufügen von ViewModels zum Ordner "`App/viewmodels`"
4. Hinzufügen von HTML-und Knockout-Daten Bindungen zu ihren neuen Ansichten
5. Aktualisieren der Navigations Routen in `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Exemplarische Vorgehensweise für HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

Index. cshtml ist die Ausgangs Route und-Ansicht für die MVC-Anwendung. Sie enthält alle standardmeta-Tags, CSS-Links und JavaScript-Verweise, die erwartet werden. Der Text enthält ein einzelnes `<div>` in dem der gesamte Inhalt (ihre Ansichten) bei der Anforderung platziert wird. Der `@Scripts.Render` verwendet "require. js", um den Einstiegspunkt für den Anwendungscode auszuführen, der in der `main.js`-Datei enthalten ist. Ein Begrüßungsbildschirm wird bereitgestellt, um zu veranschaulichen, wie ein Begrüßungsbildschirm erstellt wird, während Ihre APP geladen wird.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/Main. js

Die `main.js` Datei enthält den Code, der ausgeführt wird, sobald die App geladen wird. An dieser Stelle möchten Sie Ihre Navigations Routen definieren, die Start Ansichten festlegen und Setup/Bootstrap ausführen, wie z. b. die Daten der Anwendung.

Die `main.js` Datei definiert mehrere der Durandal-Module, um den Start der Anwendung zu unterstützen. Mit der define-Anweisung können die Modul Abhängigkeiten aufgelöst werden, sodass Sie für die Funktion verfügbar sind. Zuerst werden die Debugmeldungen aktiviert, die Nachrichten zu den Ereignissen senden, die von der Anwendung im Konsolenfenster durchgeführt werden. Der app. Start-Code weist Durandal Framework an, die Anwendung zu starten. Die Konventionen sind so festgelegt, dass Durandal weiß, dass alle Ansichten und ViewModels in denselben benannten Ordnern enthalten sind. Zum Schluss lädt der `app.setRoot` die `shell` mithilfe einer vordefinierten `entrance` Animation.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Sichten

Sichten befinden sich im Ordner "`App/views`".

### <a name="shellhtml"></a>Shell. html

Die `shell.html` enthält das Master Layout für den HTML-Code. Alle anderen Ansichten werden an einer beliebigen Seite der `shell` Ansicht zusammengesetzt. Hot-Handtuch bietet eine `shell` mit drei dieser Regionen: einen Header, einen Inhalts Bereich und eine Fußzeile. Jede dieser Regionen wird mit Inhalt aus anderen Ansichten geladen, wenn Sie angefordert werden.

Die `compose` Bindungen für die Kopf-und Fußzeile sind im Hot-handtuch hart codiert, um auf die `nav`-und `footer` Ansichten zu verweisen. Die Compose-Bindung für den Abschnitt `#content` ist an das aktive Element des `router` Moduls gebunden. Anders ausgedrückt: Wenn Sie auf einen Navigations Link klicken, wird die entsprechende Ansicht in diesem Bereich geladen.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>NAV. html

Die `nav.html` enthält die Navigationslinks für die Spa. An dieser Stelle kann z. b. die Menüstruktur platziert werden. Häufig handelt es sich hierbei um Daten gebundene (mit Knockout) an das `router` Modul, um die im `shell.js`definierte Navigation anzuzeigen. Knockout sucht nach den Daten Bindungs Attributen und bindet diese an das `shell` ViewModel, um die Navigations Routen anzuzeigen und eine ProgressBar (mithilfe von Twitter-Bootstrap) anzuzeigen, wenn das `router` Modul in der Mitte der Navigation von einer Ansicht zu einem anderen (siehe `router.isNavigating`) ist.

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home. html und Details. html

Diese Ansichten enthalten HTML für benutzerdefinierte Ansichten. Wenn auf den `home` Link im Menü der `nav` Ansicht geklickt wird, wird die `home` Ansicht in den Inhalts Bereich der `shell` Ansicht eingefügt. Diese Ansichten können durch ihre eigenen benutzerdefinierten Ansichten erweitert oder ersetzt werden.

### <a name="footerhtml"></a>Footer. html

Die `footer.html` enthält HTML, das in der Fußzeile unten in der `shell` Ansicht angezeigt wird.

## <a name="viewmodels"></a>ViewModels

ViewModels finden Sie im Ordner "`App/viewmodels`".

### <a name="shelljs"></a>Shell. js

Das `shell` ViewModel enthält Eigenschaften und Funktionen, die an die `shell` Ansicht gebunden sind. Häufig werden die Menü Navigations Bindungen gefunden (Weitere Informationen finden Sie in der `router.mapNav` Logik).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>"Home. js" und "Details. js"

Diese ViewModels enthalten die Eigenschaften und Funktionen, die an die `home` Ansicht gebunden sind. Außerdem enthält Sie die Darstellungs Logik für die Ansicht und ist der Verbindungsaufbau zwischen den Daten und der Ansicht.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Dienste

Dienste befinden sich im Ordner "App/Dienste". Im Idealfall könnten Ihre zukünftigen Dienste wie z. b. ein DataService-Modul, das für das Senden und Veröffentlichen von Remote Daten zuständig ist, platziert werden.

### <a name="loggerjs"></a>logger.js

Hot-Handtuch stellt ein `logger` Modul im Ordner "Dienste" bereit. Das `logger`-Modul eignet sich ideal für das Protokollieren von Nachrichten in der Konsole und an den Benutzer in Popup-Protokollen.
