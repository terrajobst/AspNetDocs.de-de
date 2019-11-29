---
uid: mvc/mvc3
title: ASP.NET MVC 3
author: rick-anderson
description: (enthält das Update der Tools von April 2011) ASP.NET MVC 3 ist ein Framework zum Erstellen skalierbarer, auf Standards basierender Webanwendungen mit einem bewährten Entwurfsmuster...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 421a06c89d4dbcb05d4080033813cc6558b7c698
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74586751"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC 3

> *(enthält das Update der Tools von April 2011)*
> 
> ASP.NET MVC 3 ist ein Framework zum Erstellen skalierbarer, auf Standards basierender Webanwendungen mit bewährten Entwurfsmustern und der Leistungsfähigkeit von ASP.net und der .NET Framework.
> 
> Es wird parallel mit ASP.NET MVC 2 installiert, und Sie können es noch heute verwenden!
> 
> [Installer hier](https://go.microsoft.com/fwlink/?LinkID=208140) herunterladen

## <a name="top-features"></a>Top-Features

- Erweiterbarkeit des integrierten Gerüstbau Systems über nuget
- HTML 5-aktivierte Projektvorlagen
- Ausdrucksstarke Ansichten einschließlich der neuen Razor-Ansichts-Engine
- Leistungsstarke Hooks mit Abhängigkeitsinjektion und globalen Aktions Filtern
- Umfassende JavaScript-Unterstützung mit unaufdringlichem JavaScript, jquery-Validierung und JSON-Bindung
- *[Unten finden](#overview) Sie die vollständige Liste der Features.*

## <a name="top-links"></a>Top-Links

Neues in ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3 veröffentlicht](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.net MVC3, webmatrix, nuget, IIS Express und Orchard veröffentlicht-das Microsoft January-webrelease im Kontext](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [Ankündigung der Veröffentlichung von ASP.NET MVC 3, IIS Express, SQL CE 4, Web Farm Framework, Orchard, webmatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Anmerkungen zu dieser Version von ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Installation und Hilfe

- Installieren von ASP.NET MVC 3 mithilfe des [Webplattform-Installers (empfohlen)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Installieren von ASP.NET MVC 3 mithilfe der [ausführbaren Datei des Installations](https://go.microsoft.com/fwlink/?LinkID=208140) Programms
- Installieren [von ASP.NET MVC 3 für Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Lesen Sie das [Tutorial Einführung in ASP.NET MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) .
- Hilfe und Erörterung von ASP.NET MVC 3 in den [Foren](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 Overview

ASP.NET MVC 3 baut auf ASP.NET MVC 1 und 2 auf und bietet hervorragend Funktionen, die Ihren Code vereinfachen und eine tiefere Erweiterbarkeit ermöglichen. Dieses Thema enthält eine Übersicht über viele der neuen Features, die in dieser Version enthalten sind, in die folgenden Abschnitte unterteilt:

- [Erweiterbares Gerüstbau mit mvcgerüold-Integration](#BM_MvcScaffolding)
- [HTML 5-aktivierte Projektvorlagen](#BM_HTML5)
- [Die Razor-Ansichts-Engine](#BM_TheRazorViewEngine)
- [Unterstützung für mehrere Ansichts-Engines](#BM_Support_for_Multiple_View_Engines)
- [Controller Verbesserungen](#BM_Controller_Improvements)
- [JavaScript und AJAX](#BM_JavaScript_and_Ajax_Improvements)
- [Verbesserte Modell Validierung](#BM_Model_Validation_Improvements)
- [Verbesserungen in Abhängigkeit](#BM_Dependency_Injection_Improvements)
- [Weitere neue Features](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Erweiterbares Gerüstbau mit mvcgerüold-Integration

Dank des neuen Gerüstbau Systems ist es einfacher, produktiv zu arbeiten, wenn Sie noch nicht mit dem Framework vertraut sind, und häufig verwendete Entwicklungsaufgaben zu automatisieren, wenn Sie Erfahrung haben und bereits wissen, was Sie tun.

Dies wird vom neuen nuget- *Gerüstbau* Paket namens **mvcgerüstbau**unterstützt. Der Begriff "Gerüstbau" wird von vielen Softwaretechnologien verwendet, um "schnelles Erstellen eines grundlegenden Gliederung der Software zu ermöglichen, die Sie dann bearbeiten und anpassen können". Das Gerüstbau Paket, das für ASP.NET MVC erstellt wird, ist in verschiedenen Szenarien von Vorteil:

- **Wenn Sie ASP.NET MVC zum ersten Mal erlernen**, denn es bietet Ihnen eine schnelle Möglichkeit, einen nützlichen, funktionierenden Code zu erhalten, den Sie dann entsprechend Ihren Anforderungen bearbeiten und anpassen können. Es erspart Ihnen das Trauma, eine leere Seite zu sehen, ohne zu wissen, wo Sie anfangen sollten!
- **Wenn Sie ASP.NET MVC Well kennen und nun einige neue Add-on-Technologien** , wie z. b. ein Objekt relationaler Mapper, eine Ansichts-Engine, eine Test Bibliothek usw., untersuchen, da der Ersteller dieser Technologie möglicherweise auch ein Gerüstbau Paket dafür erstellt hat.
- **Wenn Ihre Arbeit das wiederholte erstellen ähnlicher Klassen oder Dateien umfasst**, können Sie benutzerdefinierte gerüstolders erstellen, die Test-und Bereitstellungs Skripts ausgeben, oder was Sie benötigen. Alle Personen in Ihrem Team können auch Ihre benutzerdefinierten Gerüst Entwickler verwenden.

Weitere Funktionen in mvcgerüstbau sind:

- Unterstützung C# für-und VB-Projekte
- Unterstützung für Razor-und aspx-Ansichts-Engines
- Unterstützt Gerüstbau in ASP.NET MVC-Bereiche und mithilfe von benutzerdefinierten Ansichts Layouts
- Sie können die Ausgabe problemlos anpassen, indem Sie T4-Vorlagen bearbeiten.
- Sie können mithilfe von benutzerdefinierten PowerShell-Logik-und benutzerdefinierten T4-Vorlagen völlig neue Gerüst Vorlagen hinzufügen. Diese (und alle benutzerdefinierten Parameter, die Sie Ihnen zugewiesen haben) werden automatisch in der Konsolen Registerkarte Vervollständigungsliste angezeigt.
- Sie können nuget-Pakete mit zusätzlichen Gerüst Werten für verschiedene Technologien erhalten (z. b. gibt es eine Machbarkeitsstudie für die LINQ to SQL), die Sie kombinieren und einander zuordnen.

Das ASP.NET MVC 3 Tools Update bietet hervorragend Visual Studio-Unterstützung für dieses Gerüstbau System, z. b.:

- Das Dialog Feld "Controller hinzufügen" unterstützt jetzt das vollständige automatische Gerüstbau von Aktionen zum Erstellen, lesen, aktualisieren und Löschen von Controllern und In der Standardeinstellung wird der Datenzugriffs Code mithilfe von EF Code First.
- Dialog Feld "Controller hinzufügen" unterstützt *erweiterbare Gerüstbau* über nuget-Pakete wie *mvcgerüstbau*. Dies ermöglicht das Anbinden von benutzerdefinierten Gerüsten in das Dialogfeld, mit dem Sie Gerüsten für andere Datenzugriffs Technologien wie NHibernate oder sogar Jet mit ODBCDirect erstellen können, wenn Sie so gerne sind!

Weitere Informationen zum Gerüstbau in ASP.NET MVC 3 finden Sie in den folgenden Ressourcen:

- Die Post-Reihe von Steve Sanderson, einschließlich: 

    1. [Einführung: Gerüstbau Ihres ASP.NET MVC 3-Projekts mit dem mvcgerüstbau-Paket](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Standard Verwendung: Typische Anwendungsfälle und Optionen](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [1: n-Beziehungen](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Gerüstbau Aktionen und Komponenten Tests](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Überschreiben der T4-Vorlagen](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Dieser Beitrag: Erstellen von benutzerdefinierten gerünissen](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Scott Hanselman hat seinen Beitrag aus seiner PDC 2010-Sitzung [mit dem Aufbau eines Blogs mit Microsoft "Unbenanntes Paket von Web Love"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Anmerkungen zu dieser Version von MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>HTML 5-Projektvorlagen

Das Dialogfeld "Neues Projekt" enthält ein Kontrollkästchen Aktivieren von HTML 5-Versionen von Projektvorlagen. Diese Vorlagen nutzen modernizr 1,7, um Kompatibilitäts Unterstützung für HTML 5 und CSS 3 in untergeordneten Browsern bereitzustellen.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Die Razor-Ansichts-Engine

ASP.NET MVC 3 enthält eine neue Ansichts-Engine mit dem Namen Razor, die die folgenden Vorteile bietet:

- Razor-Syntax ist bereinigt und präzise und erfordert eine Mindestanzahl von Tastatureingaben.
- Razor ist einfach zu erlernen, weil es auf vorhandenen Sprachen wie C# und Visual Basic basiert.
- Visual Studio enthält IntelliSense und die Kennzeichnung von Code für die Razor-Syntax.
- Razor-Ansichten können Komponenten getestet werden, ohne dass die Anwendung ausgeführt oder ein Webserver gestartet werden muss.

Einige neue Razor-Features umfassen Folgendes:

- `@model` Syntax zum Angeben des Typs, der an die Sicht übermittelt wird.
- `@* *@` Kommentar Syntax.
- Die Möglichkeit zum Angeben von Standardwerten (z. b. `layoutpage`) für eine gesamte Website.
- Die `Html.Raw` Methode zum Anzeigen von Text ohne HTML-Codierung.
- Unterstützung für die gemeinsame Nutzung von Code für mehrere Sichten ( *\_viewstart. cshtml* -oder *\_viewstart. vbhtml* -Dateien).

Razor umfasst auch neue HTML-Hilfsprogramme, wie z. b. die folgenden:

- `Chart`. Rendert ein Diagramm und bietet die gleichen Features wie das Diagramm Steuerelement in ASP.NET 4.
- `WebGrid`. Rendert ein Datenraster, das mit Paging-und Sortierungs Funktionen fertiggestellt wird.
- `Crypto`. Verwendet Hash Algorithmen, um korrekt gesalzene und Hashwerte zu erstellen.
- `WebImage`. Rendert ein Bild.
- `WebMail`. Sendet eine E-Mail.

Weitere Informationen zu Razor finden Sie in den folgenden Ressourcen:

- [Blogbeitrag von Scott Guthrie mit Einführung in Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Blogbeitrag von Scott Guthrie mit Einführung in das @model-Schlüsselwort](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Blogbeitrag von Scott Guthrie mit Einführung in Razor-Layouts](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Schnell Referenz zu Razor-API](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Anmerkungen zu dieser Version von MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Unterstützung für mehrere Ansichts-Engines

Im Dialogfeld **Ansicht hinzufügen** in ASP.NET MVC 3 können Sie die Ansichts-Engine auswählen, mit der Sie arbeiten möchten, und im Dialogfeld **Neues Projekt** können Sie die standardmäßige Ansichts-Engine für ein Projekt angeben. Sie können das Web Forms Ansichts Modul (aspx), Razor oder eine Open Source-Ansichts-Engine (z. b. [Spark](http://sparkviewengine.com/), [nhaml](https://code.google.com/p/nhaml/)oder [ndjango](http://ndjango.org/)) auswählen.

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Controller Verbesserungen

### <a name="global-action-filters"></a>Globale Aktionsfilter

Manchmal möchten Sie die Logik entweder vor der Ausführung einer Aktionsmethode oder nach der Ausführung einer Aktionsmethode ausführen. Zur Unterstützung dieses Vorgangs haben ASP.NET MVC 2 Aktionsfilter bereitgestellt. Aktionsfilter sind benutzerdefinierte Attribute, die ein deklaratives Mittel zum Hinzufügen von vor-und nach Aktionen für bestimmte Controller Aktionsmethoden bereitstellen. In einigen Fällen möchten Sie jedoch möglicherweise das Verhalten vor oder nach der Aktion angeben, das für alle Aktionsmethoden gilt. Mit MVC 3 können Sie globale Filter angeben, indem Sie Sie der `GlobalFilters` Auflistung hinzufügen. Weitere Informationen zu globalen Aktions Filtern finden Sie in den folgenden Ressourcen:

- [Blog von Scott Guthrie in MVC 3 Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtern in ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Neue Eigenschaft "viewbag"

MVC 2-Controller unterstützen eine `ViewData`-Eigenschaft, die es Ihnen ermöglicht, mithilfe einer spät gebundenen Wörterbuch-API Daten an eine Ansichts Vorlage zu übergeben. In MVC 3 können Sie auch eine etwas einfachere Syntax mit der `ViewBag`-Eigenschaft verwenden, um den gleichen Zweck zu erreichen. Anstatt `ViewData["Message"]="text"`zu schreiben, können Sie z. b. `ViewBag.Message="text"`schreiben. Sie müssen keine stark typisierten Klassen definieren, um die `ViewBag`-Eigenschaft zu verwenden. Da es sich um eine dynamische Eigenschaft handelt, können Sie stattdessen nur Eigenschaften erhalten oder festlegen. Diese werden zur Laufzeit dynamisch aufgelöst. Intern werden `ViewBag` Eigenschaften als Name-Wert-Paare im `ViewData` Wörterbuch gespeichert. (Hinweis: in den meisten vorab Versionen von MVC 3 wurde die `ViewBag`-Eigenschaft als `ViewModel`-Eigenschaft bezeichnet.)

### <a name="new-actionresult-types"></a>Neue "Aktions Ergebnis"-Typen

Die folgenden `ActionResult` Typen und die entsprechenden Hilfsmethoden sind in MVC 3 neu oder wurden verbessert:

- [Httpnotfoundresult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Gibt den HTTP-Statuscode 404 an den Client zurück.
- [Redirectresult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Gibt eine temporäre Umleitung (HTTP 302-Statuscode) oder eine permanente Umleitung (HTTP-Statuscode 301) zurück, abhängig von einem booleschen Parameter. In Verbindung mit dieser Änderung verfügt die [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) -Klasse nun über drei Methoden zum Durchführen dauerhafter Umleitungen: `RedirectPermanent`, `RedirectToRoutePermanent`und `RedirectToActionPermanent`. Diese Methoden geben eine Instanz von `RedirectResult` zurück, bei der die `Permanent`-Eigenschaft auf `true`festgelegt ist.
- [HttpStatus coderesult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Gibt einen vom Benutzer angegebenen HTTP-Statuscode zurück.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Verbesserungen bei JavaScript und AJAX

Standardmäßig verwenden AJAX und Validierungs Hilfen in MVC 3 einen unaufdringlichen JavaScript-Ansatz. Unaufdringliches JavaScript vermeidet das Einfügen von Inline-JavaScript in HTML. Dadurch wird Ihr HTML-Format kleiner und weniger überlastet, und es vereinfacht das austauschen und Anpassen von JavaScript-Bibliotheken. Validierungs Hilfsprogramme in MVC 3 verwenden standardmäßig auch das `jQueryValidate`-Plug-in. Wenn Sie MVC 2-Verhalten möchten, können Sie unaufdringliches JavaScript mithilfe der Datei Einstellung " *Web. config* " deaktivieren. Weitere Informationen zu den Verbesserungen von JavaScript und AJAX finden Sie in den folgenden Ressourcen:

- [Grundlegende Einführung in unaufdringliches JavaScript auf der Wikipedia-Website](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Unaufdringliche Javascript-Beitrag von Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Unaufdringliche Javascript-Validierungs Beitrag von Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Erstellen einer MVC 3-Anwendung mit Razor und unaufdringlichem JavaScript](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (Tutorial auf der ASP.NET-Website)
- [Anmerkungen zu dieser Version von MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Standardmäßige Aktivierung der Client seitigen Validierung

In früheren Versionen von MVC müssen Sie die `Html.EnableClientValidation`-Methode explizit aus einer Ansicht aufzurufen, um die Client seitige Validierung zu aktivieren. In MVC 3 ist dies nicht mehr erforderlich, da die Client seitige Validierung standardmäßig aktiviert ist. (Sie können dies mithilfe einer Einstellung in der Datei " *Web. config* " deaktivieren.)

Damit die Client seitige Validierung funktioniert, müssen Sie immer noch auf die entsprechenden jQuery-und jQuery-Validierungs Bibliotheken in Ihrer Site verweisen. Sie können diese Bibliotheken auf Ihrem eigenen Server hosten oder von einem Content Delivery Network (CDN) wie CDNs von Microsoft oder Google referenzieren.

### <a name="remote-validator"></a>Remote Überprüfung

ASP.NET MVC 3 unterstützt die neue Klasse " [remoteattribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) ", die es Ihnen ermöglicht, die Remote Validierungs Steuerelement-Unterstützung des jQuery-Validierungs-Plug-ins zu nutzen. Dadurch wird es der Client seitigen Validierungs Bibliothek ermöglicht, automatisch eine benutzerdefinierte Methode aufzurufen, die Sie auf dem Server definieren, um Validierungs Logik auszuführen, die nur serverseitig ausgeführt werden kann.

Im folgenden Beispiel gibt das `Remote`-Attribut an, dass die Client Validierung eine Aktion mit dem Namen `UserNameAvailable` für die `UsersController`-Klasse aufruft, um das `UserName` Feld zu validieren.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

Das folgende Beispiel zeigt den entsprechenden Controller.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Weitere Informationen zur Verwendung des `Remote`-Attributs finden Sie unter Gewusst [wie: Implementieren der Remote Validierung in ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) in der MSDN Library.

### <a name="json-binding-support"></a>JSON-Bindungs Unterstützung

ASP.NET MVC 3 umfasst integrierte JSON-Bindungs Unterstützung, die Aktionsmethoden ermöglicht, JSON-codierte Daten zu empfangen und Sie an Aktionsmethoden Parameter zu binden. Diese Funktion ist in Szenarien nützlich, in denen Client Vorlagen und Daten Bindungen eingesetzt werden. (Mit Client Vorlagen können Sie ein einzelnes Datenelement oder einen Satz von Datenelementen formatieren und anzeigen, indem Sie Vorlagen verwenden, die auf dem Client ausgeführt werden.) MVC 3 ermöglicht es Ihnen, Client Vorlagen problemlos mit Aktionsmethoden auf dem Server zu verbinden, die JSON-Daten senden und empfangen. Weitere Informationen zur JSON-Bindungs Unterstützung finden Sie im Abschnitt zu den **Verbesserungen in JavaScript und AJAX** im [Blogbeitrag von Scott Guthrie MVC 3 Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Verbesserte Modell Validierung

### <a name="dataannotations-metadata-attributes"></a>"DataAnnotations"-Metadatenattribute

ASP.NET MVC 3 unterstützt `DataAnnotations` Metadatenattribute, z. b. `DisplayAttribute`.

### <a name="validationattribute-class"></a>"ValidationAttribute"-Klasse

Die `ValidationAttribute`-Klasse wurde in der .NET Framework 4 verbessert, um eine neue `IsValid` Überladung zu unterstützen, die weitere Informationen über den aktuellen Überprüfungs Kontext bereitstellt, z. b. das Objekt, das überprüft wird. Dies ermöglicht umfangreichere Szenarien, in denen Sie den aktuellen Wert auf Grundlage einer anderen Eigenschaft des Modells überprüfen können. Beispielsweise können Sie mit dem neuen `CompareAttribute`-Attribut die Werte von zwei Eigenschaften eines Modells vergleichen. Im folgenden Beispiel muss die `ComparePassword`-Eigenschaft dem `Password` Feld entsprechen, damit Sie gültig ist.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Validierungs Schnittstellen

Mit der [ivalidatableobject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) -Schnittstelle können Sie die Überprüfung auf Modell Ebene durchführen. Außerdem können Sie Validierungs Fehlermeldungen bereitstellen, die für den Status des Gesamt Modells oder zwischen zwei Eigenschaften innerhalb des Modells spezifisch sind. MVC 3 ruft nun Fehler von der `IValidatableObject`-Schnittstelle ab, wenn Modell Bindung verwendet wird, und kennzeichnet oder markiert die betroffenen Felder in einer Ansicht mithilfe der integrierten HTML-Formular Hilfen automatisch.

Die [iclientvalidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) -Schnittstelle ermöglicht ASP.NET MVC, zur Laufzeit zu ermitteln, ob ein Validierungs Steuerelement die Client Validierung unterstützt. Diese Schnittstelle wurde so entworfen, dass Sie in verschiedene Validierungs Frameworks integriert werden kann.

Weitere Informationen zu Validierungs Schnittstellen finden Sie im Abschnitt " **Verbesserungen der Modell Validierung** " von [Scott Guthrie MVC 3 Preview Blog Post](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Beachten Sie jedoch, dass der Verweis auf "ivalidateobject" im Blog "ivalidatableobject" lauten sollte.)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Verbesserungen in Abhängigkeit

ASP.NET MVC 3 bietet eine bessere Unterstützung für die Anwendung von Abhängigkeitsinjektion (di) und für die Integration mit Abhängigkeitsinjektion oder IOC-Containern (Inversion of Control). Unterstützung für di wurde in den folgenden Bereichen hinzugefügt:

- Controller (registrieren und Einfügen von Controller Factorys, Einfügen von Controllern).
- Sichten (registrieren und Einfügen von Ansichts-Engines, Einfügen von Abhängigkeiten in Ansichts Seiten).
- Aktionsfilter (suchen und Einfügen von Filtern).
- Modell Binder (registrieren und Einfügen).
- Modell Validierungs Anbieter (Registrierung und Einzug).
- Modellmetadatenanbieter (Registrierung und Einzug).
- Wert Anbieter (Registrierung und Einzug).

MVC 3 unterstützt die [Common Service Locator](https://github.com/unitycontainer/commonservicelocator) Library und jeden di-Container, der die `IServiceLocator`-Schnittstelle der Bibliothek unterstützt. Außerdem wird eine neue `IDependencyResolver`-Schnittstelle unterstützt, die die Integration von di-Frameworks erleichtert.

Weitere Informationen zu di in MVC 3 finden Sie in den folgenden Ressourcen:

- [Blogbeiträge von Brad Wilson zum Dienst Standort](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Anmerkungen zu dieser Version von MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Weitere neue Features

### <a name="nuget-integration"></a>Nuget-Integration

ASP.NET MVC 3 installiert und aktiviert nuget automatisch als Teil des Setups. Nuget ist ein kostenloser Open-Source-Paket-Manager, mit dem Sie problemlos .NET-Bibliotheken und-Tools in Ihren Projekten finden, installieren und verwenden können. Es funktioniert mit allen Visual Studio-Projekttypen (einschließlich ASP.net Web Forms und ASP.NET MVC).

Mit nuget können Entwickler, die Open Source-Projekte verwalten (z. b. Projekte wie z. b. "muq", "NHibernate", Ninject, StructureMap, nunit, Windsor, rhomocks und ELMAH), Ihre Bibliotheken Verpacken und in einem Onlinekatalog registrieren. Es ist dann einfach für .NET-Entwickler, die eine dieser Bibliotheken verwenden möchten, das Paket zu finden und in Projekten zu installieren, an denen Sie arbeiten.

Mit dem Update der ASP.NET 3-Tools enthalten Projektvorlagen JavaScript-Bibliotheken vorinstallierte nuget-Pakete, damit Sie über nuget aktualisiert werden können. Entity Framework Code First wird auch als nuget-Paket vorinstalliert.

Weitere Informationen zu NuGet finden Sie in der [NuGet-Dokumentation](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Zwischenspeichern von partiellen Seiten Ausgaben

ASP.NET MVC hat das Ausgabe Zwischenspeichern von vollständigen Seiten Antworten seit Version 1 unterstützt. MVC 3 unterstützt auch das Zwischenspeichern von partiellen Seiten Ausgaben, sodass Sie Bereiche und Fragmente einer Antwort problemlos Zwischenspeichern können. Weitere Informationen zum Zwischenspeichern finden Sie im Abschnitt zum zwischen **Speichern von Teil Seiten Ausgaben** von [Scott Guthrie im Blogbeitrag von MVC 3 Release Candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) und im Abschnitt zwischen **Speichern** von untergeordneten Aktionen der Anmerkungen zu dieser [Version von MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Präzise Steuerung der Anforderungs Validierung

ASP.NET MVC verfügt über eine integrierte Anforderungs Überprüfung, die automatisch vor XSS-und HTML-Injection-Angriffen geschützt wird. Manchmal möchten Sie jedoch die Anforderungs Validierung explizit deaktivieren, z. b. Wenn Sie Benutzern das Bereitstellen von HTML-Inhalten ermöglichen möchten (z. b. in Blogeinträgen oder CMS-Inhalten). Sie können jetzt ein Attribut " [attribuwhtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) " zu Modellen hinzufügen oder Modelle anzeigen, um die Anforderungs Validierung bei der Modell Bindung auf Eigenschafts Basis zu deaktivieren. Weitere Informationen zur Anforderungs Validierung finden Sie in den folgenden Ressourcen:

- Der Abschnitt **unaufdringliche Javascript und Validierung** im [Blogbeitrag von Scott Guthrie auf der MVC 3-Release Candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Anmerkungen zu dieser Version von MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Erweiterbares "Dialog Feld" Neues Projekt "

In ASP.NET MVC 3 können Sie dem Dialogfeld " **Neues Projekt** " Projektvorlagen, Ansichts Module und Projekt Frameworks für Komponententests hinzufügen.

### <a name="template-scaffolding-improvements"></a>Verbesserungen der Vorlagen

ASP.NET MVC 3-Gerüstbau Vorlagen erleichtern die Identifizierung von Primärschlüssel Eigenschaften in Modellen und deren Behandlung entsprechend als in früheren Versionen von MVC. (Die Gerüstbau Vorlagen stellen nun z. b. sicher, dass der Primärschlüssel nicht als bearbeitbares Formularfeld festgelegt wird.)

Standardmäßig verwenden die "erstellen" und "Bearbeiten"-gerükte jetzt das `Html.EditorFor` Hilfsprogramm anstelle des `Html.TextBoxFor`-Hilfsprogramms. Dadurch wird die Unterstützung für Metadaten im Modell in Form von Daten Anmerkung-Attributen verbessert, wenn im Dialogfeld **Ansicht hinzufügen** eine Ansicht generiert wird.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Neue über Ladungen für "HTML. LabelFor" und "HTML. labelformodel"

Neue Methoden Überladungen wurden für die `LabelFor`-und `LabelForModel`-Hilfsmethoden hinzugefügt. Mit den neuen über Ladungen können Sie den Beschriftungs Text angeben oder außer Kraft setzen.

### <a name="sessionless-controller-support"></a>Unterstützung für Sitzungs lose Controller

In ASP.NET MVC 3 können Sie angeben, ob eine Controller Klasse den Sitzungszustand verwenden soll. wenn dies der Fall ist, geben Sie an, ob der Sitzungszustand Lese-/Schreibzugriff oder schreibgeschützt sein soll. Weitere Informationen zur Unterstützung für Sitzungs lose Controller finden Sie in den Anmerkungen zu dieser [Version von MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Neue "AdditionalMetadataAttribute"-Klasse

Sie können das [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) -Attribut verwenden, um das `ModelMetadata.AdditionalValues` Wörterbuch für eine Modell Eigenschaft aufzufüllen. Wenn ein Ansichts Modell z. b. über eine Eigenschaft verfügt, die nur einem Administrator angezeigt werden soll, können Sie diese Eigenschaft wie im folgenden Beispiel gezeigt mit einer Anmerkung versehen:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Diese Metadaten werden für jede Anzeige-oder Editor Vorlage verfügbar gemacht, wenn ein Produkt Ansichts Modell gerendert wird. Es liegt an Ihnen, die Metadateninformationen zu interpretieren.

### <a name="accountcontroller-improvements"></a>AccountController-Verbesserungen

AccountController in der Internet Projektvorlage wurde deutlich verbessert.

### <a name="new-intranet-project-template"></a>Neue intranetprojektvorlage

Eine neue intranetprojektvorlage ist enthalten, mit der die Windows-Authentifizierung aktiviert und der AccountController entfernt wird.
