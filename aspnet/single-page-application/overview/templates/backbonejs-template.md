---
uid: single-page-application/overview/templates/backbonejs-template
title: Backbone-Vorlage | Microsoft-Dokumentation
author: madskristensen
description: Backbone. js-Spa-Vorlage
ms.author: riande
ms.date: 04/04/2013
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 7297db7d5b35a53b40f9d9162960e529a167bd12
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074890"
---
# <a name="backbone-template"></a>Backbone-Vorlage

von [Mads Kristensen](https://github.com/madskristensen)

> Die Backbone-Spa-Vorlage wurde von Kazi Manzur Rashid verfasst.
> 
> [Herunterladen der Spa-Vorlage "Backbone. js"](https://go.microsoft.com/fwlink/?LinkId=293631)

Die Vorlage "Backbone. js" ist so konzipiert, dass Sie den Einstieg in das Erstellen von interaktiven Client seitigen Web-Apps mithilfe von " [Backbone. js](http://backbonejs.org/) " erleichtern.

Die Vorlage enthält ein erstes Gerüst zum Entwickeln einer Backbone. js-Anwendung in ASP.NET MVC. Standardmäßig bietet Sie grundlegende Benutzer Anmelde Funktionen, einschließlich Benutzerregistrierung, Anmeldung, Kenn Wort Zurücksetzung und Benutzer Bestätigung mit grundlegenden e-Mail-Vorlagen.

Anforderungen:

- [ASP.net and Web Tools 2012,2-Update](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Erstellen eines Backbone-Vorlagen Projekts

Um die Vorlage herunterzuladen und zu installieren, klicken Sie oben auf die Schaltfläche herunterladen Die Vorlage ist als Visual Studio-Erweiterungs Datei (VSIX) verpackt. Möglicherweise müssen Sie Visual Studio neu starten.

Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten. Wählen Sie unter **Visualisierung C#** die Option **Web**aus. Wählen Sie in der Liste der Projektvorlagen die Option **ASP.NET MVC 4-Webanwendung**aus. Geben Sie dem Projekt einen Namen, und klicken Sie auf **OK**.

![](backbonejs-template/_static/image1.png)

Wählen Sie im Assistenten für **neue Projekte** die Option Backbone. js-Spa-Projekt aus.

![](backbonejs-template/_static/image2.png)

Drücken Sie STRG + F5, um die Anwendung zu erstellen und ohne Debuggen auszuführen, oder drücken Sie F5, um das Debugging auszuführen.

![](backbonejs-template/_static/image3.png)

Wenn Sie auf "Mein Konto" klicken, wird die Anmeldeseite angezeigt:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Exemplarische Vorgehensweise: Client Code

Wir beginnen mit der Clientseite. Die Client Anwendungs Skripts befinden sich im Ordner ~/Scripts/Application. Die Anwendung wird in [typescript](http://www.typescriptlang.org/) (TS-Dateien) geschrieben, die in JavaScript (JS-Dateien) kompiliert werden.

**Anwendung**

`Application` ist in "Application. TS" definiert. Dieses Objekt initialisiert die Anwendung und fungiert als Stamm Namespace. Sie verwaltet Konfigurations-und Zustandsinformationen, die in der gesamten Anwendung gemeinsam genutzt werden, z. b. ob der Benutzer angemeldet ist.

Die `application.start`-Methode erstellt die modalen Sichten und fügt Ereignishandler für Ereignisse auf Anwendungsebene, z. b. Benutzeranmeldung, an. Anschließend wird der Standard Router erstellt und überprüft, ob eine Client seitige URL angegeben ist. Wenn dies nicht der Fall ist, wird die Umleitung an die Standard-URL (#!/) umgeleitet.

**Ereignisse**

Ereignisse sind immer wichtig, wenn lose gekoppelte Komponenten entwickelt werden. Anwendungen führen häufig mehrere Vorgänge als Reaktion auf eine Benutzeraktion aus. Der Backbone stellt integrierte Ereignisse mit Komponenten wie z. b. Modell, Sammlung und Ansicht bereit. Statt Abhängigkeiten zwischen diesen Komponenten zu erstellen, verwendet die Vorlage ein "Pub/Sub"-Modell: das `events` Objekt, das in "Events. TS" definiert ist, fungiert als Event Hub zum Veröffentlichen und Abonnieren von Anwendungs Ereignissen. Das `events`-Objekt ist ein Singleton. Der folgende Code zeigt, wie Sie ein Ereignis abonnieren und dann das Ereignis auslöst:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Typ**

In Backbone. js stellt ein Router Methoden zum Routing von Client seitigen Seiten bereit und verbindet Sie mit Aktionen und Ereignissen. Die Vorlage definiert einen einzelnen Router in "Router. TS". Der Router erstellt die aktivierbaren Sichten und behält den Zustand bei der Umstellung von Sichten bei. (Aktivierbare Sichten werden im nächsten Abschnitt beschrieben.) Anfänglich verfügt das Projekt über zwei dummysichten: "Home" und "about". Sie verfügt auch über eine NotFound-Ansicht, die angezeigt wird, wenn die Route nicht bekannt ist.

**Ansichten**

Die Sichten werden in ~/Scripts/Application/Views. definiert. Es gibt zwei Arten von Ansichten, aktivierbare Sichten und modale Dialog Sichten. Aktivierbare Sichten werden vom Router aufgerufen. Wenn eine aktivierbare Ansicht angezeigt wird, werden alle anderen aktivierbaren Sichten inaktiv. Zum Erstellen einer aktivierbaren Ansicht erweitern Sie die Ansicht mit dem `Activable` Objekt:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Beim Erweitern von mit `Activable` werden der Ansicht zwei neue Methoden hinzugefügt, `activate` und `deactivate`. Der Router ruft diese Methoden auf, um die Ansicht zu aktivieren und zu deaktiveren.

Modale Sichten werden als modale [Bootstrap](https://twitter.github.com/bootstrap/) -Dialoge implementiert. Die `Membership`-und `Profile` Sichten sind modale Ansichten. Modell Sichten können von beliebigen Anwendungs Ereignissen aufgerufen werden. Beispielsweise wird in der `Navigation` Ansicht in der Ansicht "Mein Konto" entweder die `Membership` Ansicht oder die `Profile` Ansicht angezeigt, je nachdem, ob der Benutzer angemeldet ist. Der `Navigation` fügt Click-Ereignishandler an alle untergeordneten Elemente an, die über das `data-command`-Attribut verfügen. Hier sehen Sie das HTML-Markup:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Im folgenden finden Sie den Code in "Navigation. TS", um die Ereignisse zu verbinden:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modelle**

Die Modelle werden in ~/Scripts/Application/Models. definiert. Alle Modelle haben drei grundlegende Aspekte: Standard Attribute, Validierungsregeln und einen serverseitigen Endpunkt. Im folgenden finden Sie ein typisches Beispiel:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Plug-ins**

Der Ordner ~/Scripts/Application/lib enthält einige praktische jQuery-Plug-ins. Die Datei "Form. TS" definiert ein Plug-in zum Arbeiten mit Formulardaten. Häufig müssen Sie Formulardaten Serialisieren oder Deserialisieren und alle Modell Validierungs Fehler anzeigen. Das Formular. TS-Plug-in verfügt über Methoden wie `serializeFields`, `deserializeFields`und `showFieldErrors`. Im folgenden Beispiel wird gezeigt, wie ein Formular in ein Modell serialisiert wird.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Das Plug-in "Flash Leiste. TS" bietet dem Benutzer verschiedene Arten von Feedback Nachrichten. Die Methoden sind `$.showSuccessbar`, `$.showErrorbar` und `$.showInfobar`. Im Hintergrund werden Twitter-Bootstrap-Warnungen verwendet, um gut animierte Meldungen anzuzeigen.

Das confirm. TS-Plug-in ersetzt das Bestätigungs Dialogfeld des Browsers, obwohl sich die API etwas unterscheidet:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Exemplarische Vorgehensweise: Server Code

Sehen wir uns nun die Serverseite an.

**Controller**

In einer Single-Page-Anwendung spielt der Server nur eine kleine Rolle in der Benutzeroberfläche. In der Regel rendert der Server die anfängliche Seite und sendet und empfängt dann JSON-Daten.

Die Vorlage verfügt über zwei MVC-Controller: `HomeController` rendert die anfängliche Seite und `SupportsController` verwendet, um neue Benutzerkonten zu bestätigen und Kenn Wörter zurückzusetzen. Alle anderen Controller in der Vorlage sind ASP.net-Web-API Controller, die JSON-Daten senden und empfangen. Standardmäßig verwenden die Controller die neue `WebSecurity`-Klasse, um Benutzer bezogene Aufgaben auszuführen. Sie verfügen jedoch auch über optionale Konstruktoren, mit denen Sie Delegaten für diese Aufgaben übergeben können. Dies vereinfacht das Testen und ermöglicht es Ihnen, `WebSecurity` durch ein IOC-Container durch etwas anderes zu ersetzen. Beispiel:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Sichten

Die Ansichten sind so konzipiert, dass Sie modular aufgebaut werden: jeder Abschnitt einer Seite verfügt über eine eigene dedizierte Ansicht. In einer Single-Page-Anwendung sind häufig Sichten enthalten, die keinen entsprechenden Controller aufweisen. Sie können eine Ansicht einschließen, indem Sie `@Html.Partial('myView')`aufrufen, dies wird jedoch mühsam. Um dies zu vereinfachen, definiert die Vorlage eine Hilfsmethode, `IncludeClientViews`, die alle Sichten in einem angegebenen Ordner rendert:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Wenn der Ordnername nicht angegeben wird, lautet der Standardordner Name "clientviews". Wenn die Client Ansicht auch Teilansichten verwendet, benennen Sie die Teilansicht mit einem Unterstrich (z. b. `_SignUp`). Die `IncludeClientViews`-Methode schließt alle Sichten aus, deren Name mit einem Unterstrich beginnt. Um eine Teilansicht in die Client Ansicht einzuschließen, geben Sie `Html.ClientView('SignUp')` anstelle von `Html.Partial('_SignUp')`an.

**Senden von e-Mails**

Zum Senden von e-Mails verwendet die Vorlage [Postal](http://aboutcode.net/postal). Postal wird jedoch aus dem Rest des Codes mit der `IMailer`-Schnittstelle abstrahiert, sodass Sie Sie problemlos durch eine andere Implementierung ersetzen können. Die e-Mail-Vorlagen befinden sich im Ordner "Views/Email". Die e-Mail-Adresse des Absenders wird in der Datei "Web. config" im `sender.email` Schlüssel des Abschnitts " **appSettings** " angegeben. Wenn `debug="true"` in der Datei "Web. config" verwendet wird, ist die Benutzer-e-Mail-Bestätigung nicht erforderlich, um die Entwicklung zu beschleunigen.

## <a name="github"></a>GitHub

Sie finden auch die Vorlage "Backbone. js" auf [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
