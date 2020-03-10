---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Single-Page-Anwendung: knockoutjs-Vorlage | Microsoft-Dokumentation'
author: MikeWasson
description: Knockout-Vorlage
ms.author: riande
ms.date: 01/30/2013
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 3a551db1caa9636eb7f2e04c287d3ef371263584
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467319"
---
# <a name="single-page-application-knockoutjs-template"></a>Single-Page-Anwendung: knockoutjs-Vorlage

von [Mike Wasson](https://github.com/MikeWasson)

> Die Knockout-MVC-Vorlage ist Teil von ASP.net and Web Tools 2012,2
> 
> [Download ASP.net and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650)

Das ASP.net and Web Tools 2012,2-Update enthält eine Single-Page Application (Spa)-Vorlage für ASP.NET MVC 4. Diese Vorlage ist so konzipiert, dass Sie den Einstieg in das Erstellen von interaktiven Client seitigen Web-Apps erleichtern.

Die Single-Page-Anwendung (Single-Page Application, Spa) ist der allgemeine Begriff für eine Webanwendung, die eine einzelne HTML-Seite lädt und dann die Seite dynamisch aktualisiert, anstatt neue Seiten zu laden. Nach dem anfänglichen Laden der Seite kommuniziert die Spa mit dem Server über AJAX-Anforderungen.

![](knockoutjs-template/_static/image1.png)

AJAX ist nichts neues, aber heute gibt es JavaScript-Frameworks, die das Erstellen und Verwalten einer großen anspruchsvollen Spa-Anwendung vereinfachen. Außerdem vereinfachen HTML 5 und CSS3 das Erstellen umfassender Benutzeroberflächen.

Um Ihnen den Einstieg zu erleichtern, erstellt die Spa-Vorlage eine Beispielanwendung für eine Aufgabenliste. In diesem Tutorial werden wir eine Tour durch die Vorlage machen. Zuerst sehen wir uns die Aufgabenlisten Anwendung selbst an und untersuchen dann die technologischen Komponenten, die diese funktionsfähig machen.

## <a name="create-a-new-spa-template-project"></a>Erstellen eines neuen Spa-Vorlagen Projekts

Anforderungen:

- Visual Studio 2012 oder Visual Studio Express 2012 für Web
- ASP.net Web Tools 2012,2 aktualisieren. Sie können das Update [hier](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2)installieren.

Starten Sie Visual Studio, und wählen Sie auf der Start Seite die Option **Neues Projekt** aus. Oder wählen Sie im Menü **Datei** die Option **neu** und dann **Projekt**aus.

Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten. Wählen Sie unter **Visualisierung C#** die Option **Web**aus. Wählen Sie in der Liste der Projektvorlagen die Option **ASP.NET MVC 4-Webanwendung**aus. Geben Sie dem Projekt einen Namen, und klicken Sie auf **OK**.

![](knockoutjs-template/_static/image2.png)

Wählen Sie im Assistenten für **neue Projekte** die Option **Single-Page-Anwendung**aus.

![](knockoutjs-template/_static/image3.png)

Drücken Sie F5, um die Anwendung zu erstellen und auszuführen. Wenn die Anwendung zum ersten Mal ausgeführt wird, wird ein Anmeldebildschirm angezeigt.

![](knockoutjs-template/_static/image4.png)

Klicken Sie auf den &quot;registrieren&quot; Link, und erstellen Sie einen neuen Benutzer.

![](knockoutjs-template/_static/image5.png)

Nachdem Sie sich angemeldet haben, erstellt die Anwendung eine Standard-TODO-Liste mit zwei Elementen. Sie können auf die TODO-Liste hinzufügen klicken, um eine neue Liste hinzuzufügen.

![](knockoutjs-template/_static/image6.png)

Benennen Sie die Liste um, fügen Sie der Liste Elemente hinzu, und checken Sie Sie aus. Sie können auch Elemente löschen oder eine vollständige Liste löschen. Die Änderungen werden automatisch in einer Datenbank auf dem Server persistent gespeichert (eigentlich localdb, da Sie die Anwendung lokal ausführen).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Architektur der Spa-Vorlage

Dieses Diagramm zeigt die Hauptbausteine für die Anwendung.

![](knockoutjs-template/_static/image8.png)

Auf Serverseite verarbeitet ASP.NET MVC den HTML-Code und verarbeitet außerdem die Formular basierte Authentifizierung.

ASP.net-Web-API behandelt alle Anforderungen, die sich auf die todolists und todoitems beziehen, einschließlich dem Erstellen, erstellen, aktualisieren und löschen. Der Client tauscht Daten mit der Web-API im JSON-Format aus.

Entity Framework (EF) ist die O/RM-Schicht. Er vermittelt zwischen der objektorientierten Welt von ASP.net und der zugrunde liegenden Datenbank. Die Datenbank verwendet localdb, aber Sie können dies in der Datei "Web. config" ändern. Normalerweise verwenden Sie localdb für die lokale Entwicklung und stellen dann mithilfe der EF Code First-Migration eine SQL-Datenbank auf dem Server bereit.

Auf der Clientseite verarbeitet die Knockout. js-Bibliothek Seiten Aktualisierungen von AJAX-Anforderungen. Knockout verwendet die Datenbindung, um die Seite mit den neuesten Daten zu synchronisieren. Auf diese Weise müssen Sie keinen Code schreiben, der die JSON-Daten durchläuft und das DOM aktualisiert. Stattdessen fügen Sie deklarative Attribute in den HTML-Code ein, der Knockout mitteilt, wie die Daten dargestellt werden sollen.

Ein großer Vorteil dieser Architektur besteht darin, dass die Präsentationsebene von der Anwendungslogik getrennt wird. Sie können den Web-API-Teil erstellen, ohne dass Sie wissen, wie Ihre Webseite aussehen soll. Auf der Clientseite erstellen Sie ein "Ansichts Modell", um diese Daten darzustellen, und das Ansichts Modell verwendet Knockout zum Binden an den HTML-Code. So können Sie den HTML-Code ganz einfach ändern, ohne das Ansichts Modell zu ändern. (Wir sehen uns später Knockout etwas an.)

## <a name="models"></a>Modelle

Im Visual Studio-Projekt enthält der Ordner "Models" die Modelle, die auf der Serverseite verwendet werden. (Es gibt auch Modelle auf der Clientseite.

![](knockoutjs-template/_static/image9.png)

**"Element", "Objektliste"**

Dies sind die Datenbankmodelle für Entity Framework Code First. Beachten Sie, dass diese Modelle Eigenschaften aufweisen, die aufeinander zeigen. `ToDoList` enthält eine Auflistung von todoitems, und jede `ToDoItem` weist einen Verweis auf die übergeordnete ToDoList auf. Diese Eigenschaften werden als Navigations Eigenschaften bezeichnet und stellen die 1: n-Beziehung einer Aufgabenliste und deren to-do-Elemente dar.

Die `ToDoItem`-Klasse verwendet auch das **[Foreign nkey]** -Attribut, um anzugeben, dass `ToDoListId` ein Fremdschlüssel in der `ToDoList` Tabelle ist. Dies weist EF an, der Datenbank eine FOREIGN KEY-Einschränkung hinzuzufügen.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**"-Doitemdto", "dolistdto"**

Diese Klassen definieren die Daten, die an den Client gesendet werden. "DTO" steht für "Datenübertragungs Objekt". Der DTO definiert, wie die Entitäten in JSON serialisiert werden. Im Allgemeinen gibt es mehrere Gründe für die Verwendung von DTOs:

- Um zu steuern, welche Eigenschaften serialisiert werden. Der DTO-Wert kann eine Teilmenge der Eigenschaften aus dem Domänen Modell enthalten. Dies kann aus Sicherheitsgründen der Fall sein (um sensible Daten auszublenden) oder um die Datenmenge zu reduzieren, die Sie senden.
- , Um die Form der Daten zu ändern – z. b., um eine komplexere Datenstruktur zu vereinfachen.
- , Um Geschäftslogik aus der DTO zu bewahren (Trennung von Anliegen).
- Wenn Ihre Domänen Modelle aus irgendeinem Grund nicht serialisiert werden können. Zirkel Verweise können z. b. Probleme verursachen, wenn Sie ein Objekt serialisieren. es gibt Möglichkeiten, dieses Problem in der Web-API zu behandeln (siehe [Verarbeiten von zirkulären Objekt verweisen](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); durch die Verwendung eines DTO wird das Problem jedoch einfach vermieden.

In der Spa-Vorlage enthält die DTOs die gleichen Daten wie die Domänen Modelle. Sie sind jedoch weiterhin nützlich, da Sie Zirkel Verweise aus den Navigations Eigenschaften vermeiden und das allgemeine DTO-Muster veranschaulichen.

**AccountModels.cs**

Diese Datei enthält Modelle für die Standort Mitgliedschaft. Die `UserProfile`-Klasse definiert das Schema für Benutzerprofile in der Mitgliedschafts Datenbank. (In diesem Fall handelt es sich bei den einzigen Informationen um die Benutzer-ID und den Benutzernamen.) Die anderen Modellklassen in dieser Datei werden zum Erstellen der Benutzerregistrierung und der Anmeldeformulare verwendet.

## <a name="entity-framework"></a>Entity Framework

Die Spa-Vorlage verwendet EF-Code First. In Code First Entwicklung definieren Sie die Modelle zuerst im Code, und EF verwendet das Modell, um die Datenbank zu erstellen. Sie können EF auch mit einer vorhandenen Datenbank ([Database First](https://msdn.microsoft.com/data/jj206878.aspx)) verwenden.

Die `TodoItemContext`-Klasse im Ordner "Models" wird von **dbcontext**abgeleitet. Diese Klasse stellt den "Klebstoff" zwischen den Modellen und EF bereit. Der `TodoItemContext` enthält eine `ToDoItem` Auflistung und eine `TodoList` Auflistung. Um die Datenbank abzufragen, schreiben Sie einfach eine LINQ-Abfrage für diese Sammlungen. Hier sehen Sie beispielsweise, wie Sie alle Aufgabenlisten für den Benutzer "Alice" auswählen können:

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Sie können der Auflistung auch neue Elemente hinzufügen, Elemente aktualisieren oder Elemente aus der Sammlung löschen und die Änderungen in der Datenbank beibehalten.

## <a name="aspnet-web-api-controllers"></a>ASP.net-Web-API Controller

In ASP.net-Web-API sind Controller Objekte, die HTTP-Anforderungen verarbeiten. Wie bereits erwähnt, verwendet die Spa-Vorlage die Web-API, um CRUD-Vorgänge auf `ToDoList`-und `ToDoItem` Instanzen zu aktivieren. Die Controller befinden sich im Ordner "Controllers" der Projekt Mappe.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: verarbeitet HTTP-Anforderungen für to-do-Elemente.
- `TodoListController`: verarbeitet HTTP-Anforderungen für Aufgabenlisten.

Diese Namen sind von Bedeutung, da die Web-API dem URI-Pfad mit dem Controller Namen entspricht. (Informationen zur Weiterleitung von HTTP-Anforderungen an Controller durch die Web-API finden Sie unter [Routing in ASP.net-Web-API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Sehen wir uns die `ToDoListController`-Klasse an. Sie enthält einen einzelnen Datenmember:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

Der `TodoItemContext` wird für die Kommunikation mit EF verwendet, wie zuvor beschrieben. Die-Methoden auf dem Controller implementieren die CRUD-Vorgänge. Die Web-API ordnet HTTP-Anforderungen vom Client den Controller Methoden wie folgt zu:

| HTTP-Anforderung | Controller-Methode | Beschreibung |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | Ruft eine Auflistung von to-do-Listen ab. |
| /API/TODO/-*ID* erhalten | `GetTodoList` | Ruft eine Aufgabenliste nach ID ab. |
| /API/TODO/-*ID* platzieren | `PutTodoList` | Aktualisiert eine Aufgabenliste. |
| POST /api/todo | `PostTodoList` | Erstellt eine neue Aufgabenliste. |
| /API/TODO/-*ID* löschen | `DeleteTodoList` | Löscht eine TODO-Liste. |

Beachten Sie, dass die URIs für einige Vorgänge Platzhalter für den ID-Wert enthalten. Wenn Sie z. b. eine Liste mit der ID 42 löschen möchten, wird der URI `/api/todo/42`.

Weitere Informationen zur Verwendung der Web-API für CRUD-Vorgänge finden Sie unter [Erstellen einer Web-API, die CRUD-Vorgänge unterstützt](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Der Code für diesen Controller ist recht unkompliziert. Hier sind einige interessante Punkte:

- Die `GetTodoLists`-Methode verwendet eine LINQ-Abfrage, um die Ergebnisse nach der ID des angemeldeten Benutzers zu filtern. Auf diese Weise werden einem Benutzer nur die Daten angezeigt, die ihm gehören. Beachten Sie außerdem, dass eine SELECT-Anweisung verwendet wird, um die `ToDoList` Instanzen in `TodoListDto` Instanzen zu konvertieren.
- Die Put-und Post-Methoden überprüfen den Modell Status, bevor Sie die Datenbank ändern. Wenn " **modelstate. IsValid** " den Wert "false" aufweist, geben diese Methoden HTTP 400, ungültige Anforderung. Weitere Informationen zur Modell Validierung in der Web-API finden Sie unter [Modell Validierung](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- Die Controller Klasse wird auch mit dem **[autorisiert]** -Attribut versehen. Dieses Attribut überprüft, ob die HTTP-Anforderung authentifiziert ist. Wenn die Anforderung nicht authentifiziert ist, empfängt der Client HTTP 401, nicht autorisiert. Weitere Informationen zur Authentifizierung finden Sie unter [Authentifizierung und Autorisierung in ASP.net-Web-API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

Die `TodoController`-Klasse ähnelt `TodoListController`. Der größte Unterschied besteht darin, dass keine Get-Methoden definiert werden, da der Client die to-do-Elemente zusammen mit jeder Aufgabenliste erhält.

## <a name="mvc-controllers-and-views"></a>MVC-Controller und-Ansichten

Die MVC-Controller befinden sich auch im Ordner "Controllers" der Projekt Mappe. `HomeController` rendert den Haupt-HTML-Code für die Anwendung. Die Ansicht für den Home-Controller ist in views/Home/Index. cshtml definiert. Die Start Ansicht rendert unterschiedliche Inhalte, je nachdem, ob der Benutzer angemeldet ist:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Wenn Benutzer angemeldet sind, wird die Hauptbenutzer Oberfläche angezeigt. Andernfalls wird der Anmelde Panel angezeigt. Beachten Sie, dass dieses bedingte Rendering auf der Serverseite erfolgt. Versuchen Sie niemals, sensible Inhalte auf der Clientseite&#8212;auszublenden, was Sie in einer HTTP-Antwort senden, ist für jemanden sichtbar, der die unformatierten HTTP-Nachrichten überwacht.

## <a name="client-side-javascript-and-knockoutjs"></a>Client seitiges JavaScript und Knockout. js

Nun schalten wir die Serverseite der Anwendung auf den Client um. Die Spa-Vorlage verwendet eine Kombination aus jQuery und Knockout. js, um eine reibungslose, interaktive Benutzeroberfläche zu erstellen. Knockout. js ist eine JavaScript-Bibliothek, die das Binden von HTML an Daten vereinfacht. Knockout. js verwendet ein Muster mit dem Namen "Model-View-ViewModel".

- Beim Modell handelt es sich um die Domänen Daten (TODO-Listen und TODO-Elemente).
- Die Ansicht ist das HTML-Dokument.
- Das View-Model ist ein JavaScript-Objekt, das die Modelldaten enthält. Das Ansichts Modell ist eine Code Abstraktion der Benutzeroberfläche. Die HTML-Darstellung ist nicht bekannt. Stattdessen stellt Sie abstrakte Funktionen der Sicht dar, z. b. "eine Liste von TODO-Elementen".

Die Ansicht ist Daten gebunden an das Ansichts Modell. Updates für das Ansichts Modell werden automatisch in der Ansicht widergespiegelt. Bindungen funktionieren auch in der anderen Richtung. Ereignisse im Dom (z. b. Klicks) sind Daten gebunden an Funktionen im Ansichts Modell, die AJAX-Aufrufe auslöst.

Die Spa-Vorlage organisiert das Client seitige JavaScript in drei Ebenen:

- TODO. DataContext. js: sendet AJAX-Anforderungen.
- TODO. Model. js: definiert die Modelle.
- TODO. ViewModel. js: definiert das Ansichts Modell.

![](knockoutjs-template/_static/image11.png)

Diese Skriptdateien befinden sich im Ordner "Scripts/app" der Projekt Mappe.

![](knockoutjs-template/_static/image12.png)

**TODO. DataContext** verarbeitet alle AJAX-Aufrufe an die Web-API-Controller. (Die AJAX-Aufrufe für die Anmeldung werden an anderer Stelle in "ajaxLogin. js" definiert.)

**TODO. Model. js** definiert die Client seitigen (Browser-) Modelle für die Aufgabenlisten. Es gibt zwei Modellklassen: "tdoitem" und "tdolist".

Viele der Eigenschaften in den Modellklassen sind vom Typ "ko. Observable". Observables gibt an, wie Knockout seine Magie macht. Aus der [Knockout-Dokumentation](http://knockoutjs.com/documentation/introduction.html): ein Observable-Objekt ist ein JavaScript-Objekt, das Abonnenten über Änderungen benachrichtigen kann. Wenn der Wert eines Observable-Elements geändert wird, aktualisiert Knockout alle HTML-Elemente, die an diese Observables gebunden sind. Beispielsweise hat "" für "" die Eigenschaft "Title" und "isdone" die Eigenschaft "Title":

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Sie können auch ein Observable-Element im Code abonnieren. Beispielsweise abonniert die Klasse "dedoitem" Änderungen in den Eigenschaften "isdone" und "Title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Modell anzeigen**

Das Ansichts Modell ist in TODO. ViewModel. js definiert. Das Ansichts Modell ist der zentrale Punkt, an dem die Anwendung die HTML-Seitenelemente an die Domänen Daten bindet. In der Spa-Vorlage enthält das Ansichts Modell ein Observable-Array von "Aufgabenlisten". Der folgende Code im Ansichts Modell weist Knockout an, die Bindungen anzuwenden:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML und Datenbindung

Der Haupt-HTML-Code für die Seite wird in views/Home/Index. cshtml definiert. Da die Datenbindung verwendet wird, ist der HTML-Code nur eine Vorlage für das, was tatsächlich gerendert wird. Knockout verwendet *deklarative* Bindungen. Sie binden Seitenelemente an Daten, indem Sie dem-Element ein "Data-BIND"-Attribut hinzufügen. Hier ist ein sehr einfaches Beispiel, das aus der Knockout-Dokumentation stammt:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

In diesem Beispiel aktualisiert Knockout den Inhalt des **&lt;spannen&gt;** Elements mit dem Wert `myItems.count()`. Wenn sich dieser Wert ändert, aktualisiert Knockout das Dokument.

Knockout stellt eine Reihe von unterschiedlichen Bindungs Typen bereit. Im folgenden finden Sie einige der Bindungen, die in der Spa-Vorlage verwendet werden:

- **foreach**: Hiermit können Sie eine Schleife durchlaufen und für jedes Element in der Liste das gleiche Markup anwenden. Dies wird zum Rendering der Aufgabenlisten und Aufgaben Elemente verwendet. Innerhalb von **foreach**werden die Bindungen auf die Elemente der Liste angewendet.
- **Visible**: dient zum Umschalten der Sichtbarkeit. Blenden Sie Markup aus, wenn eine Auflistung leer ist, oder machen Sie die Fehlermeldung sichtbar.
- **value**: wird verwendet, um Formular Werte aufzufüllen.
- **Click**: bindet ein Click-Ereignis an eine Funktion im Ansichts Modell.

## <a name="anti-csrf-protection"></a>Anti-CSRF-Schutz

Website übergreifende Anforderungs Fälschung (Cross-Site Request Fälschung, CSRF) ist ein Angriff, bei dem ein böswilliger Standort eine Anforderung an einen anfälligen Standort sendet, an dem der Benutzer zurzeit angemeldet ist Um CSRF-Angriffe zu verhindern, verwendet ASP.NET MVC *antifälschungstokentoken*, auch als Anforderungs Überprüfungs Token bezeichnet. Die Idee besteht darin, dass der Server ein zufällig generiertes Token in eine Webseite einfügt. Wenn der Client Daten an den Server übermittelt, muss er diesen Wert in die Anforderungs Nachricht einschließen.

Anti-fälschungstoken funktionieren, weil die böswillige Seite die Token des Benutzers aufgrund von Richtlinien für denselben Ursprung nicht lesen kann. (Die gleichen Ursprungs Richtlinien verhindern, dass Dokumente, die auf zwei unterschiedlichen Standorten gehostet werden, auf die Inhalte der anderen zugreifen.)

ASP.NET MVC bietet integrierte Unterstützung für antifälschungstoken, über die [antifälschungs](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) Klasse und das [[validateantiforgerytoken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) -Attribut. Diese Funktion ist derzeit nicht in die Web-API integriert. Die Spa-Vorlage enthält jedoch eine benutzerdefinierte Implementierung für die Web-API. Dieser Code wird in der `ValidateHttpAntiForgeryTokenAttribute`-Klasse definiert, die sich im Ordner "Filters" der Projekt Mappe befindet. Weitere Informationen zu Anti-CSRF in der Web-API finden Sie unter [verhindern von Cross-Site Request Fälschungs Angriffen (CSRF)](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Zusammenfassung

Die Spa-Vorlage ist so konzipiert, dass Sie schnell mit dem Schreiben moderner, interaktiver Webanwendungen beginnen können. Es verwendet die Knockout. js-Bibliothek, um die Darstellung (HTML-Markup) von der Daten-und Anwendungslogik zu trennen. Knockout ist jedoch nicht die einzige JavaScript-Bibliothek, die Sie zum Erstellen einer Spa verwenden können. Wenn Sie andere Optionen erkunden möchten, sehen Sie sich die von der [Community erstellten Spa-Vorlagen](../templates/index.md)an.
