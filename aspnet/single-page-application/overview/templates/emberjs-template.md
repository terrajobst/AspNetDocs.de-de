---
uid: single-page-application/overview/templates/emberjs-template
title: Emberjs-Vorlage | Microsoft-Dokumentation
author: xqiu
description: EmberJS-Vorlage
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1aefa46dd0841b1b06675409cc8a09f9a218d7ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467157"
---
# <a name="emberjs-template"></a>EmberJS-Vorlage

von [Xinyang Qiu](https://github.com/xqiu)

> Die emberjs-MVC-Vorlage wird von Nathan, Thiago Santos und Xinyang Qiu geschrieben.
> 
> [Herunterladen der emberjs-MVC-Vorlage](https://go.microsoft.com/fwlink/?LinkId=282647)

Die emberjs-Spa-Vorlage ist so konzipiert, dass Sie den Einstieg in das Erstellen von interaktiven Client seitigen Web-Apps mithilfe von emberjs erleichtern.

Die Single-Page-Anwendung (Single-Page Application, Spa) ist der allgemeine Begriff für eine Webanwendung, die eine einzelne HTML-Seite lädt und dann die Seite dynamisch aktualisiert, anstatt neue Seiten zu laden. Nach dem anfänglichen Laden der Seite kommuniziert die Spa mit dem Server über AJAX-Anforderungen.

![](emberjs-template/_static/image1.png)

AJAX ist nichts neues, aber heute gibt es JavaScript-Frameworks, die das Erstellen und Verwalten einer großen anspruchsvollen Spa-Anwendung vereinfachen. Außerdem vereinfachen HTML 5 und CSS3 das Erstellen umfassender Benutzeroberflächen.

Die emberjs-Spa-Vorlage verwendet die [Ember](http://emberjs.com/) -JavaScript-Bibliothek, um Seiten Aktualisierungen von AJAX-Anforderungen zu verarbeiten. Ember. js verwendet die Datenbindung, um die Seite mit den neuesten Daten zu synchronisieren. Auf diese Weise müssen Sie keinen Code schreiben, der die JSON-Daten durchläuft und das DOM aktualisiert. Stattdessen fügen Sie deklarative Attribute in den HTML-Code ein, der Ember. js mitteilt, wie die Daten dargestellt werden sollen.

Auf der Serverseite ist die emberjs-Vorlage fast identisch mit der [Spa-Vorlage von knockoutjs](../introduction/knockoutjs-template.md). Er verwendet ASP.NET MVC, um HTML-Dokumente zu verarbeiten, und ASP.net-Web-API, um AJAX-Anforderungen vom Client zu verarbeiten. Weitere Informationen zu den Aspekten der Vorlage finden Sie in der Dokumentation zu der [knockoutjs-Vorlage](../introduction/knockoutjs-template.md) . Dieses Thema konzentriert sich auf die Unterschiede zwischen der Knockout-Vorlage und der emberjs-Vorlage.

## <a name="create-an-emberjs-spa-template-project"></a>Erstellen eines emberjs-Spa-Vorlagen Projekts

Um die Vorlage herunterzuladen und zu installieren, klicken Sie oben auf die Schaltfläche herunterladen Möglicherweise müssen Sie Visual Studio neu starten.

Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten. Wählen Sie unter **Visualisierung C#** die Option **Web**aus. Wählen Sie in der Liste der Projektvorlagen die Option **ASP.NET MVC 4-Webanwendung**aus. Geben Sie dem Projekt einen Namen, und klicken Sie auf **OK**.

![](emberjs-template/_static/image2.png)

Wählen Sie im Assistenten für **neue Projekte** die Option **Ember. js-Spa-Projekt**aus.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Übersicht über die emberjs-Spa-Vorlage

Die emberjs-Vorlage verwendet eine Kombination aus "jQuery", "Ember. js" und "Lenker. js", um eine reibungslose interaktive Benutzeroberfläche zu erstellen.

Ember. js ist eine JavaScript-Bibliothek, die ein Client seitiges MVC-Muster verwendet.

- Eine *Vorlage*, die in der Vorlagen Sprache des handbars geschrieben ist, beschreibt die Benutzeroberfläche der Anwendung. Im Releasemodus wird der Compiler für den [Lenker](https://github.com/Myslik/csharp-ember-handlebars) verwendet, um die Vorlage für den Lenker zu bündeln und zu kompilieren.
- Ein *Modell* speichert die Anwendungsdaten, die vom Server abgerufen werden (TODO-Listen und TODO-Elemente).
- Ein *Controller* speichert den Anwendungs Zustand. Controller stellen häufig Modelldaten für die entsprechenden Vorlagen dar.
- Eine *Sicht* übersetzt primitive Ereignisse aus der Anwendung und übergibt diese an den Controller.
- Ein *Router* verwaltet den Anwendungs Status, wobei URLs und Vorlagen synchron bleiben.

Außerdem kann die Ember-Datenbibliothek verwendet werden, um JSON-Objekte (die vom Server über eine Rest-API abgerufen werden) und die Client Modelle zu synchronisieren.

Die Vorlage emberjs Spa organisiert die Skripts in acht Ebenen:

- WebAPI\_Adapter. js, WebAPI\_serializer. js: erweitert die Ember-Datenbibliothek, um mit ASP.net-Web-API zu arbeiten.
- Scripts/Hilfsprogramme. js: definiert neue Ember-Lenker Hilfen.
- Scripts/app. js: erstellt die APP und konfiguriert den Adapter und das Serialisierungsprogramm.
- Scripts/App/Models/\*. js: definiert die Modelle.
- Scripts/App/views/\*. js: definiert die Ansichten.
- Scripts/App/Controllers/\*. js: definiert die Controller.
- Scripts/App/Routes, Scripts/App/Router. js: definiert die Routen.
- Templates/\*. HSB: definiert die Lenker Vorlagen.

Betrachten wir einige dieser Skripts ausführlicher.

## <a name="models"></a>Modelle

Die Modelle werden im Ordner "Scripts/App/Models" definiert. Es gibt zwei Modelldateien: "dedoitem. js" und "dedolist. js".

**TODO. Model. js** definiert die Client seitigen (Browser-) Modelle für die Aufgabenlisten. Es gibt zwei Modellklassen: "tdoitem" und "tdolist". Im Ember sind Modelle Unterklassen von DS. Modells. Ein Modell kann über Eigenschaften mit Attributen verfügen:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Modelle können Beziehungen zu anderen Modellen definieren:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Modelle können über berechnete Eigenschaften verfügen, die an andere Eigenschaften gebunden werden:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Modelle können Beobachter Funktionen aufweisen, die aufgerufen werden, wenn sich eine beobachtete Eigenschaft ändert:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Ansichten

Die Sichten werden im Ordner Scripts/App/views definiert. Eine Ansicht übersetzt Ereignisse aus der Benutzeroberfläche der Anwendung. Ein Ereignishandler kann an Controller Funktionen zurückgreifen oder den Datenkontext einfach direkt aufzurufen.

Der folgende Code ist z. b. aus views/"" "" "" "" "" "" "" Definiert die Ereignis Behandlung für ein Eingabe Textfeld.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Controller

Die Controller werden im Ordner Skripts/App/Controller definiert. Erweitern Sie `Ember.ObjectController`, um ein einzelnes Modell darzustellen:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Ein Controller kann auch eine Sammlung von Modellen darstellen, indem `Ember.ArrayController`erweitert wird. So stellt z. b. "-Objektlisten Controller" ein Array von `todoList`-Objekten dar. Der Controller sortiert nach ToDoList-ID in absteigender Reihenfolge:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Der Controller definiert eine Funktion mit dem Namen `addTodoList`, mit der eine neue ""-Liste erstellt und dem Array hinzugefügt wird. Um zu sehen, wie diese Funktion aufgerufen wird, öffnen Sie die Vorlagen Datei namens todolisttemplate. html im Ordner Vorlagen. Der folgende Vorlagen Code bindet eine Schaltfläche an die `addTodoList`-Funktion:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Der Controller enthält auch eine `error`-Eigenschaft, die eine Fehlermeldung enthält. Im folgenden finden Sie den Vorlagen Code zum Anzeigen der Fehlermeldung (auch in "" "" "" "" "" "" ""

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Routen

"Router. js" definiert die Routen und die anzuzeigende Standardvorlage, richtet den Anwendungs Zustand ein und passt URLs zu Routen an:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

"Dedolistroute. js" lädt Daten für "dedolistroute", indem die Funktion "Setupcontroller" überschrieben wird:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember verwendet Benennungs Konventionen, um URLs, Routennamen, Controller und Vorlagen abzugleichen. Weitere Informationen finden Sie unter [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) in der emberjs-Dokumentation.

## <a name="templates"></a>Vorlagen

Der Ordner "Templates" enthält vier Vorlagen:

- Application. HSB: die Standardvorlage, die gerendert wird, wenn die Anwendung gestartet wird.
- about. HSB: die Vorlage für die Route "/about".
- Index. HSB: die Vorlage für die "/"-Stamm Route.
- "tdolist. HSB": die Vorlage für die Route "/ToDo".
- \_navbar. HSB: die Vorlage definiert das Navigationsmenü.

Die Anwendungs Vorlage verhält sich wie eine Master Seite. Sie enthält eine Kopfzeile, eine Fußzeile und ein "{{Outlet}}", um in Abhängigkeit von der Route andere Vorlagen in einzufügen. Weitere Informationen zu Anwendungs Vorlagen in Ember finden Sie unter [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

Die Vorlage "/todoList" enthält zwei Schleifen Ausdrücke. Die äußere Schleife ist `{{#each controller}}`, und die innere Schleife wird `{{#each todos}}`. Der folgende Code zeigt eine integrierte `Ember.Checkbox` Sicht, eine angepasste `App.TodoItemEditView`und eine Verknüpfung mit einer `deleteTodo` Aktion.

[!code-html[Main](emberjs-template/samples/sample12.html)]

Die `HtmlHelperExtensions`-Klasse, die in Controllers/htmlhelperextensions. cs definiert ist, definiert eine Hilfsfunktion zum Zwischenspeichern und Einfügen von Vorlagen Dateien, wenn **Debug** in der Datei "Web. config" auf " **true** " festgelegt ist. Diese Funktion wird von der ASP.NET MVC-Ansichts Datei aufgerufen, die in views/Home/app. cshtml definiert ist:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Ohne Argumente aufgerufen, rendert die Funktion alle Vorlagen Dateien im Vorlagen Ordner. Sie können auch einen Unterordner oder eine bestimmte Vorlagen Datei angeben.

Wenn **Debug** in "Web. config" den Wert " **false** " hat, enthält die Anwendung das Paket Element "~/Bundles/Templates". Dieses Bündel Element wird in BundleConfig.cs mithilfe der compilerbibliothek hinzugefügt:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
